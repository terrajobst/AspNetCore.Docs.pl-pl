---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Dodawanie sieci społecznościowych w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano sposób zintegrowanie witryny z usługami sieci społecznościowych. W tym rozdziale dowiesz się, jak umożliwia użytkownikom zakładki/link witryny sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Dodawanie sieci społecznościowych do stron sieci Web platformy ASP.NET (Razor) witryn
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób dodawania łączy sieci społecznościowych dla usługi Facebook, Twitter, Reddit i Digg do stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i sposobu uwzględniania Twitter źródeł danych, karty graczy Xbox i Gravatar obrazów.
> 
> Zawartość:
> 
> - Jak umożliwia użytkownikom zakładki/link lokacji.
> - Jak dodać Kanał informacyjny usługi Twitter.
> - Jak dodać Facebook **jak** przycisku do stron.
> - Sposób renderowania Gravatar.com obrazów.
> - Jak wyświetlić karty graczy Xbox w witrynie.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteka pomocnika sieci Web platformy ASP.NET (pakiet NuGet)
>   
> 
> W tym samouczku współdziała również z 3 stron sieci Web ASP.NET, z wyjątkiem części korzystające z biblioteki pomocnika sieci Web ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Łączenie witryny sieci Web w witrynach sieci społecznościowych

Jeśli osoby lubią coś w swojej witrynie, często chcą udostępniać znajomych. Umożliwia to łatwe przez wyświetlanie symboli (ikony), które umożliwiającego udostępnianie strony na Digg, Reddit, Facebook, Twitter lub podobne witryny.

Aby wyświetlić te symbole, Dodaj `LinkSharecode` pomocnika do strony. Osób odwiedzających stronę, można kliknąć poszczególne symbolu. Jeśli użytkownicy mają konta z tej witryny sieci społecznościowych, one następnie przesłanie łącze do strony w tej witrynie.

![Obraz 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie został jeszcze dodany niego — Utwórz stronę o nazwie *ListLinkShare.cshtml* i Dodaj następujący kod:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    W tym przykładzie gdy `LinkShare` pomocnika działa, tytuł strony jest przekazywana jako parametr, który z kolei przekazuje tytuł strony w witrynie sieci społecznościowych. Jednak można przekazać w dowolny ciąg, który ma. W tym przykładzie również określa, które witrynami sieci społecznościowych w celu dołączenia do listy. Można określić witrynami sieci społecznościowych, które mają zastosowanie do witryny.
2. Uruchom *ListLinkShare.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)
3. Kliknij symbol dla jednej z witryn, które jest zalogowany w usłudze. Link prowadzi do strony w witrynie wybranej sieci społecznościowych, gdzie można udostępniać link. Na przykład kliknięcie łącza Reddit, użytkownik jest przekierowanie do `submit to reddit` w witrynie Reddit.

     ![Obraz 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Dodawanie Twitter źródła danych

Aby dowiedzieć się, jak przy użyciu Pomocnika Twitter, która jest zgodna z bieżącą wersją interfejsu API usługi Twitter, zobacz [Pomocnik usługi Twitter](../ui-layouts-and-themes/twitter-helper.md). Ten przykład przedstawia, jak napisać własne pomocnika, więc można łatwo używać kod z wielu stron.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Wyświetlania serwisu Facebook &quot;jak&quot; przycisku

W niektórych przypadkach z najlepszym rozwiązaniem jest uzyskanie kodu bezpośrednio od dostawcy sieci społecznościowej zamiast polegania na pomocnika. Jest to szczególnie w przypadku dostawcy sieci społecznościowej aktualizuje jego opcje szybciej niż pomocnika jest aktualizowany.

Aby dodać funkcje Facebook (takich jak przycisk) do swojej witryny, można pobrać fragmentów kodu z [developers.facebook.com](https://developers.facebook.com/) lokacji. W witrynie Facebook używać swoich narzędzi do generowania fragmenty kodu, które ma zastosowanie do witryny.

Następujący wyróżniony kod jest kod, który został pobrany z narzędzia takie jak przycisk w witrynie developers.facebook.com. Należy podać własne identyfikator aplikacji.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Renderowanie obrazu Gravatar

A *Gravatar* ( &quot;globalnie rozpoznanym awatara&quot;) jest obraz, który może być używany w wielu witryn sieci Web jako Awatar &#8212; oznacza to, że obraz, który reprezentuje użytkownik. Na przykład można zidentyfikować osoby w wpis na forum w komentarzu blog, Gravatar i tak dalej. (Możesz zarejestrować własne Gravatar w witrynie Gravatar w [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Jeśli chcesz wyświetlić obrazy obok nazwy lub adresy e-mail użytkowników w witrynie sieci Web, można użyć pomocnika Gravatar.

W tym przykładzie jest używany pojedynczy Gravatar, reprezentujący samodzielnie. Innym sposobem użycia Gravatar jest innym użytkownikom, podaj swój adres Gravatar po zarejestrowaniu się w witrynie. (Można poznać sposoby innym użytkownikom zarejestrowanie w [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Następnie przy każdym możesz wyświetlić informacje dotyczące tego użytkownika, do których wyświetlania nazwę użytkownika można dodać tylko Gravatar.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
2. Tworzenie nowej strony sieci web o nazwie *Gravatar.cshtml*.
3. Dodaj następujący kod do pliku: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml` Metoda Wyświetla Gravatar obrazu na stronie. Aby zmienić rozmiar obrazu, może zawierać wiele jako drugiego parametru. Rozmiar domyślny to 80. Liczby mniejszej niż 80 upewnij obrazu mniejsze. Liczby większe niż 80 powiększenia obrazu.
4. W `Gravatar.GetHtml` metody, zastąpić `<Your Gravatar account here>` z adresem e-mail używanego konta Gravatar. (Jeśli nie masz konta Gravatar, można użyć adresu e-mail osoby, która jest).
5. Uruchom strony w przeglądarce. Zostaje wyświetlona strona dwa obrazy Gravatar dla określonego adresu e-mail. Drugi obrazu jest mniejszy niż pierwszy. 

    ![Obraz 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Wyświetlanie kart graczy Xbox

Podczas osób gier Microsoft Xbox online, każdy użytkownik ma unikatowy identyfikator. Statystyki są przechowywane dla każdego player w formularzu graczy karty, która zawiera ich reputacji, wynik graczy i ostatnio odtwarzane gry. Jeśli jesteś graczy Xbox karty graczy można wyświetlać na stronach witryny sieci za pomocą `GamerCard` pomocnika.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
2. Utwórz nową stronę o nazwie *XboxGamer.cshtml* i Dodaj następujący kod znaczników.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Możesz użyć `GamerCard.GetHtml` właściwości w celu określenia alias graczy karty do wyświetlenia.
3. Uruchom strony w przeglądarce. Na stronie są wyświetlane określonej karty graczy Xbox.

    ![Obraz 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
