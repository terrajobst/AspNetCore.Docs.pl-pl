---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "Strony sieci Web platformy ASP.NET (Razor) — często zadawane pytania | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule wymieniono niektóre często zadawane pytania dotyczące stron sieci Web platformy ASP.NET (Razor) i programu WebMatrix. Używane w samouczek platformy ASP.NET Web Pages (R... wersje oprogramowania"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-faq"></a>Strony sieci Web platformy ASP.NET (Razor) — często zadawane pytania
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> > [!NOTE] 
> > Program WebMatrix jest już zalecane jako zintegrowane środowisko programistyczne dla stron sieci Web programu ASP.NET. Użyj [programu Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) lub [kodu programu Visual Studio](https://code.visualstudio.com/).
>
> W tym artykule wymieniono niektóre często zadawane pytania dotyczące stron sieci Web platformy ASP.NET (Razor) i programu WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - Program WebMatrix 3
>   
> 
> W tym samouczku współdziała również z ASP.NET Web Pages 2, 2 programu WebMatrix i Visual Studio 2012.


- [Jaka jest różnica między strony sieci Web ASP.NET, formularzy sieci Web ASP.NET i ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Należy WebMatrix Aby pracować ze stronami sieci Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Można używać formantów formularzy sieci Web programu ASP.NET na stronie stron sieci Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Bez za pomocą programu WebMatrix można wdrażać witryny ASP.NET Web Pages?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Czy muszę obsługuje logowania przy użyciu Pomocnika WebSecurity?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Strony ASP.NET Web Pages obsługuje HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Czy można użyć języka JavaScript i jQuery ze stronami sieci Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Dodatkowe zasoby](#AdditionalResources)

Pytania dotyczące błędów i inne problemy, zobacz [przewodnik rozwiązywania problemów stron sieci Web platformy ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Jaka jest różnica między strony sieci Web ASP.NET, formularzy sieci Web ASP.NET i ASP.NET MVC?

Wszystkie trzy są technologii ASP.NET do tworzenia dynamicznych aplikacji sieci web:

- Strony sieci Web ASP.NET koncentruje się na dodanie kod dynamicznych (po stronie serwera) i dostęp do bazy danych do strony HTML i składnia funkcji proste i niewielkie.
- Formularze sieci Web ASP.NET jest oparta na modelu obiektów strony i kontrolki tradycyjnych typ okna (przyciski, listy itp.). Formularze sieci Web używają modelu opartego na zdarzeniach znany tych, którzy się Programowanie oparte na kliencie (formularze systemu Windows).
- Platformie ASP.NET MVC zaimplementowano wzorzec model-view-controller dla programu ASP.NET. Wyróżniane są "separacji" (przetwarzania danych i warstwy interfejsu użytkownika).

Wszystkie trzy struktury są w pełni obsługiwane i kontynuowany przez zespół ASP.NET. Ogólnie rzecz biorąc, wybór które framework do użycia zależy tła i doświadczenia z programem ASP.NET.

Strony sieci Web ASP.NET w szczególności zaprojektowano tak, aby ułatwić dla osób, które już posiadają HTML w celu dodania serwera przetwarzania do ich stron. Jest to dobry wybór dla uczniów lub studentów, hobbystów, osoby ogólnie rzecz biorąc, kim są nowe do programowania. Można także użyteczna dla deweloperów korzystających z technologii sieci web z systemem innym niż ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Należy WebMatrix Aby pracować ze stronami sieci Web?

Nie. Program WebMatrix jest już zalecane jako zintegrowane środowisko programistyczne dla stron sieci Web programu ASP.NET. Użyj [programu Visual Studio](program-asp-net-web-pages-in-visual-studio.md) lub [kodu programu Visual Studio](https://code.visualstudio.com/).

Jeśli nie chcesz używać programu Visual Studio lub Visual Studio Code, można zainstalować oddzielnie, używając produktów składnika [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Potrzebne są następujące produkty:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (które instaluje również platformę ASP.NET Web Pages)
- Usługi IIS Express (serwer sieci web)
- Microsoft SQL Server Compact 4.0 (baza danych)

Edytor tekstu służy do edytowania *.cshtml* (lub *.vbhtml*) stron.

Zarządzanie bazami danych programu SQL Server Compact (*sdf* pliki) bez to narzędzie jest nieco trudniejsze. Visual Studio containds narzędzia do zarządzania *sdf* baz danych. Można również uruchomić poleceń SQL w kodzie do wykonywania wielu zadań zarządzania programu SQL Server.

Aby przetestować *.cshtml* stron bez użycia zintegrowane środowisko programistyczne (IDE), można je wdrożyć na serwerze produkcyjnym. (Zobacz [można wdrożyć witryny ASP.NET Web Pages bez za pomocą programu WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Uruchomione usługi IIS Express bez użycia IDE

Po zainstalowaniu usług IIS Express na komputerze jako serwera sieci web, można użyć który do testowania stron. Można uruchomić usługi IIS Express z wiersza polecenia i powiązać ją z określonego numeru portu. Następnie należy określić port w przypadku żądania *.cshtml* pliki w przeglądarce.

W systemie Windows, otwórz wiersz polecenia z uprawnieniami administratora i zmień *C:\Program Files\IIS Express.* (Dla 64-bitowym, użyj folderu *Express \IIS C:\Program Files (x86).)* Następnie wprowadź następujące polecenie, używając rzeczywistej ścieżce do swojej witryny:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Można użyć dowolnego numer portu, który nie jest już zarezerwowany w innym procesie. (Numery portów powyżej 1024 są zwykle bezpłatna). Dla `path` wartość, użyj ścieżki folderu witryny sieci Web gdzie *.cshtml* są pliki.

Po uruchomieniu tego polecenia, aby skonfigurować usługi IIS Express do obsługi stron, można otworzyć przeglądarkę i przejdź do *.cshtml* pliku. Użyj następującego adresu URL:

`http://localhost:35896/default.cshtml`

Aby uzyskać pomoc dotyczącą opcji wiersza polecenia usług IIS Express, wprowadź `iisexpress.exe /?` w wierszu polecenia.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Można używać formantów formularzy sieci Web programu ASP.NET na stronie stron sieci Web?

Nie. Formanty formularzy, takie jak sieci Web [wyboru](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) kontroli, [formanty walidacji](https://msdn.microsoft.com/library/bwd43d0x)i [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) kontrolować tylko pracy na stronach formularzy sieci Web (*.aspx* plików). Formanty te wymagają framework strony formularzy sieci Web.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Bez za pomocą programu WebMatrix można wdrażać witryny ASP.NET Web Pages?

Tak. Możesz ręcznie skopiować plików witryny sieci Web na serwerze (zazwyczaj przy użyciu protokołu FTP). Jeśli należy ręczne wykonanie kopii, należy skopiować pliki, które obsługuje program SQL Server Compact (baza danych). Aby uzyskać więcej informacji, zobacz wpis w blogu [wdrożenie stron sieci Web aplikacji bez narzędzia](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Czy muszę obsługuje logowania przy użyciu Pomocnika WebSecurity?

Nie. `SimpleMembership` Dostawcy, który jest częścią stron sieci Web programu ASP.NET jest jedną z opcji. Dostępne są także dostawców zabezpieczeń, które są częścią programu ASP.NET (która może służyć do pracy z formularzy sieci Web). Na przykład służy uwierzytelniania formularzy w programie ASP.NET Web Pages tak samo jak w formularzach sieci Web. Na przykład sposobu użycia uwierzytelnianie formularzy, zobacz artykuł Microsoft Support [How To Implement Forms-Based uwierzytelniania w Twojej aplikacji ASP.NET przy użyciu C# .NET](https://support.microsoft.com/kb/301240). Aby pobrać prosty przykład, zobacz [wersji platformy ASP.NET "logowania &amp; hasło](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Aby uzyskać informacje dotyczące uwierzytelniania systemu Windows, zobacz we wpisie blogu [przy użyciu uwierzytelniania systemu Windows w programie ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Strony ASP.NET Web Pages obsługuje HTML5?

Tak. Strony z strony sieci Web platformy ASP.NET (*.cshtml* lub *.vbhtml* stron) są zasadniczo stron HTML zawierające kod, który jest uruchamiany na serwerze przed realizacją strony. Tak długo, jak przeglądarka obsługuje protokół HTML5, można użyć elementów HTML5 w *.cshtml* lub *.vbhtml* strony.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Czy można użyć języka JavaScript i jQuery ze stronami sieci Web?

Absolutnie. Stron z strony sieci Web platformy ASP.NET (*.cshtml* lub *.vbhtml* stron) są tylko HTML strony z kodu serwera w nich. W związku z tym niczego możesz zrobić w normalnym strony HTML przez przy użyciu języka JavaScript i jQuery możesz również wykonać *.cshtml* lub *.vbhtml* strony.

**Witryny początkowej** szablonu w programie WebMatrix zawiera szereg biblioteki jQuery. W przypadku utworzenia witryny przy użyciu tego szablonu *skryptów* folder zawiera Biblioteka podstawowa jQuery (*jquery 1.6.2.js)* i biblioteki do weryfikacji jQuery (*jquery.validate.js*itp.).

Poniżej przedstawiono niektóre wpisy na blogu ilustrujące sposoby używania jQuery ze stron sieci Web programu ASP.NET:

- [Dodawanie do stron sieci Web ASP.NET za pomocą programu WebMatrix jQuery zgodność](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) przez Rachel Appel
- [5 minut: WebMatrix interfejsu użytkownika jQuery + json + jQuery szablony](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) przez Jonas Eriksson
- [Program WebMatrix i formularze jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) przez Brind Jan

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Forum programu WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) w witrynie sieci Web ASP.NET
