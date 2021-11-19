# Bash
Bourne-again shell: terminale yazılanları yorumlar.

```
mkdir test
cd test
nano bash_file
```
bash_file diye bir file oluştu şimdi içine hello world yazalım bunu bash yorumlayıcısı ile yorumlayacağı bilgisini ilk satırda verelim.
```
#!/bin/bash
echo "Hello world!"
```  
**ctrl s, ctrl x** tamamdır. şimdi bu bash scriptini çalıştıralım  
`bash bash_file` Hello world!
bash script dosyaını çalıştırabilmek için izin alalım.  
`chmod +x batch_file`
artık `./bash_script`  denilerek çalıştırılabilir.  
Değişken tanımlamak `MYNAME="YUNUS EMRE"` | `MYNAME='Yunus Emre'`   
Hangi tırnak, neden ?  
* `echo "$((20 + 10))"` 30 
* `echo '$((20 + 10))'` $((20 + 10))  

Bash script ile işletim sistemine ait değişkenlere de erişilebilir.
* `echo $USER` yed
* `echo $HOME` /home/yed  
```
yed@yed:~$ echo $(date +%Y-%m-%d)
2021-11-19
```
```
read -p "What's your name:" MYNAME
echo "Hello $MYNAME"

read -p "Birinci sayı:" SAYI1
read -p "İkinci sayı:" SAYI2
let SONUC=SAYI1+SAYI2
echo "Toplama: "$SONUC
```
### if else  
* **Aritmetik op:** -eq, -ne, -gt, -ge, -lt, -le (equal than, not equal, greater, greater equal ...)  
* **Metin op:** 0, =!
```
#!/bin/bash
read -p "Give the fist number: " N1
read -p "Give the second number: " N2

if [ $N1 -eq $N2 ]
then
 echo "$N1 ve $N2 eşittir."
else
 echo "bunlar aynı değil bro"
fi
 echo "bunlar eşit değil bro"
```
```
#!/bin/bash
read -p "Give the word: " WORD
if [ $WORD == "ali" ]
then
 echo "Word length:" ${#WORD} "and word is" $WORD
elif [ $WORD == "veli" ]
then
 echo "Word length:" ${#WORD} "and word is" $WORD
else
  echo "The word isn't equel to 'ali' or 'veli' "
fi
```
```                              
#!/bin/bash
read -p "What is your name :" NAME
read -sp "What is your password :" PSWD
echo
if [ $NAME = "YUNUS" ] && [ $PSWD = "123" ]
then
 echo " Everything is Okay!"
else
 echo " Wrong info the name doesn't macth with $NAME"
fi
```
### For
```
#!/bin/bash
KISILER=("Yusuf" "Ramazan" "Sinan" "Mehmet");

for KISI in ${KISILER[*]}
do
 echo $KISI
done
```






### Yararlanılan Kaynaklar
[Yusuf Sezer](https://www.yusufsezer.com.tr/linux-bash/)
