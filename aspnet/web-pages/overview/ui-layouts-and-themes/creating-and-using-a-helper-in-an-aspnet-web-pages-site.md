---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Tworzenie i używanie Pomocnika we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik jest komponentów wielokrotnego użytku, obejmującą kodu i znaczników w celu wydajności...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: c2edc10e01f4d2358dd0b0bdf450348f01eb2bfa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811902"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Tworzenie i używanie pomocnika w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). A *Pomocnika* to składnik wielokrotnego użytku, który zawiera kod i znaczników w celu wykonania zadania, które mogą być uciążliwe lub złożonych.
> 
> **Zawartość:** 
> 
> - Jak utworzyć i używać prostych pomocnika.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
> 
> - `@helper` Składni.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Omówienie wątków

Jeśli trzeba wykonać te same zadania na różnych stronach w witrynie, można użyć pomocnika. ASP.NET Web Pages obejmuje pewną liczbę wątków, wiąże się z wielu innych, które można pobrać i zainstalować. (Lista wbudowanej pomocników stron ASP.NET Web Pages znajduje się w [interfejsu API platformy ASP.NET — krótki przewodnik](https://go.microsoft.com/fwlink/?LinkId=202907).) Jeśli żadne z istniejących pomocników odpowiada Twoim potrzebom, możesz utworzyć własne pomocnika.

Obiekt pomocnika umożliwia używanie typowych bloku kodu na wielu stronach. Załóżmy, że na stronie często chcesz utworzyć element Uwaga, który jest ustawiony, oprócz normalnych akapitach. Przykład uwagi są tworzone jako `<div>` elementu, został wstawiony jako pole z obramowaniem. Zamiast dodać ten sam kod znaczników do strony za każdym razem, gdy chcesz wyświetlić uwagi, można spakować znaczników jako obiekt pomocnika. Następnie można wstawić notatki z jednego wiersza kodu potrzebne dowolne miejsce.

Przy użyciu pomocnika, np. to sprawia, że kod w poszczególnych stron sieci prostsze i łatwiejsze do odczytania. Również ułatwia utrzymanie witryny, ponieważ jeśli trzeba zmienić wygląd uwagi, można zmienić znaczników w jednym miejscu.

## <a name="creating-a-helper"></a>Tworzenie pomocnika

Ta procedura pokazuje, jak utworzyć pomocnika, która tworzy Uwaga, jak opisano powyżej. Jest to prosty przykład, ale niestandardowego elementu pomocniczego może zawierać żadnych znaczników i kodu platformy ASP.NET, które są potrzebne.

1. W folderze głównym w witrynie sieci Web utwórz folder o nazwie *aplikacji\_kodu*. Jest to zarezerwowana nazwa folderu w programie ASP.NET, gdzie można umieścić kod dla składników, takich jak pomocników.
2. W *aplikacji\_kodu* folderze utwórz nową *.cshtml* plik i nadaj mu nazwę *MyHelpers.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Kod używa `@helper` składnia do deklarowania nowego pomocnika o nazwie `MakeNote`. Tego konkretnego pomocnika pozwala przekazać parametr o nazwie `content` , może zawierać kombinację tekstu i znaczników. Pomocnik wstawia ciąg znaków do treść notatki przy użyciu `@content` zmiennej.

    Należy zauważyć, że plik ma nazwę *MyHelpers.cshtml*, ale nosi nazwę Pomocnika `MakeNote`. Wiele niestandardowych pomocników można umieścić w jednym pliku.
4. Zapisz i zamknij plik.

## <a name="using-the-helper-in-a-page"></a>Na stronie przy użyciu Pomocnika

1. W folderze głównym Utwórz nowy pusty plik o nazwie *TestHelper.cshtml*.
2. Dodaj następujący kod do pliku:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Aby wywołać pomocnika został utworzony, należy użyć `@` nazwę pliku, gdzie jest pomocnika, kropka, a następnie nazwa pomocnika. (Jeśli masz wiele folderów *aplikacji\_kodu* folderu, można użyć składni `@FolderName.FileName.HelperName` wywołać Pomocnik żadnej zagnieżdżony poziom folderów). Tekst, który możesz dodać w znaki cudzysłowu w nawiasach jest tekst, który pomocnika będą wyświetlane jako część notatki na stronie sieci web.
3. Zapisz stronę i uruchom go w przeglądarce. Pomocnik generuje elementu notatki bezpośrednio gdzie wywoływana pomocnika: między dwoma akapitami.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce i jak Pomocnik wygenerowany kod znaczników, który umieszcza otoczkę wokół pozycji określony tekst.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Dodatkowe zasoby


[Poziomy menu jako pomocnika Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Ten wpis w blogu przez Mike'a Pope przedstawia sposób tworzenia menu poziomie jako pomoc przy użyciu znaczników, CSS i kod.

[Korzystanie z języka HTML5 we wzorcu ASP.NET Web Pages pomocników dla programu WebMatrix i wzorca ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Ten wpis w blogu przez Sam Abraham pokazuje pomocnika, który renderuje HTML5 `Canvas` elementu.

[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). W tym artykule opisano ten wpis w blogu przez Mike'a Brind `@helper` składni i `@function` składni i kiedy należy używać każdego.
