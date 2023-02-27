# Enums and Patern Maching

Bu başlığımızda enumları ve paternleri göreceğiz. Enumlar varyasyonları ile veri tipleri tanımlamamızı sağlarken parternler belirli kodları belirli şartlarda çalıştırmamıza izin verecektir. 

--- 

## Enums

Enum yapıları nedir diye baktığımızda bizleri çevirisi olan `sınıflandırmak` karşılamaktadır. Yapı olarak `bool` değerlerine benzemektedirler. Belirli bir durumun sadece belirli sayıda durumunun olduğunu ifade etmektedirler. Mesela diyelim ki bir oyun yazıyoruz ve oyuncunun oyun içerisinde hangi durumda olduğunu kod içerisinde belirtmek istiyoruz. Bu durumda enum ile oyuncunun durumunu `OYUNDA` , ,`OYUNU DURDURDU` ve `OYUNU KAYBETTİ` durumlarından biri ile kaydedebiliriz. Bu şekilde sonrasında oluşturduğumuz `enum` değerini çağırarak `eğer oyun durumu OYUNDA ise şu işlemi yap` ifadesini kullanabiliriz. 

Enumları nasıl tanımlayacağımıza baktığımızda ilk olarak `enum` anahtar kelimesini kullanmamız gerektiği karşımıza çıkmaktadır. Bu yapıyı fonksiyonların dışına tanımlayıp içerisine başlıkları girebiliriz.

```Rust
enum EvcilHayvan {kopek, kedi, balık}
```

Sonrasında oluşturduğumuz bu yapıdan herhangi bir değere erişmek istediğimizde şu yapıyı kullanabilriz:

```Rust
fn main(){
    let dog = EvcilHayvan::kopek;
}
```

Aynı şekilde enum yapılarına `method` eklemek `struct` yapılarında olduğu gibi mümkündür. 

```Rust
impl EvcilHayvan{
    fn ben_neyim(self) -> &'static str {
        match self {
            EvcilHayvan::kopek -> "Haw Haw",
            EvcilHayvan::kedi -> "Miyaw Miyaw",
            EvcilHayvan::balık -> "Gulu Gulu",
        }
    }
}
```

Üst kısımda bulunan kod karışık gelebilir bu nedenle sıra sıra inceleyelim. İlk olarak `impl` yapısı ile EvcilHayvan enumuna method ekleyeceğimizi ifade ederiz. Sonrasında altına indiğimizde fn yapısı ile methodumuzun ismini tanımlarız. Burada dikkat etmemiz gereken birkaç şey bulunmaktadır. İlk olarak `struct` yapılarına benzemeyen şekilde `self` yapısı referans değil kendisi olmaktadır. Sonrasında ise `static lifetime` ifadesi vardır. Bu durumun nedeni bir string slice döndürecek ifademizin bu işlemi yapabilmesini sağlamaktır. Sonrasında ise bir match yapısı ile her bir olasılık için Bir &str yapısı döndürülmektedir ki bunlara daha sonraki derslerimizde daha ayrıntılı olarak bakacağız.

Sonrasında aynı bir `struct` yapısı gibi istediğimiz yerde bu methodu çağırabilir ve istediğimiz işlemi yapıp yapmadığını kontrol edebiliriz.

```Rust
fn main(){
    let dog = EvcilHayvan::kopek;
    println!("{}", dog.ben_neyim) // Haw Haw
}
```

Aynı zamanda enumları struct yapıları içerisinde de kontrol edebiliriz. Hadi bunu nasıl yapacağımıza bakalım:

```Rust
enum IpAddrKind{
    V4,
    V6
}

struct IpAddr {
    kind: IpAddrKind,
    address: String
}
```

Hadi yukarıda yazdığımız kodu biraz daha inceleyelim. İlk olarak bir enum tanımlamışız ve içerisine `V4` ve `V6` değerlerini girmişiz. Daha önceden de bahsettiğimiz üzere enum yapıları belirli olasılıkları işartlendirmeye yarıyordu. Sonrasında bir `struct` yapısı oluşturup `kind` olarak oluşturduğumuz bu enum değerini girmişiz. Yani yaptığımız şey V4 ve V6 değerlerinden başka birşey olamayacak IP türümüzü String gibi büyük bir kümeden almak yerine oluşturduğumuz enum değerlerinden birini alarak güvenliği sağlamaktır. Sonrasında bu struct yapısında bir değişken oluşturabiliriz.

```Rust
fn main(){
    let dog = EvcilHayvan::kopek;
    println!("{}", dog.ben_neyim) // Haw Haw

    let ev = IpAddr {
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1")
    };
    let loopack = IpAddr{
        kind: IpAddrKind::V6,
        address: String::from("::1")
    }
}
```
