---
title: "Java'da Polimorfizm 1 - Amaç?"
comments: false
excerpt: "Java'da polimorfizm ne anlama gelmektedir ve neden polimorfizme ihtiyaç duyarız?"
header:
  teaser: "/assets/images/svg-book11.svg"
  og_image: /assets/images/svg-book11.svg
  overlay_image: /assets/images/svg-book11.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - Java polimorfizm
  - Java virtual method invocation
  - Robert C. Martin
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--info" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---

12. **Java’da Polimorfizm 1 - Amaç?**
13. Java’da Polimorfizm 2 - [İzlenecek Kurallar Nelerdir?](/java-kalitim-polimorfizm/Java-polimorfizm2/)
14. Java’da Polimorfizm 3 - [Casting](/java-kalitim-polimorfizm/Java-polimorfizm3/)
15. Java’da Polimorfizm 4.1 - [Statik ve Dinamik Bağlanma 1](/java-kalitim-polimorfizm/Java-polimorfizm4_1/)
16. Java’da Polimorfizm 4.2 - [Statik ve Dinamik Bağlanma 2](/java-kalitim-polimorfizm/Java-polimorfizm4_2/)
17. Java’da Polimorfizm 4.3 - [Dinamik Bağlanma Örnek](/java-kalitim-polimorfizm/Java-polimorfizm4_3/)
18. Java’da Polimorfizm 5 - [Soyut(Abstract) Sınıflar ve Arayüzler(Interfaces)](/java-kalitim-polimorfizm/Java-polimorfizm5/)
</div>

## Polimorfizm Nedir?

Polimorfizm ismi daha çok biyolojik bir terimdir. Polimorfizm biyolojide, iki veya daha fazla farklı fenotipin aynı tür popülasyonunda bulunması anlamına gelmektedir. Basitçe söylemek gerekirse polimorfizm, bir gen üzerinde iki veya daha fazla özelliğin olması ihtimalidir. Örneğin, bir jaguarın cilt rengi açısından birden fazla olası özelliği vardır; açık biçimde veya koyu biçimde olabilirler. Bu çeşitlilik, gen için birden fazla olası varyasyona sahip olduğundan '**polimorfizm**' olarak ifade edilir. Türkçe çevirisine **çok biçimlilik** de denilebilir.

Fakat <u>programlama dillerinde ve tür teorisinde polimorfizm, farklı türdeki varlıklara tek bir arayüzün sağlanması veya birden fazla farklı türü temsil etmek için tek bir sembolün kullanılmasıdır.</u> Bir sınıfın alt sınıfları, kendi benzersiz davranışlarını tanımlayabilir, ve yine de üst sınıfın aynı işlevlerinden bazılarını paylaşabilir.

Bu bölümde polimorfizm konusunu ayrıntılı olarak incelemeye çalışacağız. Java dilinde polimorfizm, kalıtımla birlikte sahip olduğumuz en güçlü özelliklerden biridir. Kalıtım konusunu önceki 10 bölüm boyunca etraflıca ele almıştık. Aslında birçok kez polimorfizm kavramına da değindik. Fakat tanımsal olarak ilk kez burada ele alacağız.

Hatırlarsanız kalıtım konusunun 9.[bölümü](/java-kalitim-polimorfizm/Java-inheritance9/) sonunda, aşağıdaki ifadeyi verip, sebebini polimorfizm konusuna geldiğimizde ele alacağımızı söylemiştim.

```java
Person s = new Student("Hasan", 1234);
System.out.println(s);
```

Yukarıdaki örnekten de görüleceği üzere, referans türü **Person** <u>üst-sınıfı (superclass)</u> olan ve nesne türü **Student** <u>alt-sınıfı</u> olan bir objemiz bulunmaktadır. Burada yaptığımız aslında bir polimorfizmdir. Aşağıdaki şekili bir çok kez ele almıştık. **Person** üst sınıfını miras alan **Student** (öğrenci) ve **Faculty** (öğretim üyesi) sınıfları, aslında her bir öğrenci ve öğretim üyesinin özünde bir **Person** (kişi) olduğunu ifade etmektedir. **Person** sınıfı sayesinde daha kapsayıcı bir üst sınıf elde etmiş oluyoruz. Yalnız yazılım dünyasında bu, polimorfizm tanımı için tek başına yeterli değildir. Yani kapsayıcı üst sınıfı kastediyorum! <u>Polimorfizmi anlamak için, "metotların geçersiz kılınması (overriding)" konusunu da ele almamız, ve hatta beraber değerlendirmemiz gerekmektedir.</u>

<br/>{% picture 2020-06-27-Java-inheritance9/override2.png --alt Java Method Overriding(java metot geçersiz kılma(ezme)) --img width="100%" height="100%" %}<br/>

Kalıtım konusunun 9.[bölümünde](/java-kalitim-polimorfizm/Java-inheritance9/) verdiğim tanımı burada yinelemek istiyorum.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Robert C. Martin'in Polimorfizm Tanımı:</h4>
---
Polimorfizm konusu overriding konusu ile iç içe bir konudur ve beraber değerlendirmek çok önemlidir. **Robert C. Martin**'in polimorfizm için yaptığı şu tanımı sizinle paylaşmak istiyorum.

> The lowest implementation of the method down the hierarchy is automatically invoked, that's the definition of the polymorphism in JAVA. (Yöntemin hiyerarşide en aşağıda bulunan uygulaması otomatik olarak çağrılır, bu da JAVA'daki polimorfizmin tanımıdır.)

Burada en aşağıdaki uygulamadan(lowest implementation) kasıt **override edilen metottur**. Alt sınıfta bulunan bir metot üst sınıfta bulunan benzer bir metodu geçersiz kıldığında haliyle o yöntemin hiyerarşideki en alt uygulaması olur.

</div>

> **Not:** Daha da ilerlemeden önce, referans ve nesne türleri [konusuna](/java-kalitim-polimorfizm/Java-inheritance3/) da göz gezdirmemizde yarar var.


Kalıtım konusunun 9. bölümünde `toString()` metodunun geçersiz kılınmasını, **Student** sınıfına kadar ilerletmiştik. Buna ek olarak, **Faculty** sınıfında da bu metodu geçersiz kılacağız. Amacımız, polimorfizm kavramının ne olduğunu, ve neden yazılımda kullanıldığını anlatmak olacak. Bu arada **Faculty** sınıfına ek olarak, **employeeID** üye değişkeni, ve bu **private** üye değişkenini çağıran bir de **getter** metodunu ekledik.

Kalıtımın temel hedeflerini belirlerken, **tüm nesneleri tek bir veri yapısında tutmakla** ilgili bir şeyden bahsetmiştik. Dilerseniz ilgili konuya [geri](/java-kalitim-polimorfizm/Java-inheritance3/) dönüp bakabilirsiniz. Bu, farklı türdeki birden çok nesneyi işaret eden tek bir veri yapınız olabilir.

## Java'da polimorfizm'e bir örnek

Hem **Student**, hem **Faculty** hem de **Person** objelerini tek bir veri yapısı olan **Person** veri yapısında saklamış olduk. Çünkü hem **Student** hem de **Faculty** özünde bir **Person**'dır. Tabii ki **is-a** ilişkisinden ötürü bunu söylüyorum.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java'da polimorfizm'e bir örnek:</h4>
---
```java
public class Person {
    private String name;
    public Person( String n ) {
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

``` java
public class Faculty extends Person {
    private String employeeID;
    public String getEmployeeID() {
        return employeeID;
    }
    public Faculty(String n, String m){
        super(n);
        this.employeeID = m;
    }
    public String toString(){
        return this.getEmployeeID()+": "+super.toString();
    }
}
```
</div>


``p[i]`` ekrana yazdırılırken, ``toString()`` metodunu ekstradan çağırmamız gerekmediğini önceki bölümlerde belirtmiştim. Normalde bunu, ``p[i].toString()`` şeklinde de ifade edebilirdik fakat ``println`` metodu ``toString()`` metodunu hâlihazırda çağırdığı için, sadece ``p[i]`` şeklinde çağırmak da aynı sonucu verecektir.

Burada önemli olan soru şu!!! Size göre ``p[i]`` ekrana yazdırılırken, **Person** sınıfının ``toString()`` metodu mu?, **Student** sınıfının ``toString()`` metodu mu? yoksa **Faculty** sınıfının ``toString()`` metodu mu çağrılır? Cevabı aslında önceki bölümlerde kısmen vermiştik.

``` java
Person[] p= new Person[3];
p[0] = new Person("Hasan");
p[1] = new Student("Bilal", 1234);
p[2] = new Faculty("Baran", "ABCD");

for (int i = 0; i < p.length; i++) {
    System.out.println(p[i]);
}
```
<br/>

[Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/), ve bu konuyu peşi sıra takip eden bölümlerde, aslında bu sorunun cevabını etraflıca incelemeye çalıştım fakat polimorfizm tanımını ilk olarak burada dile getiriyorum.


Görüleceği üzere **referans tipleri** tek bir veri yapısını temsilen **Person** sınıfıdır. Bu sınıf diğer sınıfları kapsayıcı bir üst sınıftır. **Student** ve **Faculty** sınıfları bu sınıfı miras aldığı için **is-a** ilişkisi kapsamında, **Student** ve **Faculty** özünde bir **Person** diyebiliyorduk ama her kişinin (yani Person'ın) bir öğrenci (Student) veya bir öğretim üyesi (Faculty) olduğunu söyleyemiyorduk. Bu yüzden **Person** sınıfını, **Student** ve **Faculty** nesnelerini tek bir veri yapısında tutan ortak bir sınıf olarak belirleyebildik.


Yalnız şöyle bir ayrıntıdan bahsetmiştik. Referans tipleri, derleme zamanı kararları alınırken, obje/nesne tipleri ise, çalışma zamanı kararları alınırken devreye giriyordu. Şayet yukarıdaki kod bloğunu çalıştırmasaydık, ve derleme zamanında hangi ``toString()`` metotları devrede olurdu deseydiniz, hepsi için **Person** sınıfınınki diyebilirdik. Yalnız program çalıştığında referans tipleri, heap alanında temsil ettiği objeler ile eşleşir. Ama bu, ``toString()`` metodunun, hemen `p[0]` için **Person**'daki ``toString()``metoduna,  `p[1]` için **Student**'daki ``toString()``metoduna, `p[2]` için ise **Faculty**'deki ``toString()``metoduna, gideceği **anlamına gelmez**.

## Metot Geçersiz Kılmanın (Overriding) Polimorfizm İle İlişkisi

Tam da bu noktada metot geçersiz kılma(overriding) işlemi, polimorfizm kavramını yazılım tarafında tamamlıyor. Burada **dinamik polimorfizm (dynamic polymorphism)** olarak bilinen bir kavram devreye girer. JVM öncelikli olarak, bu metotların alt sınıflarda **geçersiz kılınıp kılınmadığına** bakar. Şayet ilgili metot **geçersiz kılınmışsa** doğrudan geçersiz kılındığı sınıftaki metoda gider. Aksi halde şansını **referans tipinin** sınıfındaki metotta deneyecektir. Şayet referans tipinin, heap tarafında hangi nesne türüne sahip olduğunu biliyorum diyorsanız, "**casting**" yaparak sorumluluğu derleyiciden alırsınız. "Casting" konusunu ilerleyen bölümlerde ele aldığım için, burada sadece değinmekle yetiniyorum.


> Stack alanındaki referanslar, çalışma zamanında heap alanındaki hangi objeye bağlanacağını bilir. Ama her zaman nesne(**dinamik**) türüne göre ilgili yöntem çağrılmaz. Şayet overriding işlemi yoksa, referans türü neyse o sınıf içindeki metot çağrılır.

Bu örnekteki ``toString()`` metodu, alt sınıflarda geçersiz kılındığı için aşağıdaki sonucu alırız;

* ``p[0]`` referansı **Person** sınıfının ``toString()`` metodunu kullanır,
* ``p[1]`` referansı **Student** sınıfının ``toString()`` metodunu kullanır,
* ``p[2]`` referansı **Faculty** sınıfının ``toString()`` metodunu kullanır,


```java
Hasan
1234: Bilal
ABCD: Baran
```

<br/>
"**Java sanal makinesi (JVM)**", her değişkende referans alınan nesne için uygun yöntemi çağırır. Değişkenin türü tarafından tanımlanan yöntemi çağırmaz. Bu davranış, sanal yöntem çağırma(**virtual method invocation**) olarak adlandırılır, ve Java dilinde polimorfizm özelliklerinin önemli bir yanını gösterir.

Aslında bir sonraki bölümde bunun nasıl gerçekleştiğini, derleyici ve çalışma zamanı kuralları kapsamında göreceğiz. Özetle polimorfizmin bize verdiği şey, tüm nesnelerimizi büyük bir koleksiyonda tutma yeteneğidir.

> **Hatırlatma:** Dinamik olarak yazılan diller çalışma zamanında tür denetimi gerçekleştirirken, statik olarak yazılan diller derleme zamanında tür denetimi gerçekleştirir.


## Referanslar
* [Polymorphism (biology)](https://en.wikipedia.org/wiki/Polymorphism_(biology))
* [Polymorphism (computer science)](https://en.wikipedia.org/wiki/Polymorphism_(computer_science))
* [Static vs. Dynamic Type](https://inst.eecs.berkeley.edu/~cs61bl/su15/materials/guides/static-dynamic.pdf)
* [Dynamic typing vs. static typing](https://docs.oracle.com/cd/E57471_01/bigData.100/extensions_bdd/src/cext_transform_typing.html#:~:text=First%2C%20dynamically%2Dtyped%20languages%20perform,type%20checking%20at%20compile%20time.&text=If%20a%20script%20written%20in,the%20errors%20have%20been%20fixed.)
* [Static Typing vs Dynamic Typing](https://www.coderscampus.com/static-typing-vs-dynamic-typing/)
* [Difference between Dynamic and Static type assignments in Java](https://stackoverflow.com/questions/20504714/difference-between-dynamic-and-static-type-assignments-in-java/20505326)
* [New JDK 7 Feature: Support for Dynamically Typed Languages in the Java Virtual Machine](https://www.oracle.com/technical-resources/articles/javase/dyntypelang.html)
* [Dynamic Class Loading Example](https://examples.javacodegeeks.com/core-java/dynamic-class-loading-example/)
* [Dynamic Class Loading](https://www.clear.rice.edu/comp310/JavaResources/dynamic_class_load.html)
* [Java Reflection - Dynamic Class Loading and Reloading](http://tutorials.jenkov.com/java-reflection/dynamic-class-loading-reloading.html)
* [Fully Dynamic Classes With ASM](https://dzone.com/articles/fully-dynamic-classes-with-asm)
* [Polymorphism](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
