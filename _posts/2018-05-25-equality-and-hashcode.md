---
title: "Equality ve Hashcode"
comments: true
excerpt: "Madde 10 - EQUALS Metodunu Geçersiz Kılarken(*Override*) Genel Sözleşmelere Uymak"
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-1.jpg
  overlay_filter: rgba(255, 0, 0, 0.5)
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - Java
  - Effective_Java 
tags:
  - java
  - equals
  - hashCode
last_modified_at: 2018-05-26T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Bu makale Effective Java kitabı - Madde10'un anlaşılması için hazırlanmış olup, bu konuyu desteklemek için bir çok farklı makaleden de alıntı yapılmıştır. İlgili maddeyi hem çevirmeye hemde yorumlamaya çalışacağım. Kullanılan her bir makale referans olarak eklenmiştir. Yorumların olduğu bölümler italik yazı tipiyle gösterilmiş ve konunun daha iyi anlaşılması sağlanmıştır.
{: .notice}

## Genel Bakış

Eşitlik yöntemini geçersiz kılmak basit gibi görünür, ancak durumu yanlış anlamanın birçok yolu vardır ve sonuçlar gerçekten korkunç olabilir. Tabiki sorunlardan kaçınmanın en kolay yolu, eşitlik yöntemini geçersiz kılmamaktır; bu durumda, sınıfın her bir örneği yalnızca kendisine eşit olacaktır. Bu, aşağıdaki koşullardan herhangi biri geçerliyse, yapılacak en doğru şeydir. *Aşağıdaki durumların varolması halinde equals yöntemini geçersiz kılmaya gerek yoktur.* **ref19**



* **Sınıfın her örneği yaradılıştan benzersizdir.** Bu, "değerler(_values_)" yerine "aktif varlıkları(_active entities_)" temsil eden *Thread* gibi sınıflar için geçerlidir. *Object* sınıfı tarafından sağlanan `equals` uygulaması(_implementation_), bu sınıflar için tam olarak doğru davranışa sahiptir. *Bu gibi durumlarda Object sınıfının equals yöntemi beklentilerimizi karşılamaktadır, yani equals metodunu tekrardan geçersiz kılmaya(override) gerek yoktur.*



* **Sınıfın “mantıksal eşitlik(logical equality)” testi sağlamasına gerek yoktur.** Örneğin, `java.util.regex.Pattern`, iki *Pattern* örneğinin tam olarak aynı düzenli ifadeyi temsil edip etmediğini kontrol etmek için `equals` yöntemini geçersiz kılmış(override) olabilirdi, ancak tasarımcılar kullanıcıların bu işlevselliğe ihtiyaç duyacağını veya isteyeceğini düşünmez. Bu koşullar altında, *Object* sınıfından miras alınan `equals` uygulaması(_implementation_) idealdir.

* **Bir üst sınıf(superclass) zaten** `equals` **yöntemini geçersiz kılmıştır(override) ve davranışı bu sınıf için uygundur.** Örneğin, çoğu *Set* uygulaması(_implementation_) kendi `equals` yöntemini *AbstractSet*'den', *List* uygulaması(_implementation_) *AbstractList*'den, *Map* uygulaması da *AbstractMap*'den miras alır.

* **Sınıf özel(private) veya paket-özel(package-private) ise** `equals` **yönteminin çağrılmayacağından eminsinizdir.** Son derece riskten kaçınan biriyseniz, yanlışlıkla çalıştırılmadığından emin olmak için `equals` yöntemini geçersiz kılabilirsiniz:


{% highlight java %}
@Override public boolean equals(Object o) {
    throw new AssertionError(); // Method is never called
}
{% endhighlight %}

Öyleyse ``equals`` metodunu geçersiz kılmak ne zaman uygun olur? Doğru zaman, bir sınıfın, yalnızca nesne kimliğinden farklı ve bir üst sınıfının zaten ``equals`` yöntemini geçersiz kılmadığı mantıksal eşitlik kavramına sahip zamandadır. Bu genellikle değer sınıfları(_value-based classes_) için geçerlidir. Bir değer sınıfı, `Integer` veya `String` gibi bir değeri temsil eden bir sınıftır. `equals` yöntemini kullanarak değer nesnelerine yapılan referansları karşılaştıran bir programcı, aynı nesneye başvurup başvurmadıklarını değil, mantıksal olarak eşdeğer olup olmadığını bulmayı bekler. `equals` yönteminin geçersiz kılınması sadece programcının beklentilerinin karşılanması için gerekli değildir aynı zamanda örneklerin, öngörülebilir, istenen davranışları olan _map keys_ ya da _set elements_ olarak kullanılmasını sağlar.

> Java'nın yaratıcıları bunun iyi bir fikir olacağını düşündüklerinden değil, başka bir seçenek olmadığı için bu yöntemleri geçersiz kılıyoruz.

Eşitlik yöntemini geçersiz kılmaya gerek görmeyen bir tür değer sınıfı, her bir değerde en fazla bir nesnenin oluştuğundan emin olmak için örnek kontrolü(Madde 1) kullanan bir sınıftır. Enum türleri (Madde 34) bu kategoriye girer. Bu sınıflar için, mantıksal eşitlik(_logical equality_), nesne kimliği ile aynıdır, bu yüzden `Object`'in `equals` yöntemi, mantıksal `equals` yöntemi olarak işlev görür.

`equals` yöntemini geçersiz kıldığınızda, genel sözleşmesine uymalısınız. `Object` için şartnamaden olan sözleşme işte burada:

`equals` yöntemi bir denklik ilişkisini uygular. Bu özelliklere sahiptir:


> Değer sınıflarından kasıt, sadece bir değerler kümesini barındıran ve çok az veya hiç mantık içermeyen nesneler anlamına gelir. Tipik olarak içereceği tek mantıksal değer nesneleri, nesnelerin koleksiyonlarda kullanılabilmesi için equals () ve hashCode () gibi şeyler olacaktır. **ref20**
>
> Logical Equality versus Object Reference Equality - **ref21**
>
> Statik fabrika yöntemlerinin aynı nesneyi tekrarlayan çağrılarından geri döndürme yetenekleri, sınıfların herhangi bir zamanda hangi örneklerin var olduğu konusunda sıkı kontrolü sürdürmesini sağlar. Bunu yapan sınıfların örnek kontrollü(_instance-controlled_) olduğu söylenir.(Daha detaylı bilgi için Madde-1'e göz atmanızda yarar var.)
>
> Object Identity and Equality in Java ---> **ref22**


## [](#header-2)Eşitliğin beklenen özellikleri nedir?

*1)Reflexive(Dönüşlü)*
* *Null* olmayan herhangi bir referans değeri *x* için, ``x.equals(x)`` `true` döndürülmelidir.
{% highlight java %}
x.equals(x) == true
{% endhighlight %}

*2)Symmetric(Simetrik)*
* *Null* olmayan herhangi bir referans değeri *x* ve *y* için, ``x.equals(y)``, yalnızca ``y.equals(x)`` öğesi `true` değerini döndürürse, `true` değerini döndürmelidir.
{% highlight java %}
x.equals(y) ⇔ y.equals(x)
{% endhighlight %}

*3)Transitive(Geçişli)*
* *Null* olmayan herhangi bir referans değeri *x*, *y* ve *z* için, ``x.equals(y)`` `true` değerini döndürür ve ``y.equals(z)`` `true` değerini döndürürse, ``x.equals(z)`` `true` değerini döndürmelidir.

{% highlight java %}
x.equals(y) ∧ y.equals(z) ⇒ x.equals(z)
{% endhighlight %}

*4)Consistent(İstikrarlı)*
* *Null* olmayan herhangi bir referans değeri *x* ve *y* için, ``x.equals(y)`` 'nin birden çok çağrılması, *equals* karşılaştırmalarında kullanılan hiçbir bilginin değiştirilmemesi şartıyla, istikrarlı olarak `true` ya da istikrarlı olarak `false` döndürmelidir.

*5)*
* *Null* olmayan herhangi bir referans değeri *x* için, ``x.equals(null)`` false döndürmelidir.

Matematiksel eğimli olmadığınız sürece, bu biraz korkutucu görünebilir, ama görmezden gelmeyin! Bunu ihlal ederseniz, programınızın düzensiz davrandığını veya bozulacağını görebilirsiniz ve bu, başarısızlığın kaynağını saptamak için çok zor olabilir. John Donne'u(şair) açıklamak için, hiçbir sınıf bir ada değildir. Bir sınıfın örnekleri sıklıkla diğerine geçirilir. Tüm koleksiyon sınıfları dahil olmak üzere birçok sınıf, `equals` sözleşmesine uyarak kendilerine aktarılan nesnelere bağlı olur.

Artık eşitlik sözleşmesini ihlal etmenin tehlikelerinin farkında olduğunuza göre, sözleşmeyi ayrıntılı olarak inceleyelim. İyi haber şu ki, görünüşe rağmen, gerçekten çok karmaşık değildir. Bir kere anladıktan sonra, sözleşmeye uymak o kadar da zor değil.

Öyleyse bir denklik ilişkisi nedir? Basitçe konuşursak, bir dizi öğeyi, öğeleri birbirine eşit kabul edilen alt kümelere bölen bir operatördür. Bu alt kümeler, denklik sınıfları(*equivalence classes*) olarak bilinir. `equals` yönteminin kullanışlı olması için, *denklik sınıfında* ki öğelerin tümü, kullanıcının bakış açısıyla değiştirilebilir olmalıdır. Şimdi sırayla beş koşulu inceleyelim:

### Reflexive (Dönüşlü)
İlk şart, sadece bir nesnenin kendisine eşit olması gerektiğini söylüyor. Bunu istemeden ihlal etmeyi hayal etmek zor. Eğer bunu ihlal ederseniz ve sınıfınızın bir örneğini bir koleksiyona eklerseniz, `contains` yöntemi, koleksiyonun yeni eklediğiniz örneği içermediğini söyleyebilir.

### Symmetry (Simetrik)
İkinci şart, herhangi iki nesnenin eşit olup olmadığına karar vermesi gerektiğini söyler. İlk şartın aksine, bunu istemeden ihlal etmeyi hayal etmek zor değil. Örneğin, büyük / küçük harf duyarlı olmayan bir *string*'i uygulayan aşağıdaki sınıfı düşünün. *string*'in durumu `toString` tarafından korunur, ancak `equals`  karşılaştırmalarında göz ardı edilir:

{% highlight java %}
// Broken - violates symmetry!

public final class CaseInsensitiveString {

    private final String s;



    public CaseInsensitiveString(String s) {

        this.s = Objects.requireNonNull(s);

    }



    // Broken - violates symmetry!

    @Override public boolean equals(Object o) {

        if (o instanceof CaseInsensitiveString)

            return s.equalsIgnoreCase(

                ((CaseInsensitiveString) o).s);

        if (o instanceof String)  // One-way interoperability!

            return s.equalsIgnoreCase((String) o);

        return false;

    }

    ...  // Remainder omitted

}
{% endhighlight %}

Bu sınıftaki iyi niyetli `equals` yöntemi, sıradan *strings*'lerle birlikte çalışmayı saf bir şekilde dener. Büyük /küçük harf duyarsız ve sıradan bir *string*'e sahip olduğumuzu varsayalım:

{% highlight java %}
CaseInsensitiveString cis = new CaseInsensitiveString("Polish");

String s = "polish";
{% endhighlight %}

Beklendiği gibi, `cis.equals(s)` `true` döndürür. Sorun şu ki, *CaseInsensitiveString* deki `equals` yöntemi sıradan *string*'ler hakkında bilgi sahibiyken, *String* deki `equals` yöntemi, büyük / küçük harf duyarlı olmayan *string*'lere bihaberdir. Bu nedenle, ``s.equals(cis)`` açık bir simetri ihlalinde, `false` döndürür. Bir koleksiyona büyük / küçük harf duyarlı olmayan bir *string* koyduğunuzu varsayalım:

{% highlight java %}
List<CaseInsensitiveString> list = new ArrayList<>();

list.add(cis);
{% endhighlight %}

``list.contains(s)`` bu noktada ne döndürüyor? Kim bilir? Şanslıyız ki, mevcut OpenJDK uygulamasında(_implementation_), `false` döndürüyor, fakat bu sadece bir uygulama ara ürünüdür.(*implementation artifact*). Başka bir uygulamada, kolayca `true` döndürebilir veya bir çalışma zamanı istisnası fırlatabilir. Bir kere`equals` sözleşmesini ihlal ettiğinizde,
