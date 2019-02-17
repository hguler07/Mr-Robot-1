## **Mr-Robot:1 Çözümü | Mr-Robot:1 Walkthrough**

  ## **Merhaba**
**Bu yazıda VulnHub üzerinde yayınlanan zafiyetli sanal makinelerden biri olan Mr-Robot:1 makinesinin tam çözümünü inceleyeceğiz.**

### **Vulnhub Üzerinden Mr-Robot:1 İndirilmesi**
Mr-Robot:1 makinasını  "https://www.vulnhub.com/entry/mr-robot-1,151/" adresinden indirebiliriz.

### **Vulnhub Üzerinden Mr-Robot:1 İncelenmesi**
Web sitesinde bulunan açıklama kısmına bakıcak olursak.
   
   > Sanal makinamızda gizlenmiş 3 adet anahtar var.
    Bizim amacımız bu anahtarları bulmak.
	
### **Vmware Üzerinde Mr-Robot:1 Çalıştırılması**
İndirmiş olduğumuz mrRobot.ova isimli dosyamızı Vmware da çalıştırmak için;
>File -> New Dedik dedikten sonra. 
>Dosyamızı tutup Vmware içine bırakmamız yeterli. 

## **Önemli Uyarı**
Mr-Robot:1 makinamızı Vmware ye yüklerden ağ ayarlarını yapmanız gerekiyor.
Ağ ayarlarını yapmazsanız.  Mr-Robot:1 makinamız ile Kali linux arasında haberleşmede sorun olucaktır.

Bu ayarı yaptıktan sonra Mr-Robot:1 makinanızı yeniden başlatınız. 
> Mac için Ağ ayarı = >  Share with my Mac ayarını seçin.
 > Windows için ağ ayarı = > NAT:  ayarını seçin.

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
> 80. port açık ve üzerinde Apache servisi çalışıyor.
O halde makinamıza tarayıcı üzerinden ulaşım bakalım karşımıza ne çıkıcak.



