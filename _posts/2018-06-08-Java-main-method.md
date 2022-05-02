---
title: "Java Main Metodu"
comments: true
excerpt: "Bu bölümde; Java ile yazılmış bir program nasıl ayağa kalkar? Programı ayağa kaldıracak metot için gerekli asgari şartlar nelerdir? gibi soruları cevaplamaya çalışacağız"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-41.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Jon Tyson](https://unsplash.com/photos/FlHdnPO6dlw) on Unsplash"
  video:
    id: cR9uwtMQt-g
    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java
tags:
  - main metotu
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Java'da yazılan bir program mutlaka ``main`` adlı bir yöntemle başlar. Programı çalıştırdığınızda, ``main`` yönteminin hangi sınıfta olduğunu belirtirsiniz. ``main`` yöntemi aşağıdaki gibi özel bir deklerasyona sahip olmalıdır.

{% highlight java %}
public static void main(String[] args)
{% endhighlight %}

``main`` metodu, programı başlatan kod tarafından çağrılması gerektiğinden herkese açık(*public*) olmalıdır. Bir sonraki kelime ``static``'dir. Bu, ``main`` yönteminin sınıfın her özel örneğinde yaşamadığı, aksine sınıfın bütünü için sadece bir tane olduğu anlamına gelir. Sonrasında ise ``void`` ifadesini görüyorsunuz. Bu metodun herhangi bir değer döndürmediği anlamına gelir. Ardından, yöntem adı olan ``main``'i göreceksiniz. Java, bir programı çalıştırabilmek için bu ismi kullanmanızı şart koşar. Bunun dışında bir isim veremezsiniz. Bu sebepten ötürü programı ayağa kaldırmak için main ismini veriyoruz. Bunun dışında bu metot bir de parametre olarak *String* bir dizi almıştır. Yani main isminin olması programı ayağa kaldırmak için yeterli değildir. Programı ayağa kaldırmak için Java'nın bizden beklediği bir metot deklerasyonu vardır. O da ``public static void main(String[] args)`` şeklindedir. Yani metot deklerasyonu;

Aşağıdaki hazırladığım java eğitim videosunda, main metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım. Bu videoyu özellikle izlemenizi öneririm.

{% include video id="cR9uwtMQt-g" provider="youtube" %}

* Hem ``public`` olacak,
* Hem ``static`` olacak,
* Hem ``void`` dönecek,
* Hem ismi ``main`` olacak,
* Hem de ``String[]`` tipinde bir diziyi parametre olarak alacaktır.

Bu arada parametreleri normal metodlarda sıklıkla görüyoruz. Buradaki çalışma prensibi de onlardan farklı değildir. Buradaki parametre programa komut satırı argümanlarını sunar. Programı çalıştırdığınızda, bu diziye hangi *string*'lerin geçirileceğini belirtebilirsiniz. Komut satırı argümanlarını ``main`` olarak kullanmak isterseniz, kodu yeniden derlemeden programın hangi dosyayı okuyabileceğini değiştirebilirsiniz.

<!-- Kod, ilk ``string``'i içeri alınan argüman dizisinden çıkarmak için sıfırda "args" kullanır. -->

## Komut Satırı Argümanları(Command-Line Arguments)
Komut satırı argümanlarından biraz bahsetmek istiyorum. Bir Java uygulaması, komut satırından istediğiniz sayıda argüman girişini kabul edebilir. Bu, kullanıcının uygulama başlatıldığında yapılandırma bilgilerini belirlemesini sağlar. Dikkat ederseniz ``main`` yönteminin parametre değişkenleri, yani ``args``ın veri tipi bir String dizisidir. Bu programın sadece string tipinde bir diziyi argüman olarak kabul edeceği anlamına gelmektedir.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-08-Java-main-method/args.png" alt="command-line arguments">
  <figcaption>Captured by using Jing</figcaption>
</figure>

Diyelim ki komut satırı argümanı olarak kullanmak istediğiniz bir şeyler olduğunu hayal edin. Aşağıdaki örnekte ``This is a sample text``  cümlesini komut satırı argümanı eklemek istiyorum. Aşağıdaki kodla bu cümleyi args parametresi olarak programımın içine dahil edebilirim. Sistem, zaten ``args`` parametresi yoksa, parametrenin olmadığını anlayıp sadece ``main`` metodunun içine odaklanır.

``` java
public class Sample {
    public static void main(String[] args) {
        for (String s: args) {
            System.out.println(s);
        }
    }
}
```

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2018-06-08-Java-main-method/result.png" alt="command-line arguments">
  <figcaption>Captured by using Jing</figcaption>
</figure>

Görüleceği üzere komut satırı argümanlarını da kullanmak bir seçenektir.


## Main metodundaki "statik" ne anlama geliyor?
Statik anahtar sözcüğü, bir yönteme veya üye değişkenine uygulandığında, uygulandığı yöntemin veya değişkenin artık sınıf için tanımlandığı, yani sınıftan türetilen her bir nesne için **tanımlanmadığı** anlamına gelir. Dolayısıyla main artık genel bir yöntemdir ve sınıfa aittir. Sınıfın bir nesnesini oluştururken çoğaltılamaz ve bunları ``main`` içinde "çağıran nesne" olmaz. Yani ``this`` anahtar sözcüğü kullanarak main metodu içinden bir şey çağıramazsınız. Örnek yöntemlerini(instance methods) main'den çağırmak istiyorsanız, nesneler oluşturmanız ve ardından bu nesneler üzerinde örnek yöntemlerini(instance methods) çağırmanız gerekir. Bununla birlikte, diğer statik yöntemleri doğrudan çağırabilirsiniz. ``static`` anahtar kelimesinin ne anlama geldiğini daha detaylı görmek isterseniz şu [linkteki](/java/Java-static-method/) yazımı okuyabilirsiniz.


> **Not:** Üye değişkeni : İngilizce *member variable* veyahut *instance variable* olarak da tanımlanır. Yani sınıftan türetilen her bir nesnede olan özelliklerdir. Kısacası her bir nesne için özeldir. Sınıf değişkeni ise ingilizce *class variable* olarak tanımlanır. Fakat sınıf değişkeni sınıfa aittir ve tektir. Sınıftan türetilen her nesnede aynı değeri taşır.

## Referanslar

* [Command-Line Arguments](https://docs.oracle.com/javase/tutorial/essential/environment/cmdLineArgs.html)
* [Defining Methods](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html)
* [Java Program Invocation and Command-Line Arguments](http://courses.cms.caltech.edu/cs11/material/java/donnie/java-main.html#:~:text=The%20classes%20in%20the%20java,num%20%3D%200%3B%20...)
* [Command-Line Arguments](http://www.dickbaldwin.com/java/Java032.htm)
* [How To Use Command Line Arguments in Eclipse](https://www.cs.colostate.edu/helpdocs/eclipseCommLineArgs.html)
* [The command line arguments of a Java program](http://www.mathcs.emory.edu/~cheung/Courses/170/Syllabus/09/command-args.html)

<!-- reference : 126.5-71/69.5 ref25 -->
