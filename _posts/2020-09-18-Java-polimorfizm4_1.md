---
title: "Java'da Polimorfizm 4.1 - Statik ve Dinamik Bağlanma 1"
comments: true
excerpt: "Bu derste hem Java'da statik ve dinamik bağlanma arasındaki farkları ele alacağız."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-55.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [majd altaifi](https://unsplash.com/photos/rVAvAxQSmGI) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - polimorfizm
  - static/early binding(statik/erken bağlanma)
  - dynamic/late binding(dinamik/geç bağlanma)
  - overriding metot
  - overloading metot
  - ezici metot
  - geçersiz kılma
  - aşırı yükleme
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}


Bu ve hemen sonrasında gelecek bölüm bir önceki bölümle ve Java'da Kalıtım 3.1 - Static ve Dinamik Tür bölümüyle doğrudan bağlantılıdır. Bu bölümde özellikle, Java'nın Dil Şartnamesi dökümanından da aldığım önemli notları paylaşacağım. Yer yer bazı tanımlamaları olduğu gibi sunmayı planlıyorum. Ek olarak bundan sonra paylaşacağım **Statik ve Dinamik Bağlanma 2** konusuna da kesinlikle bakmanızı öneririm. Bu konunun daha iyi anlaşılmasını sağlayacak bilgiler içermektedir.


Aslında derleme ve çalışma zamanı polimorfizmi tanımlarına ek olarak aşağıdaki tanımları da kullanmamızda herhangi bir sakınca yoktur.

1. Statik/Erken bağlanma
2. Dinamik/Geç bağlanma

Çünkü yazdığınız kod bir polimorfizm sergiliyorsa, koda ait metot ve değişkenlerin bazılarının derleme zamanında, bazılarınınsa çalışma zamanında bağlanması gerekmektedir. O halde şöyle bir genelleme yapabiliriz.

* Eğer bu haritalama derleme zamanında gerçekleşirse, statik/erken bağlanma,
* Eğer bu çözülme çalışma zamanında gerçekleşiyorsa, dinamik/geç bağlanma  

gerçekleşir diyebiliriz.



## Statik/Erken Bağlanma (Static/Early Binding)

Statik, yani erken bağlanma, derleme zamanında meydana gelen olayları ifade eder. Özetle, bir işlevi çağırmak için gereken tüm bilgiler derleme zamanında biliniyorsa, erken bağlanma gerçekleşir.

> Tüm ``static``, ``private`` ve ``final`` yöntemlerin bağlanması derleme zamanında gerçekleşmektedir.

Derleyici, bu yöntemlerin geçersiz kılınamayacağını bilir. Aynı zamanda bu yöntemlere yerel sınıf(local class) nesnesi tarafından erişilebileceğinin de farkındadır. Bu nedenle derleyici "yerel sınıfın" nesnesini belirlemekte herhangi bir zorluk yaşamaz. Bu tür yöntemlerin bağlanmalarının "statik" olmasının nedeni budur. **Çünkü statik bağlanma daha iyi bir performans sağlar.**


## Dinamik/Geç Bağlanma (Dynamic/Late Binding)

Geç bağlanma(late binding), çalışma zamanına(run-time) kadar çözümlenmeyen işlev çağrılarını ifade eder. Geç bağlanma veya başka bir ifadeyle dinamik bağlanmada, derleyici çağrılacak yönteme karar vermez. Sanal(virtual) fonksiyonlar geç bağlanmayı sağlamak için kullanılır. Bu arada java'da sanal(virtual) fonksiyonlar var mıdır?

> Java'da, sanal işlev kavramı çok basittir, çünkü **static**, **private** veya **final** dışındaki tüm işlevler varsayılan(default) olarak sanal(virtual) işlevlerdir. Bazı programlama dillerinde işlevlerin sanal olduğunu belirtmek için **virtual** anahtar kelimesi kullanılır. Java'da yukarıda da belirtildiği gibi tüm **üye işlevler(member functions)** varsayılan olarak sanaldır.

--------


Aslında statik ve dinamik bağlanmaya verilebilecek en belirgin örnekler; statik bağlama için **overloading(aşırı yüklenmiş)** metotlar, dinamik bağlama için ise **overriding(geçersiz kılınan)** metotlardır. Örneğin aşağıdaki kod bloğunu ele alabiliriz.

Aşağıdaki örnekte hem overloading hem de overriding metot örneklerini göreceksiniz. Derleyici ``Car`` sınıfındaki bu overloading metotları ve  metotların kodlarını derleme zamanında çözecektir. Bu bir statik bağlama örneğidir. Fakat durum overriding metotlar için farklıdır. Onların bağlanması çalışma zamanında olacaktır. ``BMW`` sınıfındaki ``getCarBrand`` metodu buna bir örnektir.

```java
public class Car {
    public String getCarBrand(String name){
        return name;
    }
    public String getCarBrand(String name, int year){
        return name +" " +year;
    }
}
```

```java
public class BMW extends Car {

    @Override
    public String getCarBrand(String name){
        return name;
    }
}
```

```java
public class Features {
  public static void isElectricCar(Car car){
      System.out.println("No, there is not enough information");
  }
  public static void isElectricCar(BMW bmw){
      System.out.println("Yes, it is");
  }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        Car bmw = new BMW();

        System.out.println(car.getCarBrand("mercedes"));
        System.out.println(car.getCarBrand("ford", 1960));
        System.out.println(bmw.getCarBrand("bmw"));

        Features.isElectricCar(car);
        Features.isElectricCar(bmw);
    }
}
```
**Çıktı:**
```
mercedes
ford 1960
bmw
No, there is not enough information
No, there is not enough information
```

Görüleceği üzere statik metotlar statik bağlanmaya maruz kalırlar. ``isElectricCar`` statik metodu içine aldığı ``bmw`` referansını, statik bağlabnaya maruz kaldığı için ``Car`` olarak algılar ve **"No, there is not enough information"** sonucunu ekrana yazdırır.



## Statik Bağlanmada Field Erişimi

Şartname'de ifade edildiği gibi, hangi field'ın kullanılacağını belirlemede, çalışma zamanında başvurulan gerçek nesnenin sınıfının değil(yani heap alanındaki işaret edilen nesne), yalnızca birincil ifadenin tipinin kullanıldığını unutmayın. Burada birincil ifadeye karşılık gelen referans tipidir.

>Yine şartnameye göre birincil ifadeler, diğerlerinin oluşturulduğu en basit ifade türlerinin çoğunu içerir: değişmez değerler(literals), nesne oluşturmaları(object creations), alan erişimleri(field accesses), yöntem çağrıları(method invocations), yöntem başvuruları(method references) ve dizi erişimleri(array accesses). Parantezli bir ifade de sözdizimsel olarak birincil ifade olarak işlem görür.

Aşağıdaki örnek anlatılmak isteneni daha net ortaya koyacaktır.

```java
class S { int x = 0;}

class T extends S { int x = 1; }

public class Test1 {
    public static void main(String[] args) {
        T t = new T();
        System.out.println("t.x=" + t.x + when("t", t));

        S s = new S();
        System.out.println("s.x=" + s.x + when("s", s));

        s = t;
        System.out.println("s.x=" + s.x + when("s", s));
    }

    static String when(String name, Object t) {
        return " when " + name + " holds a "
                        + t.getClass() + " at run time.";
    }
}
```

Çıktı şu şekildedir:

```
t.x=1 when t holds a class T at run time.
s.x=0 when s holds a class S at run time.
s.x=0 when s holds a class T at run time.
```


Son satır, aslında erişilmeye çalışılan field'ın, başvurulan nesnenin çalışma zamanı sınıfına **bağlı olmadığını** gösterir; s değişkeni, T sınıfından bir nesnenin bir referansını tutsa bile, **s.x** ifadesi, **S** sınıfının **x** field'ını ifade eder, çünkü **s** ifadesinin türü **S**'dir. T sınıfının nesneleri, biri **T** sınıfı için ve biri de üst sınıfı olan **S** olmak üzere **x** isimli iki field içerir.

>Alan erişimlerine yönelik bu dinamik arama eksikliği, programların basit uygulamalarla verimli bir şekilde çalıştırılmasına izin verir.

<figure style="width: 700px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-09-18-Java-polimorfizm4_1/1.png" alt="Static Binding for Field Access">
  <figcaption></figcaption>
</figure>

Neden derleme zamanında overloading(aşırı yükleme) yapılabilmesine rağmen overriding(geçersiz kılma) yapılamıyor olabilir? Aslında şöyle düşünebiliriz.

```java
public void sample(List<String> list) {
    System.put.println(list.size());
}
```

sample metodunun argümanına baktığımızda bir List arayüzü(interface) ile kaşılaşıyoruz. Bildiğiniz üzere List bir abstract(yani soyut) sınıftır. Haliyle interface'leri somut(yani concrete) bir sınıf olmadan yaratamayız. Yani ``List s1 = new List()`` yapmaya çalıştığınızda alacağınız hata şu şekilde bir derleme hatası olacaktır. **'List' is abstract; cannot be instantiated**

```java
List<String> s1 = new ArrayList<String>();
List<String> s2 = new LinkedList<String>();
List<String> s3 = new Vector<String>();
```

Peki sorulması gereken soru şu? Bu List arayüzü hangi somut sınıfı temsil ediyor? ArrayList? LinkedList?  Derleme zamanında bunu bilme şansımız var mı? Şu ana kadar öğrendiklerimizden yola çıkacak olursak, List arayüzünün somut türünün ne olduğunu derleme sırasında bilmenin bir yolu yoktur. Yalnızca çalışma zamanında öğrenebilirsiniz. Dolayısıyla, size() yönteminden hangi somut sınıftan çağrılacağına yalnızca çalışma zamanında karar verilebilir.

-----

Alanlara(fields) erişmek için örnek yöntemlerin(instance methods) kullanıldığı benzer bir örneği ele alacak olursak;


```java
class S           { int x = 0; int z() { return x; } }
class T extends S { int x = 1; int z() { return x; } }
class Test2 {
    public static void main(String[] args) {
        T t = new T();
        System.out.println("t.z()=" + t.z() + when("t", t));
        S s = new S();
        System.out.println("s.z()=" + s.z() + when("s", s));
        s = t;
        System.out.println("s.z()=" + s.z() + when("s", s));
    }
    static String when(String name, Object t) {
        return " when " + name + " holds a "
                        + t.getClass() + " at run time.";
    }
}
```

```
t.z()=1 when t holds a class T at run time.
s.z()=0 when s holds a class S at run time.
s.z()=1 when s holds a class T at run time.
```

Son satır, erişilen yöntemin, başvurulan nesnenin çalışma zamanı sınıfına bağlı olduğunu gösterir; s, T sınıfına ait bir nesneye referans tuttuğunda, s.z () ifadesi, s ifadesinin türü S olmasına rağmen, T sınıfının z yöntemini ifade eder. **T sınıfının z yöntemi, S sınıfının z yöntemini geçersiz kılar.**

## Sözlük

* Bilgisayar bilimlerinde, dinamik dağıtım([Dynamic dispatch](https://en.wikipedia.org/wiki/Dynamic_dispatch)), hangi polimorfik işlemin çalışma zamanında uygulanacağını seçme işlemidir.
* Sınıf tabanlı programlamada, [downcasting](https://en.wikipedia.org/wiki/Downcasting) veya tür ayrıntılandırma(type refinement), bir temel sınıfın türetilmiş sınıflarından birine referans verme eylemidir.
* Geç bağlanma([late binding](https://en.wikipedia.org/wiki/Late_binding)), dinamik bağlanma veya dinamik bağlantı, bir nesneye çağrılan yöntemin veya bağımsız değişkenlerle çağrılan işlevin çalışma zamanında adıyla arandığı bir bilgisayar programlama mekanizmasıdır.


## Referanslar

* [https://www.geeksforgeeks.org/difference-between-early-and-late-binding-in-java/](https://www.geeksforgeeks.org/difference-between-early-and-late-binding-in-java/)
* [https://www.baeldung.com/java-static-dynamic-binding](https://www.baeldung.com/java-static-dynamic-binding)
* [Virtual Function in Java](https://medium.com/@namangupta01/virtual-function-in-java-vs-c-d75874d23#:~:text=In%20object%2Doriented%20programming%2C%20a,to%20provide%20the%20polymorphic%20behavior.)
* [https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#d5e25867](https://docs.oracle.com/javase/specs/jls/se13/html/jls-15.html#d5e25867)
* [https://www.geeksforgeeks.org/static-vs-dynamic-binding-in-java/](https://www.geeksforgeeks.org/static-vs-dynamic-binding-in-java/)
* [https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString()](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#toString())
* [https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)
* [https://docs.oracle.com/javase/6/docs/api/java/util/List.html](https://docs.oracle.com/javase/6/docs/api/java/util/List.html)
* [https://docs.oracle.com/javase/specs/jls/se6/html/classes.html](https://docs.oracle.com/javase/specs/jls/se6/html/classes.html)
* [https://www.baeldung.com/java-static-dynamic-binding](https://www.baeldung.com/java-static-dynamic-binding)
* [https://stackoverflow.com/questions/19017258/static-vs-dynamic-binding-in-java](https://stackoverflow.com/questions/19017258/static-vs-dynamic-binding-in-java)
* [https://www.baeldung.com/java-polymorphism](https://www.baeldung.com/java-polymorphism)
* [Virtual Function in Java vs. C++](https://medium.com/@namangupta01/virtual-function-in-java-vs-c-d75874d23#:~:text=In%20object%2Doriented%20programming%2C%20a,to%20provide%20the%20polymorphic%20behavior.)
* [What is Virtual Method](http://net-informations.com/faq/oops/virtual.htm)
* [https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)
* [https://stackoverflow.com/questions/32422923/why-does-java-bind-variables-at-compile-time](https://stackoverflow.com/questions/32422923/why-does-java-bind-variables-at-compile-time)
