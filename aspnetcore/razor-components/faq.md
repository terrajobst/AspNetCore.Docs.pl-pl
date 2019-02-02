---
title: Często zadawane pytania (FAQ) dotyczące składników Razor
author: guardrex
description: Znajdź odpowiedzi na często zadawane pytania dotyczące składników Razor i Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/faq
ms.openlocfilehash: 8555a95ed0dea2b8bcca173c6ba5acfa1a221cfe
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668177"
---
# <a name="frequently-asked-questions-faq-about-razor-components"></a>Często zadawane pytania (FAQ) dotyczące składników Razor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="what-are-razor-components-and-blazor"></a>Jakie są składniki Razor i Blazor?

*Składniki programu ASP.NET Core Razor* platforma aplikacji jednej strony sieci web oparta na platformie .NET, który jest wykonywany po stronie serwera w programie ASP.NET Core.

*Blazor* to rozszerzenie wykonywanie aplikacji po stronie klienta, który działa w przeglądarce, za pomocą format WebAssembly składników Razor. Blazor udostępnia wszystkie korzyści zapewnianych przez strukturę interfejsu użytkownika sieci web po stronie klienta przy użyciu platformy .NET na komputerze klienckim.

## <a name="im-new-to-net-whats-net"></a>Jestem jesteś nowym użytkownikiem platformy .NET. Co to jest .NET?

[.NET](https://www.microsoft.com/net) jest bezpłatne, dla wielu platform typu open source platformy deweloperskiej, do tworzenia wielu różnych typów aplikacji (pulpit, mobilnych, gier, sieci web). .NET zawiera zarządzane środowisko uruchomieniowe, zestaw standardowych [biblioteki](/dotnet/api), i obsługę wielu nowoczesnych języków programowania: [C#](/dotnet/csharp/), [ F# ](/dotnet/fsharp/), i [VB](/dotnet/visual-basic/). Możesz [Rozpocznij pracę z platformą .NET w 10-minutowy materiał](https://www.microsoft.com/net/learn/get-started/windows).

## <a name="why-would-i-use-net-for-web-development"></a>Dlaczego użyje .NET do tworzenia aplikacji internetowych

Przy użyciu platformy .NET w przeglądarce oferuje wiele korzyści, które może sprawić, tworzenie aplikacji sieci web łatwiejsze i bardziej wydajne:

* **Stabilny i spójny**: .NET oferuje standardowych interfejsów API, narzędzi i infrastruktury kompilacji na wszystkich platformach .NET, które są stałe, wyposażone w bogaty i łatwy w użyciu.
* **Nowoczesnych języków innowacyjne**: języków .NET, takich jak [ C# ](/dotnet/csharp/) i [ F# ](/dotnet/fsharp/) upewnij się, przynosi radość do programowania i stają się coraz lepsze za pomocą innowacyjnych nowe funkcje języka.
* **Najlepsze narzędzia**: [Programu Visual Studio](https://www.visualstudio.com/) rodziny produktów udostępnia doskonałe środowisko programistyczne dla platformy .NET w Windows, Linux i macOS.
* **Szybka i skalowalna**: .NET od dawna wydajność, niezawodność i bezpieczeństwo na serwerze. Przy użyciu platformy .NET, jako rozwiązanie w pełnym stosie ułatwia skalowanie aplikacji.

## <a name="how-can-you-run-net-in-a-web-browser"></a>Jak możesz uruchomić .NET w przeglądarce sieci web?

Uruchamianie platformy .NET w przeglądarce jest możliwe dzięki stosunkowo nowej technologii standardowych sieci web o nazwie [format WebAssembly](http://webassembly.org/). Format WebAssembly jest "przenośny, rozmiaru i obciążenia — czas oszczędne pod względem formacie odpowiednim do kompilacji w sieci Web." Kodzie skompilowanym z format WebAssembly można uruchomić w dowolnej przeglądarce szybkością natywnych. Aby uruchomić pliki binarne .NET w przeglądarce sieci web, używamy środowiska uruchomieniowego .NET (w szczególności [Mono](http://www.mono-project.com/news/2017/08/09/hello-webassembly/)) który został wcześniej skompilowany na format WebAssembly.

## <a name="does-blazor-compile-my-entire-net-based-app-to-webassembly"></a>Blazor kompilacja Moje całą. Aplikacja oparta na sieci, na format WebAssembly?

Nie, aplikacja Blazor składa się z normalnym skompilowanych zestawów platformy .NET, które są pobierane i uruchomić w przeglądarce sieci web przy użyciu opartych na format WebAssembly środowisko uruchomieniowe platformy .NET. Tylko .NET środowisko uruchomieniowe jest kompilowany do format WebAssembly. Że wspomniane, wsparcie dla pełnej [statyczne wcześniej kompilacji time (AoT)](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/) aplikacji na format WebAssembly może być coś, co możemy dodać kolejne podróży w dół.

## <a name="wouldnt-the-app-download-size-be-huge-if-it-also-includes-a-net-runtime"></a>Rozmiar pobieranych elementów aplikacji nie bylibyśmy w ogromnej, jeśli zawiera również środowisko uruchomieniowe .NET?

Niekoniecznie. Środowiska uruchomieniowe platformy .NET mają wszystkie kształty w rozmiarach. Wczesne prototypy Blazor używane compact środowisko uruchomieniowe platformy .NET (łącznie z wykonania zestawu, wyrzucanie elementów bezużytecznych, wątki), skompilowane do kilku, 60KB format WebAssembly. Blazor działa teraz w Mono, która jest obecnie dostępna znacznie większe. Jednak możliwości optymalizacji rozmiaru abound, łącznie z scalania i przycinanie danych binarnych środowiska uruchomieniowego i aplikacji. Inne potencjalne środki zaradcze rozmiar pobierania obejmują buforowanie i korzystanie z sieci CDN.

## <a name="what-features-will-razor-components-and-blazor-support"></a>Jakie funkcje zostaną składniki Razor i Blazor pomocy technicznej?

Składniki razor i Blazor obsługuje wszystkie funkcje framework nowoczesnych aplikacji jednej strony:

* Model component do tworzenia konfigurowalna interfejsu użytkownika
* Routing
* Układy
* Formularze i Walidacja
* Wstrzykiwanie zależności
* JavaScript interop
* Podczas programowania na żywo ponownego ładowania w przeglądarce
* Renderowanie po stronie serwera
* Pełny .NET zarówno w przeglądarkach i debugowania w środowisku IDE
* Rozbudowana funkcja IntelliSense i narzędzia
* Możliwość uruchamiania na starszych przeglądarkach (inne niż format WebAssembly) za pomocą platformy ASP.NET Core Razor składników
* Publikowanie i przycinania rozmiar aplikacji

## <a name="can-i-use-blazor-without-running-net-on-the-server"></a>Czy można używać Blazor, bez konieczności uruchamiania .NET na serwerze?

Tak, można wdrożyć aplikacji Blazor jako zbiór plików statycznych, bez konieczności każdy rodzaj pomocy technicznej platformy .NET na serwerze.

## <a name="can-i-use-blazor-with-aspnet-core-on-the-server"></a>Czy mogę korzystać Blazor z platformą ASP.NET Core na serwerze

Tak! Blazor integruje się z [platformy ASP.NET Core](/aspnet/core) na serwerze jako *składniki programu ASP.NET Core Razor* rozwiązanie rozwoju bezproblemową i spójną pełnym stosie sieci web.

## <a name="is-blazor-a-net-port-of-an-existing-javascript-framework"></a>Jest Blazor port .NET istniejącej struktury języka JavaScript?

Jest Blazor *nowej struktury* ZAINSPIROWANE z istniejącymi strukturami nowoczesnych aplikacji jednej strony, takich jak platformy React, usługi Angular i Vue.

## <a name="how-can-i-try-out-blazor"></a>Jak można wypróbować Blazor?

Tworzenie pierwszego Blazor web app wyewidencjonowanie naszych [przewodnik wprowadzenie](xref:razor-components/get-started).

## <a name="why-is-blazor-an-experimental-project"></a>Dlaczego jest Blazor "eksperymentalne"?

Blazor to eksperymentalne projekt, ponieważ nadal istnieją wiele pytań do odpowiedzi o jego efektywność i odwołania. Do celów tej początkowej fazie eksperymentalne są:

* Działa za pośrednictwem kwestiach technicznych.
* Oceny zainteresowania i do nasłuchiwania na opinie.

Jesteśmy optymistycznej o przyszłość firmy Blazor; ale w tej chwili nie jest zatwierdzona produktu Blazor i należy rozważyć wstępne alfa.

Składniki Razor programu ASP.NET Core jest obsługiwane w programie ASP.NET Core 3.0 lub nowszej.

## <a name="is-this-silverlight-all-over-again"></a>To jest program Silverlight wielu różnych źródeł ponownie?

Nie, Blazor to struktura sieci web platformy .NET oparte na kodzie HTML i CSS, która działa w przeglądarce przy użyciu standardów sieci web Otwórz. Nie wymaga wtyczki i działa na starszych przeglądarkach i urządzeniach przenośnych.

## <a name="does-blazor-use-xaml"></a>Blazor używa XAML?

Nie, Blazor to struktura sieci web w formacie HTML, CSS i innych standardowych technologii sieci web.

## <a name="is-webassembly-supported-in-all-browsers"></a>Format WebAssembly jest obsługiwane we wszystkich przeglądarkach?

Tak, format WebAssembly zdobyła consensus przeglądarek i [wszystkich nowoczesnych przeglądarek obsługuje teraz format WebAssembly](https://caniuse.com/#search=webassembly).

## <a name="does-blazor-work-on-mobile-browsers"></a>Blazor działa w przeglądarkach dla urządzeń przenośnych?

Tak, [nowoczesnych przeglądarek dla urządzeń przenośnych obsługują także format WebAssembly](https://caniuse.com/#search=webassembly).

## <a name="what-about-older-browsers-that-dont-support-webassembly-for-example-does-blazor-work-in-ie"></a>Jak wygląda starszych przeglądarkach, które nie obsługują format WebAssembly? Na przykład Blazor działa w programie Internet Explorer?

W przypadku starszych przeglądarkach, które nie obsługują format WebAssembly, użycie składniki Razor programu ASP.NET Core [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model). Aplikacje Razor składniki po stronie serwera Podaj doskonałej zgodności i wydajności za pomocą starsze przeglądarki.

## <a name="can-i-use-net-standard-libraries"></a>Można użyć biblioteki .NET Standard?

Tak, środowisko uruchomieniowe platformy .NET umożliwiający Blazor obsługuje .NET Standard 2.0. Interfejsy API, które nie są obsługiwane w przeglądarce klienta Blazor rzutowania *nieobsługiwane* wyjątków.

## <a name="dont-you-need-features-like-garbage-collection-and-threading-added-to-webassembly-to-make-this-work"></a>Nie potrzebujesz funkcji, takich jak wyrzucanie elementów bezużytecznych i wątkowość dodane do format WebAssembly, aby umożliwić?

Nie, format WebAssembly w jego bieżącym stanie nie jest wystarczająca. Środowisko uruchomieniowe platformy .NET obsługuje swój własny wyrzucania elementów bezużytecznych i wątkowość uwagi.

## <a name="can-i-use-existing-javascript-libraries"></a>Czy można używać istniejących bibliotek JavaScript?

Tak, aplikacje mogą wywoływać JavaScript za pomocą międzyoperacyjnych interfejsów API języka JavaScript.

## <a name="can-i-access-the-dom-from-an-app"></a>Można uzyskać dostęp do modelu DOM z aplikacji?

Można uzyskać dostęp do modelu DOM za pomocą międzyoperacyjności języka JavaScript z kodu .NET. Jednak framework oparty na komponentach minimalizuje potrzebę bezpośredniego dostępu do modelu DOM.

## <a name="why-mono-why-not-net-core-or-corert"></a>Dlaczego narzędzia Mono? Dlaczego nie platformy .NET Core lub CoreRT?

[Narzędzie mono](http://www.mono-project.com/) to implementacja typu open source sponsorowana Microsoft .NET Framework. Narzędzie mono jest używany przez [Xamarin](https://www.xamarin.com/) do tworzenia aplikacji klienta natywnego dla systemów Android, iOS i macOS. Narzędzie mono jest również używany przez [Unity](https://unity3d.com/) do tworzenia gier. Zespół platformy Xamarin firmy Microsoft [ogłosiły plany ich](http://www.mono-project.com/news/2017/08/09/hello-webassembly/) dodanie obsługi format WebAssembly do platformy Mono, dlatego został czyni stały postęp ([Mono i format WebAssembly — aktualizacje na statyczne kompilacji 1 16 2018](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/)). Ponieważ Blazor struktury interfejsu użytkownika sieci web po stronie klienta, ukierunkowanych na format WebAssembly, narzędzie Mono jest naturalnym wyborem.

Natomiast [platformy .NET Core](https://www.microsoft.com/net/learn/get-started/windows) jest używany głównie dla aplikacji serwerowych i dla aplikacji konsoli dla wielu platform. .NET core może służyć do tworzenia zaplecza programu ASP.NET Core dla aplikacji Blazor, ale nie w przypadku tworzenia aplikacji klienckich. [CoreRT](https://github.com/dotnet/corert) środowisko uruchomieniowe platformy .NET Core zoptymalizowana pod kątem kompilacja AoT; i gdy ma ono aspiracje format WebAssembly, projekt jest nadal prac w toku i nie produktem.

## <a name="where-did-the-name-blazor-come-from"></a>Nazwa "Blazor" skąd wzięła?

Blazor sprawia, że intensywne użycie [Razor](/aspnet/core/mvc/views/razor?view=aspnetcore-2.1), składnia kod znaczników HTML i C#. **Przeglądarka + Razor = Blazor!** Gdy wymawiane, jest również nazwę swanky kurtek, używane przez hipsters mających doskonałą smak w sposób, stylu i języków programowania.
