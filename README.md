# Local Hugo Setup

This is my local Hugo testing setup, based on `docker-compose`.

New blog setup (one-time execution), per [Hugomods pre-requisites](https://docker.hugomods.com/docs/development/prerequisite/):

```sh
sudo docker run -v ${PWD}:/src -u 1000:1000 hugomods/hugo:exts-non-root hugo new site lopeztel_blog --format yaml
```

Afterwards, go to the generated directory and deploy with docker compose, per [Hugomods development with docker compose](https://docker.hugomods.com/docs/development/docker-compose/):

```sh
sudo docker-compose up server
```

## Installing a theme

I've followed the guide for [Papermod](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation) to install (as a git submodule)
