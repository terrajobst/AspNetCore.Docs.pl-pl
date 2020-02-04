---
title: ASP.NET Core testowanie obciążenia/obciążeniowego
author: Jeremy-Meng
description: Poznaj kilka istotnych narzędzi i metod testowania obciążeniowego i testowania obciążeniowego ASP.NET Core aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: cb6015f737b6a93127016ab0f21b58e34b624ff3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/04/2020
ms.locfileid: "77004295"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core testowanie obciążenia/obciążeniowego

Testowanie obciążeniowe i testowanie obciążeniowe są ważne, aby zapewnić, że aplikacja sieci Web jest wydajna i skalowalna. Ich cele są różne, chociaż często korzystają z podobnych testów.

**Testy obciążenia** &ndash; sprawdzić, czy aplikacja może obsłużyć określone obciążenie użytkownikami w pewnym scenariuszu, zachowując jednocześnie cel odpowiedzi. Aplikacja jest uruchamiana w normalnych warunkach.

**Testy obciążeniowe** &ndash;ą stabilność aplikacji testowej w przypadku uruchamiania w skrajnych warunkach, często przez długi czas. Testy zapewniają duże obciążenie użytkownikami, przerastają lub stopniowo zwiększają obciążenie, w aplikacji lub ograniczają zasoby obliczeniowe aplikacji.

Testy obciążeniowe określają, czy aplikacja poddawana obciążeniom może odzyskać sprawność po awarii i bezpiecznie wrócić do oczekiwanego zachowania. W obszarze naprężenie aplikacja nie jest uruchamiana w normalnych warunkach.

Program Visual Studio 2019 to Ostatnia wersja programu Visual Studio z funkcjami testów obciążenia. W przypadku klientów wymagających narzędzi testowania obciążenia w przyszłości zalecamy używanie alternatywnych narzędzi, takich jak Apache JMeter, Akamai CloudTest i BlazeMeter. Aby uzyskać więcej informacji, zobacz [Informacje o wersji programu Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

## <a name="visual-studio-tools"></a>Narzędzia programu Visual Studio

Program Visual Studio umożliwia użytkownikom tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci Web. Dostępna jest opcja tworzenia testów przez rejestrowanie akcji w przeglądarce internetowej.

Aby uzyskać informacje na temat sposobu tworzenia, konfigurowania i uruchamiania projektów testów obciążenia przy użyciu programu Visual Studio 2017, zobacz [Szybki Start: Tworzenie projektu testu obciążenia](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

Testy obciążenia można skonfigurować do uruchamiania lokalnego lub uruchamiania w chmurze przy użyciu usługi Azure DevOps.

## <a name="third-party-tools"></a>Narzędzia innych firm

Poniższa lista zawiera narzędzia do oceny wydajności sieci Web innych firm z różnymi zestawami funkcji:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [k6](https://k6.io)
* [Chleb](https://locust.io/)
* [Moje przepięcio zachodnie wiatru](https://websurge.west-wind.com/)
* [W ramach](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)

