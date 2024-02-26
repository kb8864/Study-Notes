# docker imagesで

```
% docker images
REPOSITORY                          TAG            IMAGE ID       CREATED         SIZE
udemy_demoapp_v1-api                latest         36b680c9d5fc   2 hours ago     815MB
```
%
docker inspect -f="{{ .ContainerConfig.Cmd }}" 36b680c9d5fc
[]

#Compose と Rails
[docker-compose.yml](https://docs.docker.jp/compose/rails.html#:~:text=%E6%9C%80%E5%BE%8C%E3%81%AB%20docker%2Dcompose.yml%20%E3%81%8C%E5%8F%96%E3%82%8A%E3%81%BE%E3%81%A8%E3%82%81%E3%81%A6%E3%81%8F%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)
# Dockerのドキュメントからコピペするコード
command: /bin/sh -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
