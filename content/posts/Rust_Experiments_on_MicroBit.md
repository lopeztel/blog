---
title: "Rust experiments on BBC MicroBit"
date: 2022-08-28T19:17:53+02:00
url: /2022/08/28/Rust_Experiments_on_MicroBit/
tags:
  - 100DaysToOffload
  - rust
  - nRF52833
  - microbit
---

I'm writing this post because I basically wanted to learn rust, and doing so
through a practical example for embedded would be a nice way to do it. I'll be
basically ripping off from the book
[Discover](https://docs.rust-embedded.org/discovery/), available at
[https://www.rust-lang.org/what/embedded](https://www.rust-lang.org/what/embedded) (it is important to mention that I am using a MicroBit v2.00)

## Rust environment

I started by installing rust via [rustup](https://www.rust-lang.org/tools/install):

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Then installing a debugger and a serial terminal cli program:

```bash
sudo dnf install gdb minicom
```

Updating udev rules for microbit, requires creating `99-microbit.rules`:

```
# CMSIS-DAP for microbit
SUBSYSTEM=="usb", ATTR{idVendor}=="0d28", ATTR{idProduct}=="0204", MODE:="666"

```

under the following path `/etc/udev/rules.d/`, then reloaded udev rules by typing:

```bash
sudo udevadm control --reload-rules
```

Something that wasn't obvious from the next steps past the environment setup in the book was where to find the source code but some searching did the trick and I found the repo and cloned it

```bash
git clone https://github.com/rust-embedded/discovery.git
```

Also not explicitly mentioned but the `embed` subcommand must be available for cargo, so I had to install it:

```bash
cargo install cargo-embed
``` 

Which in turn failed (something about libudev package not found), so I went to the [cargo-embed github repo](https://github.com/probe-rs/cargo-embed) and discovered I was missing some requirements:

```bash
sudo dnf install systemd-devel
```

## First test

Then, to prepare for my architecture (BBC Microbit v2.00 is ARMv7), some modifications to file `Embed.toml` under path `<repo_root>/microbit/src/03-setup/` had to be made:

```
[default.general]
chip = "nrf52833_xxAA" # uncomment this line for micro:bit V2
# chip = "nrf51822_xxAA" # uncomment this line for micro:bit V1

```

Now, to load the sample application under the same directory, I ran:

```bash
rustup target add thumbv7em-none-eabihf
cargo embed --target thumbv7em-none-eabihf
```

Which leads to the following output in the terminal:

```console
 cargo embed --target thumbv7em-none-eabihf
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
      Config default
      Target /home/lopeztel/discovery/microbit/target/thumbv7em-none-eabihf/debug/rtt-check
     Erasing sectors ✔ [00:00:00] [##############] 16.00KiB/16.00KiB @ 33.29KiB/s (eta 0s )
 Programming pages   ✔ [00:00:00] [##############] 16.00KiB/16.00KiB @ 11.58KiB/s (eta 0s )
    Finished flashing in 0.969s

```

Followed by a terminal window that prints the output `Hello World` (program running in the MicroBit):

![Console output](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot_2022-08-28-21-42-33.png)

---

Day 4 of my 2022's [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)
