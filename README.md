docker run -d --name sonarqube -p 6004:9000 \
--link postgres \
-v /home/apps/sonarqube/extensions:/opt/sonarqube/extensions \
-v /home/apps/sonarqube/logs:/opt/sonarqube/logs \
-v /home/apps/sonarqube/data:/opt/sonarqube/data \
-e SONARQUBE_JDBC_URL=jdbc:postgresql://postgres:5432/sonar \
-e SONARQUBE_JDBC_USERNAME=sonar \
-e SONARQUBE_JDBC_PASSWORD=sonar \
--restart always \
--privileged=true \
sonarqube:8.9.2-community


docker logs sonarqube
# 添加6004端口
firewall-cmd --zone=public --add-port=6004/tcp --permanent
 
# 重新载入
firewall-cmd --reload

docker run -d --name sonarqube \
    -p 9000:9000 \
    -e SONAR_JDBC_URL=... \
    -e SONAR_JDBC_USERNAME=... \
    -e SONAR_JDBC_PASSWORD=... \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    <image_name>
