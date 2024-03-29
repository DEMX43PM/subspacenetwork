## Tavsiye Edilen Sistem Gereksinimleri
- CPU: 2 ÇEKİRDEK
- RAM: 2 GB
- SSD: 80 GB
- İşletim Sistemi: Ubuntu 20.04LTS

## Kurulum Adımları ve Kodları
- Önce https://polkadot.js.org/extension/ sitesinden cüzdan kurulumu yapıp cüzdan oluşturuyoruz ve bize verdiği gizli kelimeleri kesinlikle saklıyoruz. Ayrıca .json dosyasını da indirip yedeklemeyi unutmayın.
- Daha sonra aşağıdaki kodlarla adım adım yükleme işlemine geçiyoruz.Script ile kurulum kısa bir zamanda tamamlanıyor o yüzden yükleme adımı için onu kullanıyoruz.
- Script ZVALID e aittir. Yeni kod yayınlamadı ve kendisine ulaşamadım. Eğer yayınlarsa o kodu alacağım buraya. Ben de eskisini yeni versiyona göre uyarlayıp githubıma yükledim. Kendisine teşekkür ederim.

```
timedatectl | grep "System clock"
```
-Bu kodun çıktısı şöyle olmalı ``System clock synchronized: yes`` 

-Eğer çıktıyı böyle vermezse aşağıdaki kodları girip tekrar kontrol ediyorsunuz.

```
apt install ntp
ntpq -p
``` 
```
sudo apt update
wget -q -O subspace.sh https://raw.githubusercontent.com/okannako/subspacenetwork/main/subspace.sh && chmod +x subspace.sh && sudo /bin/bash subspace.sh
```
- Yukarıdaki kodları sırayla girdikten sonra ilk olarak bir soruyla karşılaşıyoruz eğer önceden kurulumunuz varsa 'n' yazıp ilerliyorsunuz kurulumunuz yoksa ne yazdığınız önemli değil.
- Daha sonra polkadot cüzdan ile oluşturduğunuz cüzdanınızın adresi sonra node nuza vermek istediğiniz ismi girdikten sonra gelen soruya 'y' diyerek devam ediyoruz ve kurulumu bitiriyoruz ve bize verdiği node kontrol kodlarını bir yere kopyalıyoruzi ilerde kontrol için kullanacağız.
```
apt install curl
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "system_health", "params":[]}' http://localhost:9933
```
- Curl u yükleyip, "curl -H "Content-Type...." başlayan kodla node muzun sync olup olamadığını kontrol ediyoruz. 'isSyncing' kısmı false olduğunda node muz sisteme sync olmuş demektir.

## Eğer Telemetry'de İsminiz Çıkmıyorsa Şu İşlemleri Yapın
- Senkronize olduktan sonra şu işlemleri yapıyoruz.
```
nano /etc/systemd/system/subspace-node.service
```
- Bu kodu girerek açılan ekranda 'execstart' ın olduğu satırın sonuna şu kodu ekliyoruz
 ```
 --telemetry-url "wss://telemetry.subspace.network/submit 1"
 ```
 - Bu değişikliği yaptıktan sonra şu iki kodla node u tekrar başlatın.
 ```
systemctl daemon-reload
systemctl restart subspace-node
```

## Node Silmek

```
sudo systemctl stop subspace-node.service
sudo systemctl stop subspace-farmer.service
sudo systemctl disable subspace-farmer.service
rm -rf ~/.local/share/subspace*
rm -rf /etc/systemd/system/subspaced*
rm -rf /usr/local/bin/subspace*
```


- Başlatma işlemini yaptıktan sonra şu sitede isminizin bir süre sonra görünmesi gerekiyor.
     - https://telemetry.subspace.network/#list/0x9ee86eefc3cc61c71a7751bba7f25e442da2512f408e6286153b3ccc055dccf0
- Son olarak aşağıdaki siteye giderek ve cüzdanımıza izin vererek blok imzaladıkça kazandığınımız ödülü görebiliriz.
     - https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ffarm-rpc.subspace.network#/accounts
- Bütün adımların uygulanışını görmek için şu videoyu izleyebilirsiniz.
     - https://www.youtube.com/watch?v=F1hSXOj44tI
