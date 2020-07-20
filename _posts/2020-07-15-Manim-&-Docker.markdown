# Manim & Docker

You don't know 3blue1brown?

Check this!

[youtube](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw)

[bilibili](https://space.bilibili.com/88461692/)

[Manim](https://github.com/3b1b/manim)

A lots of lib to install! A lots of enviroments to config!!

Of course there is a docker image of [manim](https://hub.docker.com/r/eulertour/manim/tags/). But the lastest version was build a year ago. Some API are not match for current tutorial.

## Build Manim image of your own!

You could find Dockerfile in the repo. Use this file to build a image
```
docker build -t="mymanim" .
```

After finish  `docker image ls`
```
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
mymanim                  latest              7d078c1d42b8        2 weeks ago         3.01GB
<none>                   <none>              5b5fdf43ec52        2 weeks ago         919MB
```

## Use Your manim By Docker compose.

This file describles two service(docker container).
```
version: '3.1'

services:
  manim:
    # comment this line if you build the image to prevent overwriting the tag
    image: eulertour/manim:latest
    # uncomment this line to build rather than pull the image
    # build: .
    entrypoint:
      - manim
      - --media_dir=/tmp/output
    volumes:
      - ${INPUT_PATH:?INPUT_PATH environment variable isn't set}:/tmp/input
      - ${OUTPUT_PATH:?OUTPUT_PATH environment variable isn't set}:/tmp/output
    working_dir: /tmp/input
    network_mode: "none"
  mymanim:
    image: mymanim:latest
    entrypoint:
      - manim
      - --media_dir=/tmp/output
    volumes:
      - ${INPUT_PATH:?INPUT_PATH environment variable isn't set}:/tmp/input
      - ${OUTPUT_PATH:?OUTPUT_PATH environment variable isn't set}:/tmp/output
    working_dir: /tmp/input
    network_mode: "none"

```

[docker-compose run](https://docs.docker.com/compose/reference/run/)

docker-compose run is very useful when run a one-time service.

Use these command to run [examples](https://github.com/lumixraku/my-manim) of manim.
```
INPUT_PATH=~/repos/Others/my-manim \
OUTPUT_PATH=~/repos/Others/manimOutput \
docker-compose run mymanim say_i_love_u_by_math.py scene1 -l
```