---
title: RGB WiFi enabled lamp (Part 2)
author: lopeztel
date: 2021-07-01T09:21:50+00:00
url: /2021/07/01/rgb-wifi-enabled-lamp-part-2/
tags:
  - 100DaysToOffload
  - esp8266
  - RGBLamp

---
As promised in my previous [entry](https://lopeztel.xyz/blog/2021/06/24/rgb-wifi-enabled-lamp-part-1/), I&#8217;ll go into some details related to how I implemented effects on my RGB Lamp:

All the code is available at my [github repo](https://github.com/lopeztel/esp8266_NeoPixel) or the [mirror repo](https://lopeztel.noho.st/gitea/lopeztel/esp8266_NeoPixel) on my self-hosted gitea instance

The idea is to have mqtt messages triggering different behavior in my code, the design consists in triggering a periodic function to change LED RGB values depending on the message topic and payload

I&#8217;ll try to explain with code chunks, the lamp has 3 operation modes: _Solid_, _Fade_ and _Rainbow_

## Solid {#solid}

This was the very first transition implemented, simple color setting depending on a 32 bit RGB value (8 bits per color)

  * function `mqttDataCb` is called with topic `/mqtt/lights/solid`
  * payload is assigned to a boolean and other mode booleans (_fade_ and _rainbow_) are set to false. RGB combination is assigned to all LEDs

```c
void mqttDataCb(uint32_t *args, const char *topic, uint32_t topic_len,
                const char *data, uint32_t data_len) {
  char *topicBuf = (char *)os_zalloc(topic_len + 1),
       *dataBuf = (char *)os_zalloc(data_len + 1);

  MQTT_Client *client = (MQTT_Client *)args;

  os_memcpy(topicBuf, topic, topic_len);
  topicBuf[topic_len] = 0;

  os_memcpy(dataBuf, data, data_len);
  dataBuf[data_len] = 0;

  INFO_MSG("Receive topic: %s, data: %s length: %d \r\n", topicBuf, dataBuf,
           data_len);
  int isRed = strcmp(topicBuf, "/mqtt/lights/red");
  int isGreen = strcmp(topicBuf, "/mqtt/lights/green");
  int isBlue = strcmp(topicBuf, "/mqtt/lights/blue");
  int isFade = strcmp(topicBuf, "/mqtt/lights/fade");
  int isSolid = strcmp(topicBuf, "/mqtt/lights/solid");
  int isRainbow = strcmp(topicBuf, "/mqtt/lights/rainbow");
  INFO_MSG("topic comparison result: R %d, G %d, B %d, fade %d, solid%d\r\n",
           isRed, isGreen, isBlue, isFade, isSolid);
  if (isRed == 0) {
    received_red = dataBuf[0];
  } else if (isGreen == 0) {
    received_green = dataBuf[0];
  } else if (isBlue == 0) {
    received_blue = dataBuf[0];
  } else if (isFade == 0) {
    fade = dataBuf[0];
    solid = false;
    rainbow = false;
  } else if (isSolid == 0) {
    solid = dataBuf[0];
    fade = false;
    rainbow = false;
  } else if (isRainbow == 0) {
    rainbow = dataBuf[0];
    solid = false;
    fade = false;
  }

  os_free(topicBuf);
  os_free(dataBuf);

  if (solid) {
    os_timer_disarm(&fadeTimer); //Disable timer for periodic function

    INFO_MSG("Updated color is R:%d G:%d B:%d \r\n", received_red,
             received_green, received_blue);
    int i;
    for (i = 0; i < NUMPIXELS; i++) {
      // Color takes RGB values, from 0,0,0 up to 255,255,255
      setPixelColor(&ledstrip, i,
                    color_rgb(received_red, received_green, received_blue));
      system_soft_wdt_feed();
    }
    show(&ledstrip);
  } else if (fade) {
   ... //Redacted for convenience
  }else if(rainbow){
   ... //Redacted for convenience
  }
}
```

  * Nothing else to do here, the _Solid_ mode is simple and doesn&#8217;t need a periodic function to change anything, which is why the timer to trigger said function is just disabled (in case the previous mode depended on it) with `os_timer_disarm(&fade_timer)` (more on this available in [Espressif&#8217;s nonos SDK](https://www.espressif.com/sites/default/files/documentation/2c-esp8266_non_os_sdk_api_reference_en.pdf))

![Solid](https://lopeztel.noho.st/piwigo/galleries/blog_media/Demo_compressed.gif#center)

## Fade {#fade}

I call the first one _Fade_ because of the soft transitions between colors&#8230;

  * function `mqttDataCb` is called with topic `/mqtt/lights/fade`
  * after the payload value is assigned to a boolean, the periodic function timer is disarmed, some initialization happens: all LEDs are initialized to color blue, and a state machine is re-initialized (to start from a known state) This is the state machine declaration:

```c
enum fade_state{
  to_purple,
  to_red,
  to_orange,
  to_green,
  to_teal,
  to_blue
};

enum fade_state FADESTATE = to_purple; //state machine initial state
```

This is the missing chunk in function `mqttDataCb`:

```c
} else if (fade) {
  fade_red = 0; //reset fade values
  fade_green = 0;
  fade_blue = 255;
  FADESTATE = to_purple;
  // MQTT_Publish(client,"/mqtt/lights/solid",0,1,2,0);
  for (uint8_t i = 0; i < NUMPIXELS; i++) {
    // Color takes RGB values, from 0,0,0 up to 255,255,255
    setPixelColor(&ledstrip, i, color_rgb(fade_red, fade_green, fade_blue));
    system_soft_wdt_feed();
  }
  show(&ledstrip);

  //initialize timer
  os_timer_disarm(&fadeTimer);
  os_timer_setfn(&fadeTimer, (os_timer_func_t *)fadeFunction, NULL);
  os_timer_arm(&fadeTimer, FADE_DELAY, 0);
}else if(rainbow){
```

  * This function basically kickstarts the timer that calls `fadeFunction`, which in turn disarms the timer, checks which mode of operation is active and, in this case, cycles through the super fancy state machine (implemented as a simple switch statement), it just saturates or depletes a color to create the color transitions:

```c
static void ICACHE_FLASH_ATTR fadeFunction(void *arg) {
  os_timer_disarm(&fadeTimer);
  if (fade) {
    switch (FADESTATE) { //Fade state machine
    case to_purple:
      if (fade_red < 255) { // purple
        fade_red += 51;
      } else {
        FADESTATE = to_red;
      }
      break;
    case to_red:
      if (fade_blue > 0) { // red
        fade_blue -= 51;
      } else {
        FADESTATE = to_orange;
      }
      break;
    case to_orange:
      if (fade_green < 255) { // orange
        fade_green += 51;
      } else {
        FADESTATE = to_green;
      }
      break;
    case to_green:
      if (fade_red > 0) { // green
        fade_red -= 51;
      } else {
        FADESTATE = to_teal;
      }
      break;
    case to_teal:
      if (fade_blue < 255) { // teal
        fade_blue += 51;
      } else {
        FADESTATE = to_blue;
      }
      break;
    case to_blue:
      if (fade_green > 0) { // blue
        fade_green -= 51;
      } else {
        FADESTATE = to_purple;
      }
      break;
    default:
      break;
    }
    
    for (uint8_t i = 0; i < NUMPIXELS; i++) {
      setPixelColor(&ledstrip, i, color_rgb(fade_red, fade_green, fade_blue));
      system_soft_wdt_feed();
    }
    show(&ledstrip);

  }else if(rainbow){

  ... // Redacted for convenience

  }
  os_timer_setfn(&fadeTimer, (os_timer_func_t *)fadeFunction, NULL);
  os_timer_arm(&fadeTimer, FADE_DELAY, 0);
}
```

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/fade.gif#center)

## Rainbow {#rainbow}

This is the most flashy (pun intended) mode of operation, it took what I call LED remapping (basically messing around with pointers), the soldering method shown in [Thingiverse](https://www.thingiverse.com/thing:4129249) makes it hard to use simple mathematical operations to set the color LEDs periodically, initially I thought that using something like `LED_index % 9` in a switch case would be enough but that doesn&#8217;t work for the snake like pattern I have, perhaps a diagram can illustrate what I mean:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/LED_strip_compressed-me.png#center)

Essentially what I wanted was to have LEDs 0, 17, 18, 35, 36, 53, 54, 71 be &#8220;grouped&#8221; together and have the same color. Same applies to all others&#8230; So I created some **structs** that group the LEDs like this and had to manually point their elements to the corresponding LED numbers:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Led_strip_rearranged_compressed-me.png#center)

This are the corresponding chunks of code (not sure if function `ledstrip_remap()` is the best way to deal with the remapping, I&#8217;m pretty sure there should be a way to define this at compile time, food for thought):

```c
#define NUMPIXELS	72
#define COLOR_COMBINATIONS      6
#define LINES_IN_STRIP  9
#define LEDS_PER_LINE   8

typedef union{
    struct{ //LSB to MSB
        uint8_t red:8;
        uint8_t green:8;
        uint8_t blue:8;
    }b;
    uint32_t w;
}color;

typedef struct leds{
    color LED_ARRAY[NUMPIXELS]; // led complete array
}LEDS;

typedef struct ledline{
    color *LED_LINE_ELEMENTS[LEDS_PER_LINE]; // leds in line
}LEDLINE;

typedef struct ledstrip{
    LEDLINE LED_LINES_IN_STRIP[LINES_IN_STRIP]; // led lines in strip
}LEDSTRIP;


LEDS all_leds;
LEDSTRIP myledstrip;
color rainbow_array[COLOR_COMBINATIONS];


void ledstrip_remap() { // it is important to rework this function if the total
                        // number of leds in your strip is not 72 (arranged in 9
                        // lines x 8 elements each)
  // 1st ring
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[0];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[17];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[18];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[35];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[36];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[53];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[54];
  myledstrip.LED_LINES_IN_STRIP[0].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[71];

  // 2nd ring
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[1];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[16];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[19];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[34];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[37];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[52];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[55];
  myledstrip.LED_LINES_IN_STRIP[1].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[70];

  // 3rd ring
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[2];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[15];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[20];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[33];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[38];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[51];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[56];
  myledstrip.LED_LINES_IN_STRIP[2].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[69];

  // 4th ring
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[3];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[14];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[21];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[32];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[39];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[50];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[57];
  myledstrip.LED_LINES_IN_STRIP[3].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[68];

  // 5th ring
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[4];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[13];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[22];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[31];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[40];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[49];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[58];
  myledstrip.LED_LINES_IN_STRIP[4].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[67];

  // 6th ring
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[5];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[12];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[23];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[30];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[41];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[48];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[59];
  myledstrip.LED_LINES_IN_STRIP[5].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[66];

  // 7th ring
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[6];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[11];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[24];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[29];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[42];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[47];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[60];
  myledstrip.LED_LINES_IN_STRIP[6].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[65];

  // 8th ring
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[7];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[10];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[25];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[28];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[43];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[46];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[61];
  myledstrip.LED_LINES_IN_STRIP[7].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[64];

  // 9th ring
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[0] =
      &all_leds.LED_ARRAY[8];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[1] =
      &all_leds.LED_ARRAY[9];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[2] =
      &all_leds.LED_ARRAY[26];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[3] =
      &all_leds.LED_ARRAY[27];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[4] =
      &all_leds.LED_ARRAY[44];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[5] =
      &all_leds.LED_ARRAY[45];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[6] =
      &all_leds.LED_ARRAY[62];
  myledstrip.LED_LINES_IN_STRIP[8].LED_LINE_ELEMENTS[7] =
      &all_leds.LED_ARRAY[63];
}

void set_rainbow_element(uint8_t element_num, uint8_t red, uint8_t green, uint8_t blue){
    rainbow_array[element_num].b.red = red;
    rainbow_array[element_num].b.green = blue;
    rainbow_array[element_num].b.blue = green;
}

void init_rainbow_array() { // it is important to rework this function if the
                            // desired color combination array doesn't have 6
                            // elements
  set_rainbow_element(0, 0, 0, 255);   // blue
  set_rainbow_element(1, 255, 0, 255); // purple
  set_rainbow_element(2, 255, 0, 0);   // red
  set_rainbow_element(3, 255, 255, 0); // orange
  set_rainbow_element(4, 0, 255, 0);   // green
  set_rainbow_element(5, 0, 255, 255); // teal
}

void rotate_rainbow_array(){
    color temp = rainbow_array[0];
    for (uint8_t i = 0; i < COLOR_COMBINATIONS -1; i++) {
        rainbow_array[i] = rainbow_array[i+1];
    }
    rainbow_array[COLOR_COMBINATIONS-1] = temp;
}

void set_strip_line_color(uint8_t line_num, color assigned_color) {
  for (uint8_t i = 0; i < LEDS_PER_LINE; i++) {
    myledstrip.LED_LINES_IN_STRIP[line_num].LED_LINE_ELEMENTS[i]->b.red =
        assigned_color.b.red;
    myledstrip.LED_LINES_IN_STRIP[line_num].LED_LINE_ELEMENTS[i]->b.green =
        assigned_color.b.green;
    myledstrip.LED_LINES_IN_STRIP[line_num].LED_LINE_ELEMENTS[i]->b.blue =
        assigned_color.b.blue;
  }
}
```

Then the missing chunk of code in function `fadeFunction` is simpler:

```c
  }else if(rainbow){
    rotate_rainbow_array();
    for (uint8_t i = 0; i < LINES_IN_STRIP; i++) {
      if (i < COLOR_COMBINATIONS) {
        set_strip_line_color(i, rainbow_array[i]);
      } else {
        set_strip_line_color(i, rainbow_array[i - COLOR_COMBINATIONS]);
      }
    }

    for (uint8_t i = 0; i < NUMPIXELS; i++) {
      // setPixelColor takes RGB values, from 0,0,0 up to 255,255,255, 
      // this function is part of the Adafruit_NeoPixel library
      setPixelColor(&ledstrip, i,
                    color_rgb(all_leds.LED_ARRAY[i].b.red,
                              all_leds.LED_ARRAY[i].b.green,
                              all_leds.LED_ARRAY[i].b.blue));
      system_soft_wdt_feed();
    }
    show(&ledstrip);
  }
```

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/rainbow.gif#center)

This is my first attempt at explaining code in a blog post, hopefully the idea got across, at least writing this helps me organize my thoughts

---

Day 15 of my 2021&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
