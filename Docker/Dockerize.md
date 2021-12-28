# Pc'imde çalışan kodu docker üzerinden nasıl çalıştırabilirim ? 

1. Docker'ın pc ye kurulu olduğu kabul edilir. 
   
2.  nodejs kodunun docker üzerinde çalışması için (az yer kaplayan) alphine os'unun dockere pull edilir. (CMD içine aşağıdaki kod yazılarak)  

```
    docker pull alpine	
```  

3.	Docker images'ten indirilen alphine image'ı görüntelenmektedir. Run'a basılarak çalıştırılır.

![dockerize_1](https://github.com/yunusemre002/Papers/blob/main/photos/dockerize_1.gif?raw=true)

4.	Açılan ekrandan bir isim verilir. Ardından bilgisayardaki bir alan mount edilir. Bu işlem sayesinde alphin içinden Local pc'mizin bağlı klasörüne bağlanabiliriz. Bir tarafta yapılan değişiklikler anlık olarak diğer tarafta da yapılmış olur.

![dockerize_2](https://github.com/yunusemre002/Papers/blob/main/photos/dockerize_2.gif?raw=true)

5.	Oluşturulan Container: Containers / Apps sekmesinden görüntülenmektedir. Oluşturulan kontainer içine girebilmek için CLI'ya tıklanır ve bir komut satırı ile karşılaşılır. Artık Alphinimizin içerisindeyiz.

![dockerize_3](https://github.com/yunusemre002/Papers/blob/main/photos/dockerize_3.gif?raw=true)

6.	home/node ile PC'mizdeki node.JS kodunun olduğu yeri paylaşmıştık/bağlamıştık. Görüldüğü üzre alphine üzerinden görüntüleyebiliyoruz.   

![dockerize_4](https://github.com/yunusemre002/Papers/blob/main/photos/dockerize_4.gif?raw=true)

7.	Şimdi sırada node Js i çalıştırmak için Alphin içine npm kurulumu kaldı. Bu işlemide yaptıktan sonra kodumuzu çalıştırabiliriz. 

8.	apk add --update nodejs nodejs-npm komutu node js'i çalıştırmak için gerekli olan npm modülü kurulur.

![dockerize_5](https://github.com/yunusemre002/Papers/blob/main/photos/dockerize_5.gif?raw=true)

9.	Artık kodumuzu çalıştırabiliriz. Son olarak bir hatırlatma yapmak istiyorum. Eğer kodunuzun içinde database veya herhangi bir yere  bağlantı için "localhost" ifadesi geçiyorsa bu yerler Local Pc'nizin IP'si ile değiştirilmelidir. (Örneğin 192.168.90.35 olarak ayarlanmalı localhost yazan yerler.)

10.	son olarak node index.js diyerek kod çalıştırılır.

11.	iyide bu image her açıldığında bu konu çalıştır diye tetiklemek lazım kuzum onu nasıl yapacağız ? Üstte anlatılanları unutun, yeni konumuz Dockerize. 
					
# DOCKERİZE

işte burada Ege iliklier'in yardımı giriyor devreye. :)  Yukarıdakinden bağımsın gibi.

1. create a dockerfile: Dockerfile isimli bir dosya yaratışacak içine aşağıdakiler yazılacak. Bu dosyanın uzantısı olmayacak. Bu doysa kodun olduğu klasöre konulacak.

```
# Docker Base Image: Tag'lere buradan bakabilirsin https://hub.docker.com/_/python/
FROM python:3.9-alpine

# Programının çalışacağı PATH: https://docs.docker.com/engine/reference/builder/#workdir
WORKDIR /usr/src/app

# Kaynak kodlarını aktar: https://docs.docker.com/engine/reference/builder/#copy
# Dockerfile'ın bulunduğu PATH'deki tüm dosyaları Docker image'i içerisindeki /usr/src/app PATH ine atar
COPY . . 

# Uygulamanın gerekli duyduğu, pip ile indirilmesi gereken kütüphaneler varsa 
# Burada 'requirements.txt' diye bir dosyada projenin 'dependency'lerinin saklandığını varsayıyorum)
RUN pip install -r ./requirements.txt

# Programın başlangıç noktası
ENTRYPOINT ["python3", "app.py"]

# Bu Dockerfile üzerinde birçok değişiklik yapılabilir.
# Burada python uygulamaları için güzel bir örnek var. Okumanı tavsiye ederim. https://www.pybootcamp.com/blog/how-to-write-dockerfile-python-apps/
```

2. Sonra Dockerfile ile image yaratmak için: Dockerfile'ın bulunduğu PATH altında:
```
docker build -t [IMAGE_NAME] .
```

3. Yarattığın image üzerinden container yaratıp/çalıştırmak için 
```
docker run --name [CONTAINER_NAME] -d [IMAGE_NAME]
```

-d : Eğer uygulamanın arkaplanda çalışmasını istiyorsan
--name: Container'a isim vermek için
Detaylar: https://docs.docker.com/engine/reference/commandline/run/


https://hub.docker.com/_/alpine
https://superuser.com/questions/1125969/how-to-install-npm-in-alpine-linux




