---
title: "Java'da Kalıtım 9 - Overriding(Ezici) Metotlar"
comments: false
excerpt: "Bu bölümde overriding ve overloading metotların tanımlarını yapacak, ağırlıklı olarak overriding metotlar üzerine duracağız. Bunun yanı sıra metot imzası ve metot deklarasyonu arasındaki farklardan da bahsedeceğiz."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-50.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Makenzie Cooper](https://unsplash.com/photos/gl9kCw7y-pY) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - java inheritance
  - overriding metot
  - geçersiz kılma
  - ezici metot
  - polimorfizm
  - Robert C. Martin
  - Super Keyword in Java
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

İngilizce **overriding** kelimesinin türkçe karşılığı "geçersiz kılma" veyahut "ezme" anlamında düşünülebilir. Ders süresince bu ifadeleri kullanmaya gayret edeceğim.

Kalıtım hakkında ilk konuşmaya başladığımızda üç hedefimiz vardı. Overriding(geçersiz kılma) konusu gerçekten bu üç hedefe de yardım etmektedir. Dilerseniz bu hedefleri tekrar hatırlatalım:

1. Bütün ortak davranışları bir sınıfta tutmak,
2. Farklı davranışa sahip olanları ise farklı sınıflara ayırmak
3. Tüm bu nesneleri tek bir veri yapısında tutmak.

Örneğin bir yöntemi geçersiz kılmazsak 1.maddeyi uygularız. Üst sınıfımızda bir yöntem varsa, ortak olan bu kodu doğrudan kullanırız. Ancak, alt sınıflarımızın farklı davranmasını istiyorsak, tek yapmamız gereken ortak olan bir yöntemi geçersiz kılmaktır. Sonuç olarak metotları geçersiz kıldığımızda bir nevi farklı davranışlara sahip olurlar. Bu sayede de 2.maddemizi uygulamış oluruz. Sonuncu madde olan ve bir sonraki derste göreceğimiz polimorfizm konusu ile de tüm nesnelerimizin tek bir veri yapısında olmasına ve yine de uygun yöntemi çağırabilmemize izin verdiğini göreceğiz. **Yani polimorfizm uygulasak da yine de geçersiz kılınan objenin metoduna ulaşıldığını göreceksiniz.** Söylenenlerin bir kısmının havada kaldığının farkındayım.

## Overriding(Ezici) Metotlar

Bu arada, programlama öğrenen bir çok kişinin aşırı yükleme metotları(overloading) ile geçersiz kılma metotlarını(overriding) karıştırması gerçekten yaygındır, bu yüzden aşırı yükleme ve geçersiz kılma konularına hızlı bir şekilde değinmek istiyorum.

* **Overloading metotlar:** Eğer bir sınıf aynı yöntem adına, farklı parametrelerle sahip ise overloading metotlara sahip demektir. Bu, aynı sınıf içindeki farklı bir yöntem imzasıdır. Bir sonraki ders bununla ilgili konuşacağız.

* **Overriding metotlar:** Burada ise aynı yöntem imzasına sahip bir alt sınıfınız vardır, yani aynı metot ismi ve üst sınıfla aynı parametreler anlamına gelir. Fakat metot gövdesi değiştirilir. Özetle bir metot geçersiz(override) kılındığında, aslında şunu yapmış oluyorsunuz. Bir sınıfın miras aldığı sınıfta bulunan bir metodun yaptığı işlem(yani metot gövdesi), artık bu sınıfta(yani miras alan sınıfta) farklı bir şeklinde yorumlanacaktır.

### Metot İmzası ve Metot deklarasyonu Arasındaki Farklar
Burada aklınıza şu soru gelebilir. Metot imzası nedir? Aslında ben bu soruyu, metot imzası ve metot deklerasyonu arasında ne gibi farklar vardır? şeklinde biraz genişletmek istiyorum. [Java main metodu](/java/Java-main-method/) başlıklı yazımı okuduğunuzda, metot deklarasyonu hakkında fikir sahibi olabilirsiniz.

#### Metot deklarasyonu:
Aşağıda tipik bir yöntem deklarasyon örneğidir:


```java
public int resultNumbers(int a, int b){
  // method body
  int result = a+b;
  return result;
}
```

Metot deklarasyonunun sahip olduğu minimum öğeler; metodun dönüş tipi, ismi, bir çift parantez **()** ve metodun gövdesini temsil eden **{}** köşeli parentezlerdir.

Fakat daha genel olarak, bir yöntem deklarasyonu sırasıyla altı bileşeni içerir:

1. **public**, **private** gibi değiştiriciler(modifiers),
2. Yöntemin dönüş türü — yöntem tarafından döndürülen değerin veri türü veya yöntem bir değer döndürmezse, ``void`` olarak tanımlanması,
3. Yöntem ismi - yalnız alan(field) adları için kurallar yöntem adları için de geçerlidir, ancak kural biraz farklıdır,
4. Parantez içindeki parametre listesi - parantez içine () alınmış veri türlerinden önce virgülle ayrılmış giriş parametreleri listesi, - herhangi bir parametre yoksa, boş parantez kullanmalısınız,
5. İstisna listesi,
6. Yöntem gövdesi - köşeli parantez arasına alınmış yöntem gövdesi - yerel değişkenlerin bildirimi de dahil olmak üzere yöntemin kodu buraya gelir.

#### Metot imzası:

Yöntem deklarasyonu bileşenlerinden ikisi yöntem imzasınının ne olduğunu bize söyler. Bunlar, yöntemin adı ve parametre türleridir. Yani yukarıdaki yöntemin imzasını içeren kısımlarını aşağıda göstermeye çalışacağım.

```java
resultNumbers(int , int )
```
## Overriding(Ezici) Metot Örnekleri
### Örnek 1

İlk örneğimiz **Object** sınıfının kendisine bakmak olacak. Bu üst sınıfta ``toString`` adında bir yöntemimiz bulunmaktadır. ``toString`` yöntemi bir nesnenin içeriğini(sahip olduğu state'i, yani instance değişkenlerinin tuttuğu değerler) veya dize temsilini yazdırır. Nesne sınıfında olduğu için Java'daki tüm nesneler ``toString`` yöntemini geçersiz kılabilir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-27-Java-inheritance9/override1.webp"  width="100%" height="100%" loading="lazy" alt="override method">

```java
public class Person {
    private String name;
    public Person( String n ) {
        //super();
        this.name = n;
    }

    public String getName() {
        return name;
    }

    public String toString(){
        return this.getName();
    }
}
```

Yukarıdaki kod bloğunda örneğin **Object** sınıfından geçersiz kıldığımız(override) ``toString()`` yöntemini, ``getName()`` getter yöntemini döndürecek şekilde ezdik. Dönüş tipi **String** olan farklı bir şey de döndürebilirdik. Bu tamamen size bağlı..

```java
Person p = new Person("Hasan");
System.out.println(p.toString());
```

Diyelim ki bir main metodu içinde bir Person objesi oluşturup, yukarıdaki gibi bunu ekrana yazdırmak istiyorsunuz... ``toString()`` metodu **Object** sınıfının toString metodunu çağırmayacaktır. Çünkü bu metodu ezdiğimiz için, çağrılan ``toString()`` metodu, **Person** sınıfındaki olacaktır. Sonuç olarak ekrana **"Hasan"** yazdırılır. Aslında burada biraz da polimorfizm devreye girer. Polimorfizm konusu overriding konusu ile iç içe bir konudur ve beraber değerlendirmek çok önemlidir. Polimorfizm konusuna henüz girmedik ama **Robert C. Martin**'in polimorfizm için yaptığı şu tanımı sizinle paylaşmak istiyorum.

> The lowest implementation of the method down the hierarchy is automatically invoked that's the definition of the polymorphism in JAVA. (Yöntemin hiyerarşideki en düşük uygulaması otomatik olarak çağrılır, bu JAVA'daki polimorfizmin tanımıdır.)

> **Not:** Bu arada **println** yönteminin olduğu yerde **toString()** yöntemini çağırmak gereksizdir. Çünkü **println** yöntemi bu yöntemi zaten otomatik olarak çağırır. Yani **System.out.println(p);** yazmamız yeterlidir.

### Örnek 2

Şimdi yukarıdaki örneği **Student** sınıfını da işin içine dahil ederek güncelleyelim istiyorum.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-27-Java-inheritance9/override2.webp"  width="100%" height="100%" loading="lazy" alt="override method">

Görüleceği üzere ``toString()`` metodu **Person** sınıfında olduğu gibi tekrar geçersiz kılınmıştır. Burada, **Person** sınıfının ``toString()`` metodu için yaptığı yorumlamayı, **Student** sınıfı için tekrar yorumlayacağım, demek istiyorum.

**Student** sınıfı ``toString()`` metoduna ek olarak **private** değiştiricili, ``studentID`` isminde bir üye değişkenine bir de bu üye değişkenine dışarıdan ulaşımı sağlamak için **public** değiştiricili, ``getSID()`` isminde bir getter metoda sahiptir. Buradaki amacımız da Person sınıfında olduğu gibi ``toString`` yöntemini sınıf bilgilerini yazdırmak için kullanmak olacak. Yani özetle yapmak istediğim, bir öğrenci kimlik numarası ve ardından kişi hakkındaki bilgileri yazdırabilmektir.

```java
public class Student extends Person {
    private int studentID;

    public int getSID() {
        return studentID;
    }

    public Student (String n, int m) {
        super(n);
        this.studentID = m;
    }

    public String toString(){
        return this.getSID()+": "+this.getName();
    }
}
```

Buradaki ilk denememin bu şekilde olduğunu hayal edin. Hatırlarsanız, **this** anahtar sözcüğünü kullanarak sınıf içi yöntemlere ve değişkenlere ulaşabiliyordum. ``toString()`` yönteminde **this** anahtar sözcüğünü kullanarak **aynı sınıf** içinde bir getter yöntem olan ``getSID()``yöntemine, bir de **üst sınıf** getter yöntemi olan ``getName()`` yöntemine ulaşmışız. Peki üst sınıf için neden **this** anahtar kelimesini kullanıyoruz ve kullanmamıza rağmen neden hata almıyoruz? Nedeni aslında çok basit!!! Hatırlarsanız bir sınıfı miras aldığımızda, o sınıfın bütün **public** değişken ve yöntemlerini kullanabiliyorduk. **public** olmayanlara da yine public **getter** ve **setter** yöntemleri aracılığı ile erişim mümkündü... Burada da aslında tam olarak bu işlem oluyor. **this** anahtar kelimesi ``getName()`` yöntemini bulmak için önce mevcut sınıfa bakar. Şayet mevcut sınıfta bulamazsa, miras aldığı sınıfta bu yöntemi arar. Miras aldığımız sınıfta bu yöntemin olduğunu zaten biliyoruz. Bu sebepten ötürü kodumuz gayet normal çalışır ve hata almayız. <u><i>Peki bu yaklaşım, tasarımsal olarak doğru bir kodlama mıdır?</i></u> Diyelim ki **Person** sınıfı ile ilgilenen ayrı bir ekip var ve sürekli bu sınıfta bir güncelleme yapılıyor. **Person** sınıfında olduğu gibi **Student** sınıfında da ``toString()`` metodu sınıf hakkında bilgileri yazdırmak için kullanılıyor olsun. **Person** sınıfında şu şekilde değişiklikler yapıldığını hayal edelim;
* ``name`` isimli üye değişkenini ``personName`` olarak güncellenmesi,
* ``age`` isimli yeni bir üye değişkeni eklenmesi ...

Aklıma gelenler şimdilik bu kadar. Bu gibi durumlarda haliyle **Student** sınıfında da bir takım değişikliklerin yapılması gerekebilir.  Birincisi, ``this.getName()`` olarak çağırdığınız **Person** **getter** metodunu, ``this.getPersonName()`` olarak çağırmak durumunda kalabilirsiniz... Veyahut **Person** sınıfındaki ``age`` isimli üye değişkeni için yeni bir **getter** yöntem yapıp, bu yöntemi **Student** sınıfının ``toString()`` metodunda çağırmaya çalışabilirsiniz. Kısacası yapılan her bir değişiklik için bir güncelleme gerekebilir. Bu şekilde ilerlemek yerine doğrudan üst sınıfın ``toString()`` yöntemini çağırsak ve gerekli değişiklikleri bu üst sınıfın ``toString()`` yönteminde yapsak sizce nasıl olur? **Person** sınıfı ekibi yaptığı değişiklikleri sadece ``toString()`` sınıfında bildirse, bu durum **Student** sınıfını tasarlayan ekibi ilgilendirir mi? Tabii ki hayır. **Student** sınıfı ``toString()`` metodu içinde yine **Person** sınıfının ``toString()`` yöntemi çağrılacağı için, **Person**'da yapılan değişiklikleri etkilenmeden çağırmış olur.

Yani **Student** sınıfının içinde ``toString()`` metodunu aşağıdaki şekilde güncellersek, tasarımsal olarak daha şık olacaktır. Yaptığımız değişiklik görüleceği üzere ``this.getName()`` yerine ``super.toString();`` getirmek oldu.

```java
 public String toString(){
         return this.getSID()+": "+super.toString();
 }
```

Bu arada Student sınıfını daha detaylı görmek isteyebilirsiniz;

```java
public class Student extends Person {
    private int studentID;

    public int getSID() {
        return studentID;
    }

    public Student (String n, int m) {
        super(n);
        this.studentID = m;
    }

    public String toString(){
        return this.getSID()+": "+super.toString();
    }
}
```

Özetle bir main metodu içinde aşağıda kodu çağırdığımızda;

```java
Student s = new Student("Hasan", 1234);
System.out.println(s);
```

alacağımız sonuç şu şekildedir.

```java
1234: Hasan
```

Peki main metodu içinde ``s`` değişkenini, yani referansın tipini **Person** olarak değiştirirsek yine aynı sonucu mu alırız?

```java
Person s = new Student("Hasan", 1234);
System.out.println(s);
```
Sonuç yine aynı olacaktır. Sebebi polimorfizmdir. Neden böyle olduğunu bu bölüme geçtiğimizde etraflıca inceleyeceğiz.

```java
1234: Hasan
```


> **Hatırlatma:** **this** anahtar sözcüğü mevcut sınıfı temsil ederken, ``super`` anahtar sözcüğü üst sınıfı temsil eder.


## Referanslar
* [toString method](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#toString--)
* [Method overriding](https://en.wikipedia.org/wiki/Method_overriding)
* [Defining Methods](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)
* [Super Keyword in Java](https://www.javatpoint.com/super-keyword)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
