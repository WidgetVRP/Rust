# Struct'lar ve Lifetime'lar

Bu dersimizde 3 adet struct yapısından bahsedeceğiz. Structlar benzer verileri birbiri ile yeni bir değişken yapısı sayesinde gruplamamızı sağladığı için büyük önem arz etmektedir. Ayrıca `lifetime`'lara, neden var olduklarına ve arkalarında yatan tarihçeyi öğreneceğiz. Bu iki kavram da projelerimizde sıklıkla kullanacağımız ve büyük önem arz eden konular olmaktadır.

## __Structs (Yapılar)__

`Struct` lar türkçeye yapılar olarak çevrilmektedir. Aynı alandaki verileri tek başlık altında birleştirerek tek değeri ile erişmemizi sağlayan yapılardır. Rust 3 tip `struct` yapısına sahiptir. Bunlar:

- `name field`: Tüm komponentlere isim vermektedir. 
- `tuple like`: Tüm komponentleri oluşma sıralarına göre sınıflandırır.
- `unit like`: Aslında hiçbir componenti yoktur.

olmaktadır.


### 1) Name Field Struct

`Struct` yapılarını `struct` anahtar kelimesi ve yanına Camel Case ile yazılan isimle tanımlanmaktadırlar.

```Rust
struct Kullanici {

}
```

> `main()` fonksiyonu dışına herhangi bir fonksiyonun içinde olmadan yazılabilirler. 

Structlar daha önce de belirttiğimiz gibi kendimizin veri tipi oluşturmamızı sağlayan yapılardır. `Name Field Struct` yapılarında ise veri tipi içerisindeki kaydedileceği ismi ve verinin hangi tipe sahip olacağını tanımlamamız gerekmektedir.

```Rust
struct Kullanici{
    aktiflik_durumu: bool,
    kullanici_adi: String,
    giris_yapma_sayisi: u32,
}
```

Artık `Kullanici` adında bir veri tipi oluşturduk. Bu veri tipine sahip bir değişen oluşturmak istediğimizde alt kısımda olduğu gibi bir yapı kullanmamız gerekmektedir:

```Rust
fn main(){
    let kullanici1 = Kullanici{aktiflik_durumu: true, kullanici_adi: String::from("Widget"), giris_yapma_sayisi: 0}
}
```

Artık `kullanici1` adında ve `Kullanici` türünde bir değişken ürettik. Peki bu değişkenin içerisindeki verilere nasıl erişiyoruz? Burada karşımıza `.` işlemleri gelmekte. `Struct` ile oluşturduğumuz değişkenlerde  bu değişkenin yanına `.` koyarak istediğimiz özelliğine erişebiliriz.

```Rust
println!("Kullanıcı aktif mi? {}", kullanici1.aktiflik_durumu);

println!("Kullanıcının ismi ne? {}", kullanici1.kullanici_adi);

println!("Kullanıcı kaç kez sisteme girmiş? {}", kullanici.giris_yapma_sayisi)
```

Aynı zamanda belki de `Kulanici` tipinde bir değişkeni bizim için oluşturacak bir fonksiyon da yazabiliriz.  

```Rust
fn kullanici_olustur(kullanici_adi: String) -> Kullanici {
    Kullanici{
        kullanici_adi,
        aktiflik_durumu : true,
        giris_yapma_sayisi : 1,
    }
}
```

Bu şekilde kullanıcımızı fonksiyon kullanarak oluşturabiliriz. Bu kısımda `kullanici_adi` değeri dışındaki parameterlere `:` ile bir değer tanımladığımızı görüntüleyebilirsiniz. Bu durumun nedeni Rust'ın parametre olarak girilen `kullanici_adi` değişkeni ile `Kullanici` struct'ındaki anahtarın aynı olduğunu fark edecek kadar zeki olup otomatik atama sağlamasıdır. Bu yapı yerine `kullanici_adi: kullanici_adi` da diyebiliriz ancak gerek bulunmamakta. 

Fonksiyonumuzu oluşturduğumuza göre artık çağıra da biliriz.

```Rust
fn main(){
    let kullanici2 = kullanici_olustur(String::from("İrem"));
    println!("{}", kullanici1.kullanici_adi); // İrem
}
```

### 2) Tuple Like Structs

`Tuple like structs` değerleri daha önce de söylediğimiz üzere `name field`'a nazaran isimle değil eklenme sırası ile içindeki değişkenleri tanımlamaktadır. 

Bu tipte bir `tuple struct` oluşturmak için şu şekilde bir tanım yapabiliriz.

```Rust
struct Koordinatlar(i32,i32,i32);
```

Sonrasında da herhangi bir fonksiyon içeisinde aynı şekilde çağırabilirim ve içerisindeki değerlere `.` notasyonu ve index numarası ile ulaşabilirim:

```Rust
fn main(){
    let kordinat_bilgisi = Koordinatlar(1,2,3);
    println!("{}",kordinat_bilgisi.0); //1
}
```

### 3) Unit Like Struct

Peki içlerinde veri olmayan `Unit like struct` değerleri nasıl tanımlanmakta? Bu değerleri sadece `struct` anahtar kelimesi ve CamelCase isimlendirme ile tanımlayabiliriz. İç kısmına birşey koymamamız gerekmektedir.

```Rust
struct unitTipiStruct;
```

Peki bu yapılar nerede kullanılmaktadır? Aslında bu yapıyı biz öncesinde kullandık. Bir vektör içerisine belirli sayılar arasında değer oluşturmak için `1..5` ifadesini kullandık. Burada yer alan `..` yapısı struct içerisindeki `range` yani aralık değerinin kısaltması olmaktadır. Yani açılınca `1..5 => Range {start:1 ,end:5} ifadesine karşılık gelmektedir. 

Bu tip structları ileride daha ayrıntılı görecek olsak da şu anlık bizim için önemli olanlar `name field struct` yapıları olmaktadır.

## Methodlar

`Methodlar` fonksiyonlara benzerlik gösterse de struct içerisinde tanımlanmalarından dolayı farklılık gösterirler. Metotlar her zaman ilk parametre olarak `self` değerine yani kendilerine sahiptir. Bu da struct yapısını çağıran yapıya işaret etmektedir. 

Hadi bir karenin içeriğini ifade eden bir struct oluşturalım.

```Rust
struct Kare{
    genislik: u32,
    uzunluk: u32,
}
```

Sonrasında bu kare tipine sahip bir değişkeni `main()` içerisinde oluşturalım.

```Rust
fn main(){
    let kare1 = Kare{
        genislik: 5,
        uzunluk: 5,
    }
}
```

Rust içerisinde bir `struct` yapısına `method` eklemek için `impliment` kelimesinin kısaltması olan `impl` anahtar kelimesini kullanabiliriz. Sonrasında da hangi `struct` yapısına methodumuzu ekleyeceksek o yapının ismini vermemiz gerekmektedir.

```Rust
impl Kare{

}
```

Üstte ifade edildiği şekilde oluşturulduktan sonra içerisine methodlarımızı fonksiyon olarak verebiliriz. Unutmamamız gereken şey bu yapının ilk parametre olarak `self` yani `struct` yapısının kendisini aldığıdır. 

```Rust
impl Kare{
    fn alan_bul(&self) -> u32{
        self.genislik * self.uzunluk
    }
}
```

Bu şekilde kendi içersinde daha önceden tanımladığımız değerlere erişebilir ve fonksiyonu çalıştırabiliriz. Fonksiyonu çalıştırmak için `Kare` struct'ı ile oluşturduğumuz değişkene `.` koyarak fonksiyonun ismini yazabiliriz.

```Rust
fn main(){
    let kare1 = Kare{
        genislik: 5,
        uzunluk: 5,
    };
    println!("{}", kare1.alan_bul())
}
```

Rust içerisinde `impl` ile `struct`'ların içerisinde istediğimiz kadar method oluşturabiliriz.

Peki bir `mutable` yani değiştirilebilir struct yapmak istesek nasıl yapacağız? Bu işlemi yapmak için `kare1` olarak tanımladığımız değeri `mutable` yapmamız gerekmektedir. 

```Rust
fn main(){
    let mut kare1 = Kare{
        genislik: 5,
        uzunluk: 5,
    };
    println!("{}", kare1.alan_bul())
}
```

Sonrasında bu işlemi yeni bir method oluşturarak halledebiliriz.

```Rust
impl Kare{
    fn alan_bul(&self) -> u32{
        self.genislik * self.uzunluk
    }
    fn uzunluk_degistir(&mut self, yeni_uzunluk: u32){
         self.uzunluk = yeni_uzunluk;
    }
}
```

Sonrasında oluşturduğumuz bu methodu `main()` içerisinde istediğimiz uzunluk değeri ile değiştirebiliriz.

```Rust
fn main(){
    let mut kare1 = Kare{
        genislik: 5,
        uzunluk: 5,
    };
    println!("{}", kare1.uzunluk()) // 5
    kare1.uzunluk_degistir(10);
    println!("{}", kare1.uzunluk()) // 10
}
```

> Ayrıca mutable bir referans kullanmaya dikkat etmemiz gerekmektedir.

---

## __Lifetimes__

Bu başlığımızda `lifetime` ifadelerinden bahsedeceğiz. Dikkat etmemiz gereken şey Rust üzerindeki her bir referansın belirli bir `Life time`'a yani yaşam süresine sahip olduğudur. Çoğu zaman `lifetime`'lar üstü kapalıdır ve çok bahsedilmez. Bazıları da bu özelliğin Rust'ı en çok diğerlerinden ayıran özelliği olduğunu söylerler. 

`Liftime`'ların ana olarak odaklandığı şeyler `dangling referance` yani sahipsiz referansları önlemektir. Hadi örnek olarak `dangling refferance` oluşturalım.

```Rust
fn main(){
    let r;
    {
        let x = 5;
        r = &x;
    }
    println!("{}", r)
}
```

Burada kodumzu çalıştırdığımızda "barrowed value does not live long enough" hatası ile karşılaşırız. Bu durumun nedeni x'in tanılıldığı parantez biter bitmez x değerinin düşürülerek referansını da yok etmişizdir. Ancak sonrasında yazdırmaya çalıştığımızda olmayan bir referansı yazdıramayacağımız için hata oluşmuştur. Bu işlemi `lifetime` sayesinde daha iyi anlayabiliriz ve Rust Compiler bize daha kod çalışmadan bu durumu bildirir. 

`Lifetime` yazımları daha da incelediğimizde bu yapıların bir referansın ne kadar yaşayacağını değiştirmeyeceğini unutmamamız gerekir. `Lifetime` yazımlar sadece birden fazla referansın birbiri ile ilişkisini yaşam süresinin değiştirmeden ifade etmektedir. 

Bir lifetime oluşturmak için genelde `'a` yapısı kullanılmaktadır. Bu bağlamda alt kısımdaki gibi yazımlar yapabiliriz:

- `&i32`: Bir 32 bit integer değerine referans
- `&'a i32`: Bir 32 bit integer değerine özel `lifetime` ile referans
- `&'a mut i32`: Bir 32 bit integer değerine özel `lifetime` ile değişebilir referans

Hadi bir fonksiyon oluşturarak bu kavramların biraz daha neleri ifade ettiğine bakalım:


```Rust
fn örnek<'a>(x: &'a str) -> &'a str{
    return x
}
```

Life time'lar birbirine bağlandığı değerlernden en az kim yaşıyorsa ona kendini bağlamaktadır. Normalde onlarsız da kod teoride çalışabilse de Rust Compiler'ın aklı karışır ve anlayamaz. Bu nedenle ona anlatmamız gerekir. Bu life'time değerlerini fonksiyonlara verirken `generic tpye` olduğu için fonksiyon isminden hemen sonra `<>` içerisine yazarız .Biraz karışık bir kavram gibi gelebilir ancak örnekler ile ifade edeceğim. 

> Diyelim ki aileniz ile 3 araba yola çıktınız ve Antalya Trabzon yolunu kullanacaksınız. Bir arabayı siz, bir arabayı ablanız, bir arabayı babanız kullanıyor. Hepiniz "Antalya - Konya - Aksaray - Nevşehir - Kayseri - Sivas - Erzincan - Erzurum - Bayburt - Trabzon" yolunu kullanacaksınız. Ama siz Trabzona gidecekken babanız cağ kebap yemek için Erzurum'da, ablanız sucuk yemek için Kayseri'de durdu ve orada kalacaklar. Anneniz de dedi ki oğlum/kızım gelin hep birlikte bir aile fotoğrafı çektirelim yoldayken. Bu aile fotoğrafını küçük kardeşiniz çekecek ama bilmiyor hangi şehir nerede ve hangi şehir kimden sonra geliyor. Küçük kardeşimiz artık bir Rust Compiler. Küçük kardeşiniz yolu bilmediği için fotoğraf çekme görevi altında kendini ezilmiş hissediyor ve ağlayarak herkesin yola çıkmamasını sağlıyor. Sizin yapmanız gereken şey aile fotoğrafı için her bir arabanın arka arkaya gideceği yolları kardeşinize anlatmak ve en erken ayrılacak (yani lifetime'ı en kısa sürecek) ablanızdan dolayı Kayseriye kadar çekebileceğimizi söylediniz. Artık kardeşiniz anladı ve Kayseriye kadar anneniz ne zaman derse desin kardeşiniz fotoğrafı çekecek. Ama eğer kayseriden sonra derse artık çekemeyeceğini söyleyerek ağlamaya devam edecek ve yolunuzu mahfedecek.

Örneğimizde biraz daha net aklınıza oturduysa compiler birbirine aynı etiket ile bağlanan değerleri görecek ve bunlar arasında en az hayatta kalana göre hepsi sanki o kadar yaşasacakmış gibi davranacaktır. Bu nedenle hatayı en az yaşayacağın yaşadığı süre boyunca göstermeyecektir.

Peki nedir bu `lifetime` ve neden Rust içerisinde var? Normalde önceden Rust'ın ilk sürümlerinde liftime her bir değer için kullanılmaktaydı. Ancak Rust geliştiricileri yazılımcıların sürekli olarak benzer yapılar için aynı `lifetime` yazımları kullandığını fark ederek compiler içerisine bazı yazımları gömdü. Bu sayede bu sayede Rust yazılımcıları her seferinde kod içerisine bu tanımları eklemekten kurtuldu. 

Bu lifetime değerlerinin compiler tarafından algılanması için ana olarak 3 kuralı bulunmaktadır. 

- Her bir parametre referansı kendi `lifetime` parametresini almaktadır. Örneğin tek parametremiz varsa `'a` ifadesini kullanabiliriz. Ancak eğer iki parametremiz varsa `'a` ve `'b` ifadelerini kullanabiliriz. Kaç parametremiz varsa o kadar lifetime ifadesine sahip olabiliriz.

- Çıktı olarak tanımlanan değere sadece bir adet lifetime ifadesi tanımlanabilir. 

- Eğer birden çok lifetime referansı varsa ve bunlardan biri `self` yapısına veya değitirilemez `self` yapısına referans ediyorsa bu referansın bir methoda hizmet etmesinden olayı tüm parametrelerin lifetime referansı `self` değerinin lifetime referansına eşit olur.

Hadi bir örnek ile buna bakalım

```Rust
fn ornek<'a,'b>(x: &'a str, y: &'b str)-> &'b str{
    return x //error
}
```
```Rust
fn ornek<'a,'b>(x: &'a str, y: &'b str)-> &'a str{
    return x // Hata yok
}
```

Aynı zamanda `struct` yapıları da kendi içerilerinde referans tutabilirler. Ancak bu işlemi gerçekleştirmek için `lifetime` yazıma ihtiyaç duymaktadırlar.

```Rust
struct Stringim {
    text: &str // Error
}
```

```Rust
struct Stringim<'a> {
    text: &'a str // Error
}
```

Bir değişkenin sonsuza kadar yaşamasını istiyorsak onu `statik lifetime` değerinine çevirebiliriz. Bu işlemi gerçekleştirmek için `&'static` ifadesini kullanabiliriz. Bu sayede referansımız kodumuz açık kaldığı sürece varlık gösterecektir.

```Rust
let s: &'static str = "Statik yaşarım ben anı yaşarım ben"
```
