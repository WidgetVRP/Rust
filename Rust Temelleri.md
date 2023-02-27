# The Basics

## Hello World

İlk olarak rust dosyamızın yer alacağı dosyayı oluşturmamız gerekmektedir. Bu işlemi alt kısımda ifade edildiği üzere `cargo` ile yapabiliriz.

```
cargo new merhaba
```
Kodu istediğimiz klasörde çalıştırdığımızda bizler için bir çalışma ortamı hazırladığını görüntüleyebiliriz. Bu ortam içerisinde birçok dosya yer almaktadır.

Peki nedir bu Cargo? Cargo rust için paket indirmeyi, kontrol etmeyi vb. işlemleri yapan Rust'ın her alanında yardımı dokunabilen bir araçtır. Cargo ile:

- Yeni bir proje başlatabiliriz
- Projemizi çalıştırabiliriz
- Kütüphane ekleyebiliriz 
- Dokümantasyonları kontrol edebiliriz

Oluşturduğumuz `merhaba` klasörü içerisinde girdiğimizde `cargo.toml` adında bir dosyanın oluştuğunu görüntüleyebiliriz. Bu dosya içerisinde projemiz için gerekli bilgileri tutmaktadır. Bunlardan bazıları:

- name: Proje ismi olmaktadır. Farklı yerlere bakmaksızın projemiz ismini sadece bu değerin ifadesi içerisinden almaktadır.

- version: Projemizin version numarası olmaktadır. Projemizi geliştirdikçe bu sayıyı yükseltebiliriz.

- edition: Projemizin hangi rust sürümü içerisinde geliştirildiğini ifade etmektedir. 

- dependencies: Bu başlık altında sonradan ekleyebileceğimiz kütüphaneler yer alacaktır.

Oluşan `merhaba` klasörünün içerisine girdiğimizde `src` klasörünü de görüntüleyebiliriz. Bu klasör rust kodlarımızın yer alacağı klasördür. 

Şu anlık içerisinde sadece ana rust kodumuzun yer aldığı `main.rs` dosyası bulunmaktadır. Bu dosyayı çalıştırabilir veya build edebiliriz. 

```
cargo build
```

Dosyamızı build ettikten sonra `merhaba` klasörü içerisinde `target` klasörünün oluştuğunu gözlemleyebiliriz. Bu klasör projemizin sonradan çalışabilecek hâli olmaktadır.

Build ettikten sonra kodumuzu çalıştırabiliriz. Kodumuz içerisinde bizler için önceden yazılmış hello world dosyasını içermektedir. 

```
cargo run
```

Kodumuzu çalıştırdığımızda konsol'a `Hello, World!` yazıldığını görüntüleyebiliriz.

Tebrikler Rust içerisinde ilk programımızı çalıştırdık!

--- 
## Değişkenler ve Değişebilirlik

Değişkenler projelerimizde kullanmak üzere isimlendirdiğimiz verilerdir. Değişebilirlik yani "mutability" ise bu verinin nasıl manüpüle edilebileceğini ifade etmektedir. Eğer değişkense istediğimiz şekilde bu değeri değiştirebiliriz ancak değişmez ise bu değeri değiştiremeyiz. 

Bu özellikleri deneyebilmek için yeni bir klasör ve proje oluşturabiliriz. Bu işlemi her zamanki gibi `cargo new X` komudu ile yaparız. 

```
cargo new degiskenler
```

Bu komudu çalıştırdığımızda `degiskenler` adlı bir dosya oluştuğunu ve bu klasör içerisinde proje oluştuğunu gözlemleyebiliriz. Bu klasörün içerisindeki `src/main.rs` dizinine ilerlediğimizde kodu yazacağımız alan karşımıza çıkar.

Karşımıza bir fonksiyon yapısı çıkmaktadır. Bu fonksiyonda bulunan `println!` yapısını kaldırabiliriz.

```Rust
fn main() {
    
}
```

İlk olarak bir değişken oluşuturarak başlayabiliriz. Bu değişkeni oluştururken `let` anahtar kelimesini kullanabiliriz. Sonrasında değişkenin ismi ve `=` sonrasında değeri yer alacaktır.

```Rust
let x = 5;
```

Sonrasında tanımladığımız bu değişkeni konsola yazdırabiliriz. Bu işlem için `merhaba` projesinde de göröüş olabileceğiniz üzere `println!()` makrosu kullanılacaktır. Bu makro ile ilgili ileriki kısımlarda yeni bilgiler vereceğiz. 

```Rust
println!("x'in değeri {} olmaktadır", x);
```

Yukarıdaki şekilde bir yapı oluşturabiliriz. Bu yapıda `x` süslü parantezlerin içerisine Rust tarafından yerleştirilecektir.

Artık kodumuz hazır. Bu kodu çalıştırmak için `cargo build` ve `cargo run` komutlarından yaralanabiliriz.

Kodumuzu terminalde çalıştırdığımızda `x'in değeri 5 olmaktadır` çıktısını aldığımızı görüntüleyebiliriz. Ancak fark edeceğiniz üzere birden fazla uyarı yani `warning` bulunmaktadır. Bu uyarılar kodun çalışmasına engel olmasada düzeltilmesi gerekmektedir. Şimdilik görmezden gelelim.

Peki bu `x` değerini 5'den farklı bir değer ile değiştirmek istesek mesela 6, nasıl bir sonuç ile karşılaşırız?

```Rust
x = 6;
```

Kodu çalıştırmaya çalıştığımızda çalışmadığını görüntüleyebiliriz. Bu durumun nedeni `let` ile yalın bir şekilde tanımladığımız değişkenlerin otomatik olarak `değişmez` yani `immutable` halde olmasıdır. Eğer bu değişkenin değerini değişmek istiyorsak `mutable` bir tanımlama yapmamız gerekmektedir.

```Rust
fn main() {
    let mut x = 5;
    println!("x'in değeri {} olmaktadır", x);
    x = 6;
    println!("x'in değeri {} olmaktadır", x);
}
```

Değişkenimizi `mut` anahtar kelimesi ile tanımladıktan sonra değiştirmek istediğimizde herhangi bir skıntı olmadan bu işlemi yapabildiğimizi görüntüleyebiliriz. 

```
x'in değeri 5 olmaktadır
x'in değeri 6 olmaktadır
```

Eğer kodumuzu çalıştırırsak üst kısımda ifade edildiği gibi bir sonuç çıkacaktır.

Rust içerisinde değişkenlerin değişmemelerinin de kademesi olmaktadır. Bu duruma örnek olarak `sabitler` verilebilir. Bu sabitler kesinlikle değişmezler ve `const` anahtar kelimesi ile tanımlanırlar.

```
const SANIYELER: i8 = 60;
```

Sabitlere baktığımızda tanımlanırken bazı işlemlerin daha farklı olduğunu görüntüleyebiliriz. Bunları dört kısımda inceleyebiliriz:

- Sabitlerin isimleri her zaman tamamı büyük harften oluşan şekilde tanımlanmaktadır.
- Sabitler tanımlanırken her zaman türü eklenmelidir (i8). Bu konuda ilerde açıklamalar yapacağız.
- Sabitler her zaman tanımlandığı anda bir değere atanmalıdır. Diğer değişkenler `let x` şeklinde boş oluşturulabilinirken sabitler her zaman bir değere atanmalıdır.
- Sabitler fonksiyon gibi işlemlere direkt olarak tanımlanamaz.

Sonrasında bu sabiti diğer değişkenlere yaptığımız şekilde ekrana yazdırabiliriz.

```RUST
const SANIYELER: i8 = 60;
println!("Saniyeler {} değerine eşit olmaktadır",SANIYELER);
```

Kodumuzu çalıştırdığımızda aynı değerin yazdırıldığı görülebilinir.

Ayrıca not olarak `const` değerleri yanlarına `mut` anahtar kelimesini değişken olmamalarından dolayı kabul etmemektedirler.

---

## __Veri Tipleri__

Rust içerisinde birçok veri tipi vardır ve bu veri tipleri kendi aralarında büyük farklılık göstermektedir. Veri tipleri tanımlarken değişken ismimizden sonra değerini belirtmeden önce `:Tip` şeklinde bir tanımlama yapmamız gerekmektedir. Alt kısımlardaki veri tiplerinde bu yapının neyi ifade ettiğini görüntüleyebilirsiniz.

## -> Skalel Veri Tipleri

Skalel tipler tekil verileri ifade etmektedir. 4 ana başlık içerisinde incelenmektedirler. Bunlar:

- Integer (Tam sayılar)
- Flooting Point (Ondalıklı sayılar)
- Booleans (Mantıksal Değer)
- Characters (Karakterler)

## 1) Integer (Tam Sayı)

İlk olarak `Integer` değerleri ile başlayacağız. Integer değerler kendi aralarında içerilerinde tuttukları verinin büyüklüğüne göre sınıflandırılırlar. Bu sınıflandırma şu şekildedir:

- 8 Bit integer değer (i8)
- 16 Bit integer değer (i16)
- 32 Bit integer değer (i32)
- 64 Bit integer değer (i64)
- 64 Bit integer değer (i128)

Bir integer tanımlarken altta ifade edilen ifadeyi kullanmamız yeterli olacaktır.

```Rust
let x: i8 = 10;
```

Yukarıda ifade edildiği şekilde tanımladığımız değişken 8 bit yer kaplayacak ve 10 değerine sahip olacaktır.

Bu integer değerleri işaretli olabildikleri gibi işaretsiz de olabilirler. Bu ne demek? Eğer bir integer değeri tanımlarken bu değerin `+` `-` gibi işaretlere sahip olmasını istemiyorsak `unsigned` yani işaretsiz integer değeri kullanabiliriz. Ancak eğer değerimiz negatifse ve bu şekilde kullanmak istiyorsak `signed` integer değerleri kullanabiliriz. 

Signed integer değerlerimizi normalde kullandığımız `i8` vb. şekilde tanımlarken işaretsiz yani unsigned değerleri `u` anahtar kelimesi ile tanımlamaktayız. Bunlar:

- 8 Bit işaretli integer değer (u8)
- 16 Bit işaretli integer değer (u16)
- 32 Bit işaretli integer değer (u32)
- 64 Bit işaretli integer değer (u64)
- 64 Bit işaretli integer değer (u128)

Bir işaretsiz integer değerini de benzer şekilde tanımlamamız mümkündür:

```Rust
let y: u8 = 32;
```

Bu şekilde 8 bit büyüklüğünde işaretsiz 32 değeri tutan bir y değişkeni tanımlamış oluruz.

Şimdi de özelleşmiş tam sayı türleri hakkında konuşabiliriz. Rust içerisinde direkt olarak `binary`, `hex` , `octal` ve `decimal` sayıları da tanımlayabiliriz. Bunları sırasıyla altta ifade edildiği şekilde tanımlayabiliriz.

- binary: let binary = `0b`1111_1111
- hex: let hex = `0x`ff
- octal: let octal = `0o`377
- binary: let decimal = `02`_55

Üstte gösterilen tanımlarda değişken isimlerinin önemli olmadığına ve değerin başında kullanılan işaretli 2 karakterin integerın tipini ifade ettiğini görüntüleyebiliriz.

Not: Rust içerisinde `//` yapısı ile istemediğimiz kod satılarının çalışmasını kapatabiliriz. Buna yorum satırı denmektedir.

## 2) Flooting Point (Ondalıklı sayılar)

Ondalıklı sayılar da tanımlanırken integer değerlere benzer şekilde çalışmaktadırlar. İlk olarak tipini özel olarak tanımlamadan oluşturduğumuz değişkenlere bakacak olursak:

```Rust
let x = 2.0;
```

Üstte ifade edildiği şekilde tip kullanmadan tanımladığımız değişkenlerde ondalıklı sayılara otomatik olarak bir tip tanımlanmaktadır. Bu tip `f64` olmaktadır. 

Integerlara benzer şekilde ondalık sayılarda birden fazla türde tanımlanabilir. Bunlar:

- 32 bit büyüklüğe sahip flooting pointle (f32)
- 64 bit büyüklüğe sahip flooting pointle (f64)

Flootlar IEEE-754 standartlarını takip etmektedirler.

```Rust
let x:f32 = 20.0
let x:f64 = 20.1
```

## 3) Booleans (Mantıksal Değer)

Boolean değerler Rust ve diğer diller içerisinde mantıksal değerleri ifade etmektedir. Sadece `True` ve `False` değerlerini içerebilirler. Kendi içerisinde farkli şekillerde yer alamayacağı için özellikle tip belirmenin gereği olmamaktadır. Küçük harfler ile yazılırlar.

```Rust 
let t = true;
let f:bool = false;
```

## 4) Characters (Karakterler)

Rust içerisinde karakterler diğer dillerde olduğu gibi tanımlanabilirler. 

```Rust
let c = 'c';
```
 
Aslında yanlış isimlendirilmişlerdir. Char kısaltması ile tanımlıyor olsak da unicode tek bir değeri ifade etmektedir. BU değer bizim alfabemizden farklı ülkelerde kullanılan farklı alfabe yapılarından emojilerden kanjilere neredeyse herşeye atanabilir.

Characterler her zaman 4 byte olmaktadır. Tek tire ile tanımlanmaktadırlar ve genelde işlevsizlerdir. UTF-8 olmamalarından dolayı genelde yerlerine `string` yapıları kullanılmaktadır.

## 5) Aritmetik Operatörler

Çoğu kodlama dilinde olduğu gibi Rust'da aritmetik işlemleri desteklemektedir. Bu işlemler şu karakterler ile yapmak mümkündür:

- `+` toplama yapmaktadır.
- `-` çıkarma yapmaktadır.
- `*` çarpma yapmaktadır.
- `/` bölme yapmaktadır.
- `%` kalan bulmaktadır.

## -> Compound Veri Tipleri

Compound veri tipleri içerisinde birden çok veriyi taşıyabilen veri tiplerine verilen isim olmaktadır. Rust 2 adet Compound veri tipine sahiptir. Bunlar:

- Tuples (Demetler)
- Arrays (Diziler)

## 6) Tuples (Demetler)

Tuple'lar aynı veya farklı türden birden fazla veriyi gruplandırarak içerisinde tutan compound veri tipi olmaktadır. Parantezler ile `()` tanımlanmaktadırlar.

Tuple'ların belirli bir büyüklükleri vardır. Tanımlandıktan sonra büyüyemez veya küçülemezler. Şu şekilde tanımlanırlar:

```Rust
let tup = (500, "hi", true);
```

Yukarıda görüntülendiği üzere tip eklenmeden tanımlanabilirler. Eğer tipler ile tanıtmak istiyorsak şu şekilde bir tanım yapabiliriz:

```Rust
let tup :(u8, String, bool) = (500, "hi", true);
```

Tuple içerisindeki verilere `.` indexleri ile erişebiliriz. Önce tuple'ımızın adını yazarız, nokta koyarız ve son olarak 0'dan başlayarak hangi index numarasına sahip elemana erişeceğimizin sayısını gireriz.  Örneğin tuple içerisindeki `"hi"` değerine erişmek istiyorsak sıfırdan başlayarak sayarız ve `1` indexsine sahip olduğunu saptarız. Sonrasında altta bulunan kod ile bunu farklı bir değişkene atayabilir veya istediğimiz kod bloğu içerisinde kullanabiliriz.

```Rust
let merhaba = tup.1;
```

Bu tuple içerisindeki verilere erişmenin tek yolu olmamaktadır. Eğer bir tuple içerisindeki her veriye erişmek ve her birini bir değişkene atamak istiyorsak şunları yapabiliriz:

```Rust
let (x,y,z) = tup;
```

Bu şekilde sırasıyla index numaralarındaki verilerimizi `x,y ve z` değerlerine eşitleyebiliriz. 

Aynı zamanda bu değişkenlerin her birinin tiplerinin farklı olmasını sağlayabiliriz.

```Rust
let (x:i32,y:&str,z:bool) = tup;
```

## 7) Arrayler (Diziler)

Arrayler tuple'lar gibi içerilerinde birden fazla veri tutmamızı sağlayan compound veri tipleridir. Arraylerinde tuple'lar gibi sabit bir uzunluğu olmaktadır. Ancak Tuple'ların aksine arraylerin içerisinde yer alan her bir element aynı tipte olmalıdır. Birden fazla tanım yöntemi bulunmaktadır. Köşeli parantez `[]` ile tanımlanmaktadırlar.

```Rust
let dizim = [4,5,6];
```

Tupler'lar gibi içerisindeki elemanlara erişirken 0'dan başlayarak saymamız gerekmektedir. Arraylerin içerisindeki bir veriye erişmek istiyorsak bu sefer `.` yerine `[]` yapısı kullandığımızı görüntüleyebiliriz. Eğer `dizim` olarak tanımladığımız arrayin `5` verisine erişmek istiyorsak arrayimizin isminni yanına 0'dan başlayarak sayarak bulduğumuz 1 değerini köşeli parantez içerisinde yazmamız yeterlidir.

```Rust
let bes = dizim[1] // 5
``` 

Rust içerisinde arrayleri farklı şekillerde oluşturmak da mümkündür. Örneği belirli bir uzunluğa sahip belirli türden bir array oluşturmak istiyorsak tip kısmına köşeli parantezler ile türü ve bu arrayin uzunluğunu yazabiliriz.

```Rust
let array2 : [i32;3] = [7,8,9];
```

Arraylerin tuple'lar gibi kesin bir sabitliği bulunmamaktadır. Bu nedenle uygun düzenlemeler ile içerilerindeki veriler değiştirilebilir. Bu işlem için array'imizi mutable yapmamız gerekmektedir.

```Rust
let mut array2 : [i32;3] = [7,8,9];
array2[2] = 10; // Artık sonuncu eleman 9 değil 10
```

Peki arrayimizin içerisinden listeden daha büyük bir değere erişmek istersek yani mesala 3 elemanlı yukarıdaki listede 4. elemana ulaşmaya çalışırsak ne olur? Bu işlem gerçekleştiğinde Rust compiler olan `Cargo` bize hata oluşturacak ve `panic` hatası verecektir. Bu durum bizler için çok iyidir çünkü bu hataları kod içerisinde çözmemiz mümkündür. Bu adım hafıza güvenliği için hayati olmaktadır ve sonraki kısımlarda incelenecektir.  

## 8) Vektörler

Vektörler yeninden boyutlandırılabilen element dizilerilerdir. Normal arraylerin boyutları değiştirilemezken bu arraylerin tipleri değiştirilebilir. Heap üzerinde depolanmaktadırlar.

NOT: Heap Nedir?

Vektörler rust üzerinde büyük bir öneme sahiptir. Bu durumun nedeni değişken bir uzunluğa sahip liste oluşturmak istediğimizde vektörleri kullanmamız gerekmesidir. 

Vectörlerin en kolay tanımlanma yöntemi `VEC Macro` (Makrolardan ileriki derslerde bahsedeceğiz) olmaktadır. Bu makroyu şu şekilde tanımlayabiliriz:

```Rust
let mut nums = vec![1,2,3];
```
Tanımladığımız bu vectore bahsettiğimiz gibi veri eklemek mümkündür. Bu işlemi diğer kodlama dillerinde de aşina olabileceğiniz şekilde `push` yöntemi ile yapmaktayız. Vektörümüzü bir değer ekleyerek değiştireceğimizden dolayı `mutable` bir vektör olmasına dikkat etmeliyiz.

```Rust
nums.push(4);
```

Vektörlerimizi görüntülemek için ekrana `println!` makrosu ile yazdımak istediğimizde bu işlemin bir hata ile karşılaşacağını görüntüleyebiliriz.

```Rust
println!("{}",nums); //error
```

Bu hatayı çözmek için işlemi de debug modunda çalıştırmamız mümkündür. Bu moda süslü parantez içerisinde `:?` ile geçmek mümkündür.

```Rust
println!("{:?}",nums); //[1, 2, 3, 4]
```

Bu vektörlere veri ekleyebildiğimiz gibi çıkarmamız da mümkündür. Bu çıkarma işlemini yapmamın birçok yöntemi bulunmaktadır.

Yöntemlerden biri `.pop()` metodu olmaktadır. Bu method ile vektörümüzün son elamanını listeden çıkarmak mümkün olmaktadır.

```Rust
numps.pop();
println!("{:?}",nums); //[1, 2, 3]
```

Rust içerisinde vektör oluşturmanın farklı yolları da bulunmaktadır. Bunlardan biri de `Vec::new()` olmaktadır. Aslında daha önceden gördüğümüz `VEC Macro` ifadesini çağırdığımızda Rust arka planda `Vec::new()` yapısını kullanmaktadır. Bu şekilde oluşturduğumuz vektörlere de aynı şekilde tüm işlemleri yapabiliriz.

```Rust
let mut vec = Vec::new();
vec.push("Test");
vec.push("String")
```

Bu şekilde oluşturduğumuz vektörlerde başlangıç değeri atamak tanım içerisinde direkt olarak mümkün olmamaktadır.

Rust içerisinde Vektörler ile ilgili birçok önceden bizim kullanmamız için tasarlanmış methot bulunmaktadır. Bunlardan bazıları:

- `.reverse()` vektörü ters çevirmektedir.
- `.push()` vektöre eleman eklemektedir.
- `.pop()` vektörden eleman çıkarmaktadır.

Aynı zamanda üstte verilen şekli ile rust içerisinde belirli boyutlara sahip vektör oluşturmak da mümkündür. Bu işlemi altta ifade edilen kod ile gerçekleştirebiliriz.

```Rust
let mut vectorum = Vec::<i32>::with_capacity(2);
```

Bu şekilde oluşturduğumuz 2 elemanlı vektörün sonraında büyüklüğünü de vektörler içerisinde gelen bir diğer methot olan `.capacity()` işlemi ile öğrenebiliriz.

```Rust
println!("{}", vect.capacity()) // 2
```

Peki bu işlemi yapması sonucunda eğer bu vektöre 2'den fazla eleman eklemek istersek bunu gerçekleştirebilecek mi? Cevap evet olmakta. Boyutundan büyük bir veri eklemeye çalıştığımızda Rust bunu anlayacak eski vektör elemanlarını kopyalayacak hafızada yeni bir alan açarak eski ve yeni verileri uygun boyutla ekleyecek son olarak da eski veriyi silecektir.

Aynı zamanda vektörler tekrar eden veriler (Bunlardan ileride bahsedeceğiz) ile de tanımlanabilinir. Örneğin [0,5) aralığını  tanımlamak için rust içerisinde şu şekilde bir işlem yapabiliriz:

```Rust
let v: Vec<i32> = (0..5).collect();
```

## 9) Slices (Kesitler)

Kesitler, herhangi bir arrayin veya vektörün belirli bölgesini ifade edebilecek bağlantılar yani `linkler` olmaktadır. 

Kesitler direkt olarak değişkenler üzerinde  tutulamaz veya fonksiyon girdisi olarak verilemez. Hadi ilk `slice` değerimizi oluşturarak başlayalım:

```Rust
let v: Vec<i32> = (0..5).collect(); // 0'dan 5'e kadar bir vektör
let sv: &[i32]= &v;
```

Başlangıç olarak dikkat etmemiz gereken şey yukarıda kullanılan `&` ifadesidir. Bu ifade referans anlamına gelmektedir. Kurallarımızda gördüğümüz üzere de kesitlerin direkt olarak değişkene tanımlanamamasından dolayı referanslar kullanılır. Bu şekilde veriyi direkt olarak değişkenin içerisinde tutmaktansa verinin hafızada tutulduğu yerin başını işaret etmekteyiz. 

Bu işaret etmeye `fat pointer` ismi verilmektedir. Bu işaret 2 adet veri içermektedir. İlk olarak hafızada referans edilen verinin nerede başladığına, ikinci olarak da bu verinin uzunluğunun ne olduğunu belirtmektedir.

Bu referans ile normalde yapabileceğimiz vektör ve array işlemlerinin çoğunu yapabiliriz ancak hafızadan yeniden yer talep etemeden bu işlemi yapmış oluruz. 

```Rust
let v: Vec<i32> = (0..5).collect(); // 0'dan 5'e kadar bir vektör
println!("{:?}",v); // [0,1,2,3,4]
let sv: &[i32]= &v; 
println!("{:?}",sv); // [0,1,2,3,4]
```

```Rust
let v: Vec<i32> = (0..5).collect(); // 0'dan 5'e kadar bir vektör
println!("{:?}",v); // [0,1,2,3,4]
let sv: &[i32]= &v[2..4]; 
println!("{:?}",sv); // [2,3]
```

> NOT: Referanslardan biraz daha bahsetmek gerekirse normal referans `ordinary referance` tekil bir değere işaret eden sahipsiz pointer olmaktayken kesitlerde kullanılan referanslar ise tekil bir değer yerine belirli bir aralığa işaret eden sahipsiz pointerlara denk gelmektedir.

## 10) Stringler
 
Stringler bitlerden oluşan vektör olarak depolanmalarından dolayı vektörlere büyük ölçüde benzemektedir. 

Bu iki kavramın arasındaki fark her zaman UTF-8 standartında değer olmalarıdır. String değerleri heap üzerinde tutulmaktadır. String tanımlamanın birçok yöntemi vardır. Bu yöntemlerden biri `String` paketini kullanmaktır.

```Rust
let isim = String::from("Aybars");
```
Sting değerleri üretmenin farklı bir yolu da `.to_string();` yapısını kullanmaktır. Bu işlemi altta ifade edildiği şekilde gerçekleştirebiliriz

```Rust
let soyisim = "Ayan".to_string();
```
Bu iki tanımla tanımlanan string değerler aynı olmaktadır. İkiside `heap` üzerinde tutulmaktadır.

Vektörlere benzer şekilde String'leri de manüpüle edebiliriz. Bu işlemi yaparken birçok yöntemimiz vardır ancak örnek olarak bir değeri farklı bir değerle değiştirmek istiyorsak alt kısımda ifade edildiği gibi bir işlem yapabiliriz.

```Rust
let yeni_isim = isim.replace("Aybars","Göktuğ")
```
## 11) &str Değerleri

String slice veya stir olarak da adlandırılan `&str` değerleri slice ve vector gibi bir değişkenden text değerini `barrow` yani ödünç alacaktır. Diğer slice değerleri gibi 2 adet bilgi içerecektir. 

String slice değerleri düzenlenemez. String slice değerlerini altta ifade edildiği şekilde tanımlayabiliriz:

```Rust
let str1 = "merhaba";
```

String slice değerlerinin nerelerde stringlerin yerlerine kullanacağımızı merak ediyor olabilirsiniz. Şimdilik string slice değerlerinin herhangi bir string değerini referans edeceğini bilmemiz yeterli olacaktır. Bu nedenle string veya string slice değerlerinin ikisinin de kullanılabileceği yerlerde `&str` tipinin heap üzerinde veri depolamaması nedeniyle tercih edilen olmaktadır. 

Ayrıca String Slice değerleri ve Stringler arasında değişiklik yapabiliriz. Örneğin bir string slice değerini string haline getirmek istiyorsak `to_string()` yapısını kullanmamız yeterli olmaktadır.

```Rust
let str1 = "hello";
let str2 = str1.to_string();
```

Aynı şekilde eğer bir string değerini string slice değerini çevirirken değerin başına `&` ifadesi koymamız yeterli olacaktır.

```Rust
let str3 = &str2;
```

Vektörlerde olduğu gibi stringlerde birçok method içermektedir. Bunlardan bağzılarına bakılacak olunursa:

- `==` İki string değeri birbirine eşitse true değeri döndürmektedir..
- `!=` İki string değeri birbirine eşit değilse true değeri döndürmektedir. 

## 12) String Literals 

Daha önce de bahsettiğimiz üzere stringler ve string slices değerleri her zaman UTF-8 standartlarına uyan halde olacaktır. Ancak bazen bu standarta kesin olarak uymak istememekteyiz. Bu durumlarda `string literal` değerlerini kullanabiliriz. Örneğin encoded bir mesaj yazmak istiyorsak bu yapıyı kullanabiliriz. 

```Rust
let rust = "\x52\x75\x73\x74";
println!("{}", rust); // Rust
```

Özet olarak UTF-8'e uygun metin yazmak istemediğimizde bu veri tipini kullanabiliriz.

----

## __Fonksiyonlar ve Control Akışı__

Rust içerisinde de diğer `Turing Complete` dillerde olduğu gibi fonksiyonlar ve döngüler yer almaktadır. Bu fonksiyon ve döngülerin Rust içerisinde diğer dillerden biraz farklı olarak yazımı söz konusudur.

## 1) Fonksiyonlar

Fonksiyonlar programlama dilleri için büyük önem arz etmektedir. Bu durum Rust için de farklı olmamaktadır. Hatta şu ana kadar fark etmiş olabileceğiniz gibi tüm kodlarımızı bir fonksiyon olan `main()` içerisinde yazdık. 

Bu `main()` fonksiyonu Rust dosyası çalıştırılır çalıştırılmaz harekete geçecek dosya olmaktadır. Programlar için giriş noktası olmaktadır.

Yine main fonksiyonundan da görüntüleyebileceğiniz üzere fonksiyonları tanımlarken Rust üzerinde `fn` anahtar kelimesini kullanmaktayız.

```Rust
fn fonksiyon_ismi(){
    // fonksiyon içeriği
}
```

Rust aynı zamanda fonksiyon isimlendirmeleri için `Snake Case` adı verilen bir isimlendirme yapısını kullanmaktadır. Bu kısımda yazılım dilleri arasında iki adet ana yapının kullanıldığını fark edebiliriz. Bunlar `Camel Case` ve `Snake Case` olmaktadır. Alt kısımda normal bir text'in iki türde de ifade edilidiği görüntülenebilir.

- Efendiler yarın Cumhuriyeti ilan edeceğiz. (Normal Yazım)
- efendilerYarınCumhuriyetiİlanEdeceğiz. (Camel Case)
- efendiler_yarın_cumhuriyeti_ilan_edeceğiz. (Snake Case)

Fonksiyon tanımlarken ilk olarak `fn` anahtar kelimesi ile başlanır. Bu anahtar kelime Rust'a bir fonksiyon tanımlayacağımızın sinyalini verir. Sonrasında `Snake Case` ile fonksiyonumuzun ismini tanımlarız. Bu isme tam birleşik olarka bir parantez yapısı `()` eklememiz gerekmektedir. Bu yapı içerisine fonksiyonda kullanacağımız ve fonksiyon çağırılırken girdi olarak girilecek parametrelerin fonksiyon içerisinde nasıl isimlendirileceğini belirtmektedir. Son olarak da bir süslü parantez açıp kapatarak fonksiyon içerisinde çalışmasını istediğimiz kodları bu parantez yapısı içerisine yerleştiririz. 

Hadi bir fonksiyon oluşturarak başlayalım. BU fonksiyonumuz her çağırdığımızda terminale bir mesaj göndersin.

```Rust
fn merhaba_de(){
    println!("Fonksiyonumuzun içerisinden herkese kucak dolusu selamlar");
}
```

Fonksiyonumuzu tanımladık ancak nasıl çağırmamız gerekiyor? Bu işlemi yaparken de çağırmak istediğimiz kısma fonksiyonumuzun ismini parantezler ile yazmamız yeterlidir. Biz bu kodu dosyamız çalışır çalışmaz yazdırmak istediğimiz için `main()` fonksiyonu içerisine yazdırabiliriz.

```Rust
fn main(){
    merhaba_de(); //Fonksiyonumuzun içerisinden herkese kucak dolusu selamlar
}
```

Daha önceden de bahsettiğimiz gibi fonksiyonlar içerisine istediğimiz verileri parametre veya diğer adı olan argüman olarak gönderebiliriz. Bu işlemi yapmak için ilk olarak fonksiyon tanımlanırken kullandığımız parantezler içerisine içerisine eklenecek verinin fonksiyon içerisindeki ismi ve tipi yer alacaktır.

```Rust
fn merhaba_de(senin_ismin: &str){
    println!("Merhaba {}; nassın, iyisin? Anangiller, babangiller, çoluğun çocuğungiller iyiler inşallah?", senin_ismin);
}
```

Fonksiyon parametresini yerleştirdiksen sonra fonskiyonumuzu çağırırken fonksiyon parantezinin içerisine girmek istediğimiz değeri yazarsak bu değer fonksiyonun içerisinde `senin_ismin` değişkenine kaydedilir ve fonksiyon içerisinde kullanılan bir hâl alır.

```Rust
merhaba_de("Aybars"); // Merhaba Aybars; nassın, iyisin? Anangiller, babangiller, çoluğun çocuğungiller iyiler inşallah?
```

Aynı zamanda fonksiyonların girdileri olabildiği gibi çıktıları da olabilmektedir. Bu çıktıları tanımlarken fonksiyonun kod bloğu kısmı olan süslü parantez yapısına girmeden önce `->` yapısını kullanarak fonksiyonumuzun çıktı vereceği değerin türünü yazmamız gerekmektedir.

Bu örneği ifade etmek için örnek bir fonksiyon tanımlayalım. Mesela bir fonksiyonumuz olsun ve bu fonksiyon içerisine girdiğimiz değeri 10 arttırıp bu toplamın 3 ile bölümünden kalanını bulsun ve bize geri döndürsün. 

```Rust
fn topla_kalan_bul(mut deger: u64) -> u64{
    deger = deger + 10;
    let bolum_kalan:u64 = deger % 3;
    bolum_kalan
}
```

Üsteki kodumuzu incelediğimizde ilk olarak bir `topla_kalan_bul` fonksiyonu tanımladığımızı görüntüleyebiliriz. Bu fonksiyona parametre olarak `deger` argümanının göndermekteyiz. Burada da fark edebileceğimiz üzere bu değer fonksiyon içerisinde değişime uğrayacağı için `mut` olarak tanımlanmıştır. Sonrasında sonuç olarak bir `u64` tipinde değer alacağımız için `-> u64` yapısı kod bloğundan hemen önce eklenmiştir. Kod içerisinde işlemler yapıldıktan sonra fonksiyonun çıktısının çıktı olacağını belirtmek için birçok yöntem olsa da şu anlık `bolum_kalan` ifadesinin `;` kullanılmadan yazılması tercih edilmiştir.

Bu fonksiyon artık çağırıldığında bir fonksiyonun içerisine girildiğinde veya bir değişkene eşitlendiğinde çıktı olarak verdiği değer eşit olacaktır.

```Rust
fn main() {
    let kalan:u64 = topla_kalan_bul(6);
    println!("{}",kalan); /1
}
```

Aynı zamanda bir fonksiyon duruma göre birden fazla değişkeni de çıktı edebilmektedir. Ancak bu koşullu şartları incelediğimizde sonradan bahsedilecektir.

## 2) Kontrol Akışı

Kontrol akışı bir kod parçasını belirli şartın sağlanıp sağlanmaması durumunda nerede ne şekilde çalışacağını veya bir kodun tekrarlı olarak çalışıp çalışmayacağını ifade ederken kullanmamızdır. 

Kontrol akışı ifadeleri kodlama dilleri için hayati önem sağlamaktadır. Rust içerisinde yaygın kullanılan bir adet koşullu durumun ifadesi olan `if` ve 3 adet döngü bulunmaktadır. 

### 2.1) If İfadesi

İlk olarak `if` ifadesi ile başlayabiliriz. If türkçeğe `eğer` olarak çevirilmektedir ve bir şartın gerçekleşmesi koşuluyla içerisindeki kodları çalıştırmaktadır. Bir örnek ile bu yapıya bakıcak olursak:

```Rust
let sayi = 1;
if sayi > 0 {
    println!("Evet doğru, tebrikler artık birinci sınıf talebesini RUST kullanarak ezebilirsin");
} else {
    println!("Olsun be devamsızlıktan kalma, seneye öğrenirsin");
}
```

Kodumuzu incelediğimizde ilk olarak bir değişken tanımladığımızı görebiliriz. Bu değişken `1` değerine sahip `unsigned integer` bir değer olmaktadır. Sonrasında altında asıl önemli olan kısım `if` koşulu bizleri karşılamaktadır. Bu koşul `if` kelimesinden süslü parantezin başına kadar olan koşulun `true` veya `false` olmasını kontrol etmektedir. Eğer bu durum `false` ise içinde olan kod çalışmaz ancak `true` ise içinde olan kod çalışır. Bizim bu kod örneğimizde de `sayı` olarak isimlendirdiğimiz değişken `0` dan büyük olduğu için koşulumuz `true` döndürür ve iç kısmında bulunan `println!()` makrosu çalışır. Peki şart sağlanmasaydı yani `false` dönseydi ne olacaktı? O durumda da hemen altındaki `else` yapısı devreye girecek ve `else` kod blokları içerisindeki `println!()` makrosu çalışacaktı.

Daha önceden bir fonksiyonun birden fazla çıktısının olabileceğini söylemiştik hadi `if` ile birleştirerek bu olayı örneklendirelim. Fonksiyonumuz içerisine girilen sayı 10'dan büyükse `Büyüktür` 10'dan küçükse `Küçüktür` çıktısı veren bir fonksiyon yazabiliriz.

```Rust
fn buyuk_kucuk(deger:u64)-> String{
    if deger > 10{
        "büyüktür".to_string() // ";" yok
    } else {
        "küçüktür".to_string() // ";" yok
    }
}
fn main() {
    let durum:String = buyuk_kucuk(9);
    println!("{}",durum); // küçüktür
    let durum:String = buyuk_kucuk(11);
    println!("{}",durum); // büyüktür    
}
```

Yukarıda ifade edilen kodu inceleyerek nasıl bir şekilde fonksiyonlar ile akış elemanlarını kontrol ettiğimizi görüntüleyebilirsiniz. 

Aynı zamanda Rust içerisinde de farklı şartları birbirine bağlayabiliriz. Bu işlemi `else if` yapısı ile kontrol etmekteyiz. aynı if gibi bu yapıda da iç parantezine koşul almaktadır. Bu yapı `else` yapısının kapsamadığı şartlardan birini oluşturmaktadır. Bu durumu göstermek için üst kısımda yazdığımız kodu biraz daha özelleştirebiliriz.

```Rust
fn buyuk_kucuk_esit(deger:u64)-> String{
    if deger > 10{
        "büyüktür".to_string() // ";" yok
    } else if deger < 10{
        "küçüktür".to_string() // ";" yok
    } else{
        "eşittir".to_string() // ";" yok
    }
}
fn main() {
    let durum:String = buyuk_kucuk(9);
    println!("{}",durum); // küçüktür
    let durum:String = buyuk_kucuk(11);
    println!("{}",durum); // büyüktür  
    let durum:String = buyuk_kucuk(10);
    println!("{}",durum); // eşittir    
}
```

### 2.2) Loop İfadesi

Looplar türkçeye döngüler olarak çevrilmektedir ve bir işlemi sonsuza kadar yapmaya yaramaktadır. İçlerine yazılan kod bloklarını sırasıyla biri durduruna kadar sonsuza kadar yazarlar. 

```Rust
loop {
    println!("Başım dönüyoooo") // Sonsuza kadar bu mesajı yazdıracak
}
```

Bu döngü sonsuza kadar devam edecektir. Ancak istersek biz terminalde `ctrl + c` ifadesini kullanarak durdurabiliriz. 

Bu `loop` değerlerini isimsiz de oluşturabileceğimiz gibi isimler ile de tanımlayabiliriz. Ancak bu isimleri verirken dikkat etmemiz gereken şey döngü isimlerinin başlarına kesme işareti `'` koymaktır. Peki neden döngülere isim vermemiz bizim için önemli? Sizlere döngülerin sonsuz olduğunu ve birisi durdurmadıkça sonsuza kadar devam edeceğini söylemiştik, peki nasıl bu döngüyü durduracağız? İşte bu döngüleri durdurmak için `break` methodumuz bulunmakta. Bu methodu döngünün içerisinde yalın olarak kullandığımızda en iç kısımında bulunan döngüyü kırmaktayız. Bu da bizi `nested loop` yani `iç içe döngü` kavramına getirmekte. Rust içerisinde istediğiniz kadar iç içe döngü oluşturmanız mümkün. Peki bir döngü zaten sonsuzken nasıl bu döngünün dışındaki döngü gerçekleşebilir? Hadi bir örnek ile bakalım.

```Rust
loop {
    println!("Merhaba");
    loop {
        println!("Nasılsın?");
    }
}
```

Yukarda ifade edilen kodumuz bir iç içe döngü yani `nested loop` olmaktadır. Bu kodu çalıştırdığımızda şu şekilde bir çıktı ile karşılaşacağız.

```
Merhaba
Nasılsın?
Nasılsın?
Nasılsın?
Nasılsın?
Nasılsın?
Nasılsın?
Nasılsın?
...
```

Gördüğünüz gibi kodumuz çalışmaya üstten aşşağı başladığı için `Merhaba` ifadesini bir kez yazdırmış olsada altında bulunan döngüye girerek sonsuza kadar çıkamadı. Bu nedenle bir daha `Merhaba` yazdırmadı. Ancak bu durumdan kurtulmak için bir yöntemimiz olmakta. İşte bu yöntem `break` ! Break yapısı kullanarak döngüden direkt olarak çıkış yapabiliriz. Şimdi kodumuza break ilave ederek bu işlemi yeniden deneyelim.

```Rust
loop {
    println!("Merhaba");
    loop {
        println!("Nasılsın?");
        break
    }
}
```

İç `loop` içerisine break ibaresi ekledik. Şimdi çalıştırarak çıktıya bakalım:

```
Merhaba
Nasılsın?
Merhaba
Nasılsın?
Merhaba
Nasılsın?
...
```

Gördüğümüz gibi tamamen farklı bir sonuç oluştu. Kodumuzu şimdi satır satır inceleyelim. İlk olarak `Merhaba` yazdırarak başladık. Sonrasında iç `loop`'a girdik. `Nasılsın?` yazdırdık. Bir satır altına indiğimizde `break` anahtar kelimesine gelmekteyiz. Bu komut içinde bulunduğu `loop`'u kırarak dışarı çıkar. Sonrasında dış loop yeniden işleme girerek bizlere `Merhaba` yazdırır ve bu işlem sonsuza kadar devam eder. 

Fark ettiğiniz gibi `break` yapısı sadece içerisinde bulunduğu en son döngüyü kırmaktadır. Peki biz iç kısımdan daha dış bir döngüyü kırmak istesek bu işlemi nasıl yapabiliriz? İşte burada da karşımıza loop'ları isimlendirmenin nasıl işimize yarayacağı çıkmaktadır. Eğer bir loop'a isim verirsek ve sonrasında break anahtar kelimesinin yanına bu ismi koyarsak ne kadar içerde olursak olalım herhangi bir loop'u kırabiliriz. Bu durumu kod içerisinde inceleyecek olursak:

```Rust
'en_buyuk: loop{
    println!("Merhaba");
    'orta_derece: loop{
        println!("Nasılsın");
        'en_kucuk: loop{
            break 'en_buyuk;
        }
    }
}
```

Yukarıda kullandığımız kod yapısını `break`'i yanlız bırakarak yazsaydık `Merhaba` ve `Nasılsın` ifadelerinin sonsuza kadar devam ettiğini görüntüleyecektik. Ancak bu şekilde yapmadığımız ve `break 'en_buyuk` yapısı ile hangi döngüyü kıracağımızı belirttiğimiz için terminale sadece alt kısımda ifade edilen çıktı oluştu ve sonsuza kadar gitmeden direkt olarak durdu.

```
Merhaba
Nasılsın
```

> Ayrıca `break` yapısını `if` ile birleştirerek nasıl döngülerden belirli şartlar oluşunca çıkabileceğinizi görüntüleyebilirsiniz.

### 2.2) While Döngüsü

While döngüsü türkçeye bir ek olan `ederken` diye çevrilebilir. Yani bir cümle içerisinde kullanacak olursak "Eve gelince bana yardım`ederek` masayı kurabilirsin" ifadesi örnek verebiliriz. Her nasıl bu cümlede `yardım etme` işi `eve gelince` şartı ile birleştiyse `while` döngüleri de belirli bir şartın `true` olduğu her an çalışmakta olup `false` olduğu anda çalışmayı bırakıyordur. 

Bu duruma örnek vermek için 10'a kadar sayan bir program yazabiliriz.

```Rust
let mut sayi = 0;
while sayi < 10{
    println!("Sayımız: {}", sayi);
    sayi = sayi + 1;
}
```
Bu şekilde yazdığımız kodu çalıştırınca `while` döngüsünün yanında bulunan şart `false` olana kadar çalıştığını görebiliriz.

```Rust
Sayımız: 0
Sayımız: 1
Sayımız: 2
Sayımız: 3
Sayımız: 4
Sayımız: 5
Sayımız: 6
Sayımız: 7
Sayımız: 8
Sayımız: 9
```

Yukarıdaki kod çıktısında da görüntüleyebileceğimiz üzere `sayi` değerimiz sürekli artarken 10'a eşit olduğu anda şart sağlanmamış ve döngü kırılarak bitmiştir. 

> Eğer kodumuzun 10'u da içermesini istiyorsak `<` yerine `<=` ibaresini de kullanabiliriz. 

Aynı zamanda `while` loop'una özel olmadan her loop'da çalışan farklı bir değeri de yazdığımız kodda örnekleyebiliriz. Bu değer ise `continue` olmakta. Diyelim ki 10'a kadar sayılarımızı yazdırdık ancak 7 değeri bize uğursuz geldi canımız istemedi ve döngüden çıkarmak istedik. İşte tam bu anda `continue` yapısını kullanabiliriz. `continue` yapısı çalıştığı anda kodumuz sonrasında bulunan işlemleri yapmadan yeniden çalışır ki bu bizim durumumuzda bir sayı atlaması anlamına gelmekedir. Hadi kodumuzu `if` ve `continue` ifadeleri ile 7'yi atlayan bir hale getirelim.

```Rust
let mut sayi = 0;
while sayi < 10{
    if sayi == 7{
        sayi += 1;
        continue;
    }else{
        println!("Sayı: {}", sayi);
        sayi = sayi + 1
    }
}
```

Kodumuzu çalıştırdığımızda görüntüleyebileceğiniz üzere 7 sayısı olmadan tüm çıktılar sorunsuz bir şekilde ekrana gelmektedir.

```Rust
Sayı: 0
Sayı: 1
Sayı: 2
Sayı: 3
Sayı: 4
Sayı: 5
Sayı: 6
Sayı: 8
Sayı: 9
```

### 2.2) For Döngüsü

For döngüleri belirli bir koleksiyondaki elementler için bir işlemi tekarlamanın mükemmel bir yoludur. Örneğin bir vektör! 

Hadi bir örnek yapalım. Diyelim ki bir vektörümüz var ve bu vekörün içerinde 1'den 10'a kadar sayılar var. Bizde bu sayıları yazdırmak istiyoruz. Bu işlem için alt kısımda bulunduğu gibi bir döngü yazmamız gerekmektedir.

```Rust
let sayilar: Vec<i8> = (0..10).collect();
for sayi in sayilar{
    println!("Sayı: {}", sayi);
}
```

Sırasıyla kodumuza bakıcak olursak ilk olarak [0,10) aralığında bir vektör oluşurup bu vektöre `sayilar` adını vermişiz. Sonrasında for döngüsü karşımıza çıkmış. Bu döngüyü "`sayilar` vektörünün `içerisindeki`(in) her bir değeri sırasıyla `sayi` değişkenine eşitle ve sonrasında ekrana yazdır" şeklinde türkçeleştirebiliriz. Kodumuzu çalıştırırsak alt kısımdaki gibi bir sonuç çıkacağını görüntüleyebiliriz.

```Rust
Sayılar: 0
Sayılar: 1
Sayılar: 2
Sayılar: 3
Sayılar: 4
Sayılar: 5
Sayılar: 6
Sayılar: 7
Sayılar: 8
Sayılar: 9
```
> Aynı zamanda vektörü for döngüsünde `in` ifadesinden sonra `for sayi in (1..10){...}` şeklinde tanımlayabiliriz.