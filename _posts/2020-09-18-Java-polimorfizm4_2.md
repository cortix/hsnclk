---
title: "Java'da Polimorfizm 4.2 - Statik ve Dinamik Bağlanma 2"
comments: true
excerpt: "Bu derste Java'da statik ve dinamik bağlanma arasındaki farkları ele almaya devam edecek, konunun daha iyi anlaşılması için farklı bir örneği ele alacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-57.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Marina Montoya](https://unsplash.com/photos/GUi8uIw3JbQ) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - polimorfizm
  - static/early binding(statik/erken bağlanma)
  - dynamic/late binding(dinamik/geç bağlanma)
  - super keyword
  - this keyword
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}


Önceki bölümün devamı niteliğinde olan bu bölüm, statik ve dinamik bağlanma konusunun anlaşılmasını daha da artıracak.

Şimdi aşağıdaki gibi bir sınıf hiyerarşimizin olduğunu düşünelim.

<figure style="width: 200px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-09-18-Java-polimorfizm4_2/uml1.png" alt="Static and Dynamic Binding">
  <figcaption></figcaption>
</figure>


Görüleceği üzere **Person** sınıfı hiyerarşimizin en üstünde yer alan sınıf ve iki adet metodu bulunmakta. Onun hemen altında bu sınıfı miras alan bir **Student** sınıfımız var. Bu sınıf **Person** sınıfının 2 metodunu geçersiz kılmış(override). **Student** sınfını da **Undergrad** isimli başka bir sınıf miras alıyor ama bu sınıf **Student** sınıfının sadece ``method2()`` metodunu geçersiz kılıyor.


Şimdi aşağıdaki gibi bir kod bloğunu çalıştırmak istesek sizce nasıl bir sonuç alırız?

```java
public class Person {
    public void method1() {
        System.out.print("Person 1 ");
    }
    public void method2() {
        System.out.print("Person 2 ");
    }
}
```

```java
public class Student extends Person {
    public void method1() {
        System.out.print("Student 1 ");
        super.method1();
        method2();
    }
    public void method2() {
        System.out.print("Student 2 ");
    }
}
```

```java
public class Undergrad extends Student {
     public void method2() {
         System.out.print("Undergrad 2 ");
     }
}
```

```java
Person u = new Undergrad();
u.method1();
```

Şimdi aşama aşama ilerleyelim istiyorum.

1. ``Person u = new Undergrad();`` Kod çalışma zamanına geçtiğinde ``u`` referansının heap alanında **Undergrad** nesnesine bağlanacağını biliyoruz. O zaman şunu rahatlıkla söyleyebiliriz ki ``u`` referansı **Undergrad** nesnesini temsil etmektedir.
2. ``u.method1();`` referansımız haliyle ``method1`` isimli metodu ilk arayacağı yer **Undergrad** sınıfı olacaktır. Yalnız Undergrad sınıfımızda böyle bir metodun olmadığını görüyoruz. Peki Java bu durumda ne yapacak?
3. Java ilk olarak ilgili sınıfın miras aldığı bir sınıfın olup olmadığını kontrol eder. ``Undergrad extends Student`` Undergrad sınıfımız Student sınıfını miras aldığını görüyoruz. Java bu metodu bir de bu sınıfta arayacaktır.
4. Burada ``method1`` isimli yöntemi görmektesiniz. Şimdi bu yordamın içine girebiliriz. Kaşımıza ilk olarak ``System.out.print("Student 1 ");`` ifadesi çıkmaktadır. Haliyle ekrana yazdıracağımız ilk şey bu olacaktır.
5. ``super.method1();`` sonrasında bu ifade ile karşılaşıyoruz. Aslında kafa karıştırıcı asıl bölüm burası!! sizce **super** anahtar kelimesi **Undergrad** sınıfını mı temsil ediyor, yoksa **Person** sınıfını mı temsil ediyor? Cevabımız Person sınıfı olacak!! Peki neden? Statik ve dinamik bağlanma konusu tam da burada devreye giriyor. <u><i>Student sınıfındaki <b>"super.method1()"</b> çağrısı derleme zamanında çözülür, bu nedenle asıl çağıran nesne <b>Undergrad</b> sınıfına ait olsa da, Student sınıfındaki <b>super</b> anahtar kelimesi statik bağlandığından Person sınıfını ifade eder.</i></u> Yani daha derleme zamanında Java Student sınıfında bu super anahtar kelimesini farkeder ve bu sınıfın miras aldığı sınıfı arar. Ve burada derleme zamanında statik bağlamayı gerçekleştirir.
6. Şimdi de Student sınıfının miras aldığı Person sınıfındaki method1() yöntemine gireceğiz. Burada bir adet yazdırma ifadesi bulunmaktadır. ``System.out.print("Person 1 ");`` Bu işlemi de gerçekleştirdikten sonra tekrardan Student sınıfında kaldığımız yere geri döneriz. Sıradaki çağrı ``method2();`` bu şekilde verilmiş. Sanki bir çağıran nesne(calling object) yok gibi anlaşılabilir.
6. Daha önceki derslerden de hatırlayacağımız gibi java bu gibi ifadelerle karşılaştığında arka planda kendi bir **this** ifadesi ekleyecektir. Yani javanın bu ifadeden aslında anladığı şudur.. ``this.method2();``
7. Peki this anahtar kelimesi ile kastedilen hangi metotdur? Student sınıfı içindeki method2() mi? Person sınıfı içindeki ``method2()`` mi yoksa Undergrad sınıfı içindeki ``method2()`` yordamı mıdır? Cevap Undergrad sınıfıdır. Çünkü **this** anahtar kelimesi her zaman çalışma zamanındaki nesneyi yani başta asıl çağrılan nesneyi ifade eder. Buradaki çalışma zamanı nesnemizin Undergrad olduğunu biliyoruz. Bu sebepten bu metodu ilk olarak Undergrad sınıfı içerisinde aramalıyız. Bulamazsak başta olduğu gibi miras aldığı sınıflara bakabiliriz. Ancak Undergrad sınıfı içinde zaten bu metodun olduğunu görüyoruz.
8. Undergrad sınıfı içindeki method2() yordamı ``System.out.print("Undergrad 2 ");`` ifadesini yazdırmamızı istiyor. Sonuç olarak ekranda şu şekilde bir çıktı karşılaşırız.

```
Student 1 Person 1 Undergrad 2
```

## ÖZET

* ``super`` anahtar kelimesi ile yapılan çağrıların bağlanması(vehahut haritalanması da diyebiliriz) derleme zamanında gerçekleşir. Java ``super`` çağrısının bulunduğu sınıfı derleme zamanında haritalar ve doğrudan ``super`` çağrısının yapıldığı metodu bir üst sınıfta arar.
* ``this`` anahtar kelimesi ile yapılan metot çağrıları ise çalışma zamanında bağlanır. Yani Java aslında, çalışma zamanında nesnenin gerçek türünü kullanacaktır.



## Referanslar
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#d5e25867](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#d5e25867)
* [https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString()](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString())
* [https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [https://docs.oracle.com/javase/6/docs/api/java/util/List.html](https://docs.oracle.com/javase/6/docs/api/java/util/List.html)
* [https://docs.oracle.com/javase/specs/jls/se6/html/classes.html](https://docs.oracle.com/javase/specs/jls/se6/html/classes.html)
* [https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)
