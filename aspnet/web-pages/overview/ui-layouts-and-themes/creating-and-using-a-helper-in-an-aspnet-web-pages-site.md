---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "Tworzenie i używanie pomocnika w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano sposób tworzenia pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Pomocnik jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wydajności..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Tworzenie i używanie pomocnika w witryny ASP.NET Web Pages (Razor)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób tworzenia pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). A *Pomocnika* jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wykonania zadania, które mogą być niewygodny lub złożonych.
> 
> **Zawartość:** 
> 
> - Sposób tworzenia i używania prostego pomocnika.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - `@helper` Składni.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Omówienie wątków

Jeśli trzeba wykonać te same zadania na różnych stronach w swojej witrynie, można użyć pomocnika. Strony sieci Web ASP.NET zawiera pewną liczbę wątków, a istnieje wiele innych, które można pobrać i zainstalować. (Listę wbudowanych pomocników stron ASP.NET Web Pages ma na liście [interfejsu API platformy ASP.NET — dokumentacja szybkie](https://go.microsoft.com/fwlink/?LinkId=202907).) Jeśli żadna istniejących pomocników nie spełnia Twoje potrzeby, można utworzyć własne pomocnika.

Obiekt pomocnika umożliwia używanie wspólnych bloku kodu na wielu stronach. Załóżmy, że na stronie często zachodzi potrzeba utworzenia elementu Uwaga ustawiono oprócz normalne akapity. Prawdopodobnie uwagi zostanie utworzona jako `<div>` elementu który został wstawiony jako pole z obramowaniem. Zamiast za każdym razem, gdy ma być wyświetlana Uwaga, Dodaj ten sam kod znaczników do strony, można spakować znaczników jako pomocnika. Można wstawić Notatka w jednym wierszu kodu muszą dowolnego miejsca.

Przy użyciu pomocnika, jak to sprawia, że kod w każdej strony prostszy i bardziej czytelne. On również ułatwia Obsługa witryny, ponieważ musisz zmienić wygląd uwagi, można zmienić znaczników w jednym miejscu.

## <a name="creating-a-helper"></a>Tworzenie pomocnika

Tej procedury przedstawiono sposób tworzenia pomocnika tworzącą Uwaga, zgodnie z opisem tylko. Jest to prosty przykład, ale niestandardowe Pomocnik może zawierać żadnych znaczników i kodu platformy ASP.NET, które są potrzebne.

1. W folderze głównym witryny sieci Web utwórz folder o nazwie *aplikacji\_kod*. Jest to zarezerwowana nazwa folderu w programie ASP.NET, których można umieścić kod dla składników, takich jak pomocników.
2. W *aplikacji\_kod* Utwórz nowy folder *.cshtml* plik i nadaj mu nazwę *MyHelpers.cshtml*.
3. Zastąp istniejącą zawartość następujących czynności:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    W kodzie użyto `@helper` składni, aby zadeklarować nowy Pomocnik o nazwie `MakeNote`. Tego konkretnego pomocnika umożliwia przekazywanie parametru o nazwie `content` zawierające kombinację tekst i znacznik. Pomocnik wstawia ciąg do przy użyciu treści Uwaga `@content` zmiennej.

    Zwróć uwagę, że plik ma nazwę *MyHelpers.cshtml*, ale nosi nazwę Pomocnika `MakeNote`. Wiele wątków niestandardowych można umieścić w jednym pliku.
4. Zapisz i zamknij plik.

## <a name="using-the-helper-in-a-page"></a>Przy użyciu pomocnika, na stronie

1. W folderze głównym, należy utworzyć nowy pusty plik o nazwie *TestHelper.cshtml*.
2. Dodaj następujący kod do pliku:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Aby wywołać pomocnika został utworzony, należy użyć `@` następuje nazwę pliku, w którym jest pomocnika, kropki, a następnie nazwę pomocnika. (Jeśli masz wiele folderów *aplikacji\_kod* folderu, można użyć składni `@FolderName.FileName.HelperName` do wywołania Pomocnik żadnego zagnieżdżone na poziomie folderu). Tekst, który należy dodać w znaki cudzysłowu w nawiasach jest tekst, który pomocnika będą wyświetlane jako część Uwaga na stronie sieci web.
3. Strony i uruchom go w przeglądarce. Pomocnik generuje elementu notatki prawo gdzie wywoływany pomocnika: między dwoma akapitów.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce i jak pomocnika generowany kod znaczników, który umieszcza pole wokół określony tekst.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Dodatkowe zasoby


[Poziomy menu jako pomocnika Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Ten wpis w blogu przez Pope Jan przedstawiono sposób tworzenia poziomy menu jako pomocnika przy użyciu znaczników, CSS i kod.

[Wykorzystanie HTML5 w sieci Web ASP.NET strony pomocników dla programu WebMatrix i ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Ten wpis w blogu przez Sam Abraham zawiera pomocnika, który renderuje HTML5 `Canvas` elementu.

[Różnica między @Helpers i @Functions w programie WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). W tym artykule opisano ten wpis w blogu przez Brind Jan `@helper` składni i `@function` składni i kiedy należy używać każdego.
