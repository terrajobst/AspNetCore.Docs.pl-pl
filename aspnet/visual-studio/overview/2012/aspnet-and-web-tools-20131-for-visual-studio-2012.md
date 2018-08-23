---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Informacje o wersji platformy ASP.NET i narzędzi Web Tools 2013.1 dla programu Visual Studio 2012 | Dokumentacja firmy Microsoft
author: microsoft
description: Ten dokument zawiera opis wersji platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: a0b3d52910ac33c403ecbe2340c12b202c25147b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755612"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Informacje o wersji programu ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Ten dokument zawiera opis wersji platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012.


## <a name="contents"></a>Spis treści

- [Uwagi dotyczące instalacji](#install)
- [Wymagania dotyczące oprogramowania](#requirements)
- Nowe funkcje w ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Szablony](#templates)

        - [Szablonu platformy ASP.NET MVC 5](#mvc5template)
        - [Szablon sieci Web API 2 platformy ASP.NET](#apitemplate)
        - [Szablony elementów](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Funkcja tworzenia szkieletu ASP.NET](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Znane problemy i fundamentalne zmiany

    - [Funkcja tworzenia szkieletu ASP.NET](#issuescaffolding)

        - [MVC i Web interfejsu API tworzenia szkieletów - HTTP 404 Nie znaleziono błąd](#404issue)
        - [Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Wyświetlanie plik cshtml przeglądanie za pomocą lub F5 powoduje błąd serwera](#browseissue)
        - [Ponowne zapisywanie adresów URL i Tilde(~)](#rewriteissue)
    - [Szablony](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

[Zainstaluj](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Konieczne jest posiadanie programu Visual Studio 2012 lub Visual Studio Express 2012 for Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nowe funkcje w ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Usługa ładowania początkowego

Gdy użytkownik tworzenia szkieletu, widoków i kontrolerów MVC 5, znaczniki dla widoków używa [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Szablony

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Szablonu platformy ASP.NET MVC 5

Dodaliśmy nowy szablon MVC 5. Odwołuje się do najnowszych pakietów MVC 5 NuGet, a funkcja szkieletów umożliwia dodawanie, widoków i kontrolerów.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Szablon sieci Web API 2 platformy ASP.NET

Dodaliśmy nowy szablon Web API 2. Odwołuje się do najnowszych pakietów NuGet 2 interfejsu API sieci Web, a funkcja szkieletów umożliwia dodawanie, widoków i kontrolerów.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Szablony elementów

Dodaliśmy nowe szablony elementów dla widoków MVC 5, Web Pages (Razor 3) i kontrolerów Web API 2. Podczas dodawania nowych elementów instalacji powiązanych pakietów NuGet do projektu.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Gdy użytkownik tworzenia szkieletu kontrolera MVC lub interfejsu API sieci Web używający narzędzia Entity Framework, używamy Framework 6. Aby uzyskać więcej informacji na temat platformy Entity Framework, zobacz [Historia wersji programu Entity Framework](https://msdn.com/data/jj574253).

Można również pobrać i zainstalować narzędzi Entity Framework 6 Tools dla programu Visual Studio 2012. Zobacz [pobieranie programu Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, która współdziała z modelu danych.

W poprzednich wersjach programu Visual Studio tworzenia szkieletu była ograniczona do projektów programu ASP.NET MVC. Dzięki tej aktualizacji można teraz używać tworzenia szkieletów dla każdego projektu programu ASP.NET, w tym formularzy sieci Web. Ta aktualizacja nie obsługuje generowania stron dla projektu formularzy sieci Web, ale nadal umożliwia tworzenie szkieletów przy użyciu formularzy sieci Web przez dodanie zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Korzystając z tworzenia szkieletu, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczęcie projektu programu ASP.NET Web Forms, a następnie dodaj Kontroler interfejsu API sieci Web, korzystając z tworzenia szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać MVC scaffolding projekt formularzy sieci Web, należy dodać **nowy element szkieletu** i wybierz **MVC 5 zależności** w oknie dialogowym. Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; Minimalna i pełne. Jeśli wybierzesz przycisku minimalnych tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC są dodawane do projektu. Jeśli wybierzesz opcję pełnej, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletów kontrolerów async korzysta z nowych funkcji asynchronicznej z platformy Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [omówienie tworzenia szkieletu ASP.NET](../2013/aspnet-scaffolding-overview.md). W tych samouczkach przedstawiono tworzenie szkieletu za pomocą programu Visual Studio 2013, ale są one również zastosowanie do platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Dzięki tej aktualizacji programu Visual Studio 2012 obsługuje teraz, narzędzi oraz edytowanie Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet w wersji 2.7 zawiera bogaty zestaw nowych funkcji, które opisano szczegółowo w [informacjach o wersji programu NuGet w wersji 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Ta wersja programu NuGet eliminuje potrzebę dla użytkowników jawnie zezwolić na NuGet można przywrócić brakujących pakietów. Podczas instalowania NuGet w wersji 2.7, użytkownicy niejawnie wyrazić zgodę na automatyczne przywrócenie brakujących pakietów. Można jawnie zrezygnowanie z przywracania pakietów za pomocą ustawień NuGet w programie Visual Studio. Ta zmiana ułatwia działania przywracania pakietu.

## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC i Web interfejsu API tworzenia szkieletów - HTTP 404 Nie znaleziono błąd

Jeśli wystąpi błąd podczas dodawania elementu szkieletu do projektu, jest to możliwe, projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany można szkieletu zostanie wycofana, ale inne zmiany, takie jak zainstalowane pakiety NuGet zostaną nie można wycofać. Jeśli routingu zmiany konfiguracji zostaną wycofane, użytkownicy otrzymają błąd HTTP 404 podczas przechodzenia do szkieletu elementów.

Aby naprawić ten błąd dla platformy MVC, Dodaj nowy element szkieletu, a następnie wybierz zależności programu MVC 5 (minimalny lub pełna). Ten proces doda wszystkie wymagane zmiany do swojego projektu.

Aby naprawić ten błąd interfejsu API sieci Web:

1. Dodaj następujące klasy WebApiConfig do projektu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Konfigurowanie WebApiConfig.Register w aplikacji\_Start metoda w pliku Global.asax w następujący sposób:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu

Jeśli program Visual Studio Express 2012 for Web przestaje działać po dodaniu elementu szkieletu z platformą Entity Framework (na przykład kontroler internetowego interfejsu API 2 z akcjami używający narzędzia Entity Framework), może się zdarzyć, że Visual Studio Express nie można załadować obrazu natywnego zestawu w zależności od System.Web.Extensions.

Aby rozwiązać ten problem, skonfiguruj programu Visual Studio Express do pracy z obrazu MSIL System.Web.Extensions:

1. Otwórz wiersz polecenia w trybie administratora.
2. Przejdź do %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE lub % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (dla 64-bitowy Windows).
3. Otwórz VWDExpress.exe.config w edytorze tekstów.
4. Dodaj następujący wiersz w obszarze &lt;konfiguracji&gt;/&lt;środowiska uruchomieniowego&gt; elementu:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Uruchom ponownie Visual Studio Express 2012 for Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Wyświetlanie withBrowse plik cshtml WithorF5causes błąd serwera

Podczas tworzenia projektu MVC 5 w programie Visual Studio 2012 (lub Otwórz w projekcie programu Visual Studio 2012 MVC 5, który został utworzony w programie Visual Studio 2013) i spróbować wyświetlić plik cshtml przy użyciu przeglądanie za pomocą lub F5, otrzymasz komunikat o błędzie informujący - **błąd serwera w Aplikacja "/"**. Serwer próbuje przejdź do `http://localhost:XXXX/Views/../XXXX.cshtml`

Aby rozwiązać ten problem, zmień **Akcja uruchamiania** ustawienie w swoim projekcie **konkretnej strony**. Nie musisz podać wartość dla strony.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Po wprowadzeniu tej zmiany, wybierając F5 powoduje przejście do katalogu głównego aplikacji (`http://localhost:XXXX`). To zachowanie nie jest tak samo, jak w przypadku projektów MVC 5 w programie Visual Studio 2013, gdzie **bieżącej strony** ustawienie uruchamia Otwórz stronę.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Ponowne zapisywanie adresów URL i Tilde(~)

Po uaktualnieniu do wersji 3 Razor programu ASP.NET lub ASP.NET MVC 5, notacji tilde(~) może już działać poprawnie, jeśli używasz adresu URL modyfikacji oprogramowania. Ponowne zapisywanie adresów URL takich jak wpływa na notacji tilde(~) elementów HTML &lt;A /&gt;, &lt;skryptu /&gt;, &lt;łącze /&gt;, i w wyniku tylda nie jest już mapowany do katalogu głównego.

Na przykład, jeśli przepiszesz żądania **asp.net/content** do **asp.net**, atrybut href w &lt;A href = "~/content/" /&gt; jest rozpoznawana jako **/content/ zawartość /** zamiast **/**. Aby pominąć tę zmianę, można ustawić **IIS\_WasUrlRewritten** kontekstu na wartość false w każdej strony sieci Web lub w **aplikacji\_BeginRequest** w pliku Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Szablony

Po utworzeniu platformy ASP.NET MVC projekty z Visual Studio 2012, Windows 8.1 lub Windows Server 2012 R2, program Visual Studio wyświetla komunikat o błędzie z informacją "Konfigurowanie sieci Web [url] dla platformy ASP.NET 4.5 nie powiodło się."

![Błąd konfiguracji](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Zostanie wyświetlony ten błąd, ponieważ program Visual Studio 2012 nie obsługuje funkcję ASP.NET 4.5 zainstalowanego w tych wersjach systemu Windows. Aby włączyć ASP.NET 4.5, wykonaj czynności opisane w [Windows Włącz lub wyłącz funkcje](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Włącz lub wyłącz funkcje Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternatywnie można włączyć ASP.NET 4.5, za pośrednictwem wiersza polecenia.

1. Otwórz wiersz polecenia w trybie administratora.
2. Uruchom następujące polecenie, aby włączyć program ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
