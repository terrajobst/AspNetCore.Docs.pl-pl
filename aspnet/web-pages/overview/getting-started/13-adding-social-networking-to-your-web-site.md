---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Dodawanie sieci społecznościowych w sieci Web platformy ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale wyjaśniono, jak zintegrowanie witryny z usługami sieci społecznościowych. W tym rozdziale dowiesz się, jak umożliwić użytkownikom/łącze zakładki witryny sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4f987c9022056ddfa3cdcac746f688f562d9003e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398404"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Dodawanie sieci społecznościowych do wzorca ASP.NET Web Pages (Razor) witryn
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak dodać łącza sieci społecznościowej Facebook, Twitter, Reddit i Digg do stron w witrynie internetowej ASP.NET Web Pages (Razor) i sposobu uwzględniania kanały informacyjne usługi Twitter, karty gracza konsoli Xbox i Gravatar obrazów.
> 
> Zawartość:
> 
> - Jak osoby zakładki/łącza lokacji.
> - Jak dodać Kanał informacyjny usługi Twitter.
> - Jak dodać usługi Facebook **takich jak** przycisku do stron.
> - Jak renderować Gravatar.com obrazów.
> - Jak wyświetlić kartę gracza konsoli Xbox w swojej witrynie.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteka pomocnicza sieci Web platformy ASP.NET (pakiet NuGet)
>   
> 
> W tym samouczku współpracuje również z 3 stron sieci Web platformy ASP.NET, z wyjątkiem części, korzystające z biblioteki pomocnika sieci Web platformy ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Łączenie witryny sieci Web w witrynach sieci społecznościowych

Jeśli osoby, takich jak coś w swojej witrynie, często chcą udostępniać znajomym. Umożliwia to łatwe, wyświetlając symbole (ikony), które można kliknąć, aby udostępnić strony w Digg, Reddit, Facebook, Twitter lub podobne witryny.

Aby wyświetlić te symbole, Dodaj `LinkSharecode` element pomocniczy służący do strony. Osób odwiedzających stronę kliknąć poszczególne symbol. Jeśli użytkownicy mają konta przy użyciu sieci społecznościowych lokacji one wpis łącze do strony w w danej lokacji.

![Obraz 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie dodano go — Tworzenie strony o nazwie *ListLinkShare.cshtml* i Dodaj następujący kod:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    W tym przykładzie po `LinkShare` pomocnika przebiegów, tytuł strony jest przekazywany jako parametr, który z kolei przekazuje tytuł strony do witryny sieci społecznościowych. Można jednak przekazać w dowolny ciąg, który ma. Ten przykład pokazuje również, które witrynami sieci społecznościowych do uwzględnienia na liście. Można określić witrynami sieci społecznościowych, które mają zastosowanie do witryny.
2. Uruchom *ListLinkShare.cshtml* strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.)
3. Kliknij symbol dla jednej z witryn, które po zarejestrowaniu się. Łącze prowadzi do strony w witrynie wybranej sieci społecznościowych, gdzie możesz udostępnić łącze. Na przykład jeśli klikniesz Reddit link, nastąpi przekierowanie do `submit to reddit` na stronie witryny Reddit.

     ![Obraz 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Dodawanie usługi Twitter kanału informacyjnego

Aby dowiedzieć się, jak za pomocą Pomocnik usługi Twitter, która jest zgodna z bieżącą wersją interfejsu API usługi Twitter, zobacz [Pomocnik usługi Twitter](../ui-layouts-and-themes/twitter-helper.md). W tym przykładzie pokazano, jak napisać własne pomocnika, dzięki czemu można z łatwością wykorzystać kod z wielu stronach.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Wyświetlanie Facebook &quot;takich jak&quot; przycisku

W niektórych przypadkach najlepszym rozwiązaniem jest Pobierz kod bezpośrednio z dostawcę sieci społecznościowych, a nie opierając się na pomocnika. Jest to szczególnie istotne, jeśli dostawcy sieci społecznościowej aktualizuje jego opcji przekraczało pomocnika jest aktualizowana.

Aby dodać funkcje serwisu Facebook (takich jak przycisk) do lokacji, możesz pobrać fragmentów kodu z [developers.facebook.com](https://developers.facebook.com/) lokacji. Na stronie usługi Facebook umożliwia swoich narzędzi Generowanie fragmentu kodu, która jest odpowiednia dla Twojej witryny.

Następujący wyróżniony kod jest kod, który został pobrany przy użyciu narzędzia takie jak przycisk w witrynie developers.facebook.com. Należy podać własne identyfikator aplikacji.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Renderowanie obrazu Gravatar

A *Gravatar* ( &quot;rozpoznany globalnie awatara&quot;) jest obrazem, które mogą być używane w wielu witrynach sieci Web jako swojego awatara &#8212; oznacza to, obraz, który reprezentuje użytkownik. Na przykład można zidentyfikować osobę w wpis na forum, komentarza z blogu Gravatar i tak dalej. (Możesz zarejestrować własne Gravatar w witrynie internetowej Gravatar na [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Jeśli chcesz wyświetlić obrazy obok nazwy lub adresy e-mail osób w witrynie sieci Web, można użyć pomocnika Gravatar.

W tym przykładzie jest używany pojedynczy Gravatar, który reprezentuje samodzielnie. Innym sposobem użycia Gravatar jest innym użytkownikom, podaj swój adres Gravatar po zarejestrowaniu się w witrynie. (Możesz dowiedzieć się, jak umożliwić użytkownikom rejestrowanie w [Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) Następnie zawsze wtedy, gdy możesz wyświetlić informacje dotyczące tego użytkownika, można po prostu dodać Gravatar, do których wyświetlania nazwy użytkownika.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
2. Utwórz nową stronę sieci web o nazwie *Gravatar.cshtml*.
3. Dodaj następujący kod do pliku: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml` Metoda Wyświetla obraz Gravatar na stronie. Aby zmienić rozmiar obrazu, może zawierać wiele jako drugiego parametru. Domyślny rozmiar to 80. Liczby mniejszej niż 80 upewnij obraz mniejsze. Liczby większe niż 80 powiększyć obraz.
4. W `Gravatar.GetHtml` metody, zastąpić `<Your Gravatar account here>` przy użyciu adresu e-mail używanego konta Gravatar. (Jeśli nie masz konta Gravatar, można użyć adresu e-mail osoby, która jest).
5. Uruchom stronę w przeglądarce. Na stronie są wyświetlane dwa obrazy Gravatar adresu e-mail, wskazana. Drugi obraz jest mniejszy niż pierwszy. 

    ![Obraz 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Wyświetlanie kart gracza konsoli Xbox

Gdy użytkownicy grają Microsoft Xbox online, każdy użytkownik ma unikatowy identyfikator. Statystyki są przechowywane przez każdy z graczy w postaci karty świąteczne, co pokazuje ich reputacji, oceny gracza i ostatnio odtwarzane gry. Jeśli graczy na konsoli Xbox karty świąteczne można wyświetlać na stronach w witrynie za pomocą `GamerCard` pomocnika.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
2. Utwórz nową stronę o nazwie *XboxGamer.cshtml* i Dodaj następujący kod znaczników.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Możesz użyć `GamerCard.GetHtml` właściwości w celu określenia aliasu dla graczy karty mają być wyświetlane.
3. Uruchom stronę w przeglądarce. Zostanie wyświetlona strona karty gracza konsoli Xbox, określone.

    ![Obraz 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
