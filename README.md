（2）oap部署

复制代码
sudo docker run --name sky-oap \
--restart always -d \
    -p 1234:1234 \
    -p 21311:11800 \
    -p 22311:12800 \
    -e TZ=Asia/Shanghai \
    -e SW_STORAGE=elasticsearch \
    -e SW_STORAGE_ES_CLUSTER_NODES=192.168.0.203:21452 \
    apache/skywalking-oap-server:9.4.0
复制代码
（3）ui部署【需要等oap启动完成之后启动】

sudo docker run -d --name sky-ui \--restart=always -d \
-e TZ=Asia/Shanghai \
-p 21312:8080 \
-e SW_OAP_ADDRESS=http://192.168.0.203:22311 \
apache/skywalking-ui:9.4.0
 集成dockerfile（引用环境变量中的变量）：

ENTRYPOINT sh -c 'java -javaagent:/usr/local/agent/skywalking-agent.jar -jar -DSW_AGENT_NAME=$BALDR_ENV_ACTIVE::scheduler -DSW_AGENT_INSTANCE_NAME=scheduler:$BALDR_NACOS_CLIENT_IP:$BALDR_NACOS_CLIENT_PORT -DSW_AGENT_COLLECTOR_BACKEND_SERVICES=10.8.0.102:21311 /usr/local/jar/baldr-scheduler-impl-0.0.1-SNAPSHOT.jar'
 

