---
title: "Git Sorun Giderme (Git Troubleshooting) Bölüm 2 - Git Bisect Kullanımı"
comments: false
excerpt: "Git bisect komutu nedir? Commit geçmişinizde hataya ilk neden olan taahhüdü(commit) nasıl bulunur?"
header:
  teaser: "/assets/images/svg-book22.svg"
  og_image: /assets/images/svg-book22.svg
  overlay_image: /assets/images/svg-book22.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
categories:
  - git-sorun-giderme
tags:
  - git
  - git troubleshooting
  - git bisect
#last_modified_at: 2023-01-06T15:12:19-04:00
toc: true
toc_sticky: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Git-Bisect Nedir?

Git versiyon kontrol sisteminde **bisect komutu**, çoğunlukla projenizin geçmişinde hataya ilk neden olan taahhüdü(commit) bulmak için kullanır. Bunu, önce "**bug(hata)**" içerdiğini bildiğiniz bir taahhüdü(commit) "**bad**(kötü)" olarak işaretleyerek yaparsınız. Ve bu hata ortaya çıkmadan önce sorunsuz çalıştığını bildiğiniz bir taahhütü ise "**good**(iyi)" olarak işaretlersiniz. Bisect komutu, Git'teki taahhütler arasında **ikili arama(binary search)** algoritması uygular.




{% highlight bash %}
GIT-BISECT(1)                                         Git Manual                                         GIT-BISECT(1)

NAME
       git-bisect - Use binary search to find the commit that introduced a bug

SYNOPSIS
       git bisect <subcommand> <options>

DESCRIPTION
       The command takes various subcommands, and different options depending on the subcommand:

           git bisect start [--term-{new,bad}=<term> --term-{old,good}=<term>]
                            [--no-checkout] [--first-parent] [<bad> [<good>...]] [--] [<paths>...]
           git bisect (bad|new|<term-new>) [<rev>]
           git bisect (good|old|<term-old>) [<rev>...]
           git bisect terms [--term-good | --term-bad]
           git bisect skip [(<rev>|<range>)...]
           git bisect reset [<commit>]
           git bisect (visualize|view)
           git bisect replay <logfile>
           git bisect log
           git bisect run <cmd>...
           git bisect help

{% endhighlight %}


<br/>Tabii bu işaretlemelerden önce bisect state'ine girmemiz gerekmektedir. Bunun için ise ``bisect start`` komutunu kullanırız.

<!-- Bisect'i bir şeyin ilk kez nerede tanıtıldığını bulmak için kullanabilirsiniz -->

## Git bisect start

Bisect state'ini başlatmanın birkaç yolu vardır. Bunlardan biri ``git bisect start`` komutudur.

{% highlight bash %}
% git bisect start
status: waiting for both good and bad commits
{% endhighlight %}

Görüleceği üzere bu komutun çıktısı olarak "**waiting for both good and bad commits (hem iyi hem de kötü taahhütler bekleniyor)**" şeklinde bir durum mesajı alıyoruz. Bu mesaj sonrasında düzgün çalıştığını bildiğiniz bir commit'e "**good**", düzgün çalışmadığını bildiğiniz bir commit'e ise "**bad**" işaretini verebiliriz. Bunun nasıl yapıldığına birazdan geleceğim.

Diğer bir bisect başlatma yöntemi ise aramanın yapılacağı aralığı tanımlayarak yapılan bisect komutudur.

{% highlight bash %}
git bisect start HEAD HEAD~15
{% endhighlight %}

Bu komut otomatik olarak **HEAD**'i **bad** commit, **HEAD~15**'i yani **HEAD**'den 15 commit önceki commit'i ise **good** commit olarak işaretler. Tabii ki 15 commit öncesinin sorunsuz çalışan bir commit olduğunu biliyor olmanız gerekmektedir.

## Git bisect bad

``git bisect start`` komutu kullanarak bisect state'ini başlattığımızı varsayalım. Görüleceği üzere, bisect bizden **bad** ve **good** commit'leri belirlememizi bekler. Farzedelim ki son commit'imiz düzgün çalışmıyor. Ama hatanın da ilk nerede başladığını bulmak istiyoruz.

{% highlight bash %}
% git bisect bad HEAD
status: waiting for good commit(s), bad commit known
{% endhighlight %}

<br/><br/><img src="{{ site.url }}{{ site.baseurl }}/assets/images/git-sorun-giderme/2022-12-27-git-bisect/1.svg"  width="100%" height="100%" loading="lazy" alt="git-bisect usage(git-bisect kullanımı)"><br/><br/>

Son commit'imiz olan **versiyon 10**'u **bad** olarak işaretlediğimizde, komut sonrası durum mesajımız, "**waiting for good commit(s), bad commit known**"(bad commit biliniyor, good commit'ler bekleniyor) şeklinde olacaktır. Tabii ki bu komutu HEAD yazmadan da, yani ``git bisect bad`` şeklinde de yazabilirdik. Bu komut, otomatik olarak HEAD işaretçisinin olduğu commit'i **bad** olarak işaretler.

## Git bisect good

Aşağıdaki komut vasıtasıyla HEAD işaretçisinin bulunduğu commit'ten 10 commit geriye gitmek istiyorum.

{% highlight bash %}
% git checkout HEAD~10
{% endhighlight %}

Kodun bu versiyonunun düzgün çalıştığını varsayalım. ``git bisect good`` komutu ile bu commit'i **good** olarak işaretliyorum.

{% highlight bash %}
% git bisect good     
Bisecting: 4 revisions left to test after this (roughly 2 steps)
[red45b3f1b1df85803b5afcb6cfb65d773edb9b74c] commit-5 commit
{% endhighlight %}

<br/><br/><img src="{{ site.url }}{{ site.baseurl }}/assets/images/git-sorun-giderme/2022-12-27-git-bisect/2.svg"  width="100%" height="100%" loading="lazy" alt="git-bisect usage(git-bisect kullanımı)"><br/><br/>

Bu, bisect'in bu revizyonu **good** olarak işaretlemesine ve ardından bizim değerlendirmemiz için bad ve good revizyonları arasında, kabaca ortalarda bir taahhüt döndürmesine neden olur. Yani koddan da anlaşılacağı üzere **versiyon 1**'i **good** olarak işaretlediğimizde ikili arama algoritması bize **commit-5**'i rastgele döndürür.

Bisect kendini, otomatik olarak **versiyon 5**'i temsil eden commit-5'e çekeceği için ekstra bir işlem yapmama gerek olmadan tekrardan ``git bisect good`` veyahut ``git bisect bad`` komutlarını yazabilirim.

Farzedelim ki commit-5'in, yani resimden de anlaşılacağı üzere **version 5**'in düzgün çalıştığına karar verdik. Şayet bu versiyonu **good** olarak işaretlersek, **commit-5** ve öncesindeki bütün commit'ler otomatik olarak **good** olarak işaretlenir. Akabinde ikili arama algoritması bu sefer **version 5** ile **version 10** arasındaki rastgele bir commit'i döndürecektir.

{% highlight bash %}
% git bisect good
Bisecting: 2 revisions left to test after this (roughly 1 step)
[82aa19f8078952678433ef196a0ad146b213b33c] commit-7 commit
{% endhighlight %}

<br/><br/><img src="{{ site.url }}{{ site.baseurl }}/assets/images/git-sorun-giderme/2022-12-27-git-bisect/3.svg"  width="100%" height="100%" loading="lazy" alt="git-bisect usage(git-bisect kullanımı)"><br/><br/>

Görüleceği bu sefer commit-7'yi temsil eden **versiyon 7** rastgele döndürülüyor. Farzedelim ki bu commit düzgün çalışmıyor ve **bad** olarak işaretlemek istiyoruz. Bu seferde **version 7** ile **versiyon 10** arasındaki commitler otomatikman **bad** olarak işaretlenecek ve geriye son commit olan commit-6, bisect tarafından rastgele döndürülecektir.

{% highlight bash %}
% git bisect bad
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[c3f227a178fbfdf21640c972591dbbe7435f72fa] commit-6 commit
{% endhighlight %}

<br/><br/><img src="{{ site.url }}{{ site.baseurl }}/assets/images/git-sorun-giderme/2022-12-27-git-bisect/4.svg"  width="100%" height="100%" loading="lazy" alt="git-bisect usage(git-bisect kullanımı)"><br/><br/>

Diyelim ki **versiyon 6**'yı da test ettik ve düzgün çalışmadığına hükmettik. Bunu da **bad** olarak işaretlersek hatanın ilk çıktığı noktayı bulmuş oluruz. Çünkü **version 5** ve öncesinin **good** olarak işaretli olduğunu biliyoruz.

{% highlight bash %}
% git bisect bad
c3f227a178fbfdf21640c972591dbbe7435f72fa is the first bad commit
commit c3f227a178fbfdf21640c972591dbbe7435f72fa
Author: cortix <hsnclk1985@gmail.com>
Date:   Thu Dec 29 03:13:56 2022 +0300

    commit-6 commit

 SelectionSort.java | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

{% endhighlight %}

<br/><br/><img src="{{ site.url }}{{ site.baseurl }}/assets/images/git-sorun-giderme/2022-12-27-git-bisect/5.svg"  width="100%" height="100%" loading="lazy" alt="git-bisect usage(git-bisect kullanımı)"><br/><br/>


## Git bisect visualize

{% highlight bash %}
% git bisect visualize
{% endhighlight %}

Bu komut sayesinde ise ilgili commit'i **gitk** yardımcı programı aracılığı ile görselleştirebilirsiniz. Bu sayede commit hakkında daha detaylı bilgi alabilirsiniz. Örneğin hataya ilk neden olan kişinin kullanıcı bilgilerini de bu komut sayesinde görüntüleyebilirsiniz.


## Git bisect log

{% highlight bash %}
% git bisect log
{% endhighlight %}

commit'leri **good** veya **bad** olarak işaretledikten sonra, şu ana kadar yapılanları göstermek için ``git bisect log`` komutunu kullanabilirsiniz.

## Git bisect reset

Hataya neden olan ilk commit'i bulduktan sonra bisect state'inden çıkmak isteyeceksiniz. Bunun için ise ``git bisect reset`` komutunu kullanabilirsiniz.

{% highlight bash %}
% git bisect reset
Previous HEAD position was c3f227a commit-6 commit
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
{% endhighlight %}


## Git bisect run

Diyelim ki kodunuzun çalışıp çalışmadığını test edecek bir script'te sahipsiniz;

{% highlight bash %}
git bisect run my_script arguments
{% endhighlight %}

bu komutu kullanarak **bad** ve **good** işaretleme işlemlerini otomatize edebilirsiniz. Yalnız, script'inizi çalıştırdığınızda kodunuz iyiyse(yani **good/old** ise) script'i **0** koduyla, kodunuz kötüyse(yani **bad/new** ise) **125** dışında **1** ile **127 (dahil)** arasında bir kodla döndürmeniz gerektiğini unutmayın. ``git bisect run`` komutuyla ilgili detaylı bilgi için, ``git bisect --help`` komutu vasıtasıyla detaylı bilgi alabilirsiniz.

Geçerli kaynak kodu <u>test edilemediğinde</u> **125** rakamı çıktı olarak döndürülmelidir. Komut dosyası bu kodla çıkarsa, geçerli revizyon atlanır (bunun için ``git bisect skip`` bölümüne bakın).

**Not:** Bu arada **good** yerine **old**, **bad** yerine ise **new** işaretini de kullanabilirsiniz.
{: .notice--warning}

## Git bisect komutunun avantajları

Yukarıdaki örnekten de görüleceği üzere hatanın ilk ortaya çıktığı yeri 10 commit'i de tek tek test etmek yerine yaklaşık 3-4 adımda belirlemiş olduk. Bu aralık 10 yerine 100 veyahut 1000 commit'ten de oluşuyor olabilirdi. Git bisect, **ikili arama(binary search)** algoritması sayesinde test sayısını dramatik bir şekilde azaltır.


## Referanslar

* [Common UNIX Utilities](http://parallel.vub.ac.be/documentation/linux/unixdoc_download/Utilities.html)
* [Professional Git](https://www.amazon.com/Professional-Git-Brent-Laster/dp/111928497X)
* [Git documentation](https://git-scm.com/doc)
