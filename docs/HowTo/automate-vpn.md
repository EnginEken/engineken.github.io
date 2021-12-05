### Automating Global Protect Vpn Connection

- İlk ihtiyacımız olan şey python ve pip paketleri. Bunların mac üzerinde yüklü ve kurulu olması gerekiyor. MAC default olarak python3 yüklü geliyor. Sadece yükleyeceğimiz paket için pip paket manager gerekiyor. Bunun için de terminalden aşağıdaki komutu çalıştırmak gerekiyor

<div class="termy">

```console
$ python3 -m ensurepip --upgrade
```

</div>

- Sonrasında aşağıdaki komut ile Global Protect için yazılmış ve SYMC ve 30 saniyede bir değişen kod üretilmesini sağlayan paketi yüklüyoruz. Pip komutu python paketi altında oluyor yüklemeden sonra. Doğrudan home directoryden çalıştırabilmek için pipin bulunduğu dosyanon yolunu path variablelarımız arasına eklemek gerekiyor.

<div class="termy">

```console
$ pip3 install python-vipaccess
```

</div>

- Aşağıdaki komut ile bir SYMC(global protectin telefon uygulaması için ürettiği id çeşidi) id üretiliyor(i.e. SYMC65526424)

<div class="termy">

```console
$ vipaccess provision -t SYMC
```

</div>

- Bu id nin sisteme sizin adınıza kayıt ettirilmesi gerekiyor. Sonrasında bu id kullanılarak 30 saniyede bir değişen kod üretilebiliyor. Üretilen kodu görmek için aşağıdaki komut kullanılıyor.

<div class="termy">

```console
$ vipaccess show -f /Users/{{ mac username }}/.vipaccess
```

</div>

- Sonrasında kendi domain username ve password kullanarak vpn bağlantısı sağlamanız ve bu üretilen kodu girip vpn e bağlanmanız gerekiyor. Bunu da openconnect ile komut satırı üzerinden yapabilirsiniz. Global protect için de desteği var. Yüklemek için aşağıdaki komut çalıştırılıyor.

<div class="termy">

```console
$ brew install openconnect
```

</div>

- Openconnect çalıştırmak için root yetkisinin olması gerekiyor. -u opsiyonu ile infoshop username değeri veriliyor, --protocol=gp opsiyonu ile de global protect kullanılarak bağlanılacağı söylenir. Sonrasında da vpn adresinin girilmesi gerekiyor. Aşağıdaki komut çalıştırıldığında önce root şifresi, sonra domain şifreniz,  en son olarak da 30 saniyelik kod girilip vpn connection tamamlanmış oluyor.

<div class="termy">

```console
$ sudo openconnect -u {{ infoshop username }} --protocol=gp sslvpn.hepsiburada.com
```

</div>

- Mac default sudo password timeout değeri 5 dakika. Bir kez girince sonraki 5 dakika tekrar girmenize gerek yok. Ancak bizim scriptimizin düzgün çalışması için bu adımın olmaması lazım. Yoksa sürekli sudo passwordü sorup sormadığını kontrol ettirmek gerekiyor. Bunun için de aşağıdaki komut çalıştırılarak sudoers dosyası görüntülenir. %admin ile bağlayan satır aşağıdaki gibi editlenip kaydedilir.

<div class="termy">
```console
$ sudo visudo
%admin          ALL = (ALL) NOPASSWD: ALL
```
</div>

!!! warning 
	Bu edit admin grubuna sahip kullanılıcılar için root passwordü sorulmasını engelliyor. Security açısından önerilmez. 

- Artık otomatize etme kısmına geçebiliriz. Bunun için aşağıdaki iki satırı bir dosyaya yazmamız(test.sh) gerekiyor.

<div class="termy">

```console
!/bin/bash
(echo "{{ infoshop password }}"; vipaccess show -f /Users/{{ mac username }}/.vipaccess) | sudo -S openconnect --protocol=gp -u {{ infoshop username }} sslvpn.hepsiburada.com 2>&1 
```

</div>

- Oluşturduğunuz script dosyasını executable yapmanız gerekiyor. Bunu da aşağıdaki komut ile yapabilirsiniz.

<div class="termy">

```console
$ chmod +x ./test.sh (Dosyayı create ettiğiniz path girilmeli)
```

</div>

- Artık ./test.sh ile scriptinizi çalıştırdığınızda vpn bağlantınızın yapıldığını ve vpn rotalarının gönderildiğini göreceksiniz.

--Eğer şifrenizi script içerisinde echo komtu ile çağırmak istemiyorsanız macbookda hali hazırda bulunan keychain içerisine şifreyi kaydedip oradan çağırabilirsiniz. Yeni şifre eklemek için aşağıdaki komut çalışıtırılmalı.

<div class="termy">

```console
$ security add-generic-password -s 'InfoshopPass'  -a 'eeken' -w 'password123'
```

</div>

- Bu komut ile şifrenizi keychaine ekledikten sonra aşağıdaki komutu çalıştırarak girdiğiniz şifreyi elde edebilirsiniz.

<div class="termy">

```console
$ security find-generic-password -w -s 'InfoshopPass' -a 'eeken'
```

</div>

- Şifrenizi secure bir yerde saklayıp script içerisinde görünmeden çağırmak istediğiniz takdirde de scripti aşağıdaki haline getirmeniz gerekmektedir.

<div class="termy">

```console
$ (security find-generic-password -w -s 'InfoshopPass' -a 'eeken'; vipaccess show -f /Users/{{ mac username }}/.vipaccess) | sudo -S openconnect --protocol=gp -u {{ infoshop username }} sslvpn.hepsiburada.com 2>&1
```
</div>