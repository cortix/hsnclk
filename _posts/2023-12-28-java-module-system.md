---
title: "The State of the Java Module System - Mark Reinhold (Java Modül Sisteminin Durumu) (ÇEVİRİ)"
comments: false
excerpt: "Arkadaşlar bu yazı Mark Reinhold'un, Java'da modül sistemi ile ilgili kaleme aldığı meşhur bir makaledir. Çevirisini bizzat kendim yaptım."
header:
  teaser: "/assets/images/svg-book6.svg"
  og_image: /assets/images/svg-book6.svg
  overlay_image: /assets/images/svg-book6.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-module-system
  - recommends
tags:
  - Java modül sistemi
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Türkçe kaynağa destek olmak için çevirisini bizzat kendim yaptım.
{: .notice}



## The State of the Module System (modül sisteminin durumu)

> Bu belge biraz güncel değildir. Temel kavramların hiçbiri değişmedi ancak ``requires public`` yönergesi/direktifi ``requires transitive`` olarak yeniden adlandırıldı ve çeşitli ek yetenekler eklendi. Güncelleme hazırlık aşamasındadır ve hazır olduğunda burada yayınlanacaktır. [^1]

Bu, [Project Jigsaw](https://openjdk.org/projects/jigsaw/)'da prototipi oluşturulan ve [JSR 376: The Java Platform Module System](https://openjdk.org/projects/jigsaw/spec/) için önerilen Java SE Platformundaki geliştirmelere ilişkin resmi olmayan bir genel bakıştır. İlgili bir [belgede](https://openjdk.org/jeps/261), JSR kapsamı dışında olan JDK'ya özgü araçlar (JDK-specific tools) ve API'lerde yapılan geliştirmeler açıklanmaktadır.

[JSR'de](https://openjdk.org/projects/jigsaw/spec/) tanımlandığı gibi, modül sisteminin spesifik hedefleri şunları sağlamaktır:

* Kırılgan, hataya açık sınıf yolu (*class-path*) mekanizmasını, program bileşenlerinin birbirlerine açık bağımlılıklarını bildirecek bir araçla değiştirmek için **güvenilir yapılandırma (reliable configuration)**, bununla birlikte,
* Bir bileşenin hangi `public` türlerinin diğer bileşenler tarafından erişilebilir olduğunu ve hangilerinin erişilebilir olmadığını bildirmesine olanak tanıyan **güçlü kapsülleme (strong encapsulation)**.

Bu özellikler, uygulama geliştiricilerine, kütüphane (*library*) geliştiricilerine ve Java SE Platformu uygulayıcılarına doğrudan ve ayrıca dolaylı olarak fayda sağlayacaktır, çünkü bunlar ölçeklenebilir bir platform, daha fazla platform bütünlüğü ve gelişmiş performans sağlayacaktır.


Bu, bu dökümanın ikinci baskısıdır. [İlk baskıya](https://openjdk.org/projects/jigsaw/spec/sotms/2015-09-08) göre bu baskı, [uyumluluk ve migrasyon](https://openjdk.org/projects/jigsaw/spec/sotms/#compatibility--migration) (compatibility and migration) ile ilgili materyaller sunar, [yansıtıcı okunabilirlik](https://openjdk.org/projects/jigsaw/spec/sotms/#reflective-readability) (reflective readability) tanımını gözden geçirir, anlatının(narrative) akışını iyileştirmek için metni yeniden sıralar ve daha kolay gezinme için iki seviyeli (*two-level*) bir bölüm ve alt bölüm hiyerarşisi halinde düzenlenir.

Tasarımda hâlâ pek çok [açık sorunlar](https://openjdk.org/projects/jigsaw/spec/issues/) (*open issues*) bulunmaktadır ve bunların çözümleri bu belgenin gelecek sürümlerinde yansıtılacaktır.


## DEFINING MODULES (modüllerin tanımlanması)

Hem geliştiriciler için ulaşılabilir hem de mevcut araç zincirleri tarafından desteklenebilir bir şekilde <u>güvenilir yapılandırma</u> (*reliable configuration*) ve <u>güçlü kapsülleme</u> (*strong encapsulation*) sağlamak için modülleri, temel bir yeni tür Java program bileşeni olarak ele alıyoruz. Bir *modül*, adlandırılmış (*named*), kendi kendini tanımlayan (*self-describing*) bir kod ve veri koleksiyonudur. Kodu (yani modülün kodu), tür içeren bir dizi paket olarak organize edilmiştir, *diğer bir deyişle*, Java sınıfları ve arayüzleri; verileri(*its data*), kaynakları (*resources*) ve diğer statik bilgileri içerir.


### 1.1 Module declarations (modül deklarasyonları/bildirimleri)

Bir modülün kendi kendini açıklaması (*self-description*), Java programlama dilinin yeni bir yapısı olan modül deklarasyonunda (*module declaration*) ifade edilir. Mümkün olan en basit modül deklarasyonu yalnızca <u>modülünün adını</u> belirtir:

{% highlight java mark_lines="1" linenos %}
module com.foo.bar {
}
{% endhighlight %}

Modülün hem <u>derleme</u> hem de <u>çalışma</u> zamanında, başka modüllere adıyla(*by name*) bağımlı olduğunu bildirmek için, bir veya daha fazla `requires` cümleciği (*clause*) eklenebilir:

{% highlight java mark_lines="2" linenos %}
module com.foo.bar {
  requires org.baz.qux;
}
{% endhighlight %}

Son olarak, modülün belirli paketlerdeki yalnızca `public` olan bütün türlerini (*types*) diğer modüller tarafından kullanılabilir hale getirdiğini bildirmek için, `exports` cümleciği (*clause*) eklenebilir:

{% highlight java mark_lines="3 4" linenos %}
module com.foo.bar {
  requires org.baz.qux;
  exports com.foo.bar.alpha;
  exports com.foo.bar.beta;
}
{% endhighlight %}

Bir modülün deklarasyonu hiçbir `exports` cümleciği (*clause*) içermiyorsa, o zaman hiçbir türü diğer modüllere aktarmaz.

(**Benim notum :** Yani ilgili modülü `requires` ile talep etsek bile, bu modülün dışarı aktarılan (yani `exports` edilen) bir paketi yoksa o modüle ulaşılmaz.)
{: .notice--warning}

Bir modül deklarasyonunun kaynak kodu (*source code*), geleneksel olarak, modülün kaynak dosya (*module’s source-file*) hiyerarşisinin kökündeki (*root*) `module-info.java` adlı bir dosyaya yerleştirilir. `com.foo.bar` modülü için kaynak dosyalar, örneğin şunları içerebilir:

{% highlight java linenos %}
module-info.java
com/foo/bar/alpha/AlphaFactory.java
com/foo/bar/alpha/Alpha.java
...
{% endhighlight %}

Bir modül deklarasyonu, geleneksel olarak, sınıf dosyası çıktı dizinine ( *class-file output directory*) benzer şekilde yerleştirilen `module-info.class` adlı bir dosyaya derlenir.

Paket adları gibi modül adları da <u>çakışmamalıdır</u>. Bir modülü adlandırmanın önerilen yolu, paketlerin adlandırılmasında uzun süredir önerilen <u>ters-alan-adı</u> (*reverse-domain-name*) paternini kullanmaktır. Bu nedenle bir modülün adı genellikle dışa aktarılan (*export* edilen) paketlerin adlarının <u>öneki</u> olacaktır, ancak bu ilişki zorunlu değildir.

Bir modülün deklarasyonu bir sürüm dizesi (*version string*) içermediği gibi, bağlı olduğu modüllerin sürüm dizeleri üzerinde de kısıtlamalar içermez. <u>Bu kasıtlıdır:</u> sürüm-seçimi (*version-selection*) sorununu çözmek modül sisteminin [bir amacı değildir](https://openjdk.org/projects/jigsaw/spec/reqs/02#version-selection); bu sorunu inşa-araçları (*build tools*) ve konteyner (*container*) uygulamalarına bırakmak en iyisidir.

Modül deklarasyonları çeşitli nedenlerden dolayı kendilerine ait bir dil veya notasyon/gösterim yerine Java programlama dilinin bir parçasıdır. En önemlilerinden biri, [fazlar arasında aslına uygunluğu](https://openjdk.org/projects/jigsaw/spec/reqs/02#fidelity-across-all-phases) (*fidelity across phases*) sağlamak için modül bilgilerinin hem derleme zamanında hem de çalışma zamanında mevcut olması gerektiğidir, başka bir deyişle, modül sisteminin hem derleme zamanında hem de çalışma zamanında aynı şekilde çalışmasını sağlamaktır. Bu da, birçok hata türünün önlenmesine veya en azından, teşhis edilmesi ve onarılması daha kolay olduğunda, daha erken - derleme zamanında - bildirilmesine olanak tanır.

Modül deklarasyonlarının, bir modüldeki diğer kaynak dosyalarla (*source files*) birlikte, Java sanal makinesinin tüketimi için bir sınıf dosyasına (*class file*) derlenen bir kaynak dosyada (*source file*) ifade edilmesi, aslına uygunluğu (*fidelity*) sağlamanın doğal yoludur. Bu yaklaşım geliştiricilere hemen tanıdık gelecek, ve IDE'ler ve inşa araçları (*build tools*) tarafından desteklenmesi zor olmayacaktır. Özellikle bir IDE, bileşenin proje açıklamasında (*component’s project description*) zaten mevcut olan bilgilerden `requires` cümlecikleri (*requires clauses*) sentezleyerek mevcut bir bileşen (*existing component*) için bir başlangıç modül (*initial module*) deklarasyonu önerebilir.

### 1.2 Module artifacts (modül yapıları/yapıtları/artifektler)

Mevcut araçlar(*tool*’lar) JAR dosyalarını zaten oluşturabilir, manipüle edebilir ve tüketebilir; bu nedenle adaptasyon ve taşıma (*migration*) kolaylığı için modüler JAR dosyaları tanımlıyoruz. Modüler bir JAR dosyası, kök dizininde (*root directory*) bir `module-info.class` dosyasını da içermesi dışında, tüm olası yönlerden sıradan bir JAR dosyasına benzer. Yukarıdaki `com.foo.bar` modülü için modüler bir JAR dosyası örneğin aşağıdaki içeriğe sahip olabilir:

{% highlight java linenos %}
META-INF/
META-INF/MANIFEST.MF
module-info.class
com/foo/bar/alpha/AlphaFactory.class
com/foo/bar/alpha/Alpha.class
...
{% endhighlight %}

Modüler bir JAR dosyası modül olarak kullanılabilir, bu durumda `module-info.class` dosyası modülün deklarasyonunu içerecek şekilde alınır. Alternatif olarak sıradan sınıf yoluna (*class path*) yerleştirilebilir, bu durumda `module-info.class` dosyası <u>göz ardı edilir</u>.  Modüler JAR dosyaları, bir kütüphanenin bakımcısının (*maintainer of a library*), hem Java SE 9 ve sonraki sürümlerde modül olarak çalışan hem de tüm sürümlerde sınıf yolunda (*class path*) normal bir JAR dosyası olarak çalışan tek bir yapıtı/artifekti (*a single artifact*) taşımasına izin verir. Bir jar aracı (*jar tool*) içeren Java SE 9 uygulamalarının, modüler JAR dosyaları oluşturmayı kolaylaştıracak şekilde bu aracı geliştireceğini umuyoruz.

Java SE Platformunun referans uygulaması olan JDK'yı modüler hale getirmek amacıyla, yerel kodu (*native code*), yapılandırma dosyalarını (*configuration files*) ve JAR dosyalarına doğal olarak hiç sığmayan diğer veri türlerini barındırmak için, JAR dosyalarının ötesine geçen yeni bir yapay format (*new artifact format*) tanıtacağız. Bu format, modül deklarasyonlarını kaynak dosyalarda (*source files*) ifade etmenin ve bunları sınıf dosyalarına (*class files*) derlemenin başka bir avantajından yararlanır; yani sınıf dosyaları herhangi bir özel yapıt/artifekt formatından (artifact format) bağımsızdır. Geçici olarak "JMOD" olarak adlandırılan bu yeni formatın standartlaştırılmasının gerekip gerekmediği açık bir sorudur.

### 1.3 Module descriptors (modül tanımlayıcıları)

Modül bildirimlerini/deklarasyonlarını sınıf dosyaları (*class files*) halinde derlemenin son avantajı, sınıf dosyalarının zaten tam olarak tanımlanmış ve genişletilebilir bir formata ([precisely-defined and extensible format](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.7.1)) sahip olmasıdır. Dolayısıyla `module-info.class` dosyalarını, kaynak düzeyi (*source-level*) modül deklarasyonlarının derlenmiş formlarını içeren **modül tanımlayıcıları** (*module descriptors*) olarak, aynı zamanda deklarasyonun ilk derlenmesinden sonra eklenen sınıf-dosyası özniteliklerinde (*class-file attributes*) kaydedilen ek bilgi türleri olarak da daha genel bir açıdan değerlendirebiliriz.

Bir IDE veya inşa-zamanı paketleme aracı (*build-time packaging tool*), örneğin, bir modülün sürümü (*module’s version*), başlığı (*title*), açıklaması (*description*) ve lisansı gibi belgelere dayanan bilgilerini içeren nitelikleri ekleyebilir. Bu bilgiler, belgeleme (*documentation*), teşhis (*diagnosis*) ve hata ayıklamada (*debugging*) kullanılmak üzere modül sisteminin yansıtma (*module system’s reflection*) olanakları aracılığıyla derleme zamanında ve çalışma zamanında okunabilir. Ayrıca işletim sistemine özgü paket yapıtlarının (*OS-specific package artifacts*) oluşturulmasında aşağı yönlü araçlar (*downstream tools*) tarafından da kullanılabilir. Belirli bir öznitelik kümesi standartlaştırılacaktır, ancak Java sınıf dosyası formatı (*java class-file format*) genişletilebilir (*extensible*) olduğundan, diğer araçlar (*tools*) ve çerçeveler (*frameworks*) gerektiği gibi ek öznitelikler tanımlayabilecektir. Standart dışı niteliklerin (*non-standard attributes*) modül sisteminin davranışı üzerinde hiçbir etkisi olmayacaktır.

### 1.4 Platform modules (platform modülleri)

Java SE 9 Platform Spesifikasyonu/şartnamesi, platformu bir dizi modüle bölmek için modül sistemini kullanacaktır. Java SE 9 Platformu’nun bir uygulaması, platform modüllerinin tamamını veya muhtemelen yalnızca bir kısmını içerebilir.

Her durumda, modül sistemi tarafından özel olarak bilinen tek modül, `java.base` olarak adlandırılan temel modüldür (base module). Temel modül, modül sisteminin kendisi de dahil olmak üzere platformun tüm çekirdek paketlerini tanımlar ve dışa aktarır (*export* eder):

{% highlight java mark_lines="6" linenos %}
module java.base {
  exports java.io;
  exports java.lang;
  exports java.lang.annotation;
  exports java.lang.invoke;
  exports <b>java.lang.module</b>;
  exports java.lang.ref;
  exports java.lang.reflect;
  exports java.math;
  exports java.net;
  ...
}
{% endhighlight %}

Temel modül (*base modül*) her zaman mevcuttur. Diğer tüm modüller dolaylı olarak temel modüle bağlıyken, temel modül başka hiçbir modüle bağlı değildir. 

Geriye kalan platform modülleri **“java.”** isim önekini paylaşacak ve muhtemelen veritabanı bağlantısı için `java.sql`, XML işleme için `java.xml` ve logging için `java.logging`’i içerecektir. Java SE 9 Platform Şartnamesinde tanımlanmayan ancak bunun yerine JDK'ya özgü olan modüller, kural gereği **“jdk."** ad önekini paylaşacaktır.

## 2 USING MODULES (modülleri kullanma)

Bireysel modüller, modül yapılarında (*module artifacts*) tanımlanabilir veyahut derleme zamanı veya çalışma zamanı ortamında yerleşik (*built-in*) olarak tanımlanabilir. Her iki fazda da bunlardan faydalanmak için modül sisteminin bunları yerleştirmesi ve ardından güvenilir konfigürasyon (*reliable configuration*) ve güçlü kapsülleme (*strong encapsulation*) sağlayacak şekilde birbirleriyle nasıl ilişki kurduklarını belirlemesi gerekir.

### 2.1 The module path (modül yolu)

Yapıtlarda (*artifacts*) tanımlanan modüllerin yerini saptamak için modül sistemi, ana sistem (*host system*) tarafından tanımlanan **modül yolunu** (*module path*) arar. Modül yolu, her bir öğesi ya bir modül yapıtı (*module artifact*) ya da modül yapıtlarını içeren bir dizin (*directory containing module artifacts*) olan bir dizidir (*sequence*). Modül yolunun elemanları, uygun bir modülü tanımlayan ilk artifact için sırayla aranır.

Modül yolu (*module path*), sınıf yolundan (*class-path*) önemli ölçüde farklıdır ve daha sağlamdır. Sınıf yolunun (*class-path*) doğal kırılganlığı, artifektlerin kendi aralarında hiçbir ayrım yapmadan, yol üzerindeki (yani *path*’deki) tüm artifektlerde bireysel/farklı (*individual*) türleri bulmanın bir yöntemi olması gerçeğinden kaynaklanmaktadır. Bu, bir yapıtın (yani *artifact*’in) eksik olup olmadığını önceden söylemeyi imkansız hale getirir. Ayrıca, bu yapıtlar (*artifacts*) aynı mantıksal program bileşeninin farklı sürümlerini veya tamamen farklı bileşenleri temsil etse bile, farklı yapıtların (*different artifacts*) aynı paketlerdeki türleri tanımlamasına da olanak tanır.

Modül yolu (*module path*), aksine, bireysel türleri bulmak yerine tüm modüllerin yerini saptamanın bir yöntemidir. Modül sistemi, modül yolundan (*module path*) gelen bir yapıt (*artifact*) ile belirli bir bağımlılığı yerine getiremezse veya aynı dizinde aynı adı taşıyan modülleri tanımlayan iki yapıyla (*artifact*) karşılaşırsa, derleyici veya sanal makine bir hata bildirecek ve çıkış yapacaktır.

Modül yolu (*module path*) üzerindeki yapılar (*artifact*) tarafından tanımlananlarla birlikte derleme zamanı veya çalışma zamanı ortamına yerleşik (*built-in*) modüller, toplu olarak gözlemlenebilir modüller evreni (*universe of observable modules*) olarak anılır.

### 2.2 Resolution (çözünme)

Yukarıdaki `com.foo.bar` modülünü ve ayrıca platformun `java.sql` modülünü kullanan bir uygulamamız olduğunu varsayalım. Uygulamanın çekirdeğini içeren modül şu şekilde bildirilir:

{% highlight java linenos %}
module com.foo.app {
  requires com.foo.bar;
  requires java.sql;
}
{% endhighlight %}

Bu ilk uygulama modülü göz önüne alındığında, modül sistemi, bu bağımlılıkları yerine getirmek (*fulfill*) için ek gözlemlenebilir modüller (*observable modules*) yerleştirerek `requires` clause’larla ifade edilen bağımlılıkları çözer ve ardından her modülün her bağımlılığı yerine getirilene (karşılanana kadar) kadar bu modüllerin bağımlılıklarını vb. şeyleri çözer. Bu geçişli-kapatma (*transitive-closure*) hesaplamasının sonucu, başka bir modül tarafından yerine getirilen bir bağımlılığa sahip her modül için, ilk modülden ikinciye yönlendirilmiş bir kenar (*directed edge*) içeren bir modül grafiğidir (*module graph*).

Modül sistemi, `com.foo.app` modülü için bir modül grafiği oluşturmak amacıyla, `java.sql` modülünün bildirimini inceler:

{% highlight java linenos %}
module java.sql {
  requires java.logging;
  requires java.xml;
  exports java.sql;
  exports javax.sql;
  exports javax.transaction.xa;
}
{% endhighlight %}

Ayrıca yukarıda gösterilen `com.foo.bar` modülünün ve ayrıca `org.baz.qux`, `java.logging` ve `java.xml` modüllerinin bildirimlerini de denetler; Kısaca belirtmek gerekirse, diğer modüllere bağımlılık bildirmediklerinden bu son üçü burada gösterilmemiştir.

Tüm bu modül bildirimlerine dayanarak `com.foo.app` modülü için hesaplanan grafik aşağıdaki düğümleri (*nodes*) ve kenarları (*edges*) içerir:


<br/>{% picture 2023-12-28-java-module-system/module-1.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Bu şekilde koyu mavi çizgiler, `requires` cümleciklerinde ifade edildiği gibi açık (*explicit*) bağımlılık ilişkilerini temsil ederken, açık mavi (aslında grimsi) çizgiler her modülün temel modül (*base module*) üzerindeki örtülü (*implicit*) bağımlılıklarını temsil eder.

### 2.3 Readability (okunabilirlik)

Modül grafiğinde bir modül doğrudan diğerine bağlı/bağımlı olduğunda, ilk modüldeki kod, ikinci modüldeki türlere başvurabilecektir. Bu nedenle, ilk modülün ikinciyi okuduğunu veya eşdeğer olarak ikinci modülün birinci tarafından okunabilir olduğunu söylüyoruz. Dolayısıyla, yukarıdaki grafikte `com.foo.app` modülü `com.foo.bar` ve `java.sql` modüllerini okur, ancak `org.baz.qux`, `java.xml` veya `java.logging` modüllerini okumaz. `java.logging` modülü `java.sql` modülü tarafından okunabilir, ancak diğerleri tarafından okunamaz. (Her modül tanımı gereği kendini okur.)

Bir modül grafiğinde tanımlanan okunabilirlik ilişkileri **güvenilir konfigürasyonun** (*reliable configuration*) temelini oluşturur: Modül sistemi, her bağımlılığın tam olarak başka bir modül tarafından karşılanmasını, modül grafiğinin <u>döngüsel olmamasını</u> (*acyclic*), her modülün belirli bir paketi tanımlayan en fazla bir modülü okumasını ve aynı adlı paketleri (*identically-named packages*) tanımlayan modüllerin birbirine müdahale etmemesini sağlar.

Güvenilir konfigürasyon (*reliable configuration*) yalnızca daha güvenilir olmakla kalmaz; aynı zamanda daha hızlı da olabilir. Bir modüldeki kod, bir paketteki bir türe başvuruda bulunduğunda, o paketin ya o modülde ya da tam olarak o modül tarafından okunan modüllerden birinde tanımlanacağı garanti edilir. Belirli bir türün tanımını ararken, bu nedenle onu birden fazla modülde veya daha kötüsü tüm sınıf yolu (*class path*) boyunca aramaya gerek yoktur.

### 2.4 Accessibility (erişilebilirlik)

Bir modül grafiğinde tanımlanan okunabilirlik ilişkileri, modül deklarasyonlarındaki `exports` cümleleriyle birleştiğinde **güçlü kapsüllemenin** (*strong encapsulation*) temelini oluşturur: Java derleyicisi ve sanal makine, bir modülün bir paketindeki `public` türlerin, yalnızca ilk modülün yukarıda tanımlandığı anlamda ikinci modül tarafından okunabilir olması durumunda ve ilk modül bu paketi dışa aktardığında (*export* ettiğinde), başka bir modüldeki kodla erişilebilir (*accessible*) olduğunu kabul eder (ya da düşünür). Yani, farklı modüllerde iki S ve T türü -tanımlanmışsa- ve T herkese açıksa (yani `public` ise), S'deki kod şu durumlarda T'ye erişebilir:

* S'nin modülü T'nin modülünü okuyorsa ve
* T'nin modülü T'nin paketini dışa aktarıyorsa (*export*).

Bu şekilde erişilemeyen modül sınırları (*module boundaries*) arasında başvurulan bir tür, `private` bir yöntem veya alanın (*field*) kullanılamaz olduğu gibi kullanılamaz: Bunu kullanmaya yönelik herhangi bir girişim, derleyici tarafından bir hatanın bildirilmesine veya Java sanal makinesi tarafından bir `IllegalAccessError` atılmasına veya *reflective* çalışma zamanı API'leri tarafından bir `IllegalAccessException` atılmasına neden olacaktır. Bu nedenle, bir tür `public` olarak bildirildiğinde bile, eğer paketi modül bildiriminde dışa aktarılmamışsa (yani *export* edilmemişse), yalnızca o modüldeki kodla erişilebilir olacaktır.

Modül sınırları (*module boundaries*) boyunca başvurulan bir yöntem veya alan (*field*), bu anlamda çevreleyen türü (*enclosing type*) erişilebilirse ve üyenin kendisinin beyanı da erişime izin veriyorsa erişilebilir.

Yukarıdaki modül grafiği açısından <u>güçlü kapsüllemenin</u> (*strong encapsulation*) nasıl çalıştığını görmek için, her modülü dışa aktardığı (*exports* ettiği) paketlerle etiketliyoruz:

<br/>{% picture 2023-12-28-java-module-system/module-2.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

`com.foo.app` modülündeki kod, `com.foo.bar.alpha` paketinde bildirilen `public` türlere erişebilir çünkü `com.foo.app`, `com.foo.bar` modülüne bağımlı olduğundan ve dolayısıyla `com.foo.bar`, `com.foo.bar.alpha` paketini dışa aktardığından (yani *export* ettiği için) onu okur. Eğer `com.foo.bar` dahili bir `com.foo.bar.internal` paketi içeriyorsa, `com.foo.app` içindeki kod, `com.foo.bar` onu dışa aktarmadığından (*export* etmediğinden) bu paketteki hiçbir türe erişemez. `com.foo.app` içindeki kod, `org.baz.qux` paketindeki türlere başvuruda bulunamaz çünkü `com.foo.app`, `org.baz.qux` modülüne bağımlı değildir ve bu nedenle onu okumaz(okuyamaz).

### 2.5 Implied readability (zımni/örtük/örtülü okunabilirlik)

Bir modül başka bir modül okuyorsa, bazı durumlarda mantıksal olarak diğer bazı modülleri de okuması gerekir.

Örneğin platformun `java.sql` modülü `java.logging` ve `java.xml` modüllerine bağımlıdır; bunun nedeni yalnızca bu modüllerdeki türleri kullanan uygulama kodunu içermesi değil, aynı zamanda imzaları bu modüllerdeki türlere başvuruda bulunan türleri tanımlamasıdır (*it defines types whose signatures refer to types in those modules*).

Özellikle `java.sql.Driver` arayüzü,

{% highlight java linenos %}
public Logger getParentLogger();
{% endhighlight %}

`public` yöntemini bildirir; burada **Logger**, `java.logging` modülünün dışa aktarılan (*export* edilen) `java.util.logging` paketinde bildirilen bir türdür.

Örneğin, `com.foo.app` modülündeki kodun, bir *logger* elde etmek ve ardından bir mesajı günlüğe kaydetmek için (yani *log*’lamak için) bu yöntemi çağırdığını varsayalım:

{% highlight java linenos %}
String url = ...;
Properties props = ...;
Driver d = DriverManager.getDriver(url);
Connection c = d.connect(url, props);
d.getParentLogger().info("Connection acquired");
{% endhighlight %}

`com.foo.app` modülü yukarıdaki gibi bildirilirse bu çalışmaz: `getParentLogger` yöntemi, `java.logging` modülünde bildirilen bir tür olan ve `com.foo.app` modülü tarafından okunamayan bir **Logger** döndürür ve dolayısıyla **Logger** sınıfında `info` yönteminin çağrılması hem derleme zamanında hem de çalışma zamanında başarısız olur çünkü o sınıfa ve dolayısıyla o yönteme erişilemez.

Bu soruna bir çözüm, hem `java.sql` modülüne bağımlı hem de `getParentLogger` yöntemi tarafından döndürülen **Logger** nesnelerini kullanan kod içeren her modülün her yazarının, `java.logging` modülüne bağımlılık bildirmeyi de hatırlamasını ummaktır. Bu yaklaşım elbette güvenilmez çünkü en az sürpriz ilkesini (*principle of least surprise*) ihlal ediyor: Bir modül ikinci bir modüle bağımlıysa, birinci modülü kullanmak için gereken her türün, tür ikinci modülde tanımlanmış olsa bile, yalnızca birinci modüle bağımlı olan bir modül tarafından hemen erişilebilir olmasını beklemek doğaldır.

Bu nedenle modül deklarasyonlarını, bir modülün bağlı olduğu ek modüllere (ve) ona bağlı herhangi bir modüle okunabilirlik verebilecek şekilde genişletiyoruz. Bu tür ima edilen/zımni okunabilirlik (*implied readability*), `requires` cümleciğine `transitive` değiştiricinin dahil edilmesiyle ifade edilir. `java.sql` modülünün bildirimi aslında şöyledir:

(**Benim notum :** `transitive` olan bu değiştirici önceden bu `public`'di, sonraki güncellemelerde `transitive` olarak değiştirilmiş)
{: .notice--warning}

{% highlight java linenos %}
module java.sql {
  requires transitive java.logging;
  requires transitive java.xml;
  exports java.sql;
  exports javax.sql;
  exports javax.transaction.xa;
}
{% endhighlight %}

`transitive` değiştiriciler, `java.sql` modülüne bağımlı herhangi bir modülün yalnızca `java.sql` modülünü değil aynı zamanda `java.logging` ve `java.xml` modüllerini de okuyacağı anlamına gelir. Bu nedenle, yukarıda gösterilen `com.foo.app` modülü için modül grafiği, bu modül tarafından ima edildiğinden `java.sql` modülüne yeşil kenarlarla bağlanan iki ek koyu mavi kenar içerir:

<br/>{% picture 2023-12-28-java-module-system/module-3.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

`com.foo.app` modülü artık `java.logging` ve `java.xml` modüllerinin dışa aktarılan (*export* edilen) paketlerindeki tüm `public` türlere erişen kodu içerebilir, her ne kadar deklarasyonunda bu modüllerden bahsedilmese de.

Genel olarak, eğer bir modül, imzası ikinci modüldeki bir pakete başvuruda bulunan bir türü içeren bir paketi dışarı aktarırsa(yani *export* ederse), o zaman birinci modülün bildirimi, ikinciye `requires transitive` bir bağımlılık içermelidir. Bu, ilk modüle bağlı olan diğer modüllerin ikinci modülü otomatik olarak okuyabilmesini ve dolayısıyla o modülün dışa aktarılan (*exported*) paketlerindeki tüm türlere erişebilmesini sağlayacaktır.

## 3 COMPATIBILITY & MIGRATION (uyumluluk ve migrasyon/taşıma)

Şu ana kadar modülleri sıfırdan nasıl tanımlayacağımızı, bunları modül artifekleri halinde nasıl paketleyeceğimizi ve bunları platformda yerleşik olan veya artifeklerde tanımlanan diğer modüllerle birlikte nasıl kullanacağımızı gördük.

Çoğu Java kodu, elbette, modül sisteminin tanıtımından önce yazılmıştır ve bugün olduğu gibi, değişmeden çalışmaya devam etmelidir. Bu nedenle modül sistemi, platformun kendisi modüllerden oluşsa bile, sınıf yolunda (*class path*) JAR dosyalarından oluşan uygulamaları derleyebilir ve çalıştırabilir. Ayrıca mevcut uygulamaların esnek ve kademeli bir şekilde modüler forma taşınmasını da sağlar.

### 3.1 The unnamed module (isimsiz/adlandırılmamış modül)

Bilinen hiçbir bir modülde paketi tanımlanmayan bir türün yüklenmesi için bir talepte bulunulursa, modül sistemi onu sınıf yolundan (*class path*) yüklemeye çalışacaktır. Bu başarılı olursa, her türün bir modülle ilişkili olmasını sağlamak için tür, isimsiz modül (*unnamed module*) olarak bilinen özel bir modülün üyesi olarak kabul edilir. *unnamed* modül, yüksek düzeyde, mevcut isimsiz paket (*[unnamed package](https://docs.oracle.com/javase/specs/jls/se8/html/jls-7.html#jls-7.4.2)*) kavramına benzer. Diğer tüm modüllerin elbette isimleri vardır, dolayısıyla bundan sonra bunlara isimli modüller (*named module*) diyeceğiz.

*unnamed* modül diğer tüm modülleri okur. Sınıf yolundan (*class path*’den) yüklenen herhangi bir türdeki kod, böylece diğer tüm okunabilir modüllerin dışa aktarılan (*exported*) türlerine erişebilecektir; bunlar varsayılan olarak tüm *named* yerleşik platform modüllerini içerecektir. Bu nedenle, Java SE 8 üzerinde derleyen ve çalışan mevcut bir sınıf yolu (*class-path*) uygulaması, yalnızca standart, kullanımdan kaldırılmamış (*non-deprecated*) Java SE API'lerini kullandığı sürece Java SE 9 üzerinde tam olarak aynı şekilde derlenecek ve çalışacaktır.

*unnamed* modül tüm paketlerini dışa aktarır (*export* eder). Bu, aşağıda göreceğimiz gibi esnek migrasyona/taşıma (*flexible migration*) olanak sağlar. Ancak bu, *named* modülündeki kodun *unnamed* modülündeki türlere erişebileceği anlamına gelmez. Aslında bir *named* modülü, *unnamed* modülüne bağımlılık bile beyan edemez. Bu kısıtlama kasıtlıdır, çünkü *named* modüllerinin sınıf yolunun (*class path*) keyfi içeriğine bağlı/bağımlı olmasına izin vermek <u>güvenilir konfigürasyonu</u> (*reliable configuration*) imkansız hale getirecektir.

Bir paket hem *named* modülde hem de *unnamed* modülde tanımlanmışsa, *unnamed* modüldeki paket dikkate alınmaz. Bu, sınıf yolunun (*class-path*’in) kaosu karşısında bile <u>güvenilir yapılandırmayı</u> (*reliable configuration*) korur, hâlâ her modülün belirli/verilen bir paketi tanımlayan en fazla bir modülü okumasını sağlar. Yukarıdaki örneğimizde, sınıf yolundaki (*class-path*’deki) bir JAR dosyası, `com/foo/bar/alpha/AlphaFactory.class` adında bir sınıf (*class*) dosyası içeriyorsa, `com.foo.bar.alpha` paketi `com.foo.bar` modülü tarafından dışa aktarıldığından (yani `export` edildiğinden) bu dosya hiçbir zaman yüklenmeyecektir.

### 3.2 Bottom-up migration (aşağıdan yukarı migrasyon/taşıma)

Sınıf yolundan (*class-path*’den) yüklenen türlerin *unnamed* modülün üyeleri olarak işlenmesi, mevcut bir uygulamanın bileşenlerini JAR dosyalarından modüllere inkremental, <u>aşağıdan-yukarı</u> (*bottom-up* fashion) bir şekilde taşımamıza (*migrate*) olanak tanır.

Örneğin, yukarıda gösterilen uygulamanın başlangıçta Java SE 8 için, sınıf yoluna (*class path*) yerleştirilen bir dizi benzer şekilde adlandırılmış JAR dosyaları olarak oluşturulduğunu varsayalım. Eğer bunu Java SE 9'da olduğu gibi çalıştırırsak, JAR dosyalarındaki türler *unnamed* modülde tanımlanacaktır. Bu modül, tüm yerleşik platform modülleri de dahil olmak üzere diğer tüm modülleri okuyacaktır; Basitlik açısından, bunların daha önce gösterilen `java.sql`, `java.xml`, `java.logging` ve `java.base` modülleriyle sınırlı olduğunu varsayalım. Böylece modül grafiğini elde ederiz:

<br/>{% picture 2023-12-28-java-module-system/module-4.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

`org-baz-qux.jar`’ı hemen *named* bir modüle dönüştürebiliriz, çünkü onun diğer iki JAR dosyasındaki herhangi bir türe başvuruda bulunmadığını biliyoruz, bu nedenle *named* bir modül olarak *unnamed* modülde geride bırakılacak türlerin hiçbirine başvuruda/atıfta bulunmayacaktır. (Bunu tesadüfen de olsa orijinal örnekten biliyoruz, ancak zaten bilmiyorsak, **[jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)** gibi bir araç yardımıyla keşfedebiliriz. ) `org.baz.qux` için bir modül deklarasyonu yazıyoruz, bunu modülün kaynak koduna ekliyoruz, derliyoruz ve sonucu modüler bir JAR dosyası olarak paketliyoruz. Daha sonra o JAR dosyasını modül yoluna (*module path*) yerleştirirsek ve diğerlerini sınıf yolunda (*class path*) bırakırsak, geliştirilmiş modül grafiğini elde ederiz.

<br/>{% picture 2023-12-28-java-module-system/module-5.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

`com-foo-bar.jar` ve `com-foo-app.jar` içindeki kod çalışmaya devam ediyor çünkü *unnamed* modül, artık yeni `org.baz.qux` modülünü de içeren her *named* modülü okuyor.

`com-foo-bar.jar`'ı ve ardından `com-foo-app.jar`'ı modüler hale getirmek için benzer şekilde ilerleyebiliriz, sonunda daha önce gösterilerek amaçlanan modül grafiği elde edilir:

<br/>{% picture 2023-12-28-java-module-system/module-6.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Orijinal JAR dosyalarındaki türler hakkında ne yaptığımızı bilerek, elbette üçünü de tek bir adımda modüler hale getirebiliriz. Bununla birlikte, `org-baz-qux.jar` bağımsız olarak, belki de tamamen farklı bir ekip veya organizasyon tarafından korunursa, o zaman diğer iki bileşenden önce modüler hale getirilebilir ve aynı şekilde `com-foo-bar.jar`, `com-foo-app.jar`’dan önce modüler hale getirilebilir.

### 3.3 Automatic modules (otomatik modüller)

<u>Aşağıdan-yukarıya migrasyon/taşıma</u> (*bottom-up migration*) basittir, ancak her zaman mümkün değildir. `org-baz-qux.jar`’ın bakımcısı (*maintainer*) onu henüz uygun bir modüle dönüştürmemiş olsa bile (ya da belki hiçbir zaman dönüştürmeyecek) `com-foo-app.jar` ve `com-foo-bar.jar` bileşenlerimizi yine de modüler hale getirmek isteyebiliriz.

`com-foo-bar.jar` içindeki kodun `org-baz-qux.jar` içindeki türlere başvuruda bulunduğunu zaten biliyoruz. `org-baz-qux.jar`’ı sınıf yolunda (*class path*) bırakır, `com-foo-bar.jar`’ı `com.foo.bar` adlı *named* modüle dönüştürürsek, fakat o zaman bu kod artık çalışmaz:  `org-baz-qux.jar` içindeki türler *unnamed* modülde tanımlanmaya devam edecek ancak *named* bir modül olan `com.foo.bar`, *unnamed* modüle bağımlılık bildiremez.

O halde bir şekilde `org-baz-qux.jar`’ın *named* bir modül olarak görünmesini ayarlamalıyız, böylece `com.foo.bar` ona bağlı olabilir. `org.baz.qux`’un kaynak kodunu çatallayabilir (*fork* edebilir) ve onu kendimiz modüler hale getirebiliriz, ancak eğer bakımcı (*maintainer*) bu değişikliği *upstream repository* ile birleştirmek istemezse, o zaman *fork*’a ihtiyacımız olduğu sürece onu korumak zorunda kalacağız.

Bunun yerine `org-baz-qux.jar`'ı, sınıf yolu (*class path*) yerine modül yoluna (*module path*) değiştirilmeden yerleştirerek otomatik bir modül (*automatic module*) olarak işleyebiliriz. Bu, adı `org.baz.qux` olan, JAR dosyasından türetilen bir gözlemlenebilir modül (*observable module*) tanımlayacaktır, böylece diğer otomatik olmayan modüller (*non-automatic modules*) alışılmış şekilde ona bağlı olabilir/bağlanabilir:

<br/>{% picture 2023-12-28-java-module-system/module-7.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Bir otomatik modül (*automatic module*), bir modül deklarasyonuna sahip olmadığından örtülü (*implicitly*) olarak tanımlanan *named* bir modüldür. Bunun aksine, tipik bir *named* modül, bir modül bildirimiyle açıkça (*explicitly*) tanımlanır; bundan sonra bunlara **açık modüller** (*explicit modules*) olarak değineceğiz.

Bir otomatik modülün (*automatic module*) hangi diğer modüllere bağlı olabileceğini önceden söylemenin pratik bir yolu yoktur. Bu nedenle, bir modül grafiği çözümlendikten sonra, ister otomatik (*automatic*) ister açık (*explicit*) olsun, diğer tüm *named* modülleri okumak için bir otomatik modül (*automatic module*) yapılır.

<br/>{% picture 2023-12-28-java-module-system/module-8.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

(Bu yeni okunabilirlik kenarları (*readability edges*) modül grafiğinde döngüler (*cycles*) yaratır ve bu da mantık yürütmeyi biraz daha zorlaştırır, ancak biz bunları daha esnek migrasyona/taşımaya (*more-flexible migration*) olanak sağlamanın tolere edilebilir ve genellikle geçici bir sonucu olarak görüyoruz.)

Benzer şekilde, bir otomatik modüldeki (*automatic module*) paketlerden hangisinin diğer modüller tarafından veya hala sınıf yolunda (*class path*) bulunan sınıflar tarafından kullanılmak üzere tasarlandığını söylemenin pratik bir yolu yoktur. Bu nedenle, otomatik bir modüldeki (*automatic module*) her paket, gerçekte yalnızca dahili kullanım (*internal use*) için tasarlanmış olsa bile dışa aktarılmış (*export* edilmiş) olarak kabul edilir:

<br/>{% picture 2023-12-28-java-module-system/module-9.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Son olarak, otomatik bir modüldeki (*automatic module*) dışa aktarılan (*export* edilen) paketlerden birinin, imzası başka bir otomatik modülde tanımlanan bir türe başvuruda bulunan bir tür içerip içermediğini söylemenin pratik bir yolu yoktur. Örneğin; önce `com.foo.app`’i modüler hale getirirsek ve hem `com.foo.bar` hem de `org.baz.qux`’u otomatik modüller (*automatic modules*) olarak işlersek/ele alırsak, o zaman aşağıdaki grafiği elde ederiz

<br/>{% picture 2023-12-28-java-module-system/module-10.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Karşılık gelen her iki JAR dosyasındaki tüm sınıf dosyalarını okumadan, `com.foo.bar`’daki "`public`" bir türün, dönüş tipi `org.baz.qux`'ta tanımlanan "`public`" bir yöntemi bildirip bildirmediğini bilmek imkansızdır. Bu nedenle otomatik bir modül (*automatic module*), diğer tüm otomatik modüllere (*automatic module*) zımni/örtük okunabilirlik (*implied readability*) sağlar:

<br/>{% picture 2023-12-28-java-module-system/module-11.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Artık `com.foo.app` içindeki kod `org.baz.qux` içindeki türlere erişebilir, ancak aslında öyle olmadığını biliyoruz.

Otomatik modüller (*automatic modules*), sınıf yolunun (*class path*) kaosu ile açık modüllerin (*explicit modules*) disiplini arasında bir orta yol sunar. JAR dosyalarından oluşan mevcut bir uygulamanın, yukarıda gösterildiği gibi <u>yukarıdan aşağıya</u> (*top down*) veya <u>yukarıdan aşağıya</u> (*top-down*) ve <u>aşağıdan yukarıya</u> (*bottom-up*) yaklaşımlarının bir kombinasyonuyla modüllere taşınmasına(*migrasyon*) olanak tanır. Genel olarak, sınıf yolunda (*class path*) keyfi bir JAR dosyası bileşenleri kümesiyle başlayabilir, karşılıklı bağımlılıklarını analiz etmek için *jdeps* gibi bir araç kullanabilir, kaynak kodunu kontrol ettiğimiz bileşenleri açık modüllere (*explicit modules*) dönüştürebilir ve bunları kalan JAR dosyaları ile birlikte olduğu gibi modül yoluna (*module path*) yerleştirebiliriz. Kaynak kodunu bizim kontrol etmediğimiz bileşenler için JAR dosyaları, onlar da açık modüllere (*explicit modules*) dönüştürülene kadar otomatik modüller (*automatic modules*) olarak ele alınacaktır/işlenecektir.

### 3.4 Bridges to the class path (sınıf yoluna köprüler)

Mevcut JAR dosyalarının çoğu otomatik modül (*automatic modules*) olarak kullanılabilir, ancak bazıları kullanılamaz. Sınıf yolundaki (*class path*) iki veya daha fazla "JAR dosyası" aynı pakette türler içeriyorsa, o zaman bunlardan en fazla biri otomatik bir modül (*automatic module*) olarak kullanılabilir, çünkü modül sistemi hâlâ, her *named* modülün belirli bir paketi tanımlayan en fazla bir *named* modülü okuduğunu ve benzer şekilde adlandırılmış/aynı isimli paketleri tanımlayan bu *named* modüllerin birbirine müdahale etmediğini garanti eder. Bu gibi durumlarda, genellikle JAR dosyalarından yalnızca birine gerçekten ihtiyaç duyulduğu ortaya çıkar. Eğer diğerleri kopya (*duplicates*) veya neredeyse kopya/taslak (*near-duplicates* buna *draft* denilebilir) ise ve bir şekilde yanlışlıkla sınıf yoluna (*class path*) yerleştirilmişse, o zaman biri otomatik modül (*automatic module*) olarak kullanılabilir ve diğerleri atılabilir. Bununla birlikte, sınıf yolundaki (*class path*) birden fazla JAR dosyası kasıtlı olarak aynı pakette tür içeriyorsa, o zaman bunların sınıf yolunda (*class path*) kalmaları gerekir.

Bazı JAR dosyaları otomatik modüller (*automatic modules*) olarak kullanılamadığında bile migrasyonu etkinleştirmek için, otomatik modüllerin (*automatic modules*) açık modüllerdeki (*explicit modules*) kod ile hâlâ sınıf yolunda (*class path*) bulunan kod arasında köprü görevi görmesini sağlıyoruz: Diğer tüm *named* modüllerin okunmasına ek olarak, *unnamed* modülün okunması için bir otomatik modül (*automatic module*) de yapılır. Uygulamamızın orijinal sınıf yolu (*class path*), örneğin, aynı zamanda `org-baz-fiz.jar` ve `org-baz-fuz.jar` JAR dosyalarını da içerseydi, o zaman (aşağıdaki) grafiğe sahip olurduk.

<br/>{% picture 2023-12-28-java-module-system/module-12.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

*unnamed* modül, daha önce de belirtildiği gibi tüm paketlerini dışa aktarır (*export* eder), böylece otomatik modüllerdeki (*automatic modules*) kod, sınıf yolundan (*class path*) yüklenen herhangi bir `public` türe erişebilecektir.

Sınıf yolundaki (*class path*) türleri kullanan otomatik bir modül (*automatic module*), bu türleri ona bağlı olan açık modüllere (*explicit modules*) maruz bırakmamalıdır, çünkü açık (*explicit*) modüller *unnamed* modüle bağımlılık beyan edemez. Açık modül (*explicit module*) olan `com.foo.app`'teki kod, örneğin, `com.foo.bar`'daki `public` bir türe başvuruyorsa ve bu türün imzası, hala sınıf yolunda (*class path*’te) bulunan JAR dosyalarından birindeki bir türe atıfta/başvuruda bulunuyorsa, o zaman `com.foo.app`'teki kod bu türe erişemeyecektir, çünkü `com.foo.app` *unnamed* modüle bağımlı/bağlı olamaz. Bu durum, `com.foo.app`'in geçici olarak otomatik bir modül (*automatic module*) olarak ele alınmasıyla/işlenmesiyle giderilebilir, böylece kodu (`com.foo.app`’in kodu), sınıf yolundaki (*class path*’deki) ilgili JAR dosyası otomatik bir modül (*automatic module*) olarak ele alınıncaya veya açık bir modüle (*explicit module*) dönüştürülene kadar, sınıf yolundan (*class path*) türlere erişebilir.

## 4 SERVICES (servisler/hizmetler)

Program bileşenlerinin servis arayüzleri (*service interfaces*) ve servis sağlayıcılar (*service providers*) aracılığıyla gevşek bağlanması (*loose coupling*), büyük yazılım sistemlerinin oluşturulmasında güçlü bir araçtır. Java, çalışma zamanında sınıf yolunu (*class path*) arayarak servis sağlayıcılarının (*service providers*) yerini saptayan *[java.util.ServiceLoader](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html)* sınıfı aracılığıyla uzun süredir desteklenen servislere sahiptir. Modüllerde tanımlanan servis sağlayıcılar (*service providers*) için, bu modülleri gözlemlenebilir modüller (*observable modules*) kümesi arasında nasıl yerleştireceğimizi, bağımlılıklarını nasıl çözeceğimizi ve sağlayıcıları (*providers*) ilgili servisleri kullanan kod için nasıl kullanılabilir hale getireceğimizi düşünmeliyiz.


Örneğin, `com.foo.app` modülümüzün bir MySQL veritabanı kullandığını ve

{% highlight java linenos %}
module com.mysql.jdbc {
  requires java.sql;
  requires org.slf4j;
  exports com.mysql.jdbc;
}
{% endhighlight %}

deklarasyonuna sahip gözlemlenebilir bir modülde (*observable module*) bir MySQL JDBC sürücüsü (*driver*) sağlandığını varsayalım:  burada `org.slf4j`, sürücü (*driver*) tarafından kullanılan bir günlük tutma kütüphanesidir (*logging library*) ve `com.mysql.jdbc` ise, `java.sql.Driver` servis arayüzü (*service interface*) implementasyonunu içeren bir pakettir. (Sürücü (*driver*) paketini dışa aktarmak (*export* etmek) aslında gerekli değildir ancak bunu netlik sağlamak amacıyla burada yapıyoruz.)

`java.sql` modülünün bu sürücüyü (*driver*) kullanabilmesi için, `ServiceLoader` sınıfının sürücü (*driver*) sınıfını, yansıma (*reflection*) yoluyla başlatabilmesi (*instantiate* edebilmesi) gerekir; bunun gerçekleşmesi için modül sisteminin sürücü (*driver*) modülünü modül grafiğine eklemesi ve bağımlılıklarını çözmesi gerekir, böylece:

<br/>{% picture 2023-12-28-java-module-system/module-13.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Bunu başarmak için modül sistemi, önceden-çözümlenmiş modüller aracılığıyla servislerin herhangi bir kullanımını (*uses of services*) tanımlayabilmeli ve ardından gözlemlenebilir modüller (*observable modules*) kümesi içinden sağlayıcıların (*providers*) yerini tespit edip, çözebilmelidir.

Modül sistemi, `ServiceLoader::load` yöntemlerinin çağrılması için modül artifeklerindeki sınıf dosyalarını (class files) tarayarak servislerin kullanımlarını (uses of services) tanımlayabilir, ancak bu hem yavaş hem de güvenilmez olacaktır. Bir modülün belirli bir servisi (service) kullanması, o modülün tanımının temel bir yönüdür; bu nedenle, hem verimlilik hem de netlik açısından bunu modülün beyanında **uses** cümlesiyle ifade ediyoruz:

{% highlight java linenos %}
module java.sql {
  requires transitive java.logging;
  requires transitive java.xml;
  exports java.sql;
  exports javax.sql;
  exports javax.transaction.xa;
  uses java.sql.Driver;
}
{% endhighlight %}

Modül sistemi, `ServiceLoader` sınıfının bugün yaptığı gibi, **META-INF/services** kaynak girişleri için modül artifektlarini tarayarak servis sağlayıcıları (*service providers*) tanımlayabilir. Bununla birlikte, bir modülün belirli bir servisin uygulanmasını (*implementation of a particular service*) sağlaması da aynı derecede temeldir, bu nedenle bunu modülün beyanında bir **provides** yan tümcesiyle ifade ediyoruz:

{% highlight java linenos %}
module com.mysql.jdbc {
  requires java.sql;
  requires org.slf4j;
  exports com.mysql.jdbc;
  provides java.sql.Driver with com.mysql.jdbc.Driver;
}
{% endhighlight %}

Artık sadece bu modüllerin beyanlarını/deklarasyonlarını okuyarak, birinin diğeri tarafından sağlanan bir servisi kullandığını görmek çok kolay.

Modül deklarasyonlarında servis-sağlama (*service-provision*) ve servis-kullanımı (*service-use*) ilişkilerinin bildirilmesi, gelişmiş verimlilik ve netliğin ötesinde avantajlara sahiptir. Her iki türden servis bildirimleri, servis arayüzünün (*service interface*) (örneğin, `java.sql.Driver`) bir servisin hem sağlayıcıları (*providers*) hem de kullanıcıları (*users*) tarafından erişilebilir olmasını sağlamak için derleme zamanında yorumlanabilir. Servis-sağlayıcı (*service-provider*) deklarasyonları, sağlayıcıların (*providers*) (örneğin, `com.mysql.jdbc.Driver`) beyan edilen servis arayüzlerini (*declared service interfaces*) gerçekten uygulamalarından emin olmak için daha fazla yorumlanabilir. Servis kullanım (*service-use*) deklarasyonları, son olarak, gözlemlenebilir sağlayıcıların (*observable providers*) çalışma zamanından önce uygun şekilde derlenmesini ve bağlanmasını sağlamak için erken (*ahead-of-time*) derleme ve bağlantı araçları (*compilation and linking tools*) ile yorumlanabilir.

Migrasyon/taşıma amaçları için, otomatik bir modülü (*automatic module*) tanımlayan bir JAR dosyası "**META-INF/services**" kaynak girişleri içeriyorsa, bu tür girişlerin her biri, daha sonra o modülün varsayımsal bildiriminde karşılık gelen bir **provides** cümlesiymiş gibi ele alınır/işlenir.

Bir otomatik modülün (*automatic module*) mevcut her servisi kullandığı kabul edilir.

## 5 ADVANCED TOPICS (gelişmiş konular)

Bu belgenin geri kalanı, önemli olmakla birlikte çoğu geliştiricinin ilgisini çekmeyebilecek ileri seviye konuları ele almaktadır.

### 5.1 Reflection (yansıma)

Modül grafiğini çalışma zamanında *reflection* yoluyla kullanılabilir hale getirmek için `java.lang.reflect` paketinde bir `Module` sınıfı ve `java.lang.module` adlı yeni bir pakette ilgili bazı türleri tanımlıyoruz. `Module` sınıfının bir örneği (*instance*), çalışma zamanında tek bir modülü temsil eder. Her tür bir modül içindedir, bu nedenle her `Class` nesnesinin yeni `Class::getModule` yöntemiyle döndürülen ilişkili bir `Module` nesnesi vardır.

Bir `Module` nesnesi üzerindeki temel işlemler şunlardır:

{% highlight java linenos %}
package java.lang.reflect;

public final class Module {
  public String getName();
  public ModuleDescriptor getDescriptor();
  public ClassLoader getClassLoader();
  public boolean canRead(Module target);
  public boolean isExported(String packageName);
}
{% endhighlight %}

burada `ModuleDescriptor`, örnekleri (*instance*’ları) modül tanımlayıcılarını (*module descriptors*) temsil eden `java.lang.module` paketindeki bir sınıftır; `getClassLoader` yöntemi modülün sınıf yükleyicisini (*class loader*) döndürür; `canRead` yöntemi, modülün hedef modülü okuyup okuyamayacağını söyler; ve `isExported` yöntemi, modülün verilen paketi dışa aktarıp aktarmadığını (*export* edip etmediğini) söyler.

`java.lang.reflect` paketi platformdaki tek yansıtma (*reflection*) özelliği değildir. Anatasyon işlemcilerini (*annotation processors*) ve dokümantasyon araçlarını desteklemek için derleme zamanı `javax.lang.model` paketine de benzer eklemeler yapılacaktır.

### 5.2 Reflective readability (yansıtıcı okunabilirlik)

Bir *framework*, çalışma zamanında diğer sınıfları yüklemek (*load*), incelemek (*inspect*) ve başlatmak (*instantiate*) için *reflection*’ı kullanan bir tesistir(metaforik anlamda tesis kullanılmış, ama kolaylaştırıcı gibi bir anlam çıkıyor). Java SE Platformunun kendisindeki *framework* örnekleri, servis yükleyiciler (*service loaders*), *resource bundles*, *dynamic proxy*’ler ve *serialization*’dır ve elbette *database persistence*, *dependency injection* ve *testing* gibi çeşitli amaçlara yönelik birçok popüler harici (*external*) *framework* kütüphanesi de vardır.

Çalışma zamanında keşfedilen bir sınıf göz önüne alındığında, bir *framework*’ün onu başlatmak (*instantiate*) için yapıcılarından (*constructor*) birine erişebilmesi gerekir. Ancak şu andaki duruma bakarsak, genellikle durum böyle olmayacaktır.

Platformun akış XML ayrıştırıcısı (*[streaming XML parser](https://docs.oracle.com/javase/8/docs/api/javax/xml/stream/package-summary.html)*), örneğin tanımlanmışsa, `ServiceLoader` sınıfı aracılığıyla keşfedilebilir herhangi bir sağlayıcıya (*provider*) tercihen, `javax.xml.stream.XMLInputFactory` sistem özelliği tarafından adlandırılan [XMLInputFactory](https://docs.oracle.com/javase/8/docs/api/javax/xml/stream/XMLInputFactory.html) servisinin uygulanmasını [yükler ve başlatır](https://docs.oracle.com/javase/8/docs/api/javax/xml/stream/XMLInputFactory.html#newFactory--) (load and instantiate). İstisna yönetimi (*exception handling*) ve güvenlik kontrollerini (*security checks*) göz ardı eden kod, kabaca şunu okur:


{% highlight java linenos %}
String providerName
= System.getProperty("javax.xml.stream.XMLInputFactory");
if (providerName != null) {
  Class providerClass = Class.forName(providerName, false,
  Thread.getContextClassLoader());
  Object ob = providerClass.newInstance();
  return (XMLInputFactory)ob;
}
// Otherwise use ServiceLoader
...

{% endhighlight %}

Modüler bir düzenlemede (*setting*), sağlayıcı sınıfını (*provider class*) içeren paket bağlam sınıfı yükleyicisi (*context class loader*) tarafından bilindiği sürece, `Class::forName` çağrısı (*invocation*) çalışmaya devam edecektir. Ancak, sağlayıcı sınıfının oluşturucusunun (*provider class’s constructor*), yansıtıcı (*reflective*) `newInstance` yöntemi aracılığıyla çağrılması (*invocation*) çalışmayacaktır: Sağlayıcı (*provider*) sınıf yolundan (*class path*) yüklenmiş olabilir, bu durumda *unnamed* modül içinde olacak veya bazı *named* bir modüllerde olabilir, ancak her iki durumda da *framework*’ün kendisi `java.xml` modülündedir. Bu modül yalnızca temel modüle (*base module*) bağlıdır ve bu nedenle onu okur (*read*) ve dolayısıyla başka herhangi modüldeki bir sağlayıcı sınıfı (*a provider class in any other module*) *framework* tarafından erişebilir olmayacaktır.

Sağlayıcı sınıfını (*provider class*) çerçeveye (*framework*) erişilebilir kılmak için sağlayıcının modülünü (*provider’s module*) çerçevenin modülü (*framework’s module*) tarafından okunabilir hale getirmemiz gerekir. [Bu belgenin daha önceki bir sürümünde olduğu gibi](https://openjdk.org/projects/jigsaw/spec/sotms/2015-09-08#increasing-readability), her çerçevenin çalışma zamanında modül grafiğine açıkça (*explicitly*) gerekli okunabilirlik kenarını (*necessary readability edge*) eklemesini zorunlu (*mandate*) kılabilirdik, ancak tecrübe/deneyimler, bu yaklaşımın hantal ve migrasyona bir engel olduğunu gösterdi.

Bu nedenle, bunun yerine, *reflection API*’sini, sadece bir türü yansıtan (*reflects upon*) herhangi bir kodun, o türü tanımlayan modülü okuyabilen bir modülde olduğunu varsayacak şekilde revize ediyoruz. Bu, yukarıdaki örneğin ve bunun gibi diğer kodların değişiklik olmadan çalışmasını sağlar. Bu yaklaşım <u>güçlü kapsüllemeyi</u> (*strong encapsulation*) zayıflatmaz: İster derlenmiş koddan (*compiled code*) ister yansıma (*reflection*) yoluyla olsun, tanımlayıcı modülünün (*defining module*) dışından erişilebilmesi için bir `public` type hala dışa aktarılan (*export* edilen) bir pakette olmalıdır.

### 5.3 Class loaders (sınıf yükleyiciler)

Her tür bir modüldedir ve çalışma zamanında her modülün bir sınıf yükleyicisi (*class loader*) vardır, ancak bir sınıf yükleyiciyi yalnızca bir modül mü yüklüyor? Modül sistemi, aslında, modüller ve sınıf yükleyiciler (*class loaders*) arasındaki ilişkilere çok az kısıtlama getirir. Bir sınıf yükleyici (*class loader*), modüller birbirine müdahale etmediği ve herhangi belirli bir modüldeki tüm türler yalnızca bir yükleyici (*loader*) tarafından yüklendiği sürece, bir modülden veya birçok modülden türleri yükleyebilir.

Bu esneklik, platformun yerleşik sınıf yükleyicilerinin mevcut hiyerarşisini (*platform’s existing hierarchy of built-in class loaders*) korumamıza izin verdiği için uyumluluk (*compatibility*) için kritik öneme sahiptir. Önyükleme (*bootstrap*) ve genişletme (*extension*) sınıfı yükleyicileri (*class loaders*) hala mevcuttur ve platform modüllerinden türleri yüklemek için kullanılır. Uygulama sınıf yükleyicisi (*application class loader*) de hala mevcuttur ve modül yolunda (*module path*) bulunan artifektlerden (*artifacts*) türleri yüklemek için kullanılır.

Bu esneklik, halihazırda karmaşık hiyerarşiler ve hatta özel sınıf yükleyicilerin grafiklerini (*graphs of custom class loaders*) oluşturan mevcut uygulamaları modüler hale getirmeyi de kolaylaştıracaktır.
çünkü bu tarz yükleyiciler, delegasyon paternlerini  değiştirmeye gerek kalmadan modüllerdeki *load type*’lara yükseltilebilir.

### 5.4 Unnamed modules (isimsiz/adlandırılmamış modüller)

Bir tür, adlandırılmış (*named*), gözlemlenebilir bir modülde (*observable module*) tanımlanmamışsa, o zaman isimsiz (*unnamed*) modülün bir üyesi olarak kabul edildiğini daha önce öğrenmiştik, ancak bu isimsiz (*unnamed*) modül hangi sınıf yükleyiciyle (*class loader*) ilişkilidir?

Görünüşe göre, her sınıf yükleyicisi (*class loader*), yeni, `ClassLoader::getUnnamedModule` yöntemi tarafından döndürülen kendi benzersiz adsız (*unique unnamed*) modülü vardır. Bir sınıf yükleyici (*class loader*), adlandırılmış (*named*) bir modülde tanımlanmamış bir tür yüklerse, bu türün o yükleyicinin adsız modülünde (*that loader’s unnamed module*) olduğu kabul edilir, yani, (ilgili) türün `Class` nesnesinin `getModule` yöntemi, yükleyicisinin adsız modülünü (*its loader’s unnamed module*) döndürecektir. Amiyane tabirle isimsiz modül (*unnamed module*) olarak adlandırılan bu modül, bu durumda sadece, türleri, bilinen herhangi bir modül tarafından tanımlanmayan paketlerde olduklarında, sınıf yolundan (*class path*) yükleyen, uygulama sınıfı yükleyicinin (*application class loader*) isimsiz modülüdür (*unnamed module*).

### 5.5 Layers (katmanlar)

Modül sistemi, modüller ve sınıf yükleyiciler (*class loaders*) arasındaki ilişkileri dikte etmez/zorla kurdurmaz , ancak belirli bir türü yüklemek için bir şekilde uygun yükleyiciyi (*loader*) bulabilmesi gerekir. Bu nedenle çalışma zamanında, bir modül grafiğinin başlatılması (*instantiation of a module graph*), grafikteki her modülü, o modülde tanımlanan türlerin yüklenmesinden sorumlu benzersiz sınıf yükleyiciye (*unique class loader*) eşleyen bir katman (*layer*) üretir. Önyükleme katmanı (*boot layer*), daha önce tartışıldığı gibi, uygulamanın başlangıç modülünü (*initial module*) gözlemlenebilir modüllere (*observable modules*) karşı çözerek, Java sanal makinesi tarafından başlangıçta (*startup*) oluşturulur.

Çoğu uygulama ve kesinlikle mevcut uygulamaların tümü, önyükleme katmanı (*boot layer*) dışında bir katman (*layer*) kullanmayacaktır. Bununla birlikte çoklu katman (*multiple layers*),  uygulama sunucuları (*application servers*), IDE'ler ve test koşumları/donanımları (*test harnesses*) gibi eklenti (*plug-in*) veya konteyner mimarilerine sahip gelişmiş/sofistike uygulamalarda yararlı olabilir. Bu tür uygulamalar, bir veya daha fazla modülden oluşan barındırılan uygulamaları (*hosted applications*) yüklemek (*load*) ve çalıştırmak (*run*) için şimdiye kadar açıklanan dinamik sınıf yüklemeyi (*dynamic class loading*) ve yansıtıcı modül sistemi API’sini (*reflective module-system API*) kullanabilir. Bununla birlikte, sıklıkla iki tür ek esnekliğe ihtiyaç duyulur:

* Barındırılan bir uygulama (*hosted applications*), zaten mevcut olan bir modülün farklı bir sürümünü gerektirebilir/sürümüne ihtiyaç duyabilir. Örneğin, bir Java EE web uygulaması, `java.xml.ws` modülünde bulunan JAX-WS yığınının (*JAX-WS stack*) çalışma zamanı ortamında yerleşik olanından farklı bir sürümüne ihtiyaç duyabilir. 
* Barındırılan bir uygulama (*hosted applications*), daha önce keşfedilmiş olan sağlayıcıların (*providers*) dışında servis sağlayıcılara (*service providers*) ihtiyaç duyabilir. Barındırılan bir uygulama, kendi tercih ettiği sağlayıcıları (*own preferred providers*) bile ekleyebilir. Örneğin bir web uygulaması, [Woodstox akış XML ayrıştırıcısının](https://github.com/FasterXML/woodstox) (*Woodstox streaming XML parser*) tercih edilen sürümünün bir kopyasını içerebilir, bu durumda `ServiceLoader` sınıfı diğerlerine tercihen bu sağlayıcıyı döndürmelidir.

Bir konteyner uygulaması, bu uygulamanın ilk modülünü (*initial module*) farklı bir gözlemlenebilir modül evrenine (*universe of observable modules*) karşı çözerek, mevcut bir katmanın üstünde barındırılan uygulama (*hosted application*) için yeni bir katman (*layer*) yaratabilir. Böyle bir evren yükseltilebilir platform modüllerinin (*upgradeable platform modules*) alternatif versiyonları ve alt katmanda (*lower layer*) zaten mevcut olan diğer platformu-olmayan modülleri (*non-platform modules*) içerebilir; çözümleyici bu alternatif modüllere öncelik verecektir. Böyle bir evren, alt katmanda zaten keşfedilenlerden farklı servis sağlayıcıları da içerebilir. `ServiceLoader` sınıfı, alt katmandan sağlayıcıları döndürmeden önce bu sağlayıcıları yükleyecek ve döndürecektir.

Katmanlar istiflenebilir: Önyükleme katmanının (*boot layer*) üzerine yeni bir katman oluşturulabilir ve daha sonra bunun üzerine başka bir katman oluşturulabilir. Normal çözünürlük işleminin (*resolution process*) bir sonucu olarak, belirli bir katmandaki modüller, o katmandaki veya herhangi bir alt katmandaki modülleri (*any lower layer*) okuyabilir. Bu nedenle, bir katmanın modül grafiğinin, referans olarak, altında bulunan her katmanın modül grafiklerini içerdiği düşünülebilir.

### 5.6 Qualified exports (nitelikli dışa aktarımlar)

Bazı türlerin bir dizi modül arasında erişilebilir olmasını, ancak diğer tüm modüller tarafından erişilemez kalmasını sağlamak için düzenlemeler yapmak zaman zaman gereklidir.

JDK'nın standart `java.sql` ve `java.xml` modüllerinin implementasyonlarında yer alan kod, örneğin, `java.base` modülünde bulunan dahili (*internal*) `sun.reflect` paketinde tanımlanan türleri kullanır. Bu kodun `sun.reflect` paketindeki türlere erişmesi için bu paketi `java.base` modülünden basitçe dışa aktarabiliriz (*export*):

{% highlight java linenos %}
module java.base {
  ...
  exports sun.reflect;
}
{% endhighlight %}

Bununla birlikte, her modül `java.base`'i okuduğundan, bu, `sun.reflect` paketindeki her türü her modül için erişilebilir hale getirir ve bu, bu paketteki bazı sınıflar ayrıcalıklı (*privileged*), güvenlik duyarlı (*security-sensitive*) metotlar tanımladığı için istenmeyen bir durumdur.

Bu nedenle modül deklarasyonlarını, bir paketin bir veya daha fazla "özel olarak adlandırılmış modüle" aktarılmasına izin verecek ve başkalarına izin vermeyecek şekilde genişletiyoruz. `java.base` modülünün deklarasyonu aslında `sun.reflect` paketini yalnızca belirli bir JDK modülleri kümesine aktarır:

{% highlight java linenos %}
module java.base {
  ...
  exports sun.reflect to
      java.corba,
      java.logging,
      java.sql,
      java.sql.rowset,
      jdk.scripting.nashorn;
}
{% endhighlight %}

Bu nitelikli dışarı aktarımlar (*qualified exports*) paketlerden dışa aktarıldıkları (*exported*) belirli modüllere başka türde bir kenar(*edge*) (burada altın renklendirilmiş olan) eklenerek bir modül grafiğinde görselleştirilebilir.

<br/>{% picture 2023-12-28-java-module-system/module-14.png --alt Java'da Modül Sistemi --img width="100%" height="100%" %}<br/>

Daha önce belirtilen erişilebilirlik kuralları (*accessibility rules*) şu şekilde geliştirilmiştir: İki tür S ve T farklı modüllerde tanımlanmışsa ve T herkese açıksa (yani `public` ise), S'deki kod aşağıdaki durumlarda T'ye erişebilir:

* S'nin modülü T'nin modülünü okuyorsa, ve
* T'nin modülü T'nin paketini ya doğrudan S'nin modülüne ya da tüm modüllere aktarıyorsa.

Ayrıca, bir paketin tüm modüller yerine belirli bir modüle aktarılıp aktarılmadığını söylemek için *reflective* `Module` sınıfını bir yöntemle genişletiyoruz:


{% highlight java linenos %}
public final class Module {
  ...
  public boolean isExported(String packageName, Module target);
}
{% endhighlight %}

Nitelikli dışa aktarımlar (*qualified exports*), dahili (*internal*) türleri yanlışlıkla amaçlananlar dışındaki modüller için erişilebilir hale getirebilir; bu nedenle bunların dikkatli kullanılması gerekir. Örneğin kötü niyetli biri, `sun.reflect` paketindeki türlere erişmek için bir modüle `java.corba` adını verebilir. Bunu önlemek için, inşa zamanında (*build time*) bir dizi ilgili modülü analiz edebilir ve her modülün tanımlayıcısında (*module’s descriptor*), ona bağlı olmasına izin verilen modüllerin içeriğinin karmalarını (*hashes*) kaydedebilir (*record*) ve nitelikli dışa aktarımlarını (*qualified exports*) kullanabiliriz. Çözümleme (*resolution*) sırasında, başka bir modülün nitelikli dışa aktarımında (*qualified export*) adı verilen herhangi bir modül için, içeriğindeki *hash*'in ikinci modülde o modül adı için kaydedilen *hash* ile eşleştiğini doğrularız. Nitelikli dışa aktarımlar (*qualified exports*), bunları beyan eden ve kullanan modüller bu şekilde birbirine bağlandığı sürece güvenilmeyen bir ortamda kullanmak için güvenlidir.

## SUMMARY (özet)

Burada açıklanan modül sisteminin birçok yönü/görünüşü (*facets*) vardır, ancak çoğu geliştiricinin bunlardan yalnızca bazılarını düzenli olarak kullanması gerekecektir. Modül deklarasyonları, modüler JAR dosyaları, modül yolu (*module path*), okunabilirlik (*readability*), erişilebilirlik (*accessibility*), isimsiz modül (*unnamed module*), otomatik modüller (*automatic modules*) ve modüler servisler (*modular services*) gibi temel kavramların önümüzdeki yıllarda çoğu Java geliştiricisine oldukça tanıdık gelmesini bekliyoruz. Bunun aksine, yansıtıcı okunabilirlik (*reflective readability*), katmanlar (*layers*) ve "nitelikli dışa aktarma (*qualified exports*) gibi daha gelişmiş özelliklere nispeten az sayıda kişi ihtiyaç duyacaktır.

## ACKNOWLEDGEMENTS (teşekkürler)

Bu belge Alan Bateman, Alex Buckley, Mandy Chung, Jonathan Gibbons, Chris Hegarty, Karen Kinnear, ve Paul Sandoz’dan katkılar içermektedir.

## ÇEVİRİ

Hasan Çelik (bizzat ben deniz:) )



[^1]: <[The State of the Module System](https://openjdk.org/projects/jigsaw/spec/sotms/)>






