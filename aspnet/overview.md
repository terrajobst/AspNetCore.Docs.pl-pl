---
uid: overview
title: Omówienie programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wprowadzenie do platformy ASP.NET, bezpłatna architektura służąca do tworzenia witryn sieci Web, aplikacji sieci web i interfejsów API sieci web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 5bdebcc226050afc2469840dc4a4dc97ec6b80b2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755768"
---
# <a name="aspnet-overview"></a>Omówienie programu ASP.NET

ASP.NET to bezpłatne środowisko internetowe umożliwiające tworzenie doskonałych witryn oraz aplikacji internetowych za pomocą kodu HTML, CSS i JavaScript. Można również tworzyć interfejsy API sieci Web i używać technologii czasu rzeczywistego, takich jak Web Sockets.

[Platforma ASP.NET Core](https://docs.microsoft.com/aspnet/core/) stanowi alternatywę dla platformy ASP.NET.  Zobacz [wskazówki dotyczące wyboru między ASP.NET i ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Wprowadzenie

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), bezpłatne środowisko IDE dla platformy ASP.NET w Windows.

## <a name="websites-and-web-applications"></a>Aplikacje sieci web i witryn sieci Web

 Program ASP.NET oferuje trzy platformy tworzenia aplikacji sieci web: formularze sieci Web, ASP.NET MVC i stron ASP.NET Web Pages. Wszystkie trzy struktury są stabilne i dojrzałych i możesz tworzyć aplikacje sieci web przy użyciu dowolnego z nich. Niezależnie od tego, w jakiej możesz wybrać platformy otrzymasz wszystkie korzyści i funkcje platformy ASP.NET wszędzie.

Każdego szablonu jest przeznaczony dla stylu różnych rozwoju. Możesz wybrać jedną zależy od kombinacji zasobów programowania (wiedzy, umiejętności i środowisko programistyczne), typ aplikacji, którą tworzysz i podejście do tworzenia, które komfortowo, jednocześnie.

Poniżej przedstawiono omówienie każdego z struktury i kilka pomysłów na to, jak dokonać wyboru między nimi. Jeśli wolisz wprowadzenie wideo, zobacz [tworzenie witryn internetowych ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) i [co to jest narzędzia sieci Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Jeśli masz doświadczenie | Styl projektowania | Doświadczenie | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Formularze sieci Web | Win Forms, WPF, .NET | Szybkie opracowywanie przy użyciu bogata Biblioteka formantów, które hermetyzują kod znaczników HTML | RAD średniego poziomu, zaawansowany |
| MVC       | Ruby on Rails, platformy .NET  | Pełną kontrolę nad kod znaczników HTML, kodu i znaczników rozdzielonych i łatwe do pisania testów. Najlepszym wyborem dla aplikacji mobilnych i jednostronicowej (SPA). | Średniego poziomu, zaawansowany |
| Model Web Pages  | Classic ASP, PHP     | Kod znaczników HTML, a kod razem w tym samym pliku | Nowy, średniego poziomu |

### <a name="web-forms"></a>Formularze sieci Web

ASP.NET Web Forms umożliwiają tworzenie dynamicznych witryn sieci Web przy użyciu znanego modelu przeciągania i upuszczania, oparte na zdarzeniach. Powierzchni projektowej oraz setkom kontrolek i składników pozwalają szybko tworzyć złożone, zaawansowane witryny opartej na interfejsie użytkownika z dostępem do danych. 

[Dowiedz się więcej o formularzy sieci Web](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC zapewnia zaawansowany, bazujący na wzorcach sposób tworzenia dynamicznych witryn internetowych, które umożliwia wyraźne oddzielenie obaw i zapewnia pełną kontrolę nad znacznikami dla przyjemne, elastyczne programowanie. Platforma ASP.NET MVC zawiera wiele funkcji umożliwiających szybkie, przyjazne projektowanie oparte na testach Programowanie w celu tworzenia zaawansowanych aplikacji korzystających z najnowszych standardów sieci web. 

[Dowiedz się więcej o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Strony ASP.NET Web Pages o składni Razor zapewniają szybki, przystępny i nieskomplikowany sposób łączenia kodu serwera z kodem HTML w celu tworzenia dynamicznej zawartości internetowej. Łączenie z bazami danych, dodawanie filmu wideo, połącz się z witrynami sieci społecznościowych i obejmuje wiele innych funkcji, które pomagają tworzyć piękne witryny, które są zgodne z najnowszych standardów sieci web.

[Dowiedz się więcej na temat stron sieci Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Uwagi dotyczące formularzy sieci Web, MVC i Web Pages

Wszystkie trzy struktury ASP.NET są oparte na programie .NET Framework i udostępniaj podstawowych funkcji programu .NET i programu ASP.NET. Na przykład wszystkie trzy struktury oferuje model zabezpieczeń logowania na podstawie członkostwa, a wszystkie trzy udostępniać te same urządzenia do zarządzania żądaniami, obsługa sesji i tak dalej, które są częścią podstawowych funkcji programu ASP.NET.

Ponadto trzy struktur są całkowicie niezależne i wybierając jedną nie wyklucza korzystania z innej. Ponieważ struktury mogą współistnieć w tej samej aplikacji sieci web, nie jest niczym niezwykłym, aby wyświetlić poszczególne składniki aplikacji napisanych przy użyciu różnych platform. Na przykład przeznaczonych dla klientów części aplikacji mogą być tworzone w MVC w celu optymalizacji znaczników, podczas dostępu do danych i administracyjnych fragmenty są opracowywane w formularzach sieci Web, aby móc korzystać z formantów danych i prostego data access.

## <a name="web-apis"></a>Interfejsy Web API

Web API platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. Web API platformy ASP.NET jest idealną platformą do tworzenia aplikacji typu RESTful na .NET Framework.

[Dowiedz się więcej na temat interfejsu API sieci Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Z technologii czasu rzeczywistego

Biblioteki SignalR platformy ASP.NET jest nową biblioteką dla deweloperów platformy ASP.NET, który ułatwia opracowywanie funkcji sieci web w czasie rzeczywistym. SignalR umożliwia komunikację dwukierunkową między serwerem a klientem. Serwery można wypchnąć zawartości do połączonych klientów natychmiast po jej udostępnieniu. SignalR obsługuje gniazda sieci Web i powraca do innych technik zgodne starsze przeglądarki. SignalR obejmuje funkcje interfejsu API umożliwiający zarządzanie połączeniami (na przykład nawiązywać połączenia i zdarzenia rozłączenia), grupowanie połączeń i autoryzację.

[Dowiedz się więcej na temat biblioteki SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Aplikacje mobilne i lokacje 

Program ASP.NET może obsługiwać natywnych aplikacji mobilnych za pomocą interfejsu API sieci Web zaplecza, a także mobilnej witryny sieci web przy użyciu środowisk elastyczne, takich jak Twitter Bootstrap. Jeśli tworzysz natywnych aplikacji mobilnych jest łatwe tworzenie interfejsu API sieci Web opartych na formacie JSON na dostęp do danych uchwyt, uwierzytelnianie i powiadomienia wypychane dla aplikacji. Jeśli tworzysz interaktywnych witryn mobilnych, można użyć dowolnego CSS framework lub systemu siatki Otwórz kopii zapasowej lub wybierz zaawansowany system urządzenia przenośnego, np. jQuery Mobile lub Sencha i niezawodnych aplikacji mobilnych za pomocą PhoneGap.

[Dowiedz się więcej o programowaniu rozwiązań mobilnych, aplikacji i witryny](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplikacje jednej strony 

ASP.NET pojedynczej strony aplikacji (SPA) pomaga w tworzeniu aplikacji, które zawierają istotne interakcji po stronie klienta przy użyciu języków HTML 5, CSS 3 i JavaScript. Visual Studio zawiera szablon służący do tworzenia aplikacji jednostronicowej przy użyciu struktura knockout.js i Web API platformy ASP.NET. Oprócz wbudowany szablon SPA utworzonych przez społeczność SPA są także dostępne szablony do pobrania.

[Dowiedz się więcej o programowaniu aplikacji jednostronicowej](single-page-application/index.md)

## <a name="webhooks"></a>Elementy webhook

Elementy Webhook jest lekkiego wzorca HTTP, podając modelu publikowania/subskrybowania proste dołączenie razem interfejsów API sieci Web i usług SaaS. Przypadku wystąpienia zdarzenia w usłudze, powiadomienie jest wysyłane w formularzu żądania HTTP POST do zarejestrowanych subskrybentów. Żądanie POST zawiera informacje o zdarzeniu, co umożliwia odbiorcy podejmowanie odpowiednich działań.

Elementy Webhook są udostępniane przez dużą liczbę usługi takie jak Dropbox, GitHub, usłudze Instagram, MailChimp, PayPal, Slack, Trello i wielu innych. Na przykład element WebHook można wskazać, że plik zmienił się w usłudze Dropbox, zmiany kodu został zatwierdzony w usłudze GitHub lub płatność została zainicjowana w systemie PayPal lub karta została utworzona w usłudze Trello.

[Dowiedz się więcej na temat elementów Webhook](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
