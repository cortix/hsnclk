---
title: "Java Static Metodu, Değişkenler ve Parametreler"
comments: true
excerpt: "Java Static Metodu Nedir? Hangi Durumlarda Kullanılır? Değişkenler ve Parametreler"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-42.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Peter Oslanec](https://unsplash.com/photos/AsTuH7M7ImY) on Unsplash"
  video:
    id: cR9uwtMQt-g
    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java
tags:
  - static metot
  - değişkenler
  - parametreler
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Merhabalar arkadaşlar, blog yazımı okumadan önce aşağıdaki videoyu izlemenizi öneririm. Bu konuda yeni yazılımcıların çok sıkıntı yaşadığını bildiğim için detaylı bir eğitim videosu hazırlamaya karar verdim. Umarım kafanızdaki sorulara cevap verebilirim. Şimdiden iyi seyirler.

{% include video id="cR9uwtMQt-g" provider="youtube" %}

## Konu ile alakalı bir bilgilendirme

Arkadaşlar, bu konu her ne kadar spesifik bir konu gibi gözükse de bir bağlam içerisinde konuyu ele almak çok daha önemlidir. Demek istediğim aslında statik nedir? sorusunu daha geniş bağlamda düşünmek gerekiyor. Bunun için statik olmayanlar değişken ve metotlar nedir? Statik ve statik olmayanlar hafızada nasıl saklanıyor gibi farklı soruları da beraberinde düşünmek gerekiyor. Bunun için aşağıdaki kategorik içeriklerime sırasıyla bakmanızı şiddetle öneriyorum.

* **Java’da Hafıza Modeli** [serisi](/categories/#java-hafiza-yonetimi)
* **Java’da Kalıtım ve Java’da Polimorfizm** [serisi](/categories/#java-kalitim-polimorfizm)

Çünkü bağlamdan kopuk bir ilerle zayıf bir temel üzerine güçlü yapılar inşaa etmenize izin vermez. Bu sebepten ötürü üniversitelerde müfredatları belli bir sırayla almanız tavsiye edilir. Umarım yazılarımın bir faydası olur. Şimdiden iyi okumalar.

## Statik nedir?

**Statik** anahtar kelimesine sahip  bir değişken veyahut metot, aslında **sınıfa ait olan**, sınıftan yaratılan **objelere ait olmayan** anlamına gelir. Hatta **statik konteksi** sınıfın hafızası gibi düşünebilirsiniz. Bu konteks sınıftan yaratılan bütün objelerle ortak olarak paylaşılır. Haliyle sınıftan yaratılan objeler bu hafızaya doğrudan erişebilirken, statik konteksten(yani sınıfın bu hafızasından) ilgili objelere doğrudan erişim yoktur. Bu durumu yukarıdaki videoda çok net bir şekilde izah ettiğimi düşünüyorum. Dilerseniz oradan bakabilirsiniz.

Birçok yerde ``static`` değiştiricisiyle karşılaşmışızdır. Hatta bu anahtar kelimeyi hem metodlar için hem de değişkenler için kullanıyoruz.

* Peki bu ne anlama geliyor?
* Hangi durumlarda static anahtar kelimesini kullanmamız gerekir?
* JVM static yöntem ve değişkenleri nasıl ve nerede saklar?
* **Non-static method '' cannot be referenced from a static context** hatasını neden alıyoruz? (Cevabı videonun içinde)

## Değişkenler


**Dinamik Değişkenler-Instance Variables (Anlık değişkenler/Statik Olmayan Alanlar):** Aynı sınıf şablonundan bir dizi nesne oluşturulduğunda, bunların her biri, ayrı ayrı kendi dinamik değişkenlerine sahiptir. Teknik olarak, nesneler, kendi durumlarını "statik olmayan alanlara", yani ``statik`` anahtar kelime olmadan beyan edilen alanlara depolar. Statik olmayan alanlar da ``Instance Variables/dinamik değişkenler`` olarak bilinir. Çünkü tuttuğu değerler bir sınıfın her örneğine, bir başka deyişle her nesneye özgüdür;

**Sınıf Değişkenleri-Class Variables (Statik Alanlar):** Bazen, tüm nesneler için ortak olan değişkenlere sahip olmak istersiniz. Bu statik anahtar kelimesi ile gerçekleştirilir. Beyanında ``statik`` değiştiriciye sahip olan alanlara statik alanlar(*static fields*) veya sınıf değişkenleri(*class variables*) denir. Herhangi bir nesneden ziyade sınıfla ilişkilendirilirler. Sınıfın her örneği, bellekte sabit bir konumda bulunan bir sınıf değişkenini paylaşır. Herhangi bir nesne, bir sınıf değişkeninin değerini değiştirebilir, ancak sınıf değişkenleri, sınıfın bir örneğini oluşturmadan da manipüle edilebilir. Bu, derleyiciye, sınıfın kaç kez örneklendirildiğine bakılmaksızın, bu değişkenin tam olarak varlığını sürdüren bir kopyası olduğunu söyler. Buna ek olarak ``final`` anahtar kelimesinin eklenmesi değişkenin değerinin değişmeyeceği anlamına gelir.

{% highlight java %}
static int num = 6;
{% endhighlight %}

Yukarıdaki ``static`` field örneğinde num adında bir değişken tanımlanmış ve 6 değeri bu değişkene atanmıştır. Bu static değere sahip sınıf üzerinden bu değere tekrar erişilmek istendiğinde bu değer tekrar alınır. Buna ek olarak bu değer istendiğinde değiştirilebilir. Aşağıdaki örnekte bunu daha detaylı anlatacağım. Bu ifadeye birde ``final`` anahtar kelimesi eklenirse bu ifade artık bellekte sabitlenir ve değişmez. Yani tek bir değerle ilklendirilebilir. ``final`` örneğine en güzel örnek pi sayısı olabilir. Bilindiği üzere pi sayısı sabittir ve değişmez. Kısacası ``static`` değiştiricisi, ``final`` değiştirici ile birlikte, sabitleri tanımlamak için de kullanılır.

{% highlight java %}
static final double PI = 3.141592653589793;
{% endhighlight %}

**Yerel Değişkenler - Local Variables :**
Bir nesne kendi durumunu *fields* larda nasıl sakladığı gibi, bir yöntem geçici durumunu yerel değişkenlerde saklar. Yerel bir değişkenin deklarasyon syntax'ı, bir field'ın deklarasyonuna benzerdir (örneğin, int num = 0;). Yerel olarak bir değişken belirten özel bir anahtar kelime yoktur; bu belirleme tamamen değişkenin beyan edildiği yerden yani bir yöntemin açılış ve kapanış parantezleri arasında gelir. Bu nedenle, yerel değişkenler yalnızca bildirildikleri yöntemlerle görülebilir; sınıfın geri kalanından erişilemezler. Yani sadece bir metodun içinde deklare edildiklerinden, onlara erişim de ilgili metod üzerinden olur.   

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

Yukarıdaki [örnekte](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=14&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ne demek istediğimi sanırım daha net anlatabilirim. Dikkat edilecek olursa **InnerClas**'ın içinde iki tane **x** değişkeni var.  Bunlardan biri void *deneGor()* metoduna ait olan **x** değişkeni(yani yerel değişken), diğeri ise bir static olmayan dinamik bir değişken(yani instance variable). Ben burada *System.out.println(x)* yaparak x'i yazdırmak istiyorum.. Peki hangi **x** değişkenini yazdıracak dersiniz? Kod üzerinde *Back*, *Forward* butonlarını kullanarak kodun nasıl çalıştığını görebilirsiniz. Aslında burada, *deneGor* metodunun içindeki **x** yerel değişkenini ekrana basacaktır. Kısaca yaptığımız adımları gözden geçirelim.

- ilk önce main metod içinde InnerClas sınıfının bir nesnesini yarattık.
- Sonra bu nesneyi **c** değişkenine atadık.
- **c** değişkeni sayesinde InnerClas içindeki metod ve dinamik değişkenlere artık ulaşabiliriz.
- görüldüğü üzere **c** değişkeni *deneGor* metodunu çağırdı,
- akabinde *deneGor* metodu ise kendi içindeki yerel değişkeni(local variable) olan **x**'i ekrana bastı.

Eğer *System.out.println(x)* yerine *System.out.println(this.x)* yazmış olsaydık, yerel değişken(local variable) yerine ekrana instance variable olan x'i basacaktı. Java'da her zaman **this** anahtar sözcüğü instance variable'ı, yani nesnenin değişkenlerini niteler. Bu yüzden tanımları bilmekte yarar var. *this* anahtar sözcüğü kullanıldığında ise [sonuç](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28this.x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=15&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) aşağıdaki gibi olacaktır.

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

Yerel değişkenler(local variables) ile ilgili bir diğer önemli nokta ise şudur. Yerel değişkenleri tanımlarken erişim değiştiriciler(yani access modifiers) kullanamayız(final dışında). Access Modifiers'ları hatırlayacak olursak;

■ public
■ protected
■ default
■ private
■ final

<figure >
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-09-Java-static-method/illegal_start.png" alt="Illegal Start">
  <figcaption>Görsel Jing'de hazırlanmıştır.</figcaption>
</figure>

Yukarıdaki görselde yerek değişkene bir erişim değiştirici uygulamaya çalıştım. Görüldüğü gibi derleme hatası aldım. Koda şu [linkten](http://www.pythontutor.com/visualize.html#code=public%20class%20Deneme%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20InnerClas%20c%3D%20new%20InnerClas%28%29%3B%0A%20%20%20%20%20%20%20%20c.deneGor%28%29%3B%0A%20%20%20%20%7D%0A%20%20%20%20static%20class%20InnerClas%20%7B%0A%20%20%20%20%20%20%20%20int%20x%20%3D%205%3B%0A%20%20%20%20%20%20%20%20%20void%20deneGor%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20private%20int%20x%20%3D%207%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28this.x%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) ulaşabilirsiniz.


**Parametreler - Parameters :**
Bir yönteme veya oluşturucuya(*constructor*) bilgi aktarmak için kullanılan argümanlar parametre olarak adlandırılır. Bir yöntem veya *constructor* için deklarasyon, bu yöntem veya *constructor* için bağımsız değişkenlerin sayısını ve türünü bildirir.

{% highlight java %}
public String methodA(int num) {
    // method body goes here
}
{% endhighlight %}

Yukarıdaki örnektede görüleceği üzere methodA'yı kullanabilmek için int tipinde bir sayıya ihtihacınız olduğu anlamına geliyor. Yani bu şartı sağlamadan bu metodu kullanamazsınız. Aksi halde derleme hatası ile karşılaşırsınız. Unutulmaması gereken en önemli şey, parametrelerin her zaman "alanlar(*fields*)" olarak değil "değişkenler(*variables*)" olarak sınıflandırılmasıdır.


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

Bir Java sanal makinesindeki (JVM) bir sınıfın kullanım ömrü, bir nesnenin kullanım ömrü ile birçok benzerliğe sahiptir. Bir nesne, statik olmayan değişkenlerinin değerleri ile temsil edilen bir duruma sahip olabildiği gibi, bir sınıf, sınıf değişkenlerinin değerleri ile temsil edilen bir duruma sahip olabilir. JVM, başlatma kodunu çalıştırmadan önce statik olmayan(*instance variables , non-static members*) değişkenleri varsayılan başlangıç ​​değerlerine ayarladığı gibi, aynı JVM, başlatma kodunu çalıştırmadan önce sınıf değişkenlerini varsayılan başlangıç ​​değerlerine ayarlar. Ve nesneler gibi sınıflarda, çalışan uygulama tarafından uzun süre başvurulmuyorsa, toplanan çöp durumunda olabilirler. Yine de, sınıflar ve nesneler arasında önemli farklılıklar vardır. Belki de en önemli fark, statik olmayan metodlar ile sınıf metodlarının çağrılma şeklidir: Statik olmayan yöntemler (çoğunlukla) dinamik olarak bağlıdır, ancak sınıf yöntemleri(yani static yöntemler) statik olarak bağlıdır. Üç özel durumda, statik olmayan yöntemler dinamik olarak bağlı değildir:

* statik olmayan ``private``(özel) yöntemlerinin çağrılması,
* kurucu yöntemlerinin çağrılması(constructor)
* ``super`` anahtar kelimesi ile çağrılma

Sınıflar ve nesneler arasındaki diğer bir fark, özel erişim seviyeleri tarafından verilen veri gizleme derecesidir. Bir statik olmayan değişken ``private``(özel) olarak bildirilirse, yalnızca statik olmayan yöntemleri ona erişebilir. Bu, statik olmayan verilerinin bütünlüğünü sağlamanıza ve nesnelerin iş parçacığı güvenliğini sağlamanıza olanak tanır. Programın geri kalanı, bu statik olmayan değişkenlerine doğrudan erişemez, ancak statik olmayan değişkenlerini işlemek için statik olmayan yöntemlerini gözden geçirmelidir. Bir sınıfın iyi tasarlanmış bir nesne gibi davranmasını sağlamak için, sınıf değişkenlerini ``private``(özel) yapabilir ve bunları manipüle eden sınıf yöntemlerini tanımlayabilirsiniz. Yine de, bu şekilde thread güvenliğini ve hatta veri bütünlüğünü garanti edemezsiniz, çünkü belirli bir kodun onlara ``private``(özel) sınıf değişkenlerine doğrudan erişim sağlayan özel bir ayrıcalığı vardır: statik olmayan yöntemler ve hatta statik olmayan değişkenlerin başlatıcıları(*initializers*) , doğrudan bu ``private``(özel) sınıf değişkenlerine erişebilir.

Böylece, statik alanlar ve sınıfların metotları, statik olmayan alanlara ve nesnelerin yöntemlerine pek çok açıdan benzer olsa da, bunları tasarımlarda kullanma şeklinizi etkileyecek önemli farklılıklara sahiptir.

## JVM static yöntem ve değişkenleri nasıl ve nerede saklar?

JDK 8'den önce HotSpot JVM, kalıcı Nesil(*Permanent  Generation*) olarak adlandırılan üçüncü bir nesle(*third generation*) sahipti. Statik yöntemler (aslında tüm yöntemler) ve bunun yanısıra statik değişkenler, sınıf meta verileri heap alanına bitişik(*contiguous*) olan ``Permgen`` adında bir alanda tutulurdu. Kısaca bahsetmem gerekirse, bu alanda sınıf yöntem ve değişkenlerinin yanısıra, sınıfların JVM iç gösterimi(*internal representation*) ve meta verileri ve *interned strings*'ler yer almaktaydı. JDK 8 den sonra bu generation'ının yerini ``metaspace`` almıştır. ``Permgen``den farklı olarak bu alan Java Heap ile bitişik değildir(*Not contiguous*). Metaspace "native memory" den ayrılmıştır ve ``metaspace`` için maksimum alan kullanılabilir sistem hafızasıdır. Fakat bu ``MaxMetaspaceSize`` JVM seçeneği ile sınırlandırılabilir. Yalnız öncesinde bu hafıza ``Permgen``de sınırlı idi. Özetle JDK 8 öncesi ve sonrası olmak üzere iki farklı cevabımız bulunmaktadır.

Daha detaylı bilgi almak isterseniz aşağıdaki referanslara göz gezdirebilirsiniz.

## Referanslar:

* [https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html)
* [https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)
* [https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)
* [https://www.coursera.org/learn/java-programming-design-principles/](https://www.coursera.org/learn/java-programming-design-principles/)
* [OCA/OCP Java SE 7 Programmer I & II Study Guide (Exams 1Z0-803 & 1Z0-804)](https://www.amazon.com/Programmer-Study-1Z0-803-1Z0-804-Certification/dp/0071772006)
* [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
* [https://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf](https://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf)
* [https://docs.oracle.com/javase/specs/](https://docs.oracle.com/javase/specs/)
* [https://www.youtube.com/watch?v=YujkxK3RfS0](https://www.youtube.com/watch?v=YujkxK3RfS0)
* [About G1 Garbage Collector, Permanent Generation and Metaspace](https://blogs.oracle.com/poonam/about-g1-garbage-collector%2c-permanent-generation-and-metaspace/comment-submitted?cid=12f52320-ad77-4ead-b736-b88be15dc3d0)
* [https://docs.oracle.com/javase/7/docs/webnotes/tsg/TSG-VM/html/memleaks.html](https://docs.oracle.com/javase/7/docs/webnotes/tsg/TSG-VM/html/memleaks.html)
* [https://docs.oracle.com/javase/9/troubleshoot/JSTGD.pdf](https://docs.oracle.com/javase/9/troubleshoot/JSTGD.pdf)
* [https://docs.oracle.com/javase/10/troubleshoot/troubleshoot-memory-leaks.htm#JSTGD266](https://docs.oracle.com/javase/10/troubleshoot/troubleshoot-memory-leaks.htm#JSTGD266)
* [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/considerations.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/considerations.html)
* [https://www.slideshare.net/rgrebski/on-heap-cache-vs-offheap-cache-53098109](https://www.slideshare.net/rgrebski/on-heap-cache-vs-offheap-cache-53098109)
* [What is a PermGen leak?](https://plumbr.io/blog/memory-leaks/what-is-a-permgen-leak)
* [https://docs.oracle.com/javase/9/gctuning/toc.htm](https://docs.oracle.com/javase/9/gctuning/toc.htm)
* [https://dzone.com/articles/java-memory-management](https://dzone.com/articles/java-memory-management)
* [Java Virtual Machine Troubleshooting Course](https://apexapps.oracle.com/pls/apex/f?p=44785:50:113240267925965:::50:P50_COURSE_ID,P50_EVENT_ID:185,5777)
* [Java 8: From PermGen to Metaspace](https://dzone.com/articles/java-8-permgen-metaspace)
* [Permgen vs Metaspace in Java](https://www.baeldung.com/java-permgen-metaspace)
