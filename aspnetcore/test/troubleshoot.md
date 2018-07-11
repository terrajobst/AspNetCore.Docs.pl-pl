---
title: Rozwiązywanie problemów z projektami ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów, ostrzeżenia i błędy w projektach programu ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938397"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Rozwiązywanie problemów z projektami ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Poniższe łącza zapewniają wskazówki dotyczące rozwiązywania problemów:

* [Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot)
* [Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Ndc Developers Conference (2018 r. Londyn;): Diagnozowanie problemów w aplikacji programu ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog platformy ASP.NET: Rozwiązywanie problemów z problemów z wydajnością programu ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Ostrzeżenia dotyczące zestawu .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32- i 64-bitowe wersje programu .NET Core SDK są instalowane.

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> Zarówno 32- i 64 bitowych wersjach programu .NET Core SDK są instalowane. Tylko szablony z wersji 64-bitowych zainstalowanej w lokalizacji "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/both32and64bit.png)

To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowych (x 64), jak i 32-bitowych (x86) [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane. Typowe przyczyny, które można zainstalować obie wersje obejmują:

* Pierwotnie pobrany przy użyciu komputera z 32-bitowy Instalator zestawu .NET Core SDK, ale następnie skopiować go na i zainstalować je na komputerze 64-bitowym.
* 32-bitowych .NET Core SDK został zainstalowany przez inną aplikację.
* Niewłaściwa wersja został pobrany i zainstalowany.

Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK jest zainstalowany w wielu lokalizacjach

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> .NET Core SDK jest zainstalowany w wielu lokalizacjach. Tylko szablony z zestawów SDK zainstalowanych na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/multiplelocations.png)

Ten komunikat jest wyświetlany, gdy masz co najmniej jedna instalacja zestawu SDK programu .NET Core w katalogu, poza *C:\\Program Files\\dotnet\\sdk\\*. Zwykle dzieje się tak w przypadku zestawu .NET Core SDK został wdrożony na maszynie za pomocą kopiowania/wklejania zamiast Instalatora MSI.

Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.

### <a name="no-net-core-sdks-were-detected"></a>Nie wykryto żadnych zestawów .NET Core SDK

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> Nie wykryto żadnych zestawów .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/NoNetCore.png)

To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów .NET Core SDK na maszynie. Aby rozwiązać ten problem:

* Zainstaluj lub sprawdzić, czy jest zainstalowany zestaw .NET Core SDK.
* Upewnij się, że `PATH` zmienna środowiskowa wskazuje lokalizację, w którym jest zainstalowany zestaw SDK. Instalator zwykle ustawia `PATH`.
