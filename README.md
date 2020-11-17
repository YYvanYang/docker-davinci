# docker-davinci

Docker image for [davinci](https://github.com/edp963/davinci) data dashboard.

## Tags

* latest

Tag | Base JRE | Image size | Running memory
---|---|---|---
latest | adoptopenjdk:8-jre-openj9 | ~630MB | ~200MiB
alpine | openjdk:8-jre-alpine | ~290MB | ~600MiB

## Usage

```sh
# pull latest image
docker pull zhangsean/davinci:latest
# prepare davinci.sql
docker run --rm -v $PWD:/tmp zhangsean/davinci:latest cp bin/davinci.sql /tmp/
# run local mysql and init davinci db tables.
docker run -itd --name mysql \
  -p 3306:3306 \
  -v $PWD/davinci.sql:/docker-entrypoint-initdb.d/davinci.sql \
  -e MYSQL_DATABASE=davinci \
  -e MYSQL_ROOT_PASSWORD=pass \
  mysql:5.7
# run davinci
docker run -itd --name davinci \
  -p 8080:8080 \
  --link mysql:localdb \
  -e SPRING_DATASOURCE_URL="jdbc:mysql://localdb/davinci?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&allowMultiQueries=true" \
  -e SPRING_DATASOURCE_USERNAME="root" \
  -e SPRING_DATASOURCE_PASSWORD="pass" \
  -e SPRING_MAIL_HOST="smtp.qq.com" \
  -e SPRING_MAIL_PORT="465" \
  -e SPRING_MAIL_PROPERTIES_MAIL_SMTP_SSL_ENABLE="true" \
  -e SPRING_MAIL_USERNAME="someone@qq.com" \
  -e SPRING_MAIL_PASSWORD="bomgxxoocgkobjdc" \
  -e SPRING_MAIL_NICKNAME="Davinci" \
  zhangsean/davinci:latest
```

## More info

[edp963/davinci](https://github.com/edp963/davinci)
