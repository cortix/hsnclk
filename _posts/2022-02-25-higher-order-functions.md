---
title: "Higher Order Functions (Üst Düzey Fonksiyonlar) (Programlama Stili PART 2)"
comments: false
excerpt: "Birinci-sınıf vatandaş(first-class citizen), birinci-sınıf fonksiyon(first-class function) ve higher-order(üst düzey) fonksiyonları açıklamaya çalışacağım."
header:
  #teaser: "/assets/images/2022-02-25-higher-order-functions/higher.png"
  #og_image: /assets/images/page-header-og-image.png
  #og_image: /assets/images/2022-02-25-higher-order-functions/higher.png
  teaser: "/assets/images/svg-book15.svg"
  og_image: /assets/images/svg-book15.svg
  overlay_image: /assets/images/svg-book15.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - programming-style
tags:
  - Programming-style
  - Java higher order functions
  - First-class citizens
  - First-class functions
#last_modified_at: 2023-01-06T15:12:19-04:00
toc: true
toc_sticky: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--warning" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Programlama Stili Serisi</h4>
---

1. Programlama Stili Part 1 - [Imperative and Declarative Style Programming (Zorunlu ve Bildirimsel Stil Programlama)](/programming-style/imperative-and-declarative-programming/)
2. **Programlama Stili Part 2 - Higher Order Functions (Üst Düzey Fonksiyonlar)**

</div>

Her şeyden önce, üst düzey fonksiyonları(yani higher order functions) anlamak için bazı tanımları bilmeliyiz. "**birinci sınıf vatandaş**(first-class citizen)" terimiyle başlamak istiyorum

## Birinci sınıf vatandaş (First-class citizen)

Wiki tanımına göre, programlama dili tasarımında, belirli bir programlama dilinde **birinci sınıf vatandaş** (ayrıca tür, nesne, varlık veya değer), genel olarak diğer varlıklar tarafından kullanılabilen, tüm işlemleri destekleyen bir varlıktır. Bu işlemler tipik olarak **argüman olarak iletilmeyi**(passed as an argument), **bir işlevden döndürülmeyi**(returned from a function) ve **bir değişkene atanmayı**(assigned to a variable) içerir.

Bu tanıma göre, yukarıda **vurgulanan işlemleri** destekleyen her şey *birinci sınıf vatandaş(first-class citizen)* olabilir. Buraya bir virgül koyup **birinci sınıf işlev**(first-class function) olan bir sonraki tanıma geçmek istiyorum.

## Birinci-sınıf işlev (First-class function)

*Birinci sınıf vatandaşın(first-class citizen)* ne olduğu hakkında biraz fikrimiz var. O halde, işlevlere *birinci sınıf vatandaşlar* gibi davranan bir programlama dilinin **birinci sınıf işlevlere**(first-class function) sahip olduğu söylenebilir.

## Üst Düzey Fonksiyonlar (Higher Order Functions)

Normal **fonksiyonlarda** genel olarak;

* Nesneleri fonksiyonlara geçirebiliriz
* Fonksiyonlarda nesneler oluşturabiliriz
* Fonksiyonlardan da nesne döndürebiliriz

**Üst düzey fonksiyonlarda**;

* Fonksiyonları fonksiyonlara geçirebiliriz
* Fonksiyonlar içinde fonksiyonlar oluşturabiliriz
* Fonksiyonlardan da fonksiyon döndürebiliriz.

Açıklığa kavuşturmak için iki kod örneği paylaşmak istiyorum.

Örneğin;

```java
Thread t1 = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("This is a thread");
    }
});

t1.start();
```

Yukarıdaki örnekte, `Thread` kurucusuna(yani constructor) bir `Runnable` nesnesi iletiyoruz. Burada **nesne** *birinci sınıf vatandaş* olarak davranır.

```java
Thread t2 = new Thread( () -> System.out.println("Another thread"));

t2.start();
```

Fakat fonksiyonlara veya kodlara da *birinci sınıf vatandaşmış* gibi davranabiliriz. Bu örnekte, *lambda ifadesi*(anonim) olan bir **fonksiyonu**, `Thread` kurucusuna argüman olarak iletiyoruz. Bu onu *üst düzey(yüksek dereceli) fonksiyon* yapar çünkü parametre olarak başka bir fonksiyon alır.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
<u>İngilizce terimlerin</u> türkçe anlamlarının tam olarak neye karşılık geldiğini bilmediğim için parantez içerisinde ingilizce karşılıklarını da yazmaya gayret ettim. Umarım kafanız karışmaz.

* **Higher-order functions :** üst düzey fonksiyonlar, yada yüksek dereceli fonksiyonlar olarak geçiyor.
* **First-class citizen :** birinci sınıf vatandaş
* **First-class function :** ise birinci sınıf fonksiyon olarak geçiyor.
</div>

## Referanslar:
* [First-class citizen](https://en.wikipedia.org/wiki/First-class_citizen)
* [First-class function](https://en.wikipedia.org/wiki/First-class_function)
* [Higher-order function](https://en.wikipedia.org/wiki/Higher-order_function)
* [Anonymous function](https://en.wikipedia.org/wiki/Anonymous_function)
* [Understanding Functional Programming - Venkat Subramaniam](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
