---
title: "Java'da Kalıtım 4 - Görünürlük Değiştiricileri"
comments: true
excerpt: "Bu derste Java'daki public, protected, package(default), private değiştirici işaretlerini ele alacağız. Bu değiştiricilerin, java'da görünürlüğü nasıl etkilediği hakkında fikir sahibi olacaksınız"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-46.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Toa Heftiba](https://unsplash.com/photos/8EF8GmQfGlg) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - değiştiriciler
  - public
  - private
  - protected
  - package(default)
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Kalıtımın önceki derslerinde ``public`` ve ``private`` değiştiricilerini kullanmıştık. Hatta bu görünürlük değiştiricilerinin bir sınıftan diğerine miras alınırken oluşturduğu etkileri gözlemlemiştik. Hatırlarsanız alt sınıflardan, parent sınıftaki ``private`` üye değişkenlere, yine parent sınıfta oluşturduğumuz ``public`` ``getter`` metotlar aracılıyla erişmiştik.

Şimdi işleyeceklerimiz ise, farklı görünürlük değiştiricilerine ve bunların sınıflar arası görünürlük etkilerine bakmak olacak. Özellikle kalıtım konusunu baz alarak bu konuyu ele alacağız. Bu dersi işlerken, [Java Sınıf Deklarasyonu ve Değiştiriciler](/java/Java-class-access/) konusuna da eş zamanlı göz gezdirmenizde yarar var.

Burada ele alacağımız değiştiriciler ``public`` ve ``private`` ile beraber şunlardır;

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/access.png" alt="access modifiers">
  <figcaption></figcaption>
</figure>

1. **public:** bu değiştiriciye sahip olan bir değişken veyahut metot, program içindeki bütün sınıflardan erişime açıktır. Yani herkes bu değiştiriciye sahip metot veya değişkene erişebilir.
2. **protected:** Şayet bu değiştiriye sahipseniz, 3 şekilde erişim sağlanabilir.
  * Birinci: aynı sınıf erişiminden kasıt, sınıfın içinde bulunan bir metot veya constructor içinden doğrudan erişimdir.
  * İkinci: aynı paket erişiminden kastımız ise, aşağıdaki şekillerde de göreceğiniz gibi, oluşturduğunuz sınıfları gruplamak için yapılan bir klasörlerin içinden yapılan erişimdir. Yani bu paket içinde olan sınıflar bu değiştiriciye sahip metot ve değişkenlere erişebilir.
  * Üçüncü ise herhangi bir alt sınıftan erişimdir. Alt sınıflar bir sınıfı miras alırsa(extends), miras aldığı sınıfın alt sınıfı olurlar ve belirli kurallar etrafında bu değiştiriciye sahip metot ve değişkenlere erişim haklarını elde ederler.
3. **package(default):** Bu değiştiriciye sahip olduğunuzda, alt sınıftan erişim hakkını kaybedersiniz. <u><i>Yalnız şu detayı belirtmekte yarar var. Eğer alt sınıf aynı paketin içinde ise yine erişim mümkündür.</i></u> Bunun dışında tıpkı protected değiştiricide olduğu gibi, aynı paket ve aynı sınıftan erişim yine vardır.
4. **private:** Bu değiştirici ile dış dünyadan bağlantı bir bakıma kesilir. Yani sadece aynı sınıf içinden erişim mümkün olur. Sınıf dışı erişimi delmek için yine sınıf içinde oluşturulan public getter ve setter metotlardan yararlanılır. Aslında bu, daha temiz kod yazımı için geliştirilen yöntemlerden biridir. Böylelikle sınıf üye değişkenlerinin bir nevi gizliliği sağlanmış olur.

> **Not:** Çok gerekmekmediği sürece protected ve package(default) değiştiriciler tavsiye edilmez. private ve public değiştiricilerin kullanılması tavsiye edilir. Hatta sınıf içi üye değişkenlerinin private erişime sahip olması ve metotların public olması çoğu zaman önerilir. Sınıf içi private metotlar genellikle yardımcı(helper) metot olarak adlandırılır.


### Örnek

Aşağıdaki şekil sınıflar arası hiyerarşiyi ve aynı zamanda rasgele oluşturulmuş bir tasarımı göstermektedir. Buradaki amacımız sadece ``Sample`` sınıfı içinde bulununan üye değişkenlerin görünürlük seviyelerini göstermek olacak.

Aşağıda 5 farklı sınıf arasında oluşturulmuş bir tasarımı görmektesiniz. Bunlardan ``Sub1`` ve ``Sub2`` sınıfları, ``Sample`` sınıfına doğrudan organik olarak bağlıdır. Çünkü görüleceği üzere ``extends`` anahtar kelimesi ile bu sınıfı miras aldıklarını görüyoruz. Yalnız ``Sub1`` sınıfının paket içinde ``Sub2`` sınıfının ise paket dışında olduğuna dikkat edin. Bunun yanı sıra ``Other1`` ve ``Other2`` isimli sınıflarımız da mevcuttur. ``Other1`` sınıfının ``Sample`` sınıfı ile aynı pakette olmasının dışında hiçbir organik bağ bulunmamaktadır. Aslında aynı pakette bulunmak da doğrudan bir bağ anlamına gelmez. Ama görünürlük kuralları çerçevesinde bu sınıfın bazı haklara sahip olacağını bize söyler. ``Other2`` sınıfı ise ne paket içinde ne de ``Sample`` sınıfı ile doğrudan bir bağ içindedir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/access1.png" alt="extends">
  <figcaption></figcaption>
</figure>

Şekilde okla belirtilen aslında uml diagram gösteriminde ``extends`` anahtar kelimesini temsil etmektedir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/access2.png" alt="extends">
  <figcaption></figcaption>
</figure>

Bu şekilde ise paketi görebilmeniz için ön plana çıkardım. Hangi sınıfların paket içinde kaldığına dikkat edin istiyorum.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/access3.png" alt="extends">
  <figcaption></figcaption>
</figure>

## Public Erişim

Görüleceği üzere ``Sample`` sınıfı içinde bulunan ``x`` üye değişkeni ``public`` değiştiricisine sahiptir. Burada sınıfları renkli bırakmamdaki sebep, ``x`` değişkenine hangi sınıfların erişebileceğini göstermektektir. Görüleceği üzere bu değişken hem paket içinden hem paket dışından, hem de alt sınıflardan erişime açıktır.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/public_access.png" alt="public_access modifier">
  <figcaption></figcaption>
</figure>

## Protected Erişim

Şimdi ise ``protected`` değiştiricisine sahip ``y`` üye değişkenine odaklanmanızı istiyorum. Burada ``Other2`` sınıfı dışında bütün sınıflar renkli gösterilmiştir. Kuralımız, hem aynı sınıftan, hem paket içinden hem de alt sınıflardan erişimi mümkün kılıyordu. ``Other1`` ve ``Sub1`` aynı paket içinde olduğundan bu değişkene erişim söz konusudur. ``Sub1`` aynı zamanda çifte hakka sahiptir. Çünkü bu sınıf, ``Sample`` sınıfının bir alt sınıfıdır. Yani Sample sınıfını miras alır. Sub2 ise sadece bir alt sınıf olduğu için bu erişim hakkını elde etmiştir. Fakat ``Other2`` sınıfı ne aynı pakette bulunmakta ne de bir alt sınıf olmaktadır. Bu yüzden ``y`` değişkenine erişimi yoktur.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/protected_access.png" alt="protected_access modifier">
  <figcaption></figcaption>
</figure>

> **ÖNEMLİ:** Erişimden bahsederken bir yandan da doğru kodlama ve tasarım tekniklerinden bahsetmek doğru olacaktır. Örneğin, sırf aynı pakette olduğu için **Other1** sınıfı neden **y** değişkenine erişebiliyor? Bu gerçekten mantıklı mı?  Yani böyle bir tasarım yapmamız doğru olur muydu? Genel olarak, bu tarz bir yaklaşım iyi bir tasarım oluşturmaz ve bu yüzden çok gerekmedikçe **protected** değiştiricisinin kullanılması önerilmez.


## Package(default) Erişim

Burada odaklanacağımız değişken ise ``package`` değiştiricisine sahip ``z`` üye değişkenidir. Bu değiştirici kodlanırken, değişkenin önüne ``package`` ve ``default`` yazılmazlar. Boş bırakıldıklarında java bunların ``package(default)`` zaten olduğunu bilir. Bu erişimin bir diğer ismi de ``package-private`` olarak da geçer. Bu erişimdeki kural, aynı sınıf ve aynı pakette olma koşuludur. Görüleceği üzere ``Sub2`` sınıfı bir alt sınıf olmasına rağmen aynı pakette olmadığı için erişim hakkını kaybetmiştir. ``Sub1`` ve ``Other1`` ise ``Sample`` ile aynı pakette olduğundan bu değişkene erişim hakkına sahiptirler.


<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/package_access.png" alt="package_access modifier">
  <figcaption></figcaption>
</figure>

> **ÖNEMLİ:** Burada da tıpkı **protected** erişimde olduğu gibi **Other1** sınıfı **Sample** ile bir organik bağı olmadığı halde sırf aynı pakette olduğu için **z** değişkenine erişim hakkı elde edebiliyor. Az önceki soruyu yinelemek istiyorum. Bu gerçekten doğru bir tasarım mı? **protected** erişimde olduğu gibi bu tarz bir yaklaşım da iyi bir tasarım oluşturmaz ve bu yüzden çok gerekmedikçe **package(default)** değiştiricisinin kullanılması önerilmez.

## Private Erişim

Görüleceği üzere sadece aynı sınıftan erişim mümkündür. ``private`` erişim çok kullanılan bir tasarım tekniğidir.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance4/private_access.png" alt="private_access modifier">
  <figcaption></figcaption>
</figure>

## Özet

* ``public`` ve ``private`` erişimi sıklıkla kullanabilirsiniz ama çok gerekmedikçe ``protected`` ve ``package(default)`` erişimin kullanılması tavsiye edilmez.
* Örnek değişkenlerinizi ``private`` tutun, ancak başkalarının kullanmasını istediğiniz yöntemlerin herkese açık olmasını(``public``) sağlayın.


## Referanslar
* [https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [OCA Java SE 8 Programmer I Exam Guide (Exams 1Z0-808)](https://www.amazon.com/Java-Programmer-Guide-Exams-1Z0-808/dp/1260011399)
* [https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
