## Laboratory work 8

Установим Докер на компьютер. Воспользуемся сайтом: https://docs.docker.com/engine/install/ubuntu/ - гайд по установке. 

Напишем Dockerfile со следующим содержимым:
```
FROM ubuntu:18.04

RUN apt update
RUN apt install -yy gcc g++ cmake

COPY . print/
WORKDIR print

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install

ENV LOG_PATH /home/logs/log.txt

VOLUME /home/logs

WORKDIR _install/bin

ENTRYPOINT ./demo
```
И изменим файл yml
```
name: Docker CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build Docker image
        run: docker build -t logger .
        
      - name: Run Docker container
        run: docker run -v "$(pwd)/logs/:/home/logs/" logger

      - name: Check logs directory
        run: ls -R logs/

      - name: Upload logs
        uses: actions/upload-artifact@v4
        with:
          name: logs
          path: logs/
```
