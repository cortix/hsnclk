---
title: "Java'da Hafıza Modeli 1 - İlkel Veri Tipleri"
comments: true
excerpt: "Java'da İlkel Veri Tipleri Bellekte Nasıl Saklanır? Bu durumun net anlaşılması için nasıl simüle edebiliriz?"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-34.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Vincent van Zalinge](https://unsplash.com/photos/vUNQaTtZeOo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - İlkel Tipler İçin Hafıza Modeli
last_modified_at: 2020-02-19T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Java programlama dili statik olarak yazılmıştır(statically-typed), yani tüm değişkenlerin kullanılmadan önce bildirilmesi gerekir. Bu da, değişkenin türünü ve adını belirtmeyi içerir.

```java
int var = 1;
//Bir değişkenin veri türü içerebileceği değerleri ve üzerinde
//yapılabilecek işlemleri belirler. int'ye ek olarak, Java
//programlama dili diğer yedi ilkel veri türünü de destekler.
//Bunları aşağıda görebilirsiniz.
```

Veri türleri iki gruba ayrılır:
* İlkel veri türleri
* İlkel olmayan veri türleri String, Arrays(diziler) ve Classes(Sınıflar)

Örnek kod, programınıza "var" adlı bir alanın var olduğunu, sayısal verileri tuttuğunu ve başlangıç ​​değeri "1" olduğunu söyler. Bir alan, ilkel veya referans türünde(yani ilkel olmayanlar) olabilir. Sekiz ilkel tip vardır: *boolean* , *byte* , *char* , *short* , *long* , *float* ve *double*'dır. Bir referans türü, arabirimler(*interfaces*), diziler(*arrays*) ve *enumerated* türler dahil olmak üzere, doğrudan veya dolaylı bir `java.lang.Object` alt sınıfı olan herhangi bir şey olabilir.

Java'da tanımlanan bu sekiz ilkel veri tipi nesne olarak kabul edilmez ve doğrudan yığın(*stack*) üzerinde depolanırlar. Fakat nesneler, ilkel veri tiplerinin aksine daha komplex veri tipleri olduğundan *heap* adı verilen özel bir alanda depolanırlar. Her ne kadar farklı hafıza birimleri olsa da heap de stack de bilgisayarın RAM'inde yer alır. Heap ve stack dışında da JVM tarafından ayrılan bölümler vardır. Yalnız bu makalenin konusu sadece bu ikisini içerdiği için diğerlerine girmeyeceğim.

Bu yazıdaki asıl amacım, java'da ilkel(primitive) tür ve ilkel olmayan türler olan nesne ve dizilerin hafızada nasıl oluştuğunu resmederek göstermeye çalışmaktır.

İlkel tür, dil tarafından önceden tanımlanır ve ayrılmış(reserved keyword) bir anahtar sözcükle adlandırılır. (Yani int, double, short gibi...) İlkel değerler sahip olduğu durumu diğer ilkel değerlerle paylaşmaz.

Yukarıda listelenen sekiz ilkel veri türüne ek olarak, Java programlama dili de `java.lang.String` sınıfı aracılığıyla karakter dizileri için özel destek sağlar. Karakter dizginizi çift tırnak içine almak otomatik olarak yeni bir String nesnesi oluşturur; örneğin, String s = "bu bir dizedir" ;. Dize nesneleri değiştirilemez, yani oluşturulduktan sonra değerleri değiştirilemez. String sınıfı teknik olarak ilkel bir veri türü değildir, ancak dil tarafından kendisine verilen özel destek göz önüne alındığında, muhtemelen bunu böyle düşünme eğiliminde olacaksınız. String'in java heap memory alanında string pool adında özel bir alana sahip olduğu için onu başka blog yazılarında bahsedeceğim. Şimdilik böyle bir statüye sahip olduğunu bilmemiz yeterli olacaktır.

## İlkel Veri Türleri İçin Hafıza Modeli

Dilerseniz bir örnekle başlayalım.

```java
int deg1;
deg1 = 5;
int deg2;
deg2 = deg1;
deg1 = 12;  
System.out.println("deg1 = "+ deg1 + ", deg2 = "+ deg2);
```

Adım adım yukarıdaki kod bloğu nasıl çalışır, bunu resmetmeye çalışalım.

<!-- 1.variable declaration: Draw a box and label it with the variable's name -->

<figure style="width: 150px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-primitive-types/deg1.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

> Bu kutuyu, hafızada primitive type(ilkel tür) için açılan boşluğu yani alanı temsil ediyor gibi düşünebilirsiniz.

* **1.satır:** Bu ilk satır bir değişken tanımlama işlemidir. Aslında Java'ya, tamsayı(int) tipinde bir değeri saklamak için yeni bir alan istiyorum, diyorsunuz. Ve o alanı(space) deg1 etiketiyle etiketleyeceğinizi belirtiyorsunuz. Şimdi hafıza modelimizde bunu göz önünde canlandırabilmek için bir kutu çizeceğiz. Bu kutu değişkeni temsil etmem gereken alanı(space) işaret ediyor. Sonrasında bu değişkeni deg1 ismiyle etiketliyorum.

<!-- 2.variable assignment: put the value of the right hand side(RHS) into the box for the variable on the left hand side(LHS). -->

<figure style="width: 250px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-primitive-types/deg1-2.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **2.satır:** Kodun bir sonraki satırı ise atama komutudur(assignment statement). Burada da Java'ya şunu söylüyorsunuz. Java sağ tarafta bulunan değeri al(yani 5'i) ve sol tarafta bulunan değişkene(yani deg1 olana) yerleştir(yani atama yap). Hafıza modelimize geri dönecek olursak, 5 sayısını alıp kutunun içine yerleştiriyor gibi düşünebiliriz.

<figure style="width: 150px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-primitive-types/deg2.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **3.satır:** Bu satırda, birinci satırda olduğu gibi yeni bir değişken tanımlıyoruz. Bu değişkenin ismini de deg2 olarak etiketlediğimizi düşünelim. Deg1 için yaptıklarımızı aynen bunun içinde yapacağız. Bir kutu oluşturacak ve bu kutuyu deg2 ismiyle etiketleyeceğiz.
> Etiketlemekten kastım, aslında bir isim vereceğiz demek istiyorum. İngilizce label karşılığı etiketlemek olduğu için tercih ettim.

<figure style="width: 250px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-primitive-types/deg2-2.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **4.satır:** Bu satır bir başka atama komutunun olduğu satırdır. Bir önceki atama işlemlerinden biraz daha farklı olduğunu görebilirsiniz. Çünkü işlemin sağ tarafında bulunan değer bir rakam değil, başka bir değişkendir. Burada bizden istenen, deg1 değişkeninin sahip olduğu değerin aynısını deg2 değişkenine de atamaktır. Bu durumu, Deg1 kutusunun içindeki değerin aynısını deg2 kutusunun içine yapıştırarak  hafıza modelimizde resmedebiliriz. Resimden de görüleceği üzere deg1 ve deg2 değişkenleri aynı değeri saklamaya başladı. Dikkat edilecek olursa, bu iki değişken de tamamen farklı ve hiçbir şekilde birbirlerine bağlı değiller. Sadece geçici olarak aynı değeri saklıyorlar.

<figure style="width: 250px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-02-29-Java-memory-models-primitive-types/deg1-3.png" alt="async-variable">
  <figcaption></figcaption>
</figure>

* **5.satır:** Bu satırda bir başka atama komutu ile karşılaşıyoruz. Bu sefer deg1 değişkenine 12 değerini atamamız isteniyor.
Yani hafıza modelimizde düşünecek olursak: 12 değerini al ve deg1 değişkeninin olduğu kutuya yerleştir. Yani bu, eski değerimiz olan 5'in yerini şimdi 12 alacak demektir. Dikkat ederseniz değişen sadece deg1 değişkeni!! Deg2 değişkeni için yaptığımız bir işlem olmadı. Daha önce de belirttiğimiz üzere bu iki değişken arasında aynı değeri tutmalarının dışında bir benzerlik ve bağlantı yok. Tesadüfen aynı değeri tutan iki değişken gibi düşünülebilir.  

* **6.satır:** Şimdi ise son satırımız olan print komutunun olduğu yere geldik. Bu bölümde bizden istenen deg1 ve deg2 değerlerini ekrana yazdırmak olacaktır. Yani hafıza modelimize bakarak konuşacak olursak, komutun bize söylediği; her bir değişkenin sahip olduğu kutuların içindeki değerleri oku ve ekrana yazdır!!! gibi düşünülebilir. Haliyle çıktımız aşağıdaki gibi olacaktır.

```java
deg1 = 12 , deg2 = 5
```

## Sözlük

* **statically-typed:** değişken tiplerinin açıkça bildirildiği ve derleme zamanında belirlendiği bir programlama dili karakteristiğidir. Bu, derleyicinin belirli bir değişkenin kendisinden istenen eylemleri gerçekleştirip gerçekleştiremeyeceğine karar vermesini sağlar.


## Referanslar

* Bütün şekiller [https://www.lucidchart.com](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [https://www.w3schools.com/java/java_data_types.asp](https://www.w3schools.com/java/java_data_types.asp)
* [https://www.baeldung.com/java-initialization](https://www.baeldung.com/java-initialization)
* [https://www.baeldung.com/java-primitives](https://www.baeldung.com/java-primitives)
