[A'dan Z'ye Docker](https://ozgurozturk.net/docker-udemy) eğitimimdeki öğrencilerimin yararlanabilmesi adına Docker Cheat Sheet yani Türkçeleştirirsek "Docker komutlarının nasıl kullanıldığının bir listesi" hazırladım. Yararlı bir derleme olduğunu düşünüyorum. Daha geniş bir kitleye ulaşabilmesi için de burada yayınlıyorum. Kolay gelsin.  

***Docker Cheat Sheet (Türkçe)***
___

* ***Container***

**Container çalıştırma:**

`docker container run image:tag`

Örnek: `docker container run nginx:latest`
___
**Detach modda container çalıştırma (-d):**

`docker container run -d image:tag`

Örnek: `docker container run -d nginx:latest`
___
**Varsayılan uygulama yerine başka uygulama ile container başlatma:**

`docker container run image:tag uygulama`

Örnek: `docker container run ubuntu:latest ping 127.0.0.1`
___
**Container'a bir isim vererek çalıştırma**

`docker container run --name isim image:tag`

Örnek: `docker container run --name container1 -d nginx:latest`
___
**Çalışan bir container içerisinde başka bir uygulama çalıştırma:**

`docker container exec container_id_or_name uygulama`

Örnek: `docker container exec 12a793b3fec0 ping 127.0.0.1`
___
**Çalışan bir container'a shell bağlantısı oluşturma:**

`docker container exec -it container_id_or_name sh`

Örnek: `docker container exec -it 12a793b3fec0 sh`
___
**Container'ı detach modda ve shell bağlantısı ile oluşturma (dit):**

`docker container run -dit image:tag sh`

Örnek: `docker container run -dit nginx:latest sh`
___
**Detach modda ve shell bağlantısı ile oluşturulmuş container'a bağlanma:**

`docker attach container_id_or_name`

Örnek: `docker attach 12a793b3fec0`
___
**Container durdurma:**

`docker container stop container_id_or_name`

Örnek: `docker container stop 12a793b3fec0`
___
**Container silme:**

`docker container rm container_id_or_name`

Örnek: `docker container rm 12a793b3fec0`
___
**Çalışan containerı silme (-f):**

`docker container rm -f container_id_or_name`

Örnek: `docker container rm -f 12a793b3fec0`
___
**Container kapatıldığı zaman aynı zamanda silinmesi (-rm):**

`docker container run -rm image:tag`

Örnek: `docker container run -rm nginx:latest` (-rm ile container kapandığı zaman otomatik olarak silinmesini söylüyoruz)
___
**Container ile ilgili detayları inceleme:**

`docker container inspect container_id_or_name`

Örnek: `docker container inspect 12a793b3fec0`
___
**Sistemdeki tüm containerları (çalışan ve durdurulmuş) silme**

`docker container rm -f $(docker ps -aq)`
___
**Sistemdeki çalışan containerları listeleme:**

`docker container ls` ya da 

`docker container ps`
___
**Sistemdeki tüm containerları listeleme:**

`docker container ls -a` ya da 

`docker container ps -a`

docker ps -a
___
**Çalışan container'daki processleri listeleme:**

`docker top container_id_or_name`

Örnek: `docker top 12a793b3fec0`
___
**Çalışan container'ın Cpu, Ram, I/O kullanımını görme:**

`docker stats container_id_or_name`

Örnek: `docker stats 12a793b3fec0`
___
**Container'ın memory kullanımını sınırlama (--memory, --memory-swap):**

`docker container run --memory=rakam(b,k,m,g) --memory-swap=rakam(b,k,m,g) image:tag`

Örnek: `docker container run --memory=1g --memory-swap=2g nginx:latest` (memory-swap ile swap alanı da tanımlayabiliriz.b=byte k=kilobyte m=megabyte g=gigabyte)
___
**Container'ın cpu kullanımını sınırlama (--cpus, --cpuset-cpus):**

`docker container run --cpus="core_adeti" image:tag`

Örnek: `docker container run --cpus="3" nginx:latest` (sistemden kaç core'a erişebileceğini belirledik)

`docker container run --cpuset-cpus="core_numarası" image:tag`

Örnek: `docker container run --cpuset-cpus="0,4" nginx:latest` (sistemdeki hangi corelara erişebileceğini belirledik)
___
**Container'a enviroment variable tanımlama:**

`docker container run --env enviroment_variable=değeri image:tag`

Örnek: `docker container run --env VAR1=deneme1 --env VAR2=deneme2 nginx:latest`
___
**Containerdan hosta ya da tam tersi dosya kopyalama:**

`docker cp container_id_or_name:path host_path`

Örnek: `docker cp 12a793b3fec0:/usr/src/uygulama/ .`

___
___
___

* ***Image***

**Docker CLI aracılığıyla registery'de oturum açma:**

`docker login registery_url`

Örnek: `docker login localhost:8080`
___
**Sisteme bir imaj çekme:**

`docker image pull image:tag`

Örnek: `docker image pull nginx:latest`
___
**Docker hub'a (ya da başka bir repository) image gönderme:** 

`docker image push repository/image:tag`

Örnek: `docker image push ozgurozturknet/adanzyedocker:latest`
___
**Mevcut bir imaja yeni tag ekleme**

`docker image tag image:tag yeniimage:tag`

Örnek: `docker image tag nginx:latest ozgurozturknet\nginx:v1`
___
**Image ile ilgili detayları inceleme:**

`docker image inspect image:tag`

Örnek: `docker image inspect nginx:latest`
___
**Image layerlarını listeleme:**

`docker image history image:tag`

Örnek: `docker image history nginx:latest`
___
**Dockerfile kullanarak yeni bir imaj yaratma:** 

`docke image build -t image:tag .`

Örnek: `docker image build -t ozgurozturknet\merhaba-dunya:latest .` (Dockerfile dosyası komutun çalıştırıldığı folder'da bulunmalı)
___
**Image oluştururken build arg kullanma:**

`docker image build --build-arg arg=deger -t image:tag .`

Örnek: `docker image build --build-arg VERSION=3.7.1 -t nginx:latest .`
docker image build -t mycont:v1 --build-arh VERSION=1 .
___
**Sistemdeki tüm imageleri listeleme:**

`docker image ls`
___
**Sistemden bir imajı silme:**

`docker image rm image:tag`

Örnek: `docker image rm nginx:latest`
___
**Containerdan image yaratma:**

`docker commit container_id_or_name image:tag`

Örnek: `docker commit 12a793b3fec0 ozgurozturknet/img:latest`
___
**Image'i bir dosyaya kaybetmek ve kaydedilmiş bir dosyadan image oluşturmak:**

`docker save image:tag -o dosyaadi.tar`

Örnek: `docker save ozgurozturknet/img:latest -o image.tar`

`docker load -i dosyaadi.tar`

Örnek: `docker load -i imagecon1.tar`

___
___
___

* ***Volume***

**Volume oluşturma:**

`docker volume create volume_ismi`

Örnek: `docker volume create ilkvolume`
___
**Volume ile ilgili detayları inceleme:**

`docker volume inspect volume_id_ismi`

Örnek: `docker volume inspect ilkvolume`
___
**Sistemdeki tüm volumeleri listeleme:**

`docker volume ls`
___
**Volume'u container'a bağlama (-v):**

`docker container run -v volume_ismi:container_icindeki_path image:tag`

Örnek: `docker container run -v ilkvolume:/var/www/html image:tag`
___
**Volume'u container'a sadece okunur şekilde bağlama (:ro):**

`docker container run -v volume_ismi:container_icindeki_path:ro image:tag`

Örnek: `docker container run -v ilkvolume:/var/www/html:ro image:tag`
___
**Host üstündeki bir klasör ya da dosyayı bind mount olarak bağlama:**

`docker container run -v host_klasör_path:container_icindeki_path image:tag`

`docker container run -v c:\websitesi:/usr/share/nginx/html nginx:latest `
___
**Volume silme:**

`docker volume rm volume_ismi`

Örnek: `docker volume rm ilkvolume`

___
___
___

* ***Network***

**Kullanıcı tanımlı bridge network oluşturma (bridge):**

`docker network create --driver=bridge network_ismi`

Örnek: `docker network create --driver=bridge kopru`
___
**Kullanıcı tanımlı bridge network oluşturma (ip bilgilerini belirleyerek):**

`docker network create --driver=bridge --subnet=cidr --ip-range=cdir --gateway=ip_adresi network_ismi `

Örnek: `docker network create --driver=bridge --subnet=10.10.0.0/16 --ip-range=10.10.10.0/24 --gateway=10.10.10.10 kopru`
___
**Sistemdeki tüm volumeleri listeleme:**

`docker network ls`
___
**Volume ile ilgili detayları inceleme:**

`docker network inspect network_ismi`

Örnek: `docker network inspect kopru`
___
**Container'ı varsayılan dışında bir network'e bağlayarak çalıştırma:**

`docker container run --network network_ismi image:tag`

Örnek: `docker container run --network kopru nginx:latest`
___
**Çalışan bir container'ı başka bir network'e bağlama:**

`docker network connect network_ismi container_id_or_name`

Örnek: `docker network connect kopru 12a793b3fec0`
___
**Çalışan bir container'ın bağlı olduğu networkle bağlantısını kesme:**

`docker network disconnect network_ismi container_id_or_name`

Örnek: `docker network disconnect kopru 12a793b3fec0`
___
**Port publish ederek bir container çalıştırma (-p):**

docker container run -p host_portu:container_portu/tcp_yada_udp image:tag

Örnek: docker container run -p 8080:80 -p 53:53/udp nginx:latest

___
___
___

* ***Logging***

**Container tarafından oluşturulan logları görmek:**

`docker logs container_id_or_name`

Örnek: `docker logs 12a793b3fec0`
___
**Container tarafından oluşturulan logları uzun formatta detaylı görmek:**

`docker logs --details container_id_or_name`

Örnek: `docker logs --details 12a793b3fec0`
___
**Container tarafından oluşturulan logları belirli tarih aralığında görmek:**

`docker logs --since tarih_saat --until tarih_saat container_id_or_name`

Örnek: `docker logs --since 2020-01-13T11:34:43.154304300Z 12a793b3fec0` (since verilen andan itibaren olanları, until ise verilen ana kadar olanları listeler)
___
**Container tarafından oluşturulan logların belirli sayıda son oluşanlarını görmek:**

`docker logs --tail sayı container_id_or_name`

Örnek: `docker logs --tail 10 12a793b3fec0` (son 10 log çıktısını listeler)
___
**Container tarafından oluşturulan logları anlık olarak izlemek:**

`docker logs -f container_id_or_name`

Örnek: `docker logs -f 12a793b3fec0` (loglar oluştukça ekranda gözükecektir. Ctrl-C ile bağlantı kesilebilir)


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


