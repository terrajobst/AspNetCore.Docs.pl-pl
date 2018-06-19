---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077923"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Co to jest nowe w programie ASP.NET MVC 5

### <a name="one-aspnet"></a>Jeden ASP.NET

Szablony projektów sieci Web MVC integrują się z nowego środowiska ASP.NET jeden. Można dostosować projekt MVC i skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu ASP.NET jeden. Samouczek wprowadzający do platformy ASP.NET MVC 5 można znaleźć w folderze [wprowadzenie do platformy ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje o uaktualnianiu projektów MVC 4 do MVC 5, zobacz [sposób uaktualnienia programu ASP.NET MVC 4 i projekt interfejsu API sieci Web platformy ASP.NET MVC 5 i Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów MVC zostały zaktualizowane do korzystania z tożsamości ASP.NET do uwierzytelniania i zarządzania tożsamościami. Samouczek uwierzytelniania serwisu Facebook i Google i nowy interfejs API członkostwa można znaleźć w folderze [tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) i [wdrażanie aplikacji platformy ASP.NET MVC Secure z Członkostwo, OAuth i bazy danych SQL do witryny sieci Web platformy Azure z systemem Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Szablon projektu MVC została zaktualizowana w celu użycia [Bootstrap](http://getbootstrap.com/) zapewnienie elegancki i elastyczny wyglądu i działania, które można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

[Filtry uwierzytelniania](http://www.dotnetcurry.com/showarticle.aspx?ID=957) nowy rodzaj filtru na platformie ASP.NET MVC przed filtry autoryzacji w potoku platformy ASP.NET MVC, umożliwiają określenie uwierzytelniania logiki na działania, które są na kontroler lub globalnie do wszystkich kontrolerów. Filtry uwierzytelniania przetworzyć poświadczeń w żądaniu i podaj odpowiednie podmiot zabezpieczeń. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi do nieautoryzowanego żądania. Zobacz [filtrów uwierzytelniania platformy ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtry uwierzytelniania w aplikacji ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Zastępuje filtru

Teraz można zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając [zastąpienia filtru](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtry zastąpienie określ zestaw typów filtrów, które nie powinny być uruchamiane dla danego zakresu (akcji lub kontrolera). Dzięki temu można skonfigurować filtry, które są stosowane globalnie, ale następnie wykluczyć określone filtry globalne mające zastosowanie do określonych akcji i kontrolerów. Zobacz [funkcja przesłania nowego filtru ASP.NET MVC 5 i ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [sposobu korzystania z funkcji programu ASP.NET MVC 5 filtrowania zastąpień](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), i [filtru zastępuje w programie ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Atrybut routingu

ASP.NET MVC obsługuje teraz [trasami atrybutów](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), dzięki użyciu udział Timowi McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Atrybut routingu, można określić trasy przez dodawanie adnotacji do Twojej akcji i kontrolerów.

## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Firma Microsoft rozszerzoną środowisko tworzenia nowych projektów sieci web w programie Visual Studio 2013. W **nowego projektu sieci Web ASP.NET** okna dialogowego można wybrać typ projektu, skonfigurować dowolną kombinację technologii (Web Forms, MVC, Web API), skonfiguruj opcje uwierzytelniania i Dodaj jednostkowy projekt testowy.

![Nowy projekt ASP.NET](mvc5/_static/image1.png)

Nowe okno dialogowe umożliwia zmianę domyślne opcje uwierzytelniania dla wielu szablonów. Na przykład podczas tworzenia projektu formularzy sieci Web programu ASP.NET można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (członkostwa ASP.NET lub społecznościowych dostawcy logowania)
- Konta organizacyjne (Active Directory w aplikacji internetowej)
- Uwierzytelnianie systemu Windows (Active Directory w intranecie aplikacji)

![Opcje uwierzytelniania](mvc5/_static/image2.png)

Aby uzyskać więcej informacji na temat nowego procesu tworzenia projektów sieci web, zobacz [tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat nowej opcji uwierzytelniania, zobacz [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

Rusztowania ASP.NET to platforma generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, który współdziała z modelem danych.

W poprzednich wersjach programu Visual Studio szkieletów została ograniczona do projektów platformy ASP.NET MVC. Z programu Visual Studio 2013 można teraz używać szkieletów dla żadnego projektu ASP.NET, w tym formularzy sieci Web. Visual Studio 2013 aktualnie nie obsługuje generowania stron dla projektu formularzy sieci Web, ale można nadal używać szkieletów z formularzy sieci Web, dodając zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Jeśli przy użyciu funkcji szkieletów, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład uruchomienie z projektem formularzy sieci Web ASP.NET i następnie dodać Kontroler interfejsu API sieci Web za pomocą funkcja szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać szkieletów MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletu** i wybierz **zależności MVC 5** w oknie dialogowym. Dostępne są dwie opcje do tworzenia szkieletu MVC; Minimalne i pełne. W przypadku wybrania minimalny, tylko pakiety NuGet i odwołań dla platformy ASP.NET MVC zostaną dodane do projektu. Jeśli wybierzesz opcję pełne, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletu kontrolerów async korzysta z nowych funkcji asynchronicznych z programu Entity Framework 6.

Aby uzyskać więcej informacji i samouczki, zobacz [omówienie szkieletów ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Uzyskiwanie pomocy i zgłaszanie problemów

- [Znane problemy i istotne zmiany listy](../visual-studio/overview/2013/release-notes.md#knownissues)
- Uzyskiwanie pomocy i przedyskutować ASP.NET MVC 5 w [fora](https://forums.asp.net/1146.aspx)
- [Zgłosić błąd w programie ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Tworzenie żądania funkcji](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Uaktualnianie z programu ASP.NET MVC 4

Zobacz [jak uaktualnić platformy ASP.NET MVC 4 oraz projekt interfejsu API platformy ASP.NET MVC 5 i składnika Web API 2 w sieci Web](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
