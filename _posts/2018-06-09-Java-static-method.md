---
title: "Java Static Metodu, Değişkenler ve Parametreler"
comments: false
excerpt: "Java Static Metodu Nedir? Hangi Durumlarda Kullanılır? Java'da Değişkenler ve Parametreler"
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-58.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [zero take](https://unsplash.com/photos/jSB9PWaxhXo) on Unsplash"
  #video:
    #id: cR9uwtMQt-g
    #provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java
tags:
  - java class
  - static metot
  - değişkenler
  - parametreler
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Merhabalar arkadaşlar, blog yazımı okumadan önce aşağıdaki youtube [videomu](https://www.youtube.com/watch?v=cR9uwtMQt-g) izlemenizi öneririm. Bu konuda naçizane öğrendiklerimi sizlerle paylaşmak için detaylı bir video hazırlamaya karar verdim. Umarım kafanızdaki sorulara cevap verebilirim. Şimdiden iyi seyirler.


<a href="https://www.youtube.com/watch?v=cR9uwtMQt-g"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-09-Java-static-method/java-statik-nedir.webp"   width="100%" height="100%"  loading="lazy" alt="Java Memory Management"></a>


## Konu ile alakalı bir bilgilendirme

Arkadaşlar, bu konu her ne kadar spesifik bir konu gibi gözükse de bir bağlam içerisinde konuyu ele almak çok daha önemlidir. Yani, **statik nedir**? sorusunu daha geniş bağlamda düşünmemiz gerekiyor. Bunun için **statik olmayan** <u>değişken ve metotlar nedir</u>? **Statik** ve **statik olmayan** <u> değişkenler hafızada nasıl saklanır</u>? gibi farklı soruları da beraberinde düşünmemiz elzemdir. Bunun için aşağıdaki içeriklerime sırasıyla bakmanızı öneriyorum.

* **Java’da Hafıza Modeli** [serisi](/categories/#java-hafiza-yonetimi)
* **Java’da Kalıtım ve Java’da Polimorfizm** [serisi](/categories/#java-kalitim-polimorfizm)


## Java'da Statik nedir?

**Statik** anahtar kelimesine sahip  bir değişken veyahut metot, aslında **sınıfa ait olan**, sınıftan yaratılan **objelere ait olmayan** anlamına gelir. Hatta **statik konteksi** sınıfın hafızası gibi düşünebilirsiniz. Bu **konteks** sınıftan yaratılan bütün objelerle ortak olarak paylaşılır. Haliyle sınıftan yaratılan objeler bu hafızaya doğrudan erişebilirken, **statik konteksten(yani sınıfın bu hafızasından)** ilgili objelere <u>doğrudan erişim yoktur</u>. Bu durumu yukarıdaki videoda çok net bir şekilde izah ettiğimi düşünüyorum. Dilerseniz oradan bakabilirsiniz.

Birçok yerde ``static`` değiştiricisiyle karşılaşmışızdır. Hatta bu anahtar kelimeyi hem metodlar için hem de değişkenler için kullanıyoruz.

* Peki bu ne anlama geliyor?
* Hangi durumlarda **static** anahtar kelimesini kullanmamız gerekir?
* JVM **static** yöntem ve değişkenleri nasıl ve nerede saklar?
* **Non-static method '' cannot be referenced from a static context** hatasını neden alıyoruz? (Cevabı videonun içinde)

## Java'da Değişkenler


### Java Örnek Değişkenleri

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Örnek Değişkenlerinin Diğer Adları</h4>
---

* Örnek Değişkenleri
* Dinamik Değişkenler
* Instance Variables (ingilizce)
* Anlık değişkenler
* Statik Olmayan Alanlar

</div>

Aynı <u>sınıf şablonundan bir dizi nesne oluşturulduğunda</u>, bunların her biri, ayrı ayrı kendi dinamik değişkenlerine sahiptir. Teknik olarak, nesneler, kendi durumlarını(yani ingilice **state** olarak ifade edilir) "**statik olmayan alanlara**", yani ``statik`` anahtar kelime olmadan beyan edilen alanlara depolar. Statik olmayan alanlar da **Instance Variables/dinamik değişkenler** olarak bilinir. Çünkü tuttuğu değerler bir sınıfın her örneğine, bir başka deyişle her nesneye özgüdür;



<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Sınıf Şablonu</h4>
---
**Note :** Yukarıda **sınıf şablonu(blueprint)** derken kastettiğim aslında aşağıdaki gibi sıradan bir sınıf kodudur:

Her sınıf kodu, sınıftan oluşturulacak objeler için bir taslak gibidir.

```java
public class SampleClass{
  int x;
  double y;
  static int z;
  ...
}
```

Aşağıdaki resimden de anlaşılacağı üzere sınıf şablonundan yaratılan 3 obje içinde **statik olmayan alanlar** tutulur. Dikkat edecek olursanız her obje farklı **x** ve **y** değeri saklamaktadır. Bu farklı değişkenlere **objelerin state**'leri de denir. Objeden objeye değişiklik gösterdiği için **dinamik değişken** olarak da ifade edilir. Bu arada objeler yeşil renkle gösterilmektedir.

</div>

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-09-Java-static-method/java-memory-management.webp"   width="100%" height="100%"  loading="lazy" alt="Java Memory Management">

### Java Sınıf Değişkenleri

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Sınıf Değişkenlerinin Diğer Adları</h4>
---

* Sınıf Değişkenleri
* Class Variables (ingilizce)
* Statik Alanlar
* Sınıfın hafızası (çünkü ilgili sınıftan yaratılan bütün objeler için ortak bir veritabanı gibi davranır.)

**Statik alan** ise yukarıdaki resimde pembe renkle gösterilmektedir. Görüleceği üzere, sınıf şablonu içindeki **statik değişkenleri** içinde barındırmaktadır. Her obje için ortak oldukları için ve her objenin bu alana erişimi olduğu için **statik alan** olarak da ifade edilir. Yani sınıfın hafızasıdır.
</div>


Bazen, tüm nesneler için ortak olan değişkenlere sahip olmak istersiniz. Bu **statik** anahtar kelimesi ile gerçekleştirilir. Beyanında **statik** değiştiriciye sahip olan değişkenlere **statik alanlar(*static fields*)** veya **sınıf değişkenleri(*class variables*)** denir. Herhangi bir nesneden ziyade sınıfla ilişkilendirilirler. Sınıfın her örneği(yani objeler), bellekte sabit bir konumda bulunan bir **sınıf değişkenini** paylaşır.

Herhangi bir nesne, bir sınıf değişkeninin değerini değiştirebilir, ancak statik alan içinden sınıfın bir örneğine erişim mümkün değildir.

Buna ek olarak **final** anahtar kelimesinin eklenmesi değişkenin değerinin değişmeyeceği anlamına gelir.


### Yerel Değişkenler (Local Variables)

Bir nesne kendi durumunu **alanlarda(fields)** sakladığı gibi, bir yöntem de geçici durumunu **yerel değişkenlerde** saklar. Yerel bir değişkenin deklarasyonu, bir örnek değişkeninin deklarasyonuna benzerdir (örneğin, `int num = 0;`). Yerel olarak bir değişken belirten özel bir anahtar kelime yoktur; <u>bu belirleme tamamen değişkenin beyan edildiği yerden yani bir yöntemin açılış ve kapanış parantezleri arasında gelir</u>. Bu nedenle, yerel değişkenler yalnızca bildirildikleri yöntemlerle görülebilir; sınıfın geri kalanından erişilemezler. Yani sadece bir metodun içinde deklare edildiklerinden, onlara erişim de ilgili metod üzerinden olur.



```java
public class Deneme {
    public static void main(String[] args) {
        InnerClas c= new InnerClas();
        c.deneGor();
    }
    static class InnerClas {
        int x = 5;
         void deneGor() {
            int x = 7;
            System.out.println(x);
        }
    }
}
```

Yukarıdaki [örnekte](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=14&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ne demek istediğimi sanırım daha net anlatabilirim. Dikkat edilecek olursa **InnerClas**'ın içinde iki tane **x** değişkeni var.  Bunlardan biri `void` **deneGor()** metoduna ait olan **x** değişkeni(yani yerel değişken), diğeri ise bir **static olmayan** dinamik bir değişken(yani instance variable).

Ben burada `System.out.println(x)` yaparak **x**'i yazdırmak istiyorum.. Peki hangi **x** değişkenini yazdıracak dersiniz? Kod üzerinde **Back**, **Forward** butonlarını kullanarak kodun nasıl çalıştığını görebilirsiniz. Aslında burada, **deneGor** metodunun içindeki **x** yerel değişkenini ekrana basacaktır. Kısaca yaptığımız adımları gözden geçirelim.

- ilk önce main metod içinde InnerClas sınıfının bir nesnesini yarattık.
- Sonra bu nesneyi **c** değişkenine atadık.
- **c** değişkeni sayesinde InnerClas içindeki metod ve dinamik değişkenlere artık ulaşabiliriz.
- görüldüğü üzere **c** değişkeni *deneGor* metodunu çağırdı,
- akabinde *deneGor* metodu ise kendi içindeki yerel değişkeni(local variable) olan **x**'i ekrana bastı.

Eğer `System.out.println(x)` yerine `System.out.println(this.x)` yazmış olsaydık, **yerel değişken(local variable)** yerine ekrana **instance variable** olan **x**'i basacaktı. Java'da her zaman **this** anahtar sözcüğü instance variable'ı, yani nesnenin değişkenlerini niteler. Bu yüzden tanımları bilmekte yarar var. **this** anahtar sözcüğü kullanıldığında ise [sonuç](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28this.x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=15&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) aşağıdaki gibi olacaktır.

```java
public class Deneme {
    public static void main(String[] args) {
        InnerClas c= new InnerClas();
        c.deneGor();
    }
    static class InnerClas {
        int x = 5;
         void deneGor() {
            int x = 7;
            System.out.println(this.x);
        }
    }
}
```
<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Önemli Hatırlatma</h4>
---
Yerel değişkenler(local variables) ile ilgili bir diğer önemli nokta ise şudur. Yerel değişkenleri tanımlarken **erişim değiştiriciler(yani access modifiers)** kullanamayız(**final dışında**). Access Modifiers'ları hatırlayacak olursak;

* public
* protected
* default
* private
* final
</div>

#### Illegal start of expression derleme hatası

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-09-Java-static-method/illegal_start.webp"   width="100%" height="100%"  loading="lazy" alt="Illegal Start of expression for local variables">

Yukarıdaki görselde **yerel değişkene** bir **erişim değiştirici** uygulamaya çalıştım. Görüldüğü gibi "**illegal start of expression**" derleme hatası aldım. <u>Hatadan da anlaşılacağı üzere yerel değişkenlere erişim değiştiricileri(access modifiers) uygulanamaz.</u> Koda şu [linkten](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20private%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28this.x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ulaşabilirsiniz.


### Parametreler (Parameters)
Bir yönteme veya oluşturucuya(*constructor*) bilgi aktarmak için kullanılan argümanlar parametre olarak adlandırılır. Bir yöntem veya *constructor* için deklarasyon, bu yöntem veya *constructor* için bağımsız değişkenlerin sayısını ve türünü bildirir.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Parametre ve Argüman Arasındaki Fark</h4>
---

Aslında **parametre** ile **argüman** arasında da ufak bir **fark** vardır.

* **Parametre** yöntem deklerasyonunda kullanılan, yani yöntem imzasının bir parçası olan şeydir. Yani aşağıdaki **num** tipi int olan bir parametredir.

* **Argüman** ise bu metodu kullanmak istediğimizde yani metodu çağırdığımızda metodun içine aldığı değerdir. Örneğin bu metodun içine değer olarak **32** veya **44** int değerlerini aldığımızda bunlar aslında argüman olarak ifade edilir.

{% highlight java %}
public String methodA(int num) {
    // method body goes here
}
{% endhighlight %}
</div>

Yukarıdaki örnektede görüleceği üzere **methodA**'yı kullanabilmek için **int** tipinde bir sayıya ihtihacınız olduğu anlamına geliyor. Yani bu şartı sağlamadan bu metodu kullanamazsınız. Aksi halde derleme hatası ile karşılaşırsınız. Unutulmaması gereken en önemli şey, parametrelerin her zaman "alanlar(*fields*)" olarak değil "değişkenler(*variables*)" olarak sınıflandırılmasıdır.


## Static Anahtar Kelimesinin Ne Anlama Geldiğini Bir Örnekte İnceleyelim

Java ile yazılmış en basit kod içinde bile, isteğimiz dışında bir `static` ifadesini kullandığımız oluyor. Bunu devamlı kullandığımız main metodundan hatırlayabiliriz. ``main`` metodu ile ilgili detaylı bilgi için şu [linkteki](/java/Java-main-method/) yazımı okuyabilirsiniz.

{% highlight java %}
public static void main(String[] args) {
        //code
    }
{% endhighlight %}


Bu ifadeyi metod tanımlarken kullandığımızda, ilgili yöntemin genel anlamıyla bulunduğu sınıfa ait olduğu ve belirli başka bir örnekte olmadığı anlamına gelir. Bunun ne anlama geldiğini daha yakından görmek için, önce ilgili sınıfın her bir instance'ı için bir kopya olan statik olmayan(*non-static*) alanlara bakalım. Örnek olarak, bankacılık ile ilgili bir yazılım hazırladığınızı varsayalım. Bir banka hesabı için bir sınıf oluşturmaya ve alanları(*fields*) için, hesap bakiyesi ve hesap numarası bildirmeye karar verdiniz.

* non-static(instance) = her nesnede(in each object)

{% highlight java %}
class BankaHesabi{
  int hesapNum;
  double bakiye;
  ...
}
{% endhighlight %}

Her bir farklı banka hesabının kendi hesap numarası ve kendi bakiyesi olması gerekmektedir. Bankanızda, bu sınıfın üç örneğini, veri depolamada üç hesap için oluşturduysanız, böyle görünebilir. Burada her bir örneğin kendi hesap numarası ve bakiyesinin nasıl olduğunu görebiliriz. **Bu alanlar statik değildir ve her nesne için ayrı ayrı oluşur.**

| Kişiler | Hesap Numarası | Bakiye |
|:--------|:-------:|--------:|
| Görkem   | 100   | 12000   |
| Hasan   | 101   | 12500   |
| Bilal   | 102   | 0   
{: rules="groups"}

{% highlight java %}
class BankaHesabi{
  int hesapNum;
  double bakiye;
  int sonHesapNum;
}
{% endhighlight %}

Şimdi, bu kodu yazarken, bir değişkende atama yapmak için bir önceki hesap numarasını takip etmek istediğinizi varsayalım. Böylece yeni bir hesap oluşturduğunuzda, ona hangi numarayı vereceğinizi bileceksiniz. Eklediğimiz alan yukarıda gösterildiği gibi olursa, bu sonraki hesap numarası için oluşturduğumuz örnek beklediğimiz gibi olmayabilir. Aşağıdaki kodu görüntüleyemiyorsanız lütfen [linke](https://goo.gl/x4udgN) tıklayınız.

```java
public class Banka {
    public static void main(String[] args) {
      BankaHesabi a1 = new BankaHesabi();
      BankaHesabi a2 = new BankaHesabi();
      BankaHesabi a3 = new BankaHesabi();
    }

    static class BankaHesabi{
      int hesapNum;
      double bakiye;
      int sonHesapNum;

      public BankaHesabi(){
        hesapNum = sonHesapNum;
        sonHesapNum++;
      }
    }
}
```

Yukarıdaki kod bloğunda dikkat etmemiz gereken iki şey var. Birincisi yukarıda oluşturduğumuz *BankaHesabı* sınıfının önünde bulunan ``static`` anahtar kelimesidir. Burada sınıfı bu şekilde değiştirmemin sebebi hem static sınıfların kullanımını göstermek hemde static bir ifadenin bazı kurallarından bahsetmek olacaktır.

Mesela bir kural şudur. Yukarıdaki gibi bir örnekte, inner olarak oluşturduğumuz sınıfı non-static yapamayız. Aksi halde şu şekilde bir hata alabiliriz.

**Error: non-static variable this cannot be referenced from a static context**

Çünkü main metodu static bir metodtur. Static bir durumda static değişkenler kullanmak zorundayız. Bu Java'nın kurallarından sadece biridir. Tabiki amacımız bu kuralları derinlemesine işlemek değil. Static ifadesiyle alakalı olduğu için değinmek istedim. Örneğe geri dönecek olursak, oluşturulan her bir nesne için instance variable'lar hep sıfırdan başlamaktadır. Ama biz sonHesapNum değişkeninin bir önceki hesap numarasını takip etmek istediğini ve onu baz alarak bir atama yapmasını sağlayacaktık. O zaman sonHesapNum değişkeni önüne ``static`` anahtarını koyabiliriz.

```java
public class Banka {
    public static void main(String[] args) {
      BankaHesabi a1 = new BankaHesabi();
      BankaHesabi a2 = new BankaHesabi();
      BankaHesabi a3 = new BankaHesabi();
    }

    static class BankaHesabi{
      int hesapNum;
      double bakiye;
      static int sonHesapNum;

      public BankaHesabi(){
        hesapNum = sonHesapNum;
        sonHesapNum++;
      }
    }
}
```

Dikkat edilecek olursa **hesapNum** her yeni nesne için farklı bir rakam olarak gözükmektedir. *static* anahtarı ilgili değişkeni nesne değişkeni durumundan sınıf değişkeni durumuna geçirmiştir. Yani değişken nesneye bağlı değil, sınıfa bağlıdır. Ve ilgili sınıftan oluşturulan bütün nesneler için ortak bir değişken haline gelmiştir. Yukarıdaki [örnek](http://www.pythontutor.com/visualize.html#code=public%20class%20Banka%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20BankaHesabi%20a1%20%3D%20new%20BankaHesabi%28%29%3B%0A%20%20%20%20%20%20BankaHesabi%20a2%20%3D%20new%20BankaHesabi%28%29%3B%0A%20%20%20%20%20%20BankaHesabi%20a3%20%3D%20new%20BankaHesabi%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20%0A%20%20%20%20static%20class%20BankaHesabi%7B%0A%20%20%20%20%20%20int%20hesapNum%3B%0A%20%20%20%20%20%20double%20bakiye%3B%0A%20%20%20%20%20%20static%20int%20sonHesapNum%3B%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20public%20BankaHesabi%28%29%7B%0A%20%20%20%20%20%20%20%20hesapNum%20%3D%20sonHesapNum%3B%0A%20%20%20%20%20%20%20%20sonHesapNum%2B%2B%3B%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=26&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) üzerinden gidecek olursak, ilk nesneyi oluşturduğumuzda **sonHesapNum** değişkenini bir arttırmıştık. İkinci nesnemizi oluşturduğumuzda ise bu değişken bir önceki hali ile yani bir arttırılmış hali ile yeni nesneyi ilklendirir. Ne demek istediğimi daha iyi anlamak için yukarıdaki kodu *Back* ve *Forward* butonlarını kullanarak ilerletmeye çalışın. **sonHesapNum** değişkeninin her bir nesne için sabit(yani her nesne oluşumunda sıfırlanmadığını) kaldığını göreceksiniz.

## Statik Yöntem ve Değişkenler ile Statik Olmayan Değişken ve Yöntemlerin Arasındaki Farklılıklar

Bir Java sanal makinesindeki (JVM) bir sınıfın kullanım ömrü, bir nesnenin kullanım ömrü ile birçok benzerliğe sahiptir. Bir nesne, statik olmayan değişkenlerinin değerleri ile temsil edilen bir duruma sahip olabildiği gibi, bir sınıf, sınıf değişkenlerinin değerleri ile temsil edilen bir duruma sahip olabilir. JVM, başlatma kodunu çalıştırmadan önce **statik olmayan(*instance variables , non-static members*)** değişkenleri varsayılan başlangıç ​​değerlerine ayarladığı gibi, aynı JVM, başlatma kodunu çalıştırmadan önce sınıf değişkenlerini varsayılan başlangıç ​​değerlerine ayarlar. Ve nesneler gibi sınıflarda, çalışan uygulama tarafından uzun süre başvurulmuyorsa, toplanan çöp durumunda olabilirler.

Yine de, sınıflar ve nesneler arasında önemli farklılıklar vardır. Belki de en önemli fark, statik olmayan metodlar ile sınıf metodlarının çağrılma şeklidir: Statik olmayan yöntemler (çoğunlukla) **dinamik** olarak bağlıdır, ancak sınıf yöntemleri(yani static yöntemler) **statik** olarak bağlıdır. Üç özel durumda, statik olmayan yöntemler dinamik olarak bağlı değildir:

* statik olmayan ``private``(özel) yöntemlerinin çağrılması,
* kurucu yöntemlerinin çağrılması(constructor)
* ``super`` anahtar kelimesi ile çağrılma

Sınıflar ve nesneler arasındaki diğer bir fark, özel erişim seviyeleri tarafından verilen veri gizleme derecesidir. Bir statik olmayan değişken ``private``(özel) olarak bildirilirse, yalnızca statik olmayan yöntemleri ona erişebilir. Bu, statik olmayan verilerinin bütünlüğünü sağlamanıza ve nesnelerin iş parçacığı güvenliğini sağlamanıza olanak tanır. Programın geri kalanı, bu statik olmayan değişkenlerine doğrudan erişemez, ancak statik olmayan değişkenlerini işlemek için statik olmayan yöntemlerini gözden geçirmelidir. Bir sınıfın iyi tasarlanmış bir nesne gibi davranmasını sağlamak için, sınıf değişkenlerini ``private``(özel) yapabilir ve bunları manipüle eden sınıf yöntemlerini tanımlayabilirsiniz. Yine de, bu şekilde thread güvenliğini ve hatta veri bütünlüğünü garanti edemezsiniz, çünkü belirli bir kodun onlara ``private``(özel) sınıf değişkenlerine doğrudan erişim sağlayan özel bir ayrıcalığı vardır: statik olmayan yöntemler ve hatta statik olmayan değişkenlerin başlatıcıları(*initializers*) , doğrudan bu ``private``(özel) sınıf değişkenlerine erişebilir.

Böylece, statik alanlar ve sınıfların metotları, statik olmayan alanlara ve nesnelerin yöntemlerine pek çok açıdan benzer olsa da, bunları tasarımlarda kullanma şeklinizi etkileyecek önemli farklılıklara sahiptir.

## JVM static yöntem ve değişkenleri nasıl ve nerede saklar?

JDK 8'den önce HotSpot JVM, kalıcı Nesil(*Permanent  Generation*) olarak adlandırılan üçüncü bir nesle(*third generation*) sahipti. Statik yöntemler (aslında tüm yöntemler) ve bunun yanısıra statik değişkenler, sınıf meta verileri heap alanına bitişik(*contiguous*) olan ``Permgen`` adında bir alanda tutulurdu. Kısaca bahsetmem gerekirse, bu alanda sınıf yöntem ve değişkenlerinin yanısıra, sınıfların JVM iç gösterimi(*internal representation*) ve meta verileri ve *interned strings*'ler yer almaktaydı. JDK 8 den sonra bu generation'ının yerini ``metaspace`` almıştır. ``Permgen``den farklı olarak bu alan Java Heap ile bitişik değildir(*Not contiguous*). Metaspace "native memory" den ayrılmıştır ve ``metaspace`` için maksimum alan kullanılabilir sistem hafızasıdır. Fakat bu ``MaxMetaspaceSize`` JVM seçeneği ile sınırlandırılabilir. Yalnız öncesinde bu hafıza ``Permgen``de sınırlı idi. Özetle JDK 8 öncesi ve sonrası olmak üzere iki farklı cevabımız bulunmaktadır.

Daha detaylı bilgi almak isterseniz aşağıdaki referanslara göz gezdirebilirsiniz.

## Referanslar:

* [Passing Information to a Method or a Constructor](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html)
* [Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)
* [Understanding Class Members](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)
* [Java Programming: Principles of Software Design](https://www.coursera.org/learn/java-programming-design-principles/)
* [OCA/OCP Java SE 7 Programmer I & II Study Guide (Exams 1Z0-803 & 1Z0-804)](https://www.amazon.com/Programmer-Study-1Z0-803-1Z0-804-Certification/dp/0071772006)
* [Java Garbage Collection Basics](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
* [The Java® Virtual Machine Specification](https://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf)
* [Java Language and Virtual Machine Specifications](https://docs.oracle.com/javase/specs/)
* [Lets learn to talk to GC logs by Poonam Parhar](https://www.youtube.com/watch?v=YujkxK3RfS0)
* [About G1 Garbage Collector, Permanent Generation and Metaspace](https://blogs.oracle.com/poonam/about-g1-garbage-collector%2c-permanent-generation-and-metaspace/comment-submitted?cid=12f52320-ad77-4ead-b736-b88be15dc3d0)
* [Troubleshooting Memory Leaks](https://docs.oracle.com/javase/7/docs/webnotes/tsg/TSG-VM/html/memleaks.html)
* [Java Platform, Standard Edition Troubleshooting Guide](https://docs.oracle.com/javase/9/troubleshoot/JSTGD.pdf)
* [Java Platform, Standard Edition Troubleshooting Guide](https://docs.oracle.com/javase/10/troubleshoot/troubleshoot-memory-leaks.htm#JSTGD266)
* [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide Other Considerations](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/considerations.html)
* [On heap cache vs off-heap cache](https://www.slideshare.net/rgrebski/on-heap-cache-vs-offheap-cache-53098109)
* [What is a PermGen leak?](https://plumbr.io/blog/memory-leaks/what-is-a-permgen-leak)
* [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](https://docs.oracle.com/javase/9/gctuning/toc.htm)
* [Java Memory Management](https://dzone.com/articles/java-memory-management)
* [Java Virtual Machine Troubleshooting Course](https://apexapps.oracle.com/pls/apex/f?p=44785:50:113240267925965:::50:P50_COURSE_ID,P50_EVENT_ID:185,5777)
* [Java 8: From PermGen to Metaspace](https://dzone.com/articles/java-8-permgen-metaspace)
* [Permgen vs Metaspace in Java](https://www.baeldung.com/java-permgen-metaspace)
