# Docker-compose_override

docker-compose.ymlファイルを上書きする。

## ファイルの内容

### docker-compose.yml

``` yml
version: '3'
services:
  sample:
    image: ubuntu:latest
    environment:
      - TMP_VALUE=1
      - TMP_VALUE2=2
    volumes:
      - ./start.sh:/start.sh
    entrypoint: sh /start.sh
```

### docker-compose_override.yml

``` yml
version: '3'
services:
  sample:
    environment:
      - TMP_VALUE=100
```

### start.sh

``` sh
#!/bin/bash

echo $TMP_VALUE
echo $TMP_VALUE2
```

## 実行

後優先。

```
$ docker-compose -f docker-compose.yml -f docker-compose_override.yml up
Building with native build. Learn about native build in Compose here: https://docs.docker.com/go/compose-native-build/
Recreating docker-compose_override_sample_1 ... done
Attaching to docker-compose_override_sample_1
sample_1  | 100
sample_1  | 2
docker-compose_override_sample_1 exited with code 0
```

```
$ docker-compose -f docker-compose_override.yml -f docker-compose.yml up
Building with native build. Learn about native build in Compose here: https://docs.docker.com/go/compose-native-build/
Recreating docker-compose_override_sample_1 ... done
Attaching to docker-compose_override_sample_1
sample_1  | 1
sample_1  | 2
docker-compose_override_sample_1 exited with code 0
```