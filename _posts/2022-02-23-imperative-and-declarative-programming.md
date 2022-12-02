---
title: "Imperative ve Declarative Stil Programlama (Programlama Stili Part 1)"
comments: false
excerpt: "Bu yazıda, zorunlu ve bildirimsel stil programlama arasındaki farkı açıklamaya çalışacağım."
header:
  teaser: "assets/images/2022-02-23-imperative-and-declarative-programming/imp.webp"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-02-23-imperative-and-declarative-programming/imp.webp
  overlay_image: /assets/images/unsplash-image-60.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Omid Armin](https://unsplash.com/photos/edANaB0ZFVo) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - programming-style
tags:
  - programming-style
  - declarative
  - imperative
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_label: "CONTENT"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Zorunlu Stil Programlama (Imperative Style Programming)

Wiki tanımına göre **zorunlu programlama stili** bir programın nasıl çalıştığını açıklamaya odaklanır. Aslında kulağa hoş geliyor, ama çok da açıklayıcı değil. O'reilly aracılığıyla katıldığım canlı bir etkinlikte daha iyisini buldum.

Venkat Subramaniam'a göre Zorunlu Stil Programlama bize **bir programın ne yaptığını ve ayrıca bunu nasıl yaptığını** söyler. Bu tanımı Venkat'ın yaptığı gibi özetlemek istiyorum ->

### Ne yapıyor - Nasıl yapıyor (What to do - How to do it)

Örneğin;

```java
String[] names = {"hasan", "tom", "phil", "bev", "mike"};
for (int i = 0; i < names.length; i++) {
    if (names[i].equals("hasan")) {
        System.out.println("it is found");
    }
}
```
Yukarıdaki örnekte bu kodun ne işe yaradığını kolaylıkla anlayabiliriz.

**Ne yapıyor:** array içinde özel bir isim bulmaya çalışıyor.

**Bunu Nasıl yapıyor? :** bunu standart bir for döngüsü kullanarak yapıyor. For döngüsünde bir değişken tanımlıyor ve bunu birer birer artırarak ve aradığı adla karşılaştırarak yapıyor.

Özetle, <u>bu ismi bulmak için</u>(yani, "**ne yapıyor**" kısmını) <u>gerekli işlemleri</u> (yani "**nasıl yapıyoruz**" kısmını) adım adım gösteriyoruz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---

Zorunlu stilin özünde **değişkenlik** ve **komuta-dayalı** programlama vardır. Değişkenler veya nesneler yaratır ve süreç boyunca onların durumlarını(yani state'lerini) değiştiririz. Ayrıca, bir döngü dizini oluşturma, değerini artırma, sona ulaşıp ulaşmadığımızı kontrol etme, bir array'in n'inci öğesini güncelleme vb. gibi yürütmeye yönelik ayrıntılı komutlar veya talimatlar da sağlarız.
</div>

## Bildirimsel Stil Programlama (Declarative Style Programming)

Bildirimsel programlama stili, programın sonuca nasıl ulaşması gerektiğine dair tüm ayrıntıları belirtmeden programın neyi başarması gerektiğine odaklanır. Yine Venkat'a göre;

Bize **bir programın ne yaptığını** söyler **ama nasıl yapacağını** söylemez. Tekrardan bu tanımı şöyle özetlemek istiyorum ->

### Ne yapıyor - Nasıl yapıyor YOK (What to do - NOT How to do it)

Örneğin;

```java
List<String> listNames = Arrays.asList("hasan", "tom", "phil", "bev");
if (listNames.contains("hasan")) {
    System.out.println("it is found");
}
```
**Ne yapıyor :** önceki gibi array içinde özel bir isim bulmaya çalışıyor.

**"Bunu Nasıl yapıyor" YOK :** Zorunlu programlama stilinden farkı, bildirimsel stilin, "**nasıl yapılır**" bölümlerinin ayrıntılarını gizlemesidir. Görüleceği üzere, *contains* yöntemi bizim için tüm işi yapıyor ve karmaşıklığı gizliyor.

## Gelişmiş(enhanced) ve standart döngülerin programlama stilleri açısından karşılaştırılması

**Gelişmiş** ve **standart** for döngülerini hangi sınıflandırma içide değerlendirmeliyiz? Gelin birlikte beyin fırtınası yapalım…

Her iki durumda da ne yapacağımız bellidir. Amacımız array'in elemanlarını konsola yazdırmak. Peki ya “**nasıl yapılır**” bölümleri???

**Standart for döngüsünde;** Bir sayaç, *i*, bildirilir ve 0 olarak başlatılır. Bir boolean ifadesi, *i*'yi *names* dizisinin uzunluğuyla karşılaştırır. Eğer *i<names.length* ise, kod bloğu yürütülür. *i*, her kod bloğunun sonunda bir artırılır. Kod bloğu içinde *i* array indeksi olarak kullanılır. Gördüğünüz gibi adım adım **nasıl yapacağınızı** anlatıyoruz.

**Geliştirilmiş for döngüsünde;** Array'in her öğesini tutmak için bir string değişkeni olan *name* deklare edilir. Ancak bu döngünün tüm bu işlemleri **nasıl** yaptığını bilmiyoruz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2022-02-23-imperative-and-declarative-programming/forloops.webp"  width="100%" height="100%" loading="lazy" alt="for loops example">

Aslında hem **standart for döngüsü** *(for i = 0...)* hem de **gelişmiş(enhanced) for döngüsü** *(for var x : ...)* zorunlu(imperative) stildir. **Geliştirilmiş for döngüsü** aslında `iterator.hasNext()` ve `iterator.next()` etrafında bir sarmalayıcıdır. Diğer bir deyişle, bu yineleme(iteration) biçimi, arka planda, `Iterator` arabirimini kullanır ve onun `hasNext` ve `next` yöntemlerini çağırır. Dahası, **enhanced for döngüsünden**, bir if koşuluyla `break` ve `continue` yapabiliriz ve bu, imperative'in açıkça geliştirildiğini gördüğümüz yerdir.

> ENHANCED for döngüsü, STANDARD for döngüsünden daha bildirimseldir

Geliştirilmiş for döngüsünün **bildirim stiline** daha çok benzediğini söyleyebiliriz. Fakat tamamen **bildirimsel(declarative)** stildir diyemeyiz. Hem **standart** döngü hem de **geliştirilmiş** döngüler, doğası gereği zorunlu(imperative) olan harici yineleyicilerdir. Her iki döngüde de akışı "if `break`" veya "if `continue`" kullanarak kontrol edebiliriz. Yalnız, gelişmiş for döngüsünde, standart for döngüsüne kıyasla biraz daha az kontrol yapıyoruz. Ama yine de her ikisi de zorunlu(imperative) kontrole izin verir.


## Referanslar:
* [Imperative programming](https://en.wikipedia.org/wiki/Imperative_programming)
* [Declarative programming](https://en.wikipedia.org/wiki/Declarative_programming)
* [Understanding Functional Programming - Venkat Subramaniam](https://learning.oreilly.com/live-events/understanding-functional-programming/0636920457435/0636920058831/)
* [Functional Programming in Java](https://learning.oreilly.com/library/view/functional-programming-in/9781941222690/)
