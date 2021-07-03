 FROM | Oluşturulacak imajın hangi imajdan oluşturulacağını belirten talimat. Dockerfile içerisinde geçmesi mecburi tek talimat budur. Mutlaka olmalıdır. 
Ör: FROM ubuntu:18.04

LABEL | İmaj metadata’sına key=value şeklinde değer çiftleri eklemek için kullanılır. Örneğin team=development şeklinde bir etiket eklenerek bu imajın development ekibinin kullanması için yaratıldığı belirtilebilir.
Ör: LABEL version:1.0.8

RUN | İmaj oluşturulurken shell’de bir komut çalıştırmak istersek bu talimat kullanılır. Örneğin apt-get install xxx ile xxx isimli uygulamanın bu imaja yüklenmesi sağlanabilir. 
Ör: RUN apt-get update

WORKDIR | cd xxx komutuyla ile istediğimiz klasöre geçmek yerine bu talimat kullanılarak istediğimiz klasöre geçer ve oradan çalışmaya devam ederiz. 
Ör: WORKDIR /usr/src/app

USER | gireceğimiz komutları hangi kullanıcı ile çalıştırmasını istiyorsak bu talimat ile onu seçebiliriz. 
Ör: USER poweruser

COPY | İmaj içine dosya veya klasör kopyalamak için kullanırız
Ör: COPY /source /user/src/app

ADD | COPY ile aynı işi yapar yani dosya ya da klasör kopyalarsınız. Fakat ADD bunun yanında dosya kaynağının bir url olmasına da izin verir. Ayrıca ADD ile kaynak olarak bir .tar dosyası belirtilirse bu dosya imaja .tar olarak sıkıştırılmış haliyle değil de açılarak kopyalanır. 
Ör: ADD https://wordpress.org/latest.tar.gz /temp

ENV | Imaj içinde environment variable tanımlamak için kullanılır
Ör: ENV TEMP_FOLDER="/temp"

ARG | ARG ile de variable tanımlarsınız. Fakat bu variable sadece imaj oluşturulurken yani build aşamasında kullanılır. Imajın oluşturulmuş halinde bu variable bulunmaz. ENV ile imaj oluşturulduktan sonra da imaj içinde olmasını istediğiniz variable tanımlarsınız, ARG ile sadece oluştururken kullanmanız gereken variable tanımlarsınız.
Ör: ARG VERSION:1.0

VOLUME | Imaj içerisinde volume tanımlanamızı sağlayan talimat. Eğer bu volume host sistemde varsa container bunu kullanır. Yoksa yeni volume oluşturur. 
Ör: VOLUME /myvol

EXPOSE | Bu imajdan oluşturulacak containerların hangi portlar üstünden erişilebileceğini yani hangi portların yayınlanacağını bu talimatla belirtirsiniz. 
Ör: EXPOSE 80/tcp

ENTRYPOINT | Bu talimat ile bir containerın çalıştırılabilir bir uygulama gibi ayarlanabilmesini sağlarsınız.
Ör: ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

CMD | Bu imajdan container yaratıldığı zaman varsayılan olarak çalıştırmasını istediğiniz komutu bu talimat ile belirlersiniz. 
Ör: CMD java merhaba

HEALTHCHECK | Bu talimat ile Docker'a bir konteynerin hala çalışıp çalışmadığını kontrol etmesini söylebiliriz. Docker varsayılan olarak container içerisinde çalışan ilk processi izler ve o çalıştığı sürece container çalışmaya devam eder. Fakat process çalışsa bile onun düzgün işlem yapıp yapmadığına bakmaz. HEALTHCHECK ile buna bakabilme imkanına kavuşuruz.
Ör: HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost/ || exit 1

SHELL | Dockerfile'ın komutları işleyeceği shell'in hangisi olduğunu belirtiriz. Linux için varsayılan shell ["/bin/sh", "-c"],Windows için ["cmd", "/S", "/C"]. Bunları SHELL talimatı ile değiştirebiliriz. 
Ör: SHELL ["powershell", "-command"]


FROM openjdk:7 AS builder
COPY ./source /usr/src/myapp
WORKDIR /usr/src/myapp
RUN javac app1.java


FROM anapsix/alpine-java
WORKDIR /usr/src/myapp
COPY --from=builder /usr/src/myapp .
CMD ["java", "app1"]

FROM httpd:alpine
MAINTAINER @ozgurozturknet (ozgur@ozgurozturk.net)

RUN apk add htop gzip iputils curl apache2 tree openjdk7-jre
RUN apk update && apk upgrade

WORKDIR /usr/src/myapp
COPY /myapp .
RUN echo "<html><body><h1>A'dan Z'ye Docker Egitimine Hosgeldiniz.</h1></body></html>" > /usr/local/apache2/htdocs/index.html

FROM alpine
COPY hello.sh .
CMD ["sh", "hello.sh"] 


FROM openjdk:7 AS builder
COPY ./source /usr/src/myapp
WORKDIR /usr/src/myapp
RUN javac env1.java


FROM anapsix/alpine-java
WORKDIR /usr/src/myapp
COPY --from=builder /usr/src/myapp .
CMD ["java", "env1"]


FROM php:7.3-apache
RUN apt-get update -y && apt-get install mariadb-client-10.3 -y
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN mkdir /var/www/html/images
RUN chmod 777 /var/www/html/images
COPY ./php/ /var/www/html/


FROM mysql:5.7
COPY tabloyarat.sql /docker-entrypoint-initdb.d


FROM ubuntu:18.04
RUN apt-get update -y
RUN apt-get install default-jre -y
WORKDIR /merhaba
COPY /myapp .
CMD ["java", "merhaba"]


# Bu örnek uygulama https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md adresinden alınmıştır
# base imaj olarak nginx resmi imajını kullanıyoruz -- 
# https://github.com/nginxinc/docker-nginx/blob/5971de30c487356d5d2a2e1a79e02b2612f9a72f/mainline/buster/Dockerfile
FROM nginx:latest


# label ile de key value pair olarak bu imajla ilgili bilgileri giriyoruz. her iki talimat da imajın içindeki sistemle alakalı değil.
# her ikisi de imajın metadatasında saklanıyor. imajla alakali bilgiler. 
LABEL maintainer="Ozgur Ozturk @ozgurozturknet" version="1.0" name="hello-docker"

# bir adet env variable tanımlıyoruz. Daha sonra bunu runtime'da değiştireceğiz
ENV KULLANICI="Dunyali"

# nginx debian-slim imajı baz alınarak oluşturulmuştur.
# bu imajda geriye doğru uyumluluk sorunları var ve o nedenle update olmuyor. 
#Bu url'leri sources.list'e ekleyerek update olabilmesini sağlıyoruz
#RUN printf "deb http://archive.debian.org/debian/ jessie main\ndeb-src http://archive.debian.org/debian/ \
#jessie main\ndeb http://security.debian.org jessie/updates \
#main\ndeb-src http://security.debian.org jessie/updates main" > /etc/apt/sources.list

# Uygulama depolarından mevcut paketlerin en son listesini çekip güncelliyoruz. 
RUN apt-get update && apt-get install curl -y

# debian-slim'de bir çok tool mevcut değil. Bizim curl kullanmamız gerektiği için curl kuruyoruz


# nginx web sayfalarını /usr/share/nginx/html folder'ında barındırıyor. O nedenle bu folder'a geçiyoruz
WORKDIR /usr/share/nginx/html

# web sitemizin açılış sayfası olan Hello_docker.html dosyasını buraya kopyalıyoruz.
COPY Hello_docker.html /usr/share/nginx/html

# sistemin düzgün çalıştığını ve nginx daemon'ının web sitesini publish etmekte bir sorun yaşamadığını test ediyoruz
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1

# bu imajdan container yaratıldığı zaman çalışmasını istediğimiz komutu buraya giriyoruz
# /usr/share/nginx/html klasörüne geçiyor ve sed aracılığıyla Hello_docker.html dosyasının içerisindeki
# Kullanici kelimesini $KULLANICI env variable'ının değeriyle Hostname kelimesini de $HOSTNAME ile değiştiriyor ve 
# dosyanın içeriğini index.html'e adlı yeni bir dosyaya yazıyoruz.
# ardından nginx daemon'ı çalıştırıyoruz
CMD sed -e s/Kullanici/"$KULLANICI"/ Hello_docker.html > index1.html && sed -e s/Hostname/"$HOSTNAME"/ index1.html > index.html ; rm index1.html Hello_docker.html; nginx -g 'daemon off;'


FROM alpine:latest
WORKDIR /app1
COPY wordpress.tar.gz .
ADD wordpress.tar.gz .
ADD https://wordpress.org/latest.tar.gz .


FROM mcr.microsoft.com/java/jre:8-zulu-alpine
WORKDIR /merhaba
COPY /myapp .
ENTRYPOINT [ "java", "merhaba" ]


FROM centos:latest
ENTRYPOINT ["ping"]
CMD ["127.0.0.1"]


FROM centos:latest
ENV TEST="Bu bir denemedir"
CMD echo $TEST 


FROM mcr.microsoft.com/java/jdk:8-zulu-alpine
COPY /source /usr/src/uygulama
WORKDIR /usr/src/uygulama
RUN javac uygulama.java
CMD ["java", "uygulama"]


FROM mcr.microsoft.com/java/jdk:8-zulu-alpine
COPY /source /usr/src/uygulama
WORKDIR /usr/src/uygulama
RUN javac uygulama.java
CMD ["java" , "uygulama"]


FROM ubuntu:latest
WORKDIR /gecici
ADD https://www.python.org/ftp/python/3.7.6/Python-3.7.6.tgz .
CMD ls -al


# escape=`

# Sample Dockerfile

# Indicates that the windowsservercore image will be used as the base image.
FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Metadata indicating an image maintainer.
LABEL maintainer="jshelton@contoso.com"

# Uses dism.exe to install the IIS role.
RUN dism.exe /online /enable-feature /all /featurename:iis-webserver /NoRestart

# Creates an HTML file and adds content to this file.
RUN echo "Hello World - Dockerfile" > c:\inetpub\wwwroot\index.html


RUN powershell.exe -Command `
    $ErrorActionPreference = 'Stop'; `
    wget https://www.python.org/ftp/python/3.5.1/python-3.5.1.exe -OutFile c:\python-3.5.1.exe ; `
    Start-Process c:\python-3.5.1.exe -ArgumentList '/quiet InstallAllUsers=1 PrependPath=1' -Wait ; `
    Remove-Item c:\python-3.5.1.exe -Force

# Sets a command or process that will run each time a container is run from the new image.
CMD [ "cmd" ]


