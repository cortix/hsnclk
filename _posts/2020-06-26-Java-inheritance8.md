---
title: "Java'da Kalıtım 8 - Alıştırmalar"
comments: false
excerpt: "Bu bölümde şu ana kadar öğrendiklerimizi test etmek amacıyla birkaç tane örnek kod üzerinde çalışacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/equality.png
  overlay_image: /assets/images/unsplash-image-12.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [NOAA](https://unsplash.com/photos/sDCG1hTV8mI) on Unsplash"
#  video:
#    id: cR9uwtMQt-g
#    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - Java-kalitim-polimorfizm
tags:
  - Java inheritance
  - Java nesne oluşumu
  - Java Kurucu(constructor)
  - Java derleyici kuralları
  - Java değişken ilklendirme
  - Java this keyword
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---

1. Java’da Kalıtım 1 - [Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?](/java-kalitim-polimorfizm/Java-inheritance1/)
2. Java’da Kalıtım 2- [Extends](/java-kalitim-polimorfizm/Java-inheritance2/)
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası İçin Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. **Java’da Kalıtım 8 - Alıştırmalar**
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>

## Genel Bakış

Aşağıdaki sorularda bir önceki derslerde öğrendiklerimizi pekiştirmeye çalışacağız. Lütfen önce koda bakarak kendiniz çözmeye çalışın. Bu çok daha yararlı bir egzersizdir. IDE kullanarak çözmeye çalışmak kodun çalışma mantığını anlamanıza yardımcı olmaz. Kalem kağıt kullanabilirsiniz.

## Örnek 1

```java
public class Student extends Person {

    public Student (String n) {
        super(n);
        System.out.println("3");
    }

    public Student() {
        this("Student");
        System.out.println("2");
    }
}
```

```java
public class Person {
    private String name;
    public Person( String n ) {
        this.name = n;
        System.out.println("1");
    }
}
```

Yukarıdaki kod bloğununda önceki derslerden alışık olduğumuz **Person** ve **Student** sınıfları yer almaktadır. Aşağıdaki **main** metodu içinde yer alan kod çalıştırılmak istendiğinde komut ekranında nasıl bir çıktı ile karşılaşırız?

```java
public static void main(String[] args) {
      Student s = new Student();
}
```

* Görüleceği üzere; ``new Student()`` argümansız bir **Student** constructor'ı çağrılmaktadır.
* **Student** sınıfının argümansız metodunun ilk satırında ``this("Student")`` Student sınıfının argümanlı kurucusuna bir çağrı bulunmaktadır. Daha metoda girer girmez bizi argümanlı kurucuya(constructor) yönlendirmektedir.
* Argümanlı kurucu ise ``this("Student")`` çağrısından aldığı **"Student"** isimli string argümanı, argümanlı kurucunun ilk satırında yer alan ``super(n)`` çağrısı ile **Person** sınıfının argümanlı kurucusuna taşır.
* **Person** kurucusunun ilk satırı her ne kadar ``this.name = n`` olsada biz biliyoruz ki derleyici buraya bir ``super()`` çarğısı ekleyecektir. Çünkü önceki derslerden de bildiğimiz üzere derleyici kurallarından [3.kural,](/java-kalitim-polimorfizm/Java-inheritance6/#kural-3) kurucunun 1. satırında ya aynı sınıf kurucusunu çağıran bir **this(args<sub>opt</sub>)** ifadesi olacak ya da bir üst sınıf kurucusunu çağıran bir **super(args<sub>opt</sub>)** ifadesi olacak diyordu. Bunların ikiside yoksa java derleyicisi ``super()`` çağrısını ekleyeceğini belirtiyordu. Bu sebepten ötürü biz görmesek de java arka planda bu çağrıyı yapar. Amaç Object sınıfına kadar ulaşıp, oradan değişkenleri ilklendirerek geri dönmek.
* **Person** kurucusu içinde taşınan bu argüman, **Person** sınıfı üye değişkenine ``this.name = n`` atandıktan sonra,

    ```java
    System.out.println("1");
    ```
    ile komut satırı ekranına **"1"** yazdırılır.
* **Person** kurucusu ile işimiz bitince, bu kurucuya nereden ulaştıysak oraya geri dönmemiz gerekecektir. Bu kurucuya **Student** sınıfının argümanlı constructor'ından geldiğimizi biliyoruz. Orada kaldığımız satırdan devam ederiz. Karşımıza bir;

    ```java
    System.out.println("3");
    ```
    işlemi daha çıkmaktadır. Ekrana sonuç olarak **"3"** yazdırılır.

* Bu kurucuda da işimiz bittiğine göre buradan çıkmamız gerekecektir. O halde bu kurucuya nereden geldiğimize bakarız. Hatırlayacak olursak bu kurucuya **Student** sınıfının argümansız kurucusundan gelmiştik. Burada da yarım kalan bir işlemimiz bizi beklemektedir.
    ```java
    System.out.println("2");
    ```
    satırı ile ekrana **"2"** yazdırırız ve **main** metodu içinde başladığımız yere geri döneriz.
* Sonuç olarak ekrana **1, 3, 2** yazdırılır.

## Örnek 2

```java
public class Person {
    private String name;

    public Person( String n ) {
        super();
        this.name = n;
    }
    public void setName( String n ) {
        this.name = n;
    }
}
```

```java
public class Student extends Person {
    public Student () {
        this.setName("Student");
    }
}
```

```java
public static void main(String[] args) {
      Student s = new Student();
}
```

Benzer şekilde **main** metodu içinde bulunan kod bloğu çalıştırılmak istendiğinde hangi sonucu alırız.

* Benzer şekilde ``new Student()`` kodu ile **Student** sınıfının argümansız kurucusu çağrılmaktadır. **Student** kurucusu içinde

    ```java
    this.setName("Student");
    ```
    ifadesi ile karşılaşmaktayız. Her ne kadar ilk olarak bu satır ile karşılaşsak da biliyoruz ki derleyici bizim yerimize arka planda ``super()`` çağrısını kurucunun ilk satırına ekleyecektir. Çünkü önceki derslerden de bildiğimiz üzere derleyici kurallarından [3.kural,](/java-kalitim-polimorfizm/Java-inheritance6/#kural-3) kurucunun 1. satırında ya aynı sınıf kurucusunu çağıran bir **this(args<sub>opt</sub>)** ifadesi olacak ya da bir üst sınıf kurucusunu çağıran bir **super(args<sub>opt</sub>)** ifadesi olacak diyordu. Bunların ikiside yoksa java derleyicisi ``super()`` çağrısını ekleyeceğini belirtiyordu. Bu sebepten ötürü biz görmesek de java arka planda bu çağrıyı yapar.

* Yalnız şöyle bir sorun var. **Student** sınıfının bir üst sınıfı, yani miras aldığı sınıf olan **Person** sınıfı argümansız bir kurucuya **sahip değildir.** Bu sebepten ötürü derleme hatası alırız.

> Son soruda Java derleyicisi arka planda neden bir argümansız kurucu eklemedi diye düşünebilirsiniz. Sebebi şudur: Hali hazırda argüman alan bir kurucunuz varsa, compiler insiyatifi size bırakır. Bu durumda derleme hatası almak istemiyorsak, argümansız kurucuyu ek olarak bizim elle eklememiz gerekmektedir.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm.

Aşağıdaki hazırladığım java eğitim [videosunda](https://www.youtube.com/watch?v=cR9uwtMQt-g), **main** metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım.
</div>



## Referanslar
* [Using the this Keyword](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [this keyword in Java](https://www.javatpoint.com/this-keyword)
* [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
