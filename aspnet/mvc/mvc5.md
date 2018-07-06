---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 platformy ASP.NET MVC 5 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorców projektowych sprawdzone oraz dzięki możliwościom AS...
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 97423f9a2e66187329887c4c5e92bb20cb9a7fc4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814343"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>What's New in ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Szablony projektów sieci Web MVC bezproblemowo integrować z nowego środowiska aplikacji One ASP.NET. Można dostosować swój projekt MVC i skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu aplikacji One ASP.NET. Samouczek wprowadzający do ASP.NET MVC 5, można znaleźć w folderze [wprowadzenie do ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [uaktualnianie ASP.NET MVC 4 i projektu interfejsu Web API platformy ASP.NET MVC 5 i Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów MVC zostały zaktualizowane do użycia produktu ASP.NET Identity do uwierzytelniania i zarządzania tożsamościami. Samouczek uwierzytelniania serwisu Facebook i firmą Google i nowego członkostwa interfejsu API, można znaleźć w folderze [tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) i [wdrażanie aplikacji platformy ASP.NET MVC Secure z Członkostwa, uwierzytelnianiem OAuth i bazy danych SQL do witryny sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Usługa ładowania początkowego

Szablon projektu MVC została zaktualizowana w celu użycia [Bootstrap](http://getbootstrap.com/) zapewnienie elegancki i elastyczny wyglądu i działania, można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

[Filtry uwierzytelniania](http://www.dotnetcurry.com/showarticle.aspx?ID=957) to nowy rodzaj filtru we wzorcu ASP.NET MVC, zaplanowane przed filtry autoryzacji w potoku platformy ASP.NET MVC, która pozwala użytkownikowi na określenie uwierzytelniania logiki akcję, na kontrolerze lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzania poświadczenia w żądaniu i zapewnienia odpowiedniej jednostki. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi na nieautoryzowanego żądania. Zobacz [filtry uwierzytelniania platformy ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtry uwierzytelniania w aplikacji ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Przesłonięcia filtru

Możesz teraz zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając [zastąpienia filtra](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtry zastąpienie określają zestaw typów filtrów, które nie powinna być uruchamiana dla danego zakresu (akcji lub kontrolera). Dzięki temu można skonfigurować filtry, które są stosowane globalnie, ale następnie wykluczyć określone filtry globalne zastosowanie do określonych akcji i kontrolerów. Zobacz [przesłonięcia filtru nowych funkcji w ASP.NET MVC 5 i wzorca ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [sposób korzystania z platformy ASP.NET MVC 5 zastąpienia funkcji filtrowania](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), i [przesłonięcia filtru w programie ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Routing atrybutów

ASP.NET MVC obsługuje teraz [trasowanie atrybutów](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), dzięki udział Tim McCall, Autor [ http://attributerouting.net ](http://attributerouting.net). Z routingiem atrybutów można określić trasy, dodawanie adnotacji do Twojej akcji i kontrolerów.

## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Rozszerzyliśmy możliwości tworzenia nowych projektów sieci web w programie Visual Studio 2013. W **nowego projektu sieci Web platformy ASP.NET** okna dialogowego, można wybrać typ projektu, skonfigurować dowolną kombinację technologii (Web Forms, MVC, interfejs API sieci Web), skonfiguruj opcje uwierzytelniania i Dodaj projekt testu jednostkowego.

![Nowy projekt ASP.NET](mvc5/_static/image1.png)

Nowe okno dialogowe umożliwia zmianę domyślne opcje uwierzytelniania dla wielu szablonów. Na przykład po utworzeniu projektu ASP.NET Web Forms można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (członkostwa ASP.NET lub społecznościowych dostawcy logowania)
- Konta organizacji (Active Directory w aplikacji internetowej)
- Windows Authentication (Active Directory w intranecie aplikacji)

![Opcje uwierzytelniania](mvc5/_static/image2.png)

Aby uzyskać więcej informacji na temat nowego procesu do tworzenia projektów sieci web, zobacz [tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat nowej opcji uwierzytelniania, zobacz [produktu ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, która współdziała z modelu danych.

W poprzednich wersjach programu Visual Studio tworzenia szkieletu była ograniczona do projektów programu ASP.NET MVC. Za pomocą programu Visual Studio 2013 można teraz używać tworzenia szkieletów dla każdego projektu programu ASP.NET, w tym formularzy sieci Web. Visual Studio 2013 aktualnie nie obsługuje generowania strony dla projektu formularzy sieci Web, ale nadal umożliwia tworzenie szkieletów przy użyciu formularzy sieci Web przez dodanie zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

Korzystając z tworzenia szkieletu, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczęcie projektu programu ASP.NET Web Forms, a następnie dodaj Kontroler interfejsu API sieci Web, korzystając z tworzenia szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać MVC scaffolding projekt formularzy sieci Web, należy dodać **nowy element szkieletu** i wybierz **MVC 5 zależności** w oknie dialogowym. Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; Minimalna i pełne. Jeśli wybierzesz przycisku minimalnych tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC są dodawane do projektu. Jeśli wybierzesz opcję pełnej, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.

Obsługę tworzenia szkieletów kontrolerów async korzysta z nowych funkcji asynchronicznej z platformy Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [omówienie tworzenia szkieletu ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Uzyskiwanie pomocy i zgłaszania problemów dotyczących

- [Znane problemy i istotnej zmiany listy](../visual-studio/overview/2013/release-notes.md#knownissues)
- Uzyskaj Pomoc i omawiania ASP.NET MVC 5 w [forów](https://forums.asp.net/1146.aspx)
- [Zgłoś usterkę w programie ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Żądania funkcji](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Uaktualnianie z programu ASP.NET MVC 4

Zobacz [sposób uaktualniania wzorca ASP.NET MVC 4 i Web projekt interfejsu API platformy ASP.NET MVC 5 i Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
