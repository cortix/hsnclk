---
title: "Java'da Hafıza Modeli 1 - İlkel Veri Tipleri(Primitive Types)"
comments: false
excerpt: "Java'da İlkel Veri Tipleri Bellekte Nasıl Saklanır? Bu durumun net anlaşılması için nasıl simüle edebiliriz? String interning nedir?"
header:
  teaser: "/assets/images/svg-book9.svg"
  og_image: /assets/images/svg-book9.svg
  overlay_image: /assets/images/svg-book9.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
#  video:
#    id: jT06ibYdEXo
#    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Java İlkel Tipler İçin Hafıza Modeli
  - Java string interning
#last_modified_at: 2022-12-29T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java'da Hafıza Modeli Serisi</h4>
---

1. **Java'da Hafıza Modeli 1 - İlkel Veri Tipleri(Primitive Types)**
2. Java’da Hafıza Modeli 2 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects/)
3. Java’da Hafıza Modeli 3 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects1/)
4. Java’da Hafıza Modeli 4 - [Kapsam(Scope)](/java-hafiza-yonetimi/Java-memory-models-scope/)
5. Java’da Hafıza Modeli 5 - [Pass By Value / Pass By Reference](/java-hafiza-yonetimi/Java-memory-models-pass-by-value-reference/)
6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
</div>

## Genel Bakış

Java programlama dili statik olarak yazılmıştır(statically-typed), yani tüm değişkenlerin kullanılmadan önce bildirilmesi gerekir. Bu da, değişkenin türünü ve adını belirtmeyi içerir.

```java
int var = 1;
//Bir değişkenin veri türü içerebileceği değerleri ve üzerinde
//yapılabilecek işlemleri belirler. int'ye ek olarak, Java
//programlama dili diğer yedi ilkel veri türünü de destekler.
//Bunları aşağıda görebilirsiniz.
```
<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> İlkel Türler ve Referans Türleri</h4>
---
Veri türleri iki gruba ayrılır:
* İlkel veri türleri
* İlkel olmayan veri türleri String, Arrays(diziler) ve Classes(Sınıflar)
</div>

Örnek kod, programınıza "**var**" adlı bir alanın var olduğunu, sayısal verileri tuttuğunu ve başlangıç ​​değeri "**1**" olduğunu söyler. Bir alan, **ilkel** veya **referans türünde(yani ilkel olmayanlar)** olabilir. Sekiz ilkel tip vardır: **boolean** , **byte** , **char** , **short** , **long** , **float** ve **double**'dır. Bir referans türü, arabirimler(**interfaces**), diziler(**arrays**) ve **enumerated** türler dahil olmak üzere, doğrudan veya dolaylı bir `java.lang.Object` alt sınıfı olan herhangi bir şey olabilir.

Java'da tanımlanan bu sekiz ilkel veri tipi nesne olarak kabul edilmez ve doğrudan yığın(**stack**) üzerinde depolanırlar. Fakat nesneler, ilkel veri tiplerinin aksine daha **kompleks veri tipleri** olduğundan **heap** adı verilen özel bir alanda depolanırlar. <u>Her ne kadar farklı hafıza birimleri olsa da heap de stack de bilgisayarın <b>RAM</b>'inde yer alır</u>. Heap ve stack dışında da JVM tarafından ayrılan bölümler vardır. Yalnız bu makalenin konusu sadece bu ikisini içerdiği için diğerlerine girmeyeceğim.

Bu yazıdaki asıl amacım, java'da ilkel(primitive) tür ve ilkel olmayan türler olan nesne ve dizilerin hafızada nasıl oluştuğunu resmederek göstermeye çalışmaktır.

İlkel tür, dil tarafından önceden tanımlanır ve **ayrılmış(reserved keyword**) bir anahtar sözcükle adlandırılır. (Yani **int**, **double**, **short** gibi...) İlkel değerler sahip olduğu durumu diğer ilkel değerlerle paylaşmaz.

## String interning

Yukarıda listelenen **8 ilkel veri** türüne ek olarak, Java programlama dili de `java.lang.String` sınıfı aracılığıyla **karakter dizileri** için özel destek sağlar. <u>Karakter dizginizi çift tırnak içine almak otomatik olarak yeni bir String nesnesi oluşturur</u>; örneğin, `String s = "bu bir dizedir";`. <u>Dize nesneleri değiştirilemez, yani oluşturulduktan sonra değerleri değiştirilemez</u>. String sınıfı teknik olarak ilkel bir veri türü değildir, ancak dil tarafından kendisine verilen özel destek göz önüne alındığında, muhtemelen bunu böyle düşünme eğiliminde olacaksınız. *String* objelerinin **çift tırnak** içerisinde gösterilme şekli sayesinde gereksiz string kalabalığından da kurtulmuş oluruz.

Örneğin "`hello`" değerinde bir string oluşturulduktan sonra, <u>tekrar benzer değeri tutan bir string objesini <b>çift tırnak("")</b> kullanarak oluşturmak istesek</u>, java bu objenin daha önceden oluştuğunu bildiği ve alandan tasarruf sağlamak istediği için ilk oluşturulan "`hello`" objesini kullanır. Bu işleme "**string interning**" denir. <u>Yalnız bu durum <b>new</b> anahtar kelimesini kullanarak oluşturulan String objeleri için geçerli değildir.</u> `new` anahtar kelimesini kullanarak oluşturulan String objeleri heap hafıza alanında her defasında yeni obje oluşturmaya zorlar. Bu da basit text içeren string objeleri için ekonomik sayılmaz. String'in bu iki türlü kullanımından ötürü string objeleri primitive muamelesi görür. Ama özünde değildir. Bu arada String'in java heap memory alanında **string pool** adında özel bir alana sahip olduğu için onu başka blog yazılarında bahsedeceğim. Şimdilik böyle bir statüye sahip olduğunu bilmemiz yeterli olacaktır.

Aşağıdaki şekilde **string interning** olayının nasıl gerçekleştiğini görebilirsiniz. **a1** ve **a2** referansları, string çift tırnak kullanılarak oluşturulduğu için aynı objeyi işaret ederken, **new** anahtar kelimesi ile oluşturulan **a3** referansı ise farklı bir string objesini işaret etmektedir.

```java
String a1 = "hello";
String a2 = "hello";
String a3 = new String("hello");
```

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/string-interning.png --alt Java string interning --img width="100%" height="100%" %}<br/>


Arkadaşlar dilerseniz **string interning** ile alakalı hazırladığım bu [videoya](https://www.youtube.com/watch?v=jT06ibYdEXo) da göz gezdirebilirsiniz. Bu videoda **string interning**'in yanı sıra bu konuyla bağlantılı olduğunu düşündüğüm **2 konuyu** daha ele aldım. Bakmanızı öneririm.

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

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/deg1.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

> Bu kutuyu, hafızada primitive type(ilkel tür) için açılan boşluğu, yani alanı temsil ediyor gibi düşünebilirsiniz.

* **1.satır:** Bu ilk satır bir değişken tanımlama işlemidir. Aslında Java'ya, tamsayı(int) tipinde bir değeri saklamak için yeni bir alan istiyorum, diyorsunuz. Ve o alanı(space) **deg1** etiketiyle etiketleyeceğinizi belirtiyorsunuz. Şimdi hafıza modelimizde bunu göz önünde canlandırabilmek için bir kutu çizeceğiz. Bu kutu değişkeni temsil etmem gereken alanı(space) işaret ediyor. Sonrasında bu değişkeni **deg1** ismiyle etiketliyorum.

<!-- 2.variable assignment: put the value of the right hand side(RHS) into the box for the variable on the left hand side(LHS). -->

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/deg1-2.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

* **2.satır:** Kodun bir sonraki satırı ise atama komutudur(*assignment statement*). Burada da Java'ya şunu söylüyorsunuz. Java sağ tarafta bulunan değeri al(yani 5'i) ve sol tarafta bulunan değişkene(yani **deg1** olana) yerleştir(yani atama yap). Hafıza modelimize geri dönecek olursak, 5 sayısını alıp kutunun içine yerleştiriyor gibi düşünebiliriz.

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/deg2.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

* **3.satır:** Bu satırda, birinci satırda olduğu gibi yeni bir değişken tanımlıyoruz. Bu değişkenin ismini de **deg2** olarak etiketlediğimizi düşünelim. **deg1** için yaptıklarımızı aynen bunun içinde yapacağız. Bir kutu oluşturacak ve bu kutuyu **deg2** ismiyle etiketleyeceğiz.
> Etiketlemekten kastım, aslında bir isim vereceğiz demek istiyorum. İngilizce label karşılığı etiketlemek olduğu için tercih ettim.

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/deg2-2.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

* **4.satır:** Bu satır bir başka atama komutunun olduğu satırdır. Bir önceki atama işlemlerinden biraz daha farklı olduğunu görebilirsiniz. Çünkü işlemin sağ tarafında bulunan değer bir rakam değil, başka bir değişkendir. Burada bizden istenen, **deg1** değişkeninin sahip olduğu değerin aynısını **deg2** değişkenine de atamaktır. Bu durumu, **deg1** kutusunun içindeki değerin aynısını **deg2** kutusunun içine yapıştırarak  hafıza modelimizde resmedebiliriz. Resimden de görüleceği üzere **deg1** ve **deg2** değişkenleri aynı değeri saklamaya başladı. Dikkat edilecek olursa, bu iki değişken de tamamen farklı ve hiçbir şekilde birbirlerine bağlı değiller. Sadece geçici olarak aynı değeri saklıyorlar.

<br/>{% picture 2020-02-29-Java-memory-models-primitive-types/deg1-3.png --alt Java variable assignment(java değişken atama) --img width="100%" height="100%" %}<br/>

* **5.satır:** Bu satırda bir başka atama komutu ile karşılaşıyoruz. Bu sefer **deg1** değişkenine 12 değerini atamamız isteniyor.
Yani hafıza modelimizde düşünecek olursak: 12 değerini al ve **deg1** değişkeninin olduğu kutuya yerleştir. Yani bu, eski değerimiz olan 5'in yerini şimdi 12 alacak demektir. Dikkat ederseniz değişen sadece **deg1** değişkeni!! **deg2** değişkeni için yaptığımız bir işlem olmadı. Daha önce de belirttiğimiz üzere, bu iki değişken arasında aynı değeri tutmalarının dışında bir benzerlik ve bağlantı yok. Tesadüfen aynı değeri tutan iki değişken gibi düşünülebilir.  

* **6.satır:** Şimdi ise son satırımız olan `print` komutunun olduğu yere geldik. Bu bölümde bizden istenen **deg1** ve **deg2** değerlerini ekrana yazdırmak olacaktır. Yani hafıza modelimize bakarak konuşacak olursak, komutun bize söylediği; her bir değişkenin sahip olduğu kutuların içindeki değerleri oku ve ekrana yazdır!!! gibi düşünülebilir. Haliyle çıktımız aşağıdaki gibi olacaktır.

```java
deg1 = 12 , deg2 = 5
```

## Sözlük

* **statically-typed:** değişken tiplerinin açıkça bildirildiği ve derleme zamanında belirlendiği bir programlama dili karakteristiğidir. Bu, derleyicinin belirli bir değişkenin kendisinden istenen eylemleri gerçekleştirip gerçekleştiremeyeceğine karar vermesini sağlar.


## Referanslar

* Bütün şekiller [lucidchart](https://www.lucidchart.com)'da tarafımdan hazırlanmıştır.
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Java Data Types](https://www.w3schools.com/java/java_data_types.asp)
* [A Guide to Creating Objects in Java](https://www.baeldung.com/java-initialization)
* [Introduction to Java Primitives](https://www.baeldung.com/java-primitives)
