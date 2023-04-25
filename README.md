# temp

docker run -d --name=xxl-job-admin -v $PWD/application.properties:/application.properties  -p 8888:8888 -e PARAMS='--spring.config.location=/application.properties' xuxueli/xxl-job-admin:2.4.0
