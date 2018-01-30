---
uid: overview
title: "Omówienie programu ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Wprowadzenie do platformy ASP.NET, bezpłatna podstawę tworzenia witryn sieci Web, aplikacji sieci web i interfejsów API sieci web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 3d4c34a35e2e34ed78f481c759eda3718edb4da6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-overview"></a>Omówienie programu ASP.NET

Program ASP.NET to platforma wolnej sieci web umożliwiające tworzenie wspaniałych witryn sieci Web i aplikacji sieci web przy użyciu języka HTML, CSS i JavaScript. Można również tworzyć interfejsy API sieci Web i używać technologii czasu rzeczywistego, takich jak Web Sockets.

[Platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/) stanowi alternatywę dla platformy ASP.NET.  Zobacz [instrukcje dotyczące wyboru między ASP.NET i ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Wprowadzenie

[Pobierz program Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064), zwolnij IDE dla platformy ASP.NET w systemie Windows.

## <a name="websites-and-web-applications"></a>Aplikacje sieci web i witryn sieci Web

 Program ASP.NET oferuje trzy platform do tworzenia aplikacji sieci web: formularzy sieci Web, ASP.NET MVC i stron ASP.NET Web Pages. Wszystkie trzy platformy są stabilne i dojrzałe i aplikacji sieci web dużą można utworzyć za pomocą dowolnego z nich. Niezależnie od tego, jakie możesz wybrać platformy zostaną zainstalowane wszystkie korzyści i funkcje platformy ASP.NET wszędzie.

Każdy framework dotyczy programowanie inny styl. Możesz wybrać jedną zależy od kombinację programowania zasobów (wiedzy, umiejętności i środowisko programistyczne), typ aplikacji, które tworzysz i podejście do tworzenia, które znasz.

Poniżej znajduje się przegląd wszystkich platform i sugestii, jak wybrać między nimi. Jeśli wolisz wprowadzenie wideo, zobacz [tworzenie witryn sieci Web ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) i [co to jest narzędzia sieci Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Jeśli masz doświadczenie | Styl programowanie | Wiedzy | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Formularze sieci Web | Win Forms, WPF, .NET | Szybkie programowanie przy użyciu zaawansowanych biblioteki formantów, które hermetyzują kod znaczników HTML | RAD średni poziom, zaawansowane |
| MVC       | Ruby na szyny, .NET  | Pełną kontrolę nad znaczników HTML, kodu i znaczniki rozdzielone i łatwo można napisać testy. Najlepszym rozwiązaniem dla aplikacji mobilnych i jednej strony (SPA). | Średni poziom, zaawansowane |
| Model Web Pages  | Classic ASP, PHP     | Kod znaczników HTML i kod razem w jednym pliku | Nowy, średni poziom |

### <a name="web-forms"></a>Formularze sieci Web

Z formularzy sieci Web ASP.NET można tworzyć dynamicznych witryn sieci Web przy użyciu znanego modelu przeciągania i upuszczania, sterowanego zdarzeniami. Powierzchni projektowej oraz setkom kontrolek i składników pozwalają szybko tworzyć złożone, zaawansowane witryny sterowane za pośrednictwem interfejsu użytkownika z dostępem do danych. 

[Dowiedz się więcej o formularzy sieci Web](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC umożliwia wydajne, na podstawie wzorców do tworzenia dynamicznych witryn sieci Web, który umożliwia czyste rozdzielenie problemy i który umożliwia pełną kontrolę nad znaczników dla składników, elastyczne programowanie. Platforma ASP.NET MVC zawiera wiele funkcji umożliwiających szybkie, TDD przyjaznego programowania do tworzenia zaawansowanych aplikacji korzystających z najnowszych standardów sieci web. 

[Dowiedz się więcej o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Strony sieci Web ASP.NET i składnia Razor oferują szybki, bezpośredni i nieskomplikowany sposób łączenia kodu serwera z HTML w celu tworzenia dynamicznej zawartości sieci web. Połączenie z bazami danych, dodawanie wideo, połącz się z witrynami sieci społecznościowych i obejmuje wiele innych funkcji, które ułatwiają tworzenie doskonałych witryn, które są zgodne z najnowszych standardów sieci web.

[Dowiedz się więcej na temat stron sieci Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Uwagi dotyczące sieci Web Forms, MVC i stron sieci Web

Wszystkie trzy platformy ASP.NET są oparte na programie .NET Framework i udostępniać podstawowych funkcji programu .NET i programu ASP.NET. Na przykład wszystkie trzy struktur oferuje model zabezpieczeń logowania na podstawie tych członkostwa, a wszystkie trzy udostępnianie tych samych urządzeń do zarządzania żądaniami, obsługa sesji i tak dalej, które są częścią podstawowych funkcji programu ASP.NET.

Ponadto trzy struktur nie są całkowicie niezależne i wybierając jedną nie wyklucza przy użyciu innego. Ponieważ struktury mogą współistnieć w tej samej aplikacji sieci web, nie jest rzadko, aby wyświetlić poszczególne składniki aplikacji napisanych przy użyciu różnych platform. Na przykład klienta uwzględniającym części aplikacji mogą zostać opracowane w nazwie wzorca MVC, aby zoptymalizować znaczników, gdy dostęp do danych i części administracyjne są tworzone w formularzach sieci Web, aby móc korzystać z prostego danych dostępu i kontroli danych.

## <a name="web-apis"></a>Interfejsy Web API

Interfejs API sieci Web platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne. Interfejs API sieci Web ASP.NET jest idealną platformą do tworzenia RESTful aplikacji w programie .NET Framework.

[Więcej informacji na temat interfejsu API sieci Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Technologie w czasie rzeczywistym

Biblioteka SignalR platformy ASP.NET jest nową biblioteką dla deweloperów platformy ASP.NET, która ułatwia tworzenie funkcji sieci web w czasie rzeczywistym. SignalR umożliwia komunikację dwukierunkową między serwerem a klientem. Serwery można wypchnąć zawartości do połączonych klientów natychmiast po jej udostępnieniu. SignalR obsługuje gniazda sieci Web i powraca do innych technik zgodny dla starszych przeglądarek. Biblioteka SignalR zawiera interfejsy API umożliwiający zarządzanie połączeniami (na przykład nawiązywanie i zakańczanie zdarzeń), grupowanie połączeń i autoryzację.

[Dowiedz się więcej na temat biblioteki SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Aplikacje mobilne i w lokacjach 

Zasilanie natywnych aplikacji mobilnych z zaplecza interfejsu API sieci Web, a także za pomocą struktury elastyczny projekt, takich jak Twitter Bootstrap przenośnych witryn sieci web ASP.NET. Jeśli tworzysz natywnych aplikacji mobilnej jest łatwo utworzyć interfejs API sieci Web opartych na formacie JSON do obsługi dostępu do danych, uwierzytelnianie i powiadomień wypychanych do aplikacji. Jeśli tworzysz reakcji witrynę dla urządzeń przenośnych, można użyć dowolnego CSS framework lub system Otwórz siatki kopii zapasowej lub wybierz wydajny system przenośnych jak jQuery Mobile lub Sencha i niezawodnych aplikacji mobilnych z PhoneGap.

[Dowiedz się więcej o przenośnych rozwoju aplikacji i lokacji](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplikacje jednej strony 

ASP.NET pojedynczej strony aplikacji JEDNOSTRONICOWEJ pomaga tworzyć aplikacje, które zawierają istotne interakcji po stronie klienta, za pomocą HTML 5, CSS 3 i JavaScript. Visual Studio zawiera szablon do tworzenia aplikacji jednej strony za pomocą knockout.js i interfejsu API sieci Web platformy ASP.NET. Oprócz szablonie SPA wbudowane szablony utworzone społeczności SPA również są dostępne do pobrania.

[Dowiedz się więcej na temat tworzenia aplikacji jednostronicowej](single-page-application/index.md)

## <a name="webhooks"></a>Elementy webhook

Elementów Webhook to lekkie wzorzec HTTP zapewnienie modelu prostego pub/sub dla połączeń ze sobą usług interfejsów API sieci Web i SaaS. W przypadku zdarzeń w usłudze powiadomienie jest wysyłane w postaci żądanie HTTP POST do subskrybentów w zarejestrowany. Żądanie POST zawiera informacje dotyczące zdarzeń, dzięki czemu odbiorcy do działania w związku z tym.

Elementów Webhook są udostępniane przez wiele usług w tym Dropbox, GitHub Instagram, MailChimp, PayPal, zapas czasu, Trello i wiele innych. Na przykład elementu WebHook może oznaczać, że plik został zmieniony w Dropbox, lub zmiana kodu został zatwierdzony w witrynie GitHub, płatność została zainicjowana w PayPal lub karta została utworzona w Trello.

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
