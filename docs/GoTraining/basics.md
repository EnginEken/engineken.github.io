### Variables

 - Değişkenler go dilinde `var <var_name> <var_type>` şeklinde tanımlanırlar. Değişken isimler underscore ya da harf ile başlamalıdır. 
 - Değişken ismi büyük harf ile başlarsa o isim program içerisinde export ediliyor ve program içerisinde kullanılabilir hale geliyor. O yüzden küçük harf ya da alt çizgi ile başlamalı. 
 - Değişken sayı ile başlayamaz ve ünlem işareti içeremez. Çünkü ünlem işareti go dilinde özel bir anlama sahiptir.

``` go linenums="1"
 import "fmt"

 func main() {
 	var speed int
 	fmt.Println(speed)
 }
```

 ***

#### Blank Identifier and Short Declaration

 - Değişken tipi tanımlarken iki değişkene aynı anda type verebiliriz. `var1, var2 string` şeklinde tanımladığımızda hem var1 hem de var2 değişkeni string tipinde oluşturuluyor.
 - `func Split(path string) (dir, file string)` bu tanımlamada Split fonksiyonu girdi olarak string tipinde path değişkeni alırken çıktı olarak da dir ve file isimli iki adet string tipinde değişken dönüyor. Spllit fonksiyonu `path` paketinin bir fonksiyonu ve girilen path değişkenini split ederek file ismi ve directory değerini(iki adet değişken) dönüyor.
 - `var dir, file string` kodu ile değişkenleri tanımlayıp; `dir, file = path.Split("css/main.css")` şeklinde fonksiyonu çağırıp dönen değerleri değişkenlere assign edebiliriz.
 - Eğer dönen değişkenlerde birini istemiyorsak, örneğin sadece file değşikenini almak istiyorsak, `var file string` ve `_, file = path.Split("css/main.css")` şeklinde yazmamız yeterli. Burada dir değişkeni yerine kullanılan alt çizgi `blank identifier` olarak tanımlanıyor. Kullanmak istenilmeyen değişkenler yerine bu kullanılır.
 - Kod daha da kısaltılarak `short declaration statement` olarak tanımlanabilir. Bu durumda file değişkeninin tipini de tanımlamaya gerek kalmıyor. Fonksiyondan dönen değerin tipi ne ise o tipte oluyor. Bunu yapmak için de `:` noktalama işareti kullanılıyor. `_, file := path.Split("css/main.css")` tek satırlık kodu ile hem dönen değişken tipinde hem de değerinde bir değişken elde etmiş oluyoruz.

``` go linenums="1"
import (
	"fmt"
	"path"
)

func main() {
	var dir, file string
	dir, file = path.Split("css/main/main.css")
	fmt.Println("dir: ", dir)
	fmt.Println("file: ", file)

	_, file = path.Split("js/main/main.js") //dir değişkenini istemiyorsak '_' ile blank identifier yapabiliriz.
	fmt.Println("file: ", file)

	//_, file := path.Split("img/icons/icon.png") --> Bu kısımda en üstteki değişken tipleri tanımlamak istemediğimizde short declaration statement kullanımı.
	//fmt.Println("file: ", file)					  En üstteki değişken tipi tanımlamasını silersek file değişkeni dönen değişkenin tipini alacaktır.
}
```
***

###### Use Declarations If

1. Eğer değişkenin ilk değeri bilinmiyorsa; `score := 0` yerine `var score int` kullanımı tavsiye ediliyor. Zaten program kendi bir ilk değer int değişkenine atıyor.
2. Package scoped değişken tanımlandığında; herhangi bir fonksiyon içinde değilse paket seviyesinde bir değişken ise zaten shot declarationa izin verilmiyor. 
3. Değişkenleri daha okunur olması için gruplamak gerektiğinde;

``` go linenums="1"
var (
	//related
	video string

	//closely related
	duration int
	current int 
	)
```

###### Use Short Declaration If

1. Eğer değişkenin ilk değeri biliniyorsa; `var width, height = 100, 50` yerine `width, height := 100, 50` kullanılır. 
2. Kodun kısa ve öz olması açısından da kullanışlıdır.
3. Tanımlanan bir değişkenin değeri değiştirilip onunla beraber yeni bir değişken tanımlanacaksa; `width = 50` ve `color := "red"` yerine `width, color := 50, "red" ` kullanımı tercih edilmelidir. Buna `redeclaration` denir.
4. Örneğin if statement içerisinde short declaration kullanılarak sadece o statement a ait bir değişken oluşturulabilir. 

***

#### Type Conversion

 - Typelar arası dönüşüm yapmamızı sağlar. Bunun için `type(value)` formatı kullanılır.
 - Dikkatli olunması gerekiyor çünkü tipler değişirken değişkenlerin değerleri değişebilir.

``` go linenums="1"
import "fmt"

func main() {
	speed := 100
	current_time := 2.5
	//Bu durumda current time değişkeni float64 tipinde olduğu için int tipine
	//dönüştürülürse değer değişerek 2 değerini alıyor ama olması gereken değer 2.5
	speed = speed * int(current_time)
	fmt.Println(speed)
	//Alttaki satır da hata verecek çünkü iki float64 tipindeki değişkenin çarpımı yine float64
	//Ama bu değeri speed değerine atadığımız için ve speed değişkeni de int olduğu için hata verir
	//speed = float64(speed) * current_time

	//Correct way
	speed = int(float64(speed) * current_time)
	fmt.Println(speed)
}
```

#### Raw Stirng Literals

- String literals `""` içerisinde tanımlanırken raw string literals markdowndaki gibi back tek tırnak kullanılarak tanımlanır.
- String literals birden fazla satır string içeremez ve bunda tab ve enter gibi karakterleri `\t\n` gibi escape karakterleri kullanarak yapılabilir. Ancal raw string literals birden fazla satır içerebilir. Bununla beraber de escape karakterleri yazmamıza gerek kalmaz. Örneğin html kodunu string değişkenine atarken kullanışlı.

```go linenums="1"
import "fmt"

func main() {
	var s string
	s = "How are you?"
	s = `How are you?`
	fmt.Println(s)

	//String Literal - I need to type escape character. Go interpret this strings
	s = "<html>\n\t<body>\"How are you?</body></html>"
	fmt.Println(s)

	//Raw String Literals - I dont need to type escape charcter. It is whatever it is.
	s = `
<html>
	<body>"How are you?"</body>
</html>
	`
	fmt.Println(s)

	fmt.Println("c:\\my\\dir\\file") // windows file. Double back slash because slash is a escape sequence
	fmt.Println(`c:\my\dir\file`)    //Raw String Literals
}
```

* The output of the above code:

```console
How are you?
<html>
        <body>"How are you?</body></html>

<html>
        <body>"How are you?"</body>
</html>

c:\my\dir\file
c:\my\dir\file
```

#### String Lengths

- `len()` fonksiyonu string değişkenlerinin karakter sayılarını değil byte sayılarını döner. İngilizce olmayan karakterler 4 byte a kadar alana sahip olabilirler. Örneğin `İ` karakteri 2 byte olduğu için `"Engİn"`  stringinin len fonksiyonu sonucu 5 karakter olmasına rağmen 6 olarak dönecektir.

```go linenums="1"
	lastName := "Eken"
	firstName := "Engİn"
	fmt.Println(len(lastName), len(firstName))
```

- Bunun üstesinden gelmek için `unicode/utf8` paketini import etmemiz gerekiyor. İmport ettinten sonra aşağıdaki kod bloğu şeklinde kullanarak ingilizce ya da İngilizce olmayan herhangi bir karakteri içeren stringlerin asıl karakter sayısını bulabiliriz.

```go linenums="1"
import ("fmt"; "unicode/utf8")
func main(){
	name := "Engİn"
	fmt.Println(utf8.RuneCountInString(name))
}
```

- Yukarıdaki kod parçacığının ekran çıktısı `4 6` olarak dönecektir.

#### IOTA

- Const değişkenlerin değerlerini integer olarak 1 arttırarak değer verdirmeye yarıyor. Örneğin günlerin 0 dan başlayarak 7 ye kadar değer almasını istiyorsak aşağıdaki gibi kodu yazmamız gerekiyor:

```go linenums="1"
const (
	monday		iota	//0 değerini alıyor
	tuesday				//iota 0dan başladığı için bir arttırılıp 1 değerini alıyor. Aşağı doğru 1 arttırılarak devam ediyor.
	wednesday
	thursday
	friday
	saturday
	sunday
	)

fmt.Println(monday, tuesday, wednesday, thursday, friday, saturday, sunday)
```

#### Println vs Printf