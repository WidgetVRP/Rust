# Rust Prensipleri

Bu kısımda Rust içerisinde bulunan şu kavramları inceleyeceğiz:

- Ownership (Sahiplik)
- Referances (Referans)
- Barrowing (Ödünç Alma)

Ayrıca klonlama ve miras alma gibi sistemlerin nasıl çalıştığına bakarken `ownership`'in fonksiyonlarda nasıl çalıştığını inceleyeceğiz.

BU kısımda öğrendiklerimiz Rust üzerinde yazacağımız tüm projelerde karşımıza çıkacak ve nasıl kod yazacağımızı belirleyecektir. 

## Ownership (Sahiplik)

Hafıza ile ilgili konuştuğumuzda yazılımcılar olarak iki konuya dikkat etmemiz gerekmektedir. Bunlardan ilki hafızadan istediğimiz elemana istediğimiz zaman erişebilmek olmaktayken diğeri de sistemimize aldığımız değeri bir daha referans etmek istemememizdir. Çünkü bunları yapmazsak tanımlanmayan işlev `undifined behavior` devreye girer ve bu da çökmelere ve güvenlik açıklarına sebep olur. 

2017'de Microsoft'un açıklamasına göre programlarda yer alan açıkların %70'inden fazlası hafıza güvenliği sıkıntılarından meydana gelmekte.

Bu gibi sorunlara çözüm olmak için genelde iki adet çözüm ortaya atılmıştır. Bunlardan ilki `Garbage Collector` sistemleridir ve Java, C#, Python ve JavaScript gibi diller tarafından kullanılmaktadır. Bu sistem hafızadan istediğimiz veriyi istediğimiz an çekmemizde zorluk yaratmaktadır. Diğeri ise C ve C++ gibi diller tarafından kullanılan `Manuel freeing` sistemi olmaktadır. Bir hata yapmadıkça bu sistem çok iyi çalışssa da Yazılımcılar hata yapmaya meyilli olmaktadırlar. 

Peki Rust bu ikisinden hangisini kullanıyor? Teknik olarak hiçbirini. Rust `ownershipi` adını verdiği ve diğer kodlama dillerden kendini ayıran bir sistem kullanmakta. Rust Compiler kodumuzu boş pointer gibi birçok hafıza hatalarına karşı test ederek korumaktadır. 

Bu kavram neredeyse her yazılımcıya yeni olduğu için anlaşılması uzun sürebilecek bir konudur. Bu nedenle ne kadar fazla kullanırsak bu kavram ile o kadar fazla kendimizi rahat hissederiz.

Hadi hızlıca hafızanın yani `memory`'nin bir parçası olan `heap` ve `stack` yapılarından bahsedelim. Stack `First in - First out (FİFO)` adı verilen son giren elemanın ilk erişildiği sistemi kullanmaktadır. Bunu bir çamaşır sepetine benzetebiliriz. Stack içerisinde tutulan tüm değerler belirli bir uzunluğa sahip olmalıdır. Büyüklüğü değişebilecek her değer `heap` içerisinde tutulmaktadır. Heap'ların içi daha dağınıktır ve içerilerinden bir veri istediğimizde bir alan isteriz ve bu da bizlere verinin adresinin yerini ifada eden bir `pointer` olarak geri döner. 

`Stack` içerisine bir veri eklemek ve içerisinde veri aramak `heap`'e göre daha hızlı olmaktadır. Bir fonksiyona veri gönderdiğimizde yada fonksiyon içerisine local bir değişken oluşturduğumuzda bu değerler hep stack içerisine gitmektedir. Bu fonksiyon bitince ise tüm fonksiyon içindeki değişkenler ve girdiler `stack` den silinmektedir. 

Peki bu kavramları biraz daha iyi anladıysak `ownership` in kurallarına biraz daha bakabiliriz. Peki nedir bu kurallar? İlk kural olarak Rust içerisindeki her bir değerin `owner` yani `sahip` olarak tanımlayacağımız değişkenleri vardır. Her değerin tek `owner`'ı vardır ve bu `owner` scope'dan düşer düşmez değeri de yok olmaktadır. Bu duruma `free` bırakmak da denilmektedir.

Hadi bir örnek yaparak başlayalım. Main fonksiyonunun içerisinde 1 değerine sahip bir değişken oluşturalım.

```Rust
fn main(){
    let degisken = 1;
}
```

Bu durumda `degisken` değeri main içerisinde olduğu sürece hep `valid` yani işlevsel olacaktır. Ve aynı zamanda `1` değerinin `u32` veya `i32` gibi sabit büyüklükte bir değer olduğunu bildiğimiz için `stack` içerisinde yer aldığını söyleyebiliriz. Ancak bu fonksiyondan çıkar çıkmaz `degisken` düşürülür ve hafızadan `1` değeri silinir. Aynı işlemi boyutu değişken olduğu için `heap` da depolanan `String`'ler için de yapabiliriz. Kod içerisinde bu String değişkenleri `mutable` yapılarak boyutları değiştirebilir ancak içinde bulunduğu fonksiyondan çıktığı anda yok edilerek boş bulunan yer işletim sistemine teslim edilir.

```Rust
fn main(){
    let degisken = 1;
    let mut stringim = "merhaba".to_string();
    s.push_str("world");
}

// Fonksiyondan çıktığım için "degisken" ve "stringim" düşürüldü.
```

Bir sonraki kısmımızda bu kavramların oluşmasını sağlayan `ownership` metotlarına bakacağız. 

### 1) Move

`Move` türkçeye taşımak anlamında çevrilebilir ve Rust içerisidne de tam olarak bu işlevi yapmaktadır. 

`Move` işlemi yaptığımızda bir değerin sahibini bir değişkenden farklı bir değişkene aktarmış oluruz. Birçok tip bu işlemi gerçekleştirmektedir.

Hadi diğer kodlama dillerinde yapabileceğimiz gibi direkt olarak deneyelim ve neler olduğuna bakalım.

```Rust
let x = vec!["aybars".to_string()];
let y = x;
println!("{:?}", x);
```

Kodumuzu çalıştırdığımızda şu şekilde bir hata ile karşılaşacağız.

```
error[E0382]: borrow of moved value: `x`
```

Gördüğünüz gibi artık sahibini değiştirdiğimiz ve `y` yaptığımız değeri yazdırmaya çalışırsak kodumuz bu değerin taşındığını ifade edecektir ve hata verecektir. Çünkü artık `x` bir değere sahip değildir. Ancak eğer `y` değerini yazdırmak istersek bu durumu sıkıntısız yapacağını görüntüleyebiliriz.

```Rust
let x = vec!["aybars".to_string()];
let y = x;
println!("{:?}", y); // ["aybars"]
```

Peki bu değeri taşıdık ancak taşımadan kopyalamak isteseydik neler yapacaktık? Bir sonraki başlıklarda da bunu inceleyeceğiz.

### 2) Clone

Eğer bir değeri `ownership`'ini almadan elde etmek istiyorsak `clone` yapısını kullanabiliriz. Bu yapıda verinin derin bir kopyasını oluşturur. 

Bu komut referans ettiğimiz değişkenin değerini alacak ve yeni bir değişkene kopyalayacaktır. Bu işlemi yapmak biraz pahalı olmaktadır. Bı işlemi şöyle görüntüleyebiliriz:

```Rust
let x = vec!["aybars".to_string()];
let y = x.clone();
println!("x: {:?}", x);
println!("y: {:?}", y);
```

Kodumuzu bu şekilde çalıştırdığımızda taşımaktan kaynaklanan hatanın yer almadığını iki değerin de alt kısımda ifade edildiği şekilde yazdırıldığını görüntüleyebiliriz.

```Rust
x: ["aybars"]
y: ["aybars"]
```

Bu işlem son derecede pahalı olmaktadır ve fazlaca yer kaplamaktadır. Ancak bu durumu da derin bir kopya oluşturmadan çözebiliriz. Bu işlem için bir sonraki başlığımız olan `copy` işlemini gerçekleştiririz.

### 3) Copy

Şimdi bir koda bakalım birlikte. 

```Rust
let x = 1;
let y = x;

println!("x= {}, y = {}", x, y);
```

Daha önce öğrendiklerimize biayen bu kodun çalışmayacağını düşünürüz. Çünkü x değeri taşındı, yani öyle değil mi? Aslında hayır. Bir daha o başlığı incelerseniz birçok tipin `move` işlemini gerçekleştirdiğini görebiliriz. Ancak hepsi değil. Bazıları da `copy` olarak adlandırılan kopya işlemini kullanmaktadır. 

`Copy` işlemleri hali hazırda zaten stack içerisinde kayıtlı olan değerler için uygulanmaktadır. Bunlara integerlar, floatlar, booleanlar ve tuplelar örnek verilebilir. 

Bu nedenle `heap` üzerindeki değişkenler `move` olurken `stack` üzerindeki değişkenler `copy` olmaktadır.

### 4) Move Kavramının Derinlikleri

Bu başlıkta `move` kavramının nasıl çalıştığına biraz daha ayrıntılı bir şekilde bakacağız. 

İlk olarka bir sting değeri oluşturalım.

```Rust
fn main(){
    let string = String::from("elim sende");
}
```

Bu değişkeni oluşturduktan sonra sahipliği almak için bir fonksiyon oluşturabiliriz.

```Rust
fn takes_ownership(s: String){
    let string1 = s;
    println!("{}", string1);
}
```

Fonksiyonumuz içerisine girilen değeri alarak kendi bir değişkene eşitlemektedir. Şimdi `Main()` içerisinde bu fonksiyonu çağırabiliriz.

```Rust
fn main(){
    let string = String::from("elim sende");
    takes_ownership(string);
}
```

Kodumuzu çalıştırdığımızda sıkıntısız bir şekilde fonksiyonumuzun çalıştığını görüntüleyebiliriz. 

```
elim sende
```

Peki yeniden string değerini `main`'de fonksiyondan sonra yazdırabilir miyiz? 

```Rust
fn main(){
    let string = String::from("elim sende");
    takes_ownership(string);
    println!("{}", string); // error
}
```
Cevap hayır olmakta. Rust yapısı nedeniyle bir değişkeni parametre olarak fonksiyona versek bile bunu bir `ownership` değişikliği olarak görüntüleyecek ve yeniden kullanmamıza izin vermeyecektir.

Peki her bir parametre girdiğimizde o değerin kaybolmamasını istiyorsak ne yapabiliriz? Cevam `copy` kullanmakta saklı. 

Bu copy işlemini `stack` üzerinde tutulan verilerde direkt olarak tip değiştirerek yapabiliriz.

```Rust
fn main(){
    let deger: u64= 1;
    copy_value(deger);
    println!("{}", deger); // 1
}

fn copy_value(degerim: u64){
    let deger = degerim;
    println!("{}", deger); // 1
}
```

Yada `heap` üzerinde tutulan değerler için `.clone()` yapısını da kullanabiliriz.

```Rust
fn main(){
    let string = String::from("elim sende");
    copy_value(string.clone());
    println!("{}", string); // elim sende

}

fn copy_value(s: String){
    let string1 = s;
    println!("{}", string1); // elim sende
}
```

Ayrıca sahipliği alabildiğimiz gibi sahipliği verede biliriz. Bu işlemi alt kısımda ifade edilen fonksiyondaki gibi yapmamız mümkün olmaktadır. 

```Rust
fn main(){
    let string1:String = give_ownership();
    println!("{}", string1);
}

fn give_ownership()-> String{
    "elim sende".to_string()
}
```

Üstte yazdığımız kod ile de fonksiyon içerisinde oluşan ve normal şartlarda yok olacak değeri `return` ederek `string1` değerine eşitledik. 

Aynı işlemleri `if` döngüsünde de yapabiliriz. Eğer bir değeri `if` döngüsü içesinde değiştiysek şart sağlansın veya hiçbir zaman sağlanmasın fark etmeden sahipliği değişmiş olarak görecek ve bir önceki sahibi silecektir.

```Rust
fn main(){
    let string1:String = "test".to_string();
    if true{
        "öylesine kod aslında birşey yok görme burayı kış kış".to_string();
    } else{
        let string2 = string1;
    }
    println!("{}", string1) // Error
}
```

Yukarıda verilen kodda her zaman `if` ifadesinin çalışacağını hiçbir zaman `else` ifadesine gelmeyeceğini görüntüleyebiliriz. Yine de `else` yapısı içerisindeki `move` devreye girecekmiş gibi düşünülerek kod çalışmayacaktır. 

Son olarak `loop`'lara yani döngülere bakalım. Bir loop sonsuza veya belirli yere kadar gidecek bir işlem olduğu için `ownership` değişimi sıkıntı yaratacaktır. Bu nedenle kodumuz hata verecektir.

```Rust
let mut str1: String = "Aybars".to_string()
let mut str2: String;

loop{
    str2 = str1; // Error
}
```

## Referances &  Barrowing

Bu kısımda referanslara ve ödünç alma sistemlerine bakacağız. 

Referanslar bir değere `ownership`'ini almadan referans yapamızı sağlar. Yani değeri `barrow` (ödünç almış) yapmış oluruz. 

İki adet referansımız bulunmaktadır. Bunlar `shared` yani paylaşılmış ve `mutable` yani değiştirilebilir referenslar olmaktadır. 

`Shared referance` ifadeleri değeri okumamıza izin vermek ile beraber değiştirmemize izin vermemektedir. Bir değer karşı istediğimiz kadar istediğimiz zaman `shared referance` oluşturabiliriz. 

Diğer bir taraftan `mutable referance` değerleri hem değeri okumaya hem de değiştirmeyi sağlamaktadır. Ancak `shared referance` aksine o veri için aktif olan başka bir `mutable referance` değerine sahip olamazsınız. 

Hadi bu kavramlara bakalım:

```Rust
fn main(){
    let s = String::from("Merhaba");
    change_string(&s)
}
fn change_string(bir_string: &String){
    bir_string.push_str(" Dünya")
}
```

Bu şekilde yaptığımızda hata olarak Rust bize `s` değerini `mutable` yapmamızı söylemektedir. 

```Rust
fn main(){
    let mut s = String::from("Merhaba");
    change_string(&mut s);
    println!("{}",s);
}
fn change_string(bir_string: &mut String){
    bir_string.push_str(" Dünya")
}
```

Kodumuzu şimdi çalıştırdığımızda `mutable referance` yapısını kullandığımızı ve sorunsuz kodumuzun çalıştığını görüntüleyebiliriz.

```
Merhaba Dünya
```
---
Bu konseptler Rust dilini Rust dili yapmaktadır ve büyük önem arz etmektedir.