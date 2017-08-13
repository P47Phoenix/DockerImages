FROM openjdk:jdk-windowsservercore
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest -Uri ('https://s3-us-west-2.amazonaws.com/dynamodb-local/dynamodb_local_latest.zip') -UseBasicParsing -OutFile /dynamodb_local_latest.zip
RUN expand-archive -path c:\\dynamodb_local_latest.zip -destinationpath c:\\dynamodb_local_latest

EXPOSE 8000

CMD [ "java", "-Djava.library.path=./DynamoDBLocal_lib", "-jar", "C:\\dynamodb_local_latest\\DynamoDBLocal.jar" ]