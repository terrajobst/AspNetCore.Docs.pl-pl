---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Informacje o wersji dla platformy ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012 | Dokumentacja firmy Microsoft
author: microsoft
description: Ten dokument zawiera opis wersji platformy ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Informacje o wersji dla platformy ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten dokument zawiera opis wersji platformy ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.


## <a name="contents"></a>Spis treści

- [Informacje o instalacji](#install)
- [Wymagania dotyczące oprogramowania](#requirements)
- Nowe funkcje programu ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Szablony](#templates)

        - [Szablonu platformy ASP.NET MVC 5](#mvc5template)
        - [Szablon 2 interfejsu API sieci Web ASP.NET](#apitemplate)
        - [Szablony elementu](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET Scaffolding](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Znane problemy i fundamentalne zmiany

    - [ASP.NET Scaffolding](#issuescaffolding)

        - [MVC i Web API szkieletów - HTTP 404 błędu nie znaleziono](#404issue)
        - [Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Wyświetlanie pliku cshtml przeglądanie za pomocą lub F5 powoduje błąd serwera](#browseissue)
        - [Ponowne zapisywanie adresów URL i Tilde(~)](#rewriteissue)
    - [Szablony](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Informacje o instalacji

[Zainstaluj](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET i sieć Web narzędzi 2013.1 dla programu Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Musi mieć programu Visual Studio 2012 lub Visual Studio Express 2012 for Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nowe funkcje programu ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Po utworzeniu szkieletu kontrolerów MVC 5 z widokami, korzysta z kodu znaczników dla widoków [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Szablony

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Szablonu platformy ASP.NET MVC 5

Dodano nowy szablon MVC 5. Odwołuje się do najnowszych pakietów MVC 5 NuGet i można dodać kontrolery i widoki szkieletów.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Szablon 2 interfejsu API sieci Web ASP.NET

Dodano nowy szablon 2 interfejsu API sieci Web. Odwołuje się do najnowszych pakietów NuGet 2 interfejsu API sieci Web i można dodać kontrolery i widoki szkieletów.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Szablony elementu

Dodaliśmy nowe szablony elementów dla widoków MVC 5, stron sieci Web (Razor 3) i sieci Web API 2 kontrolerów. Instalacji powiązanych pakietów NuGet do projektu podczas dodawania nowych elementów.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Po utworzeniu szkieletu kontrolera MVC lub Web API przy użyciu programu Entity Framework, używamy Framework 6. Aby uzyskać więcej informacji na temat programu Entity Framework, zobacz [Historia wersji programu Entity Framework](https://msdn.com/data/jj574253).

Można również pobrać i zainstalować narzędzia Entity Framework 6 dla programu Visual Studio 2012. Zobacz [uzyskać programu Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

Rusztowania ASP.NET to platforma generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, który współdziała z modelem danych.

W poprzednich wersjach programu Visual Studio szkieletów została ograniczona do projektów platformy ASP.NET MVC. Dzięki tej aktualizacji można teraz używać szkieletów dla żadnego projektu ASP.NET, w tym formularzy sieci Web. Ta aktualizacja nie obsługuje generowania stron dla projektu formularzy sieci Web, ale można nadal używać szkieletów z formularzy sieci Web, dodając zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Jeśli przy użyciu funkcji szkieletów, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład uruchomienie z projektem formularzy sieci Web ASP.NET i następnie dodać Kontroler interfejsu API sieci Web za pomocą funkcja szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać szkieletów MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletu** i wybierz **zależności MVC 5** w oknie dialogowym. Dostępne są dwie opcje do tworzenia szkieletu MVC; Minimalne i pełne. W przypadku wybrania minimalny, tylko pakiety NuGet i odwołań dla platformy ASP.NET MVC zostaną dodane do projektu. Jeśli wybierzesz opcję pełne, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletu kontrolerów async korzysta z nowych funkcji asynchronicznych z programu Entity Framework 6.

Aby uzyskać więcej informacji i samouczki, zobacz [omówienie szkieletów ASP.NET](../2013/aspnet-scaffolding-overview.md). Te samouczki przedstawiają szkieletów z programu Visual Studio 2013, ale dotyczą również programu ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Dzięki tej aktualizacji programu Visual Studio 2012 obsługują teraz, narzędzi/edytowania Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zawiera bogaty zestaw nowych funkcji, które opisano szczegółowo w [informacje o wersji 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Ta wersja programu NuGet eliminuje to potrzebę dla użytkowników jawnie zezwolić na NuGet, aby przywrócić brakujących pakietów. Podczas instalowania NuGet 2.7, użytkownicy niejawnie wyrażenia zgody na automatyczne przywracanie brakujących pakietów. Użytkownicy jawnie można zrezygnować z przywracania pakietu za pomocą ustawień NuGet w programie Visual Studio. Ta zmiana upraszcza sposób działania przywracania pakietu.

## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC i Web API szkieletów - HTTP 404 błędu nie znaleziono

Jeśli wystąpi błąd podczas Dodawanie elementu szkieletu do projektu, jest to możliwe, projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany wprowadzone można szkieletów zostanie wycofana, ale inne zmiany, takie jak zainstalowane pakiety NuGet zostaną nie można wycofać. Jeśli routingu zmiany konfiguracji zostaną przywrócone, użytkownicy będą otrzymywać błąd HTTP 404 podczas nawigowania do szkieletu elementów.

Aby naprawić ten błąd dla platformy MVC, Dodaj nowy element szkieletu i wybierz zależności MVC 5 (minimalnie lub pełna). Ten proces, zostaną dodane wszystkie wymagane zmiany do projektu.

Aby naprawić ten błąd interfejsu API sieci Web:

1. Dodaj następujące klasy WebApiConfig do projektu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Skonfiguruj WebApiConfig.Register w aplikacji\_Start — metoda w pliku Global.asax w następujący sposób:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu

Jeśli program Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu z programu Entity Framework (na przykład sieci Web Kontroler interfejsu API 2 z akcjami używający narzędzia Entity Framework), może się zdarzyć, że Visual Studio Express nie można załadować obrazu macierzystego zestawu w zależności od System.Web.Extensions.

Aby rozwiązać ten problem, skonfiguruj Visual Studio Express do pracy z obrazu MSIL System.Web.Extensions:

1. Otwórz wiersz polecenia w trybie administratora.
2. Przejdź do %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE lub % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (dla 64-bitowym systemem Windows).
3. Otwórz VWDExpress.exe.config w edytorze tekstów.
4. Dodaj następujący wiersz w obszarze &lt;konfiguracji&gt;/&lt;środowiska uruchomieniowego&gt; elementu:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Uruchom ponownie program Visual Studio Express 2012 for Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Wyświetlanie withBrowse plik cshtml WithorF5causes błąd serwera

Podczas tworzenia projektu MVC 5 w Visual Studio 2012 (lub Otwórz w programie Visual Studio 2012 MVC 5 projektu, który został utworzony w programie Visual Studio 2013) i próbujesz wyświetlić przy użyciu przeglądanie za pomocą lub F5 plik cshtml, zostanie wyświetlony błąd informujący - **błąd serwera w Aplikacja '/'**. Próbuje przejdź do serwera`http://localhost:XXXX/Views/../XXXX.cshtml`

Aby rozwiązać ten problem, zmień **Akcja uruchamiania** ustawienie w swoim projekcie **określonej strony**. Nie trzeba podać wartość dla strony.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Po wprowadzeniu tej zmiany, wybierając F5 przechodzi do katalogu głównego aplikacji (`http://localhost:XXXX`). To zachowanie nie jest tak samo, jak dla projektów MVC 5 w programie Visual Studio 2013, gdzie **bieżącej strony** ustawienie uruchamia Otwórz stronę.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Ponowne zapisywanie adresów URL i Tilde(~)

Po uaktualnieniu do programu ASP.NET Razor 3 i ASP.NET MVC 5 notacji tilde(~) może już nie działają prawidłowo, jeśli używasz ponownego adresu URL. Ponowne zapisywanie adresów URL wpływa notacji tilde(~) w elementów HTML, takich jak &lt;A /&gt;, &lt;skryptu /&gt;, &lt;łącze /&gt;, i w związku z tym tylda nie jest już mapowana do katalogu głównego.

Na przykład, jeśli przepisywania żądania **asp.net/content** do **asp.net**, atrybutu href w &lt;A href = "~/content/" /&gt; jest rozpoznawana jako **/content/ zawartość /** zamiast **/**. Aby pominąć tę zmianę, można ustawić **IIS\_WasUrlRewritten** kontekstu false w każdej strony sieci Web lub w **aplikacji\_powstaniem zdarzenia BeginRequest** w pliku Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Szablony

Po utworzeniu ASP.NET MVC projektów programu Visual Studio 2012, Windows 8.1 lub Windows Server 2012 R2, programu Visual Studio z komunikatem o błędzie stwierdzający "Konfigurowanie sieci Web [url] dla platformy ASP.NET 4.5 nie powiodło się."

![Błąd konfiguracji](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Zostanie wyświetlony ten błąd, ponieważ program Visual Studio 2012 nie obsługuje funkcję ASP.NET 4.5 zainstalowanego w tych wersjach systemu Windows. Aby włączyć funkcję ASP.NET 4.5, wykonaj czynności opisane w [Włącz lub wyłącz funkcje systemu Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Włącz lub wyłącz funkcje systemu Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternatywnie można włączyć funkcję ASP.NET 4.5 za pomocą wiersza polecenia.

1. Otwórz wiersz polecenia w trybie administratora.
2. Uruchom następujące polecenie, aby włączyć funkcję ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
