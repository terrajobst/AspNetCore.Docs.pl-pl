---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "ReadMe Podgląd dewelopera 2 stron sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview ReadMe
====================
przez [firmy Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview ReadMe

14 września 2011

### <a name="contents"></a>Spis treści

#### <a id="_Toc303701284"></a>Informacje o instalacji

Do zainstalowania programu Web Pages 2 Developer Preview, masz następujące opcje:

- Instalacja wersji Beta 2 programu WebMatrix za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=226883). Program WebMatrix to zestaw narzędzi programistycznych wolnej sieci web, który zawiera strony sieci Web ASP.NET. Aby uzyskać więcej informacji, zobacz sekcję instalacja w [funkcji Top w programie ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Zainstaluj stron sieci Web 2 Developer Preview bezpośrednio za pomocą [pobrać link](https://go.microsoft.com/fwlink/?LinkID=226335). Tej metody należy użyć, jeśli chcesz tworzyć aplikacje stron sieci Web przy użyciu tekstu edytora, takiego jak Notatnik. Aby uruchamiać aplikacje sieci Web Pages 2, należy skonfigurować IIS Express 7.5. (Jest to dołączone automatycznie za pomocą programu WebMatrix.) Aby porady na temat testowania stronę stron sieci Web za pomocą usług IIS Express, zobacz "Tworzenie i testowanie ASP.NET strony za pomocą własnych tekst edytora" paska bocznego w [wprowadzenie do korzystania z programu WebMatrix i stron ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

Strony ASP.NET Web Pages 2 Developer Preview można zainstalować i uruchomić side-by-side z 1 stron sieci Web programu ASP.NET. <a id="a"></a>Aby uzyskać więcej informacji, zobacz sekcję "Uruchomiona stron sieci Web aplikacji Side-by-Side" w [funkcji Top w stron sieci Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>Dokumentacja

Samouczki i inne informacje o produkcie ASP.NET Web Pages są dostępne na stronie stron sieci Web, witryny sieci Web platformy ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Aby uzyskać informacje o nowych funkcjach i rozszerzeniach w wersji 2 stron sieci Web, zobacz [funkcji Top w stron sieci Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>Obsługa

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>To jest wersja preview i nie jest oficjalnie obsługiwana. Jeśli masz pytania dotyczące pracy z tej wersji zamieścić je na forum stron ASP.NET Web Pages ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), gdzie są często można zapewnić obsługę nieformalne członkami społeczności programu ASP.NET.

#### <a id="_Toc303701287"></a>Wymagania dotyczące oprogramowania

Strony ASP.NET Web Pages 2 wymaga programu .NET Framework 4. Działa także w wersji .NET Framework 4.5 Developer Preview.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Poprawki, znanych problemów i zmian podziału

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Jest\* metody (na przykład IsDateTime) teraz poprawne wartości zwracane dla wszystkich języków.** W przypadku niektórych metod, takich jak *IsDateTime* wcześniej zostały zwrócone *false* po powinny być zwracane *true* ponieważ wcześniej były one wykonuje testy specyficzne dla kultury. Te metody Naprawiono uwzględnienie teraz kultury. Jest to istotne zmiany; Jeśli poprzednie działanie zależy od aplikacji, zostaną przerwane.
- **Zachowanie metody Href zostało zmienione.** Wcześniej wywoływania Href("~/SomeFile") zwróci adres URL względem obecnie wykonywanym pliku. Teraz Href("~/SomeFile") zawsze zwraca ścieżkę bezwzględną z katalogu głównego aplikacji. W większości przypadków takie zachowanie nie należy różnica w wartości zwracanej. Ta zmiana została wprowadzona w celu rozwiązania niektórych scenariuszach interfejsu Ajax. Rozważmy na przykład następujący przykładowy kod: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Ten kod wcześniej może prowadzić do Images/Logo.jpg, będą niepoprawne dla żądania Ajax do tej strony. Teraz zostanie rozwiązany w katalogu głównym (/ MySite/Images/Logo.jpg).
- **Metoda HttpContext.RedirectLocal zmieniona**. Ta metoda jest teraz akceptuje tylko adresy URL, które są względem bieżącej aplikacji. W pełni kwalifikowana adresy URL są odrzucane.
- **Metoda ModelState.IsValid teraz wymaga, należy najpierw wywołać Validate**. Jeśli zmieniasz aplikacji umożliwiająca użycie nowych metod sprawdzania poprawności danych wejściowych i wywoływania *ModelState.IsValid* metody, należy teraz wywołać *Validation.Validate* wcześniej. Na przykład należy teraz wykonać tego wzorca: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 Jednak zaleca się użycie nowych metod sprawdzania poprawności danych wejściowych nie wykorzystujące *ModelState.IsValid*. Zamiast tego struktury kodu następująco: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **W programie Internet Explorer 7 i programu Internet Explorer 8, weryfikacji po stronie klienta nie działa**. Weryfikacji po stronie klienta nie działa z powodu niezgodności z jQuery 1.6.2, która jest uwzględniona w domyślnym szablonie projektu. (Weryfikacji po stronie serwera działa.).

#### <a id="_Toc303701289"></a>Zrzeczenie odpowiedzialności

© Microsoft 2011 Corporation. Wszelkie prawa zastrzeżone. Niniejszy dokument jest udostępniany "jako — jest." Informacje i poglądy wyrażone w tym dokumencie, w tym adresy URL i inne odsyłacze do witryn internetowych, mogą ulec zmianie bez uprzedzenia. Użytkownik ponosi ryzyko związane z użyciem jej.
