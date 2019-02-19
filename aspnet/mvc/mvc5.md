---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 platformy ASP.NET MVC 5 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorców projektowych sprawdzone oraz dzięki możliwościom AS...
ms.author: riande
ms.date: 10/11/2018
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 14fcf863a4ef5f6c9180cdf9e7b632ccdb1ebcb0
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410472"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>What's New in ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Szablony projektów sieci Web MVC bezproblemowo zintegrować ze środowiskiem aplikacji One ASP.NET. Można dostosować swój projekt MVC i skonfigurować uwierzytelnianie przy użyciu Kreatora tworzenia projektu aplikacji One ASP.NET. Samouczek wprowadzający do ASP.NET MVC 5, można znaleźć w folderze [wprowadzenie do ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Aby uzyskać informacje na temat uaktualniania projektów MVC 4 do MVC 5, zobacz [uaktualnianie ASP.NET MVC 4 i projektu interfejsu Web API platformy ASP.NET MVC 5 i Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Szablony projektów MVC zostały zaktualizowane do użycia produktu ASP.NET Identity do uwierzytelniania i zarządzania tożsamościami. Samouczek uwierzytelniania serwisu Facebook i firmą Google i nowego członkostwa interfejsu API, można znaleźć w folderze [tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) i [wdrażanie aplikacji platformy ASP.NET MVC Secure z Członkostwa, uwierzytelnianiem OAuth i bazy danych SQL do witryny sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Szablon projektu MVC została zaktualizowana w celu użycia [Bootstrap](http://getbootstrap.com/) zapewnienie elegancki i elastyczny wyglądu i działania, można łatwo dostosować. Aby uzyskać więcej informacji, zobacz [Bootstrap w szablonach projektu sieci web programu Visual Studio](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtry uwierzytelniania

[Filtry uwierzytelniania](http://www.dotnetcurry.com/showarticle.aspx?ID=957) to nowy rodzaj filtru we wzorcu ASP.NET MVC, zaplanowane przed filtry autoryzacji w potoku platformy ASP.NET MVC, która pozwala użytkownikowi na określenie uwierzytelniania logiki akcję, na kontrolerze lub globalnie dla wszystkich kontrolerów. Filtry uwierzytelniania przetwarzania poświadczenia w żądaniu i zapewnienia odpowiedniej jednostki. Filtry uwierzytelniania można również dodać wezwań do uwierzytelnienia w odpowiedzi na nieautoryzowanego żądania. Zobacz [filtry uwierzytelniania platformy ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtry uwierzytelniania w aplikacji ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Przesłonięcia filtru

Możesz teraz zastąpić, które filtry mają zastosowanie do metody danej akcji lub kontrolera, określając [zastąpienia filtra](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtry zastąpienie określają zestaw typów filtrów, które nie powinna być uruchamiana dla danego zakresu (akcji lub kontrolera). Dzięki temu można skonfigurować filtry, które są stosowane globalnie, ale następnie wykluczyć określone filtry globalne zastosowanie do określonych akcji i kontrolerów. Zobacz [przesłonięcia filtru nowych funkcji w ASP.NET MVC 5 i wzorca ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [sposób korzystania z platformy ASP.NET MVC 5 zastąpienia funkcji filtrowania](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), i [przesłonięcia filtru w programie ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Routing atrybutów

ASP.NET MVC obsługuje teraz [trasowanie atrybutów](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), dzięki udział Tim McCall, Autor [AttributeRouting](https://github.com/mccalltd/AttributeRouting). Z routingiem atrybutów można określić trasy, dodawanie adnotacji do Twojej akcji i kontrolerów.

## <a name="new-web-project-experience"></a>Nowe środowisko projektu sieci Web

Program Visual Studio rozszerzone możliwości tworzenia nowych projektów sieci web, począwszy od programu Visual Studio 2013. W **nowego projektu sieci Web platformy ASP.NET** okna dialogowego, można wybrać typ projektu chcesz, skonfigurować dowolną kombinację technologii (Web Forms, MVC, interfejs API sieci Web), skonfiguruj opcje uwierzytelniania, Dodaj obsługę platformy Docker i Dodaj projekt testu jednostkowego.

![Nowy projekt ASP.NET](mvc5/_static/new-aspnet-web-app-dialog.png)

Okno dialogowe umożliwia zmianę domyślne opcje uwierzytelniania dla wielu szablonów. Na przykład po utworzeniu projektu ASP.NET Web Forms można wybrać jedną z następujących opcji:

- Bez uwierzytelniania
- Indywidualne konta użytkowników (członkostwa ASP.NET lub społecznościowych dostawcy logowania)
- Konta służbowe (Active Directory w aplikacji internetowej)
- Windows Authentication (Active Directory w intranecie aplikacji)

![Opcje uwierzytelniania](mvc5/_static/change-authentication-dialog.png)

Aby uzyskać więcej informacji na temat procesu tworzenia projektów sieci web, zobacz [tworzenia projektów sieci Web platformy ASP.NET w programie Visual Studio](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Aby uzyskać więcej informacji na temat opcji uwierzytelniania, zobacz [produktu ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Funkcja tworzenia szkieletu ASP.NET

Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia on dodać schematyczny kod służący do projektu, która współdziała z modelu danych.

W wersjach programu Visual Studio przed 2013 tworzenia szkieletu była ograniczona do projektów programu ASP.NET MVC. Począwszy od programu Visual Studio 2013 umożliwia tworzenie szkieletów dla każdego projektu programu ASP.NET, w tym formularzy sieci Web. Program Visual Studio nie obsługuje obecnie generowania strony dla projektu formularzy sieci Web, ale nadal umożliwia tworzenie szkieletów przy użyciu formularzy sieci Web przez dodanie zależności MVC do projektu. Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej wersji.

Korzystając z tworzenia szkieletu, wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczęcie projektu programu ASP.NET Web Forms, a następnie dodaj Kontroler interfejsu API sieci Web, korzystając z tworzenia szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.

Aby dodać MVC scaffolding projekt formularzy sieci Web, należy dodać **nowy element szkieletu** i wybierz **MVC 5 zależności** w oknie dialogowym. Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; **Zależności minimalnej** i **pełne zależności**. Jeśli wybierzesz **zależności minimalnej**, tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC, które są dodawane do projektu. Jeśli wybierzesz **pełne zależności**, minimalnym zależności zostaną dodane oraz wymagane pliki zawartości projektu MVC.

![Dodaj okno dialogowe Tworzenie szkieletu w programie Visual Studio](overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

Obsługę tworzenia szkieletów async kontrolerów korzysta z funkcji asynchronicznej z platformy Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [omówienie tworzenia szkieletu ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="get-help-and-report-issues"></a>Get-Help i zgłaszanie problemów

- [Znane problemy i istotnej zmiany listy](../visual-studio/overview/2013/release-notes.md#knownissues)
- Uzyskaj Pomoc i omawiania ASP.NET MVC 5 w [forów](https://forums.asp.net/1146.aspx)
- [Zgłoś usterkę w programie ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Żądania funkcji](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrade-from-aspnet-mvc-4"></a>Uaktualnianie z programu ASP.NET MVC 4

Zobacz [sposób uaktualniania wzorca ASP.NET MVC 4 i Web projekt interfejsu API platformy ASP.NET MVC 5 i Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
