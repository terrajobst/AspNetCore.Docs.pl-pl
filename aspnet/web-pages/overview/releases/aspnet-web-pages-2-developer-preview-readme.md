---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 Developer (wersja zapoznawcza) ReadMe | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: riande
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 93e3f9c9d7c90f1ebfd9f482166aeb833cae73e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756546"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer (wersja zapoznawcza) w pliku ReadMe
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer (wersja zapoznawcza) w pliku ReadMe

14 września 2011

### <a name="contents"></a>Spis treści

#### <a id="_Toc303701284"></a>  Uwagi dotyczące instalacji

Aby zainstalować Web Pages 2 Developer Preview, masz następujące opcje:

- Instalowane przy użyciu programu WebMatrix 2 Beta [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=226883). Program WebMatrix jest zestaw bezpłatnych narzędzi do programowania, który zawiera strony sieci Web ASP.NET. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą instalacji w [funkcji Top w ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Zainstaluj Web Pages 2 Developer Preview bezpośrednio przy użyciu [link pobierania](https://go.microsoft.com/fwlink/?LinkID=226335). Tej metody należy użyć, jeśli chcesz tworzyć aplikacje stron sieci Web, za pomocą tekstu edytora, takiego jak Notatnik. Aby można było uruchamiać aplikacje sieci Web Pages 2, konieczne jest posiadanie usług IIS 7.5 Express. (To jest uwzględniane automatycznie za pomocą programu WebMatrix). Porady na temat testowania strony stron sieci Web za pomocą usług IIS Express, można znaleźć w pasku bocznym "Tworzenie i testowanie ASP.NET stron przy użyciu Your własnego tekstu Editor" [wprowadzenie do programu WebMatrix i stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview można zainstalować i uruchomić side-by-side z 1 stron sieci Web platformy ASP.NET. <a id="a"></a>Aby uzyskać szczegółowe informacje, zobacz sekcję "Uruchamianie stron sieci Web aplikacji Side-by-Side" w [funkcje górnej programu Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Dokumentacja

W samouczkach i innych informacji dotyczących stron ASP.NET Web Pages są dostępne na stronie stron sieci Web, witryny sieci Web platformy ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Aby uzyskać informacje o nowych funkcjach oraz ulepszeniach platform Web Pages 2, zobacz [funkcje górnej programu Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Obsługa

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> To jest wersja zapoznawcza i nie jest oficjalnie obsługiwana. Jeśli masz pytania na temat pracy z tej wersji, opublikuj je na forum stron ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), gdzie są często w stanie zapewnić obsługę nieformalne członków społeczności platformy ASP.NET.

#### <a id="_Toc303701287"></a>  Wymagania dotyczące oprogramowania

Strony ASP.NET Web Pages 2 wymaga programu .NET Framework 4. Działa w wersji .NET Framework 4.5 dla deweloperów w wersji zapoznawczej.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Poprawki, znane problemy i przełomowe zmiany

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Jest\* metod (na przykład IsDateTime) teraz poprawne wartości zwracane dla wszystkich języków.** Niektóre metody, takie jak *IsDateTime* wcześniej zwrócony *false* po powinny mieć zwrócone *true* ponieważ wcześniej wykonywały testy specyficzne dla kultury. Te metody zostały naprawione uwzględnienie teraz kultury. To jest zmianą przerywającą; Jeśli aplikacja zależy od starego zachowania, spowoduje awarię.
- **Zachowanie metody Href został zmieniony.** Wcześniej wywoływania Href("~/SomeFile") zwróci adres URL względem obecnie wykonywanym pliku. Teraz Href("~/SomeFile") zawsze zwraca ścieżkę bezwzględną z katalogu głównego aplikacji. W większości przypadków to zachowanie nie reagować w wartości zwracanej. Ta zmiana została wprowadzona, aby rozwiązać problem w niektórych scenariuszach interfejsu Ajax. Na przykład rozważmy poniższy przykład kodu: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Ten kod wcześniej może prowadzić do Images/Logo.jpg, które byłyby nieprawidłowe dla żądania Ajax do tej strony. Teraz zostanie rozwiązany w katalogu głównym (/ MySite/Images/Logo.jpg).
- **Metoda HttpContext.RedirectLocal zmieniona**. Ta metoda będzie teraz akceptować tylko adresy URL, które są względne wobec bieżącej aplikacji. W pełni kwalifikowana adresy URL są odrzucane.
- **Metoda ModelState.IsValid teraz, musisz najpierw wywołaj proces weryfikacji**. Jeśli konwertujesz aplikacja korzysta z nowych metod sprawdzania poprawności danych wejściowych, a wywołania dotyczą *ModelState.IsValid* metody, należy teraz wywołać *Validation.Validate* wcześniej. Na przykład musisz teraz wykonać tego wzorca: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Jednak zaleca się, korzystając z nowych metod sprawdzania poprawności danych wejściowych, nie używaj *ModelState.IsValid*. Zamiast tego struktury kodu następująco: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Weryfikacja po stronie klienta w programie Internet Explorer 7 oraz programu Internet Explorer 8, nie działa.**. Weryfikacja po stronie klienta nie działa z powodu niezgodności z jQuery 1.6.2, który jest dołączony do domyślnego szablonu projektu. (Weryfikacji po stronie serwera działa.).

#### <a id="_Toc303701289"></a>  Zrzeczenie odpowiedzialności

© 2011 Microsoft Corporation. Wszelkie prawa zastrzeżone. Niniejszy dokument jest udostępniany "jako-to." Informacje i poglądy wyrażone w tym dokumencie, w tym adresy URL i inne odniesienia do witryn internetowych, mogą ulec zmianie bez powiadomienia. Użytkownik jest odpowiedzialny za jej pomocą.
