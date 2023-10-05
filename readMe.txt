AZ_RESOURCE_GROUP=Narwhal
AZ_DATABASE_NAME=Narwhal-db
AZ_LOCATION=australiaeast
AZ_MYSQL_USERNAME=rusergei2010
AZ_MYSQL_PASSWORD=LesPau20102010!
AZ_LOCAL_IP_ADDRESS=202.169.123.214


az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq

az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server-name $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq

az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name allAzureIPs \
    --server-name $AZ_DATABASE_NAME \
    --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
    | jq


az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME \
    | jq



curl https://start.spring.io/starter.tgz -d type=maven-project -d dependencies=web,data-jpa,mysql -d baseDir=azure-spring-workshop -d bootVersion=3.1.4 -d javaVersion=1.8 | tar -xzvf -


logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:mysql://Narwhal-db.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=rusergei2010@Narwhal-db
spring.datasource.password=LesPau20102010!

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop



curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"description":"configuration","details":"congratulations, you have set up your Spring Boot application correctly!","done": "true"}' \
    http://127.0.0.1:8080


 mvn com.microsoft.azure:azure-webapp-maven-plugin:2.12.0:config
 mvn package com.microsoft.azure:azure-webapp-maven-plugin:2.12.0:deploy


 curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"description":"configuration","details":"congratulations, you have set up your Spring Boot application correctly!","done": "true"}' \
    http://narwhalapp1.azurewebsites.net
