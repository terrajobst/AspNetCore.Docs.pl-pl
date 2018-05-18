---
title: Rozwiązywanie problemów z dla platformy ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów ostrzeżeń i błędów z projektów platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 3bba085c69ee96b5725331b14dcf15350d66e4a4
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Rozwiązywanie problemów z projektów platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Poniższe łącza zawierają wskazówki dotyczące rozwiązywania problemów:

* [Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot)
* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnozowanie problemów w aplikacjach ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Ostrzeżenia .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Zainstalowano 32-bitowe i 64-bitowe wersje .NET Core SDK
W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/both32and64bit.png)

To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowej (x 64), jak i 32-bitowych (x86) [.NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane. Typowe przyczyny, można ją zainstalować obie wersje obejmują:

* Pierwotnie pobrany przy użyciu komputera 32-bitowego, Instalator .NET Core SDK, ale następnie skopiować go na i instalować go na komputerze 64-bitowych. 
* 32-bitowej platformy .NET Core SDK został zainstalowany przez inną aplikację.
* Nieprawidłowa wersja została pobrana i zainstalowana.

Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK jest zainstalowany w wielu lokalizacjach
W **nowy projekt** okna dialogowego dla platformy ASP.NET Core mogą pojawić się następujące ostrzeżenie: 

 Zestaw SDK .NET Core zainstalowano w wielu lokalizacjach. Tylko szablony z Jeśli zainstalowana w "C:\Program Files\dotnet\sdk\' będą wyświetlane.

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/multiplelocations.png)

Ten komunikat zostanie wyświetlony, jeśli masz co najmniej jedna instalacja zestawu SDK .NET Core w katalogu poza * C:\Program Files\dotnet\sdk\*. Zwykle ma to miejsce podczas .NET Core SDK został wdrożony na komputerze za pomocą kopiowania i wklejania zamiast Instalatora MSI.

Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.

### <a name="no-net-core-sdks-were-detected"></a>Nie wykryto nie .NET Core SDK
W **nowy projekt** okna dialogowego dla platformy ASP.NET Core mogą pojawić się następujące ostrzeżenie: 

**Nie wykryto nie .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH"**

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/NoNetCore.png)

To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów SDK Core .NET na tym komputerze. Aby rozwiązać ten problem:

* Zainstaluj lub sprawdź, czy jest zainstalowany zestaw SDK .NET Core.
* Sprawdź `PATH` zmiennej środowiskowej wskazuje lokalizację, jest zainstalowany zestaw SDK. Instalator zwykle ustawia `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a>Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji

Wywołanie platformy ASP.NET Core 2.1 i nowsze, `Html.Partial` powoduje analizatora ostrzeżenie ze względu na możliwe zakleszczenie. Komunikat ostrzegawczy jest:

*Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji. Należy rozważyć użycie `<partial>` pomocnika tagów lub `IHtmlHelper.PartialAsync`.*

Wywołuje się `@Html.Partial` powinna zostać zastąpiona `@await Html.PartialAsync` lub pomocnika częściowe tagu `<partial name="_Partial" />`.

::: moniker-end
