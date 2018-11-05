---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) — często zadawane pytania | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule wymieniono niektóre często zadawane pytania dotyczące stron ASP.NET Web Pages (Razor) i programu WebMatrix. Wersje oprogramowania używany w samouczek platformy ASP.NET Web Pages (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 27bfbe782455a5f8cf5096953c91712988b8dcab
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021056"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) — często zadawane pytania
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > Program WebMatrix nie jest już zalecany jako zintegrowane środowisko projektowe dla stron ASP.NET Web Pages. Użyj [programu Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) lub [programu Visual Studio Code](https://code.visualstudio.com/).
>
> W tym artykule wymieniono niektóre często zadawane pytania dotyczące stron ASP.NET Web Pages (Razor) i programu WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - Program WebMatrix 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2, 2 programu WebMatrix i Visual Studio 2012.


- [Jaka jest różnica między stron sieci Web platformy ASP.NET, ASP.NET Web Forms i ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Czy potrzebuję programu WebMatrix w celu pracy ze stronami sieci Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Można użyć kontrolek formularzy sieci Web platformy ASP.NET na stronie stron sieci Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Bez przy użyciu programu WebMatrix można wdrażać witryny ASP.NET Web Pages?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Czy muszę użyć pomocnika WebSecurity do obsługi logowania?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages obsługuje HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Ze stronami sieci Web można używać języków JavaScript i jQuery?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Dodatkowe zasoby](#AdditionalResources)

Masz pytania dotyczące błędów i innych problemów, zobacz [Przewodnik po rozwiązywaniu problemów w składniku ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Jaka jest różnica między stron sieci Web platformy ASP.NET, ASP.NET Web Forms i ASP.NET MVC?

Są wszystkie trzy technologie ASP.NET do tworzenia dynamicznych aplikacji sieci web:

- Strony ASP.NET Web Pages koncentruje się na dodanie dynamiczny kod (po stronie serwera) i dostęp do bazy danych do strony HTML i składnia prosty i lekkie funkcje.
- ASP.NET Web Forms jest oparty na modelu obiektu strony i formanty tradycyjny typ okna (przyciski, listy itp.). Formularze sieci Web używają modelu opartego na zdarzeniach, dobrze znanym do tych, którzy Pracowałem za pomocą opartego na kliencie (systemu Windows Windows forms).
- ASP.NET MVC zaimplementowano wzorzec model-view-controller dla programu ASP.NET. Szczególnym znajduje się na "separacji" (przetwarzania danych i warstwy interfejsu użytkownika).

Wszystkie trzy struktury są w pełni obsługiwane i w dalszym ciągu być opracowane przez zespół programu ASP.NET. Ogólnie rzecz biorąc, wybór framework, która do użycia zależy od tła i środowiska ASP.NET.

Strony ASP.NET Web Pages w szczególności zaprojektowano tak, aby ułatwić dla osób, które już znasz HTML, aby dodać serwer przetwarzania do swoich stron. Jest to dobry wybór dla uczniów, hobbystów, osoby ogólnie rzecz biorąc, którzy są nowym programistą. Można także dobry wybór dla deweloperów korzystających z technologii sieci web — ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Czy potrzebuję programu WebMatrix w celu pracy ze stronami sieci Web?

Nie. Program WebMatrix nie jest już zalecany jako zintegrowane środowisko projektowe dla stron ASP.NET Web Pages. Użyj [programu Visual Studio](program-asp-net-web-pages-in-visual-studio.md) lub [programu Visual Studio Code](https://code.visualstudio.com/).

Jeśli nie chcesz użyć programu Visual Studio lub Visual Studio Code, możesz zainstalować produktów składnik oddzielnie przy użyciu [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Potrzebne są następujące produkty:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (która instaluje także platformę ASP.NET Web Pages)
- Usługi IIS Express (serwer sieci web)
- Program Microsoft SQL Server Compact 4.0 (baza danych)

Możesz użyć edytora tekstu do edycji *.cshtml* (lub *.vbhtml*) stron.

Zarządzanie bazami danych programu SQL Server Compact (*.sdf* pliki) bez to narzędzie jest nieco trudniejsze. Visual Studio tools containds dla zarządzania *.sdf* baz danych. Można również uruchomić poleceń SQL w kodzie, aby wykonać wiele zadań zarządzania programu SQL Server.

Aby przetestować *.cshtml* stron bez użycia zintegrowanym środowisku programistycznym (IDE), można wdrożyć je na serwerze produkcyjnym. (Zobacz [czy mogę wdrożyć witrynę ASP.NET Web Pages bez przy użyciu programu WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Uruchamianie usług IIS Express, bez używania środowiska IDE

Po zainstalowaniu usług IIS Express na komputerze jako serwer sieci web, można użyć, aby przetestować na stronach. Można uruchomić usługi IIS Express z wiersza polecenia i skojarzyć go z określonego numeru portu. Następnie możesz określić port w przypadku żądania *.cshtml* pliki w przeglądarce.

W Windows, otwórz wiersz polecenia z uprawnieniami administratora, a następnie zmień *C:\Program Files\IIS Express.* (Dla 64-bitowych systemach użyj folderu *Express \IIS C:\Program Files (x86).)* Następnie wprowadź następujące polecenie, używając rzeczywistej ścieżce do swojej witryny:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Można użyć dowolnego numer portu, który nie jest już zarezerwowany przez jakiś inny proces. (Numery portów powyżej 1024 są zazwyczaj bezpłatna). Aby uzyskać `path` wartość, należy użyć ścieżki folderu witryny sieci Web gdzie *.cshtml* pliki są.

Po uruchomieniu tego polecenia, aby skonfigurować usługi IIS Express do obsługi stron sieci mogą otworzyć przeglądarkę i przejdź do *.cshtml* pliku. Użyj adresu URL, jak pokazano poniżej:

`http://localhost:35896/default.cshtml`

Aby uzyskać pomoc dotyczącą usług IIS Express opcje wiersza polecenia, wprowadź `iisexpress.exe /?` w wierszu polecenia.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Można użyć kontrolek formularzy sieci Web platformy ASP.NET na stronie stron sieci Web?

Nie. Formanty formularzy, takich jak sieci Web [pola wyboru](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) kontroli [kontrolkami walidacji](https://msdn.microsoft.com/library/bwd43d0x)i [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) kontrolować tylko pracy w stron formularzy sieci Web (*.aspx* plików). Te formanty wymagają struktury stron formularzy sieci Web.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Bez przy użyciu programu WebMatrix można wdrażać witryny ASP.NET Web Pages?

Tak. Serwer należy ręcznie skopiować pliki witryny sieci Web (zazwyczaj przy użyciu protokołu FTP). Przeprowadzenie ręcznego kopiowania należy skopiować pliki, które obsługują program SQL Server Compact (baza danych). Aby uzyskać więcej informacji, zobacz wpis w blogu [wdrożenie stron sieci Web aplikacji bez narzędzia](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Czy muszę użyć pomocnika WebSecurity do obsługi logowania?

Nie. `SimpleMembership` Dostawcy, który jest częścią strony sieci Web platformy ASP.NET stanowi jedną z opcji. Dostępne są również dostawców zabezpieczeń, które są częścią platformy ASP.NET (która może służyć do pracy z w formularzach sieci Web). Na przykład używając uwierzytelniania formularzy stron ASP.NET Web Pages tak samo jak w formularzach sieci Web. Na przykład jak używać uwierzytelniania formularzy, znajduje się w artykule firmy Microsoft Support [How To Implement Forms-Based uwierzytelniania w aplikacji ASP.NET przy użyciu języka C# .NET](https://support.microsoft.com/kb/301240). Aby pobrać prosty przykład, zobacz [wersję platformy ASP.NET "logowania &amp; hasło](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Aby dowiedzieć się, jak używać uwierzytelniania Windows, zobacz wpis w blogu [Windows przy użyciu uwierzytelniania w składniku ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages obsługuje HTML5?

Tak. Strony tworzenia przy użyciu stron ASP.NET Web Pages (*.cshtml* lub *.vbhtml* stron) są zasadniczo strony HTML, które również zawierać kod, który działa na serwerze, przed wyświetleniem strony. Tak długo, jak przeglądarka obsługująca język HTML5, można użyć elementów HTML5 w *.cshtml* lub *.vbhtml* strony.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Ze stronami sieci Web można używać języków JavaScript i jQuery?

Oczywiście. Strony tworzenia przy użyciu stron ASP.NET Web Pages (*.cshtml* lub *.vbhtml* stron) są po prostu strony HTML przy użyciu kodu serwera w nich. W związku z tym, wszystko możesz zrobić w normalnym strony HTML, przy użyciu języka JavaScript i jQuery można również wykonać *.cshtml* lub *.vbhtml* strony.

**Witryny początkowej** szablonu w programie WebMatrix zawiera szereg biblioteki jQuery. Jeśli witryna jest tworzona przy użyciu tego szablonu *skrypty* folder zawiera Biblioteka podstawowa jQuery (*jquery 1.6.2.js)* i biblioteki do weryfikacji jQuery (*jquery.validate.js*itp.).

Poniżej przedstawiono niektóre wpisy w blogu, ilustrujące sposoby na wykorzystanie jQuery przy użyciu stron ASP.NET Web Pages:

- [Dodawanie do strony sieci Web ASP.NET za pomocą programu WebMatrix jQuery zgodność](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) przez Rachel Appel
- [5 min: program WebMatrix interfejs użytkownika jQuery + json i jQuery szablony](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) przez Jonas Eriksson
- [Program WebMatrix i formularze jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) przez Mike'a Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Forum programu WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) w witrynie sieci Web platformy ASP.NET
