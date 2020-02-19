---
title: ASP.NET Core Blazor modele hostingowe
author: guardrex
description: Informacje na temat Blazor modeli hostingu i Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 54be0e032a60c69880f428e52f9d778032385dc5
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447051"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET Core Blazor modele hostingowe

Autor [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor to platforma internetowa przeznaczona do uruchamiania po stronie klienta w przeglądarce w środowisku uruchomieniowym .NET runtime ( *Blazor webassembly*) lub po stronie serwera w ASP.NET Core ( [](https://webassembly.org/) *Blazor Server*). Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.

Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.

Aby uzyskać konfigurację zaawansowaną, zobacz <xref:blazor/hosting-model-configuration>.

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Główny model hostingu dla Blazor jest uruchamiany po stronie klienta w przeglądarce w zestawie webassembly. Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki. Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu. Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.

![[! OP. NO-LOC (Blazor)] webassembly: [! OP. Aplikacja NO-LOC (Blazor)] jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/blazor-webassembly.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj szablonu **aplikacjiBlazor webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Po wybraniu szablonu **aplikacjiBlazor webassembly** można skonfigurować aplikację do korzystania z zaplecza ASP.NET Core, zaznaczając pole wyboru **hostowane w ASP.NET Core** ([Nowy blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). Aplikacja ASP.NET Core obsługuje klientów Blazor aplikacji. Aplikacja webassembly Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).

Szablony obejmują skrypt `blazor.webassembly.js`, który obsługuje:

* Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.
* Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.

Model hostingu Blazor webassembly oferuje kilka korzyści:

* Nie istnieje zależność po stronie serwera .NET. Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.
* Zasoby i możliwości klienta są w pełni wykorzystywane.
* Zadania są Odciążone z serwera do klienta programu.
* Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji. Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).

Istnieją downsides Blazor hostingu zestawu webassembly:

* Aplikacja jest ograniczona do możliwości przeglądarki.
* Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).
* Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.
* Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe. Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.

## <a name="opno-locblazor-server"></a>Serwer Blazor

W modelu hostingu serwera Blazor aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [SignalR](xref:signalr/introduction) .

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem [! OP. Nie-LOC (sygnalizujący)] połączenie.](hosting-models/_static/blazor-server.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu Blazor Server, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). Aplikacja ASP.NET Core obsługuje aplikację serwera Blazor i tworzy punkt końcowy SignalR, w którym klienci nawiązują połączenie.

Aplikacja ASP.NET Core odwołuje się do klasy `Startup` aplikacji do dodania:

* Usługi po stronie serwera.
* Aplikacja do potoku obsługi żądania.

Skrypt `blazor.server.js`&dagger; nawiązuje połączenie z klientem. Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).

Model hostingu serwera Blazor oferuje kilka korzyści:

* Rozmiar pobieranych plików jest znacznie mniejszy niż Blazor aplikacji sieci webassembly, a aplikacja jest znacznie szybsza.
* Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.
* Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.
* Klienci zubożoni są obsługiwani. Na przykład Blazor aplikacje serwera współpracują z przeglądarkami, które nie obsługują elementu webassembly i urządzeń z ograniczeniami zasobów.
* Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.

Istnieją downsides Blazor hostingu serwera:

* Wyższe opóźnienia zwykle istnieją. Każda interakcja użytkownika obejmuje przeskok sieci.
* Brak obsługi offline. Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.
* Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami. Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.
* Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core. Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).

&dagger;skrypt `blazor.server.js` jest obsługiwany z zasobów osadzonych w ASP.NET Core udostępnionej platformie.

### <a name="comparison-to-server-rendered-ui"></a>Porównanie z renderowanym przez serwer interfejsem użytkownika

Jednym ze sposobów zrozumienia Blazor aplikacji serwerowych jest zrozumienie, jak różni się od tradycyjnych modeli służących do renderowania interfejsu użytkownika w aplikacjach ASP.NET Core przy użyciu widoków Razor lub Razor Pages. Oba modele używają języka Razor do opisywania zawartości HTML, ale znacząco różnią się sposobem renderowania znaczników.

Gdy strona Razor lub widok jest renderowany, każdy wiersz kodu Razor emituje kod HTML w postaci tekstowej. Po wyrenderowaniu serwer usuwa wystąpienie strony lub widoku, w tym dowolny utworzony stan. Gdy występuje inne żądanie dotyczące strony, na przykład w przypadku niepowodzenia walidacji serwera i wyświetlenia podsumowania walidacji:

* Cała strona zostanie ponownie przerenderowana na tekst HTML.
* Strona jest wysyłana do klienta.

Aplikacja Blazor składa się z elementów wielokrotnego użytku interfejsu użytkownika o nazwie *Components*. Składnik zawiera C# kod, znacznik i inne składniki. Gdy składnik jest renderowany, Blazor generuje wykres dołączonych składników podobne do Document Object Model HTML lub XML (DOM). Ten wykres zawiera stan składnika przechowywany w właściwościach i polach. Blazor oblicza wykres składnika, aby utworzyć binarną reprezentację znaczników. Format binarny może:

* Tekst w formacie HTML (podczas prerenderowania&dagger;).
* Służy do wydajnej aktualizacji znaczników podczas normalnego renderowania.

&dagger;*renderowanie* prerenderowane &ndash; żądany składnik Razor jest kompilowany na serwerze do statycznego kodu HTML i wysyłany do klienta, gdzie jest renderowany dla użytkownika. Po nawiązaniu połączenia między klientem a serwerem, statycznie renderowane elementy składnika są zastępowane elementami interaktywnymi. Renderowanie w czasie konwersji sprawia, że aplikacja będzie bardziej odpowiadać użytkownikowi.

Aktualizacja interfejsu użytkownika w Blazor jest wyzwalana przez:

* Interakcja z użytkownikiem, na przykład wybranie przycisku.
* Wyzwalacze aplikacji, takie jak czasomierz.

Wykres jest ponownie renderowany i obliczana *jest różnica między interfejsami* użytkownika. Różnica ta jest najmniejszym zestawem zmian modelu DOM wymaganym do zaktualizowania interfejsu użytkownika na kliencie. Różnica jest wysyłana do klienta w formacie binarnym i stosowana przez przeglądarkę.

Składnik jest usuwany po przejściu przez użytkownika na klienta. Gdy użytkownik korzysta ze składnika, stan składnika (usługi, zasoby) musi być przechowywany w pamięci serwera. Ponieważ stan wielu składników może być obsługiwany przez serwer współbieżnie, wyczerpanie pamięci jest problemem, który należy rozwiązać. Aby uzyskać wskazówki dotyczące sposobu tworzenia aplikacji serwera Blazor w celu zapewnienia optymalnego wykorzystania pamięci serwera, zobacz <xref:security/blazor/server>.

### <a name="circuits"></a>Elektrycznych

Aplikacja serwera Blazor jest oparta na [ASP.NET Core SignalR](xref:signalr/introduction). Każdy klient komunikuje się z serwerem za pośrednictwem co najmniej jednego SignalR połączenia o nazwie *obwodu*. Obwód Blazorabstrakcję za pośrednictwem SignalR połączeń, które mogą tolerować tymczasowe przerwy w sieci. Gdy klient Blazor zobaczy, że połączenie SignalR zostało rozłączone, próbuje ponownie nawiązać połączenie z serwerem przy użyciu nowego połączenia SignalR.

Każdy ekran przeglądarki (karta przeglądarki lub iframe), który jest połączony z aplikacją serwera Blazor używa połączenia SignalR. Jest to jeszcze inna ważna różnica w porównaniu z typowymi aplikacjami renderowanymi przez serwer. W aplikacji renderowanej na serwerze otwieranie tej samej aplikacji na wielu ekranach przeglądarki zazwyczaj nie jest przeważnie uwzględniane w dodatkowych wymaganiach dotyczących zasobów na serwerze. W aplikacji serwera Blazor, każdy ekran przeglądarki wymaga oddzielnego obwodu i oddzielnych wystąpień stanu składnika, które mają być zarządzane przez serwer programu.

Blazor uważa *, że zamyka* kartę przeglądarki lub przechodzenie do zewnętrznego adresu URL. W przypadku bezpiecznego zakończenia obwód i skojarzone zasoby są natychmiast uwalniane. Klient może również odłączyć się niebezpiecznie, na przykład z powodu przerwy w działaniu sieci. Serwer Blazor przechowuje rozłączone obwody przez konfigurowalny interwał, aby umożliwić klientowi Ponowne nawiązywanie połączenia.

### <a name="ui-latency"></a>Opóźnienie interfejsu użytkownika

Opóźnienie interfejsu użytkownika to czas od zainicjowanej akcji do momentu zaktualizowania interfejsu użytkownika. Mniejsze wartości opóźnienia interfejsu użytkownika są bezwzględnie konieczne, aby aplikacja mogła reagować na użytkownika. W aplikacji serwera Blazor każda akcja jest wysyłana do serwera, przetwarzane i różnic interfejsu użytkownika jest wysyłana z powrotem. W związku z tym opóźnienia interfejsu użytkownika to suma opóźnień sieci i opóźnienia serwera w przetwarzaniu akcji.

W przypadku aplikacji biznesowych, która jest ograniczona do prywatnej sieci firmowej, wpływ na postrzeganie opóźnień przez użytkownika z powodu opóźnienia sieci jest zwykle niezauważalny. W przypadku aplikacji wdrożonej za pośrednictwem Internetu opóźnienie może być zauważalne dla użytkowników, szczególnie w przypadku, gdy użytkownicy są szeroko rozproszona geograficznie.

Użycie pamięci może również przyczynić się do opóźnienia aplikacji. Zwiększone użycie pamięci powoduje częste zbieranie elementów bezużytecznych lub stronicowanie pamięci na dysku, co zmniejsza wydajność aplikacji i w związku z tym zwiększa opóźnienia interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz <xref:security/blazor/server>.

aplikacje serwera Blazor powinny być zoptymalizowane w celu zminimalizowania opóźnień interfejsu użytkownika przez zmniejszenie opóźnienia sieci i użycie pamięci. Aby uzyskać podejście do mierzenia opóźnień sieci, zobacz <xref:host-and-deploy/blazor/server#measure-network-latency>. Aby uzyskać więcej informacji na temat SignalR i Blazor, zobacz:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Połączenie z serwerem

aplikacje serwera Blazor wymagają aktywnego połączenia SignalR z serwerem. Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem. O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.

Aplikacja serwera Blazor jest przedstawiona w odpowiedzi na pierwsze żądanie klienta, która konfiguruje stan interfejsu użytkownika na serwerze. Gdy klient próbuje utworzyć połączenie SignalR, klient musi ponownie nawiązać połączenie z tym samym serwerem. aplikacje serwera Blazor, które używają więcej niż jednego serwera wewnętrznej bazy danych, powinny implementować *sesje programu sticky* SignalR dla połączeń.

Zalecamy korzystanie z [usługi Azure SignalR](/azure/azure-signalr) dla aplikacji Blazor Server. Usługa umożliwia skalowanie aplikacji serwera Blazor do dużej liczby jednoczesnych połączeń SignalR. Sesje programu Sticky Notes są włączone dla usługi Azure SignalR przez ustawienie opcji `ServerStickyMode` lub wartości konfiguracji usługi na `Required`. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/server#signalr-configuration>.

W przypadku korzystania z usług IIS sesje programu Sticky są włączane przy użyciu routingu żądań aplikacji. Aby uzyskać więcej informacji, zobacz [równoważenie obciążenia HTTP przy użyciu routingu żądań aplikacji](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
