# isucon 7 qualify on docker
isucon 7をDocker化したもの

# 動かし方

```
$ docker-compose up
```

docker-composeで動かせる  
デフォルトだとGoが立ち上がる

## bench

```
$ docker-compose up -d
```

起動後に

```
$ docker-compose run bench
```

result.jsonは `/root/isubata/bench` に出力されるのでvolumesを使って取り出すと良さそう
