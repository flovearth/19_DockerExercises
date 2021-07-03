FROM ubuntu:18.04
#two lines merged
RUN apt-get update -y && apt-get install default-jre -y
WORKDIR /merhaba
COPY /myapp .
CMD ["java", "merhaba"]
