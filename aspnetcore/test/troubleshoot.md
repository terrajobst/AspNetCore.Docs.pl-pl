---
title: Rozwiązywanie problemów z projektów platformy ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów ostrzeżeń i błędów z projektów platformy ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274596"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Rozwiązywanie problemów z projektów platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Poniższe łącza zawierają wskazówki dotyczące rozwiązywania problemów:

* [Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot)
* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Konferencja NDC (Londynie 2018): Diagnozowanie problemów w aplikacjach ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog platformy ASP.NET: Rozwiązywanie problemów z platformy ASP.NET Core problemy z wydajnością](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Ostrzeżenia .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Zainstalowano 32-bitowe i 64-bitowe wersje .NET Core SDK

W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:

> Zarówno 32- i 64 bitowych wersji zestawu SDK .NET Core są zainstalowane. Tylko szablony z wersje 64-bitowym zainstalowany na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/both32and64bit.png)

To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowej (x 64), jak i 32-bitowych (x86) [.NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane. Typowe przyczyny mogą być zainstalowane obie wersje obejmują:

* Pierwotnie pobrany Instalator zestawu SDK programu .NET Core za pomocą komputera 32-bitowego, ale następnie skopiowana między i instalować go na komputerze 64-bitowych.
* 32-bitowej platformy .NET Core SDK został zainstalowany przez inną aplikację.
* Nieprawidłowa wersja została pobrana i zainstalowana.

Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK jest zainstalowany w wielu lokalizacjach

W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:

> Zestaw SDK .NET Core zainstalowano w wielu lokalizacjach. Tylko szablony z zestawów SDK zainstalowany w "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/multiplelocations.png)

Ten komunikat zostanie wyświetlony, jeśli masz co najmniej jedna instalacja zestawu SDK .NET Core w katalogu poza *C:\\Program Files\\dotnet\\sdk\\*. Zazwyczaj dzieje się tak podczas .NET Core SDK został wdrożony na komputerze za pomocą kopiowania i wklejania zamiast Instalatora MSI.

Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.

### <a name="no-net-core-sdks-were-detected"></a>Nie wykryto nie .NET Core SDK

W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:

> Nie wykryto nie .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/NoNetCore.png)

To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów SDK Core .NET na tym komputerze. Aby rozwiązać ten problem:

* Zainstaluj lub sprawdź, czy jest zainstalowany zestaw SDK .NET Core.
* Sprawdź `PATH` zmiennej środowiskowej wskazuje lokalizację, jest zainstalowany zestaw SDK. Instalator zwykle ustawia `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji

Wywołanie platformy ASP.NET Core 2.1 i nowsze, `Html.Partial` powoduje analizatora ostrzeżenie ze względu na możliwe zakleszczenie. Komunikat ostrzegawczy jest:

> Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji. Należy rozważyć użycie `<partial>` pomocnika tagów lub `IHtmlHelper.PartialAsync`.

Wywołuje się `@Html.Partial` powinna zostać zastąpiona `@await Html.PartialAsync` lub pomocnika częściowe tagu `<partial name="_Partial" />`.

::: moniker-end
