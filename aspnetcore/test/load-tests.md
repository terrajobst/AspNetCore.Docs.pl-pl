---
title: Testowanie obciążenia/obciążeniowe platformy ASP.NET Core
author: Jeremy-Meng
description: Informacje o kilku istotnych narzędzi i podejścia do testowania obciążenia i aplikacje platformy ASP.NET Core testowanie obciążeniowe.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903043"
---
# <a name="aspnet-core-loadstress-testing"></a>Testowanie obciążenia/obciążeniowe platformy ASP.NET Core

Testowanie obciążenia i testy obciążenia są ważne, aby upewnić się, że aplikacja sieci web jest wydajne i skalowalne. Swoje cele są różne, nawet jeśli często mają podobne testów.

**Testy obciążenia** &ndash; sprawdzić, czy aplikacja może obsługiwać określonego obciążenia użytkowników dla niektórych scenariuszy nadal spełniając docelowy odpowiedzi. Aplikacja jest uruchamiana w normalnych warunkach.

**Testy obciążeniowe** &ndash; testowych stabilność aplikacji, podczas działania w warunkach extreme często przez dłuższy czas. Testy umieść duże obciążenie użytkownikami, wzrostów lub stopniowo zwiększa obciążenie dla aplikacji lub ograniczać zasoby obliczeniowe aplikacji.

Testy obciążeniowe określają, czy aplikację przy dużym obciążeniu można dokonać odzyskiwania po awarii i bez problemu zmieniała powrócić do oczekiwane zachowanie. Przy dużym obciążeniu aplikacja nie jest uruchamiana w normalnych warunkach.

Visual Studio 2019 r jest najnowszej wersji programu Visual Studio za pomocą funkcji testów obciążeniowych. Dla klientów wymagających w przyszłości narzędzia do testowania obciążenia zalecamy alternatywne narzędzi, takich jak Apache JMeter Akamai CloudTest i BlazeMeter. Aby uzyskać więcej informacji, zobacz [Visual Studio 2019 informacje o wersji](/visualstudio/releases/2019/release-notes#test-tools).

Usługi testowania obciążeniowego w DevOps platformy Azure kończy się 2020 r. Aby uzyskać więcej informacji, zobacz [usługi koniec cyklu życia testowania obciążenia w chmurze](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio tools

Program Visual Studio pozwala użytkownikom na tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci web. Opcja jest dostępna do utworzenia testów poprzez nagrywanie akcji w przeglądarce sieci web.

Aby uzyskać informacje na temat sposobu tworzenie, konfigurowanie i uruchamianie testu obciążenia projektów przy użyciu programu Visual Studio 2017, zobacz [Szybki Start: Tworzenie projektu testu obciążeniowego](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017). Aby uzyskać więcej informacji, zobacz [dodatkowe zasoby](#additional-resources) sekcji.

Testy obciążenia można skonfigurować do uruchamiania w środowisku lokalnym lub uruchamiania w chmurze przy użyciu DevOps platformy Azure.

## <a name="azure-devops"></a>Azure DevOps

Można uruchomić przebiegów testów obciążeniowych przy użyciu [plany testów Azure DevOps](/azure/devops/test/load-test/index?view=vsts) usługi.

![Testowanie strony docelowej obciążenia DevOps platformy Azure](./load-tests/_static/azure-devops-load-test.png)

Usługa obsługuje następujące formaty testu:

* Program Visual Studio &ndash; testu sieci Web w programie Visual Studio.
* Archiwum HTTP &ndash; ruch HTTP przechwycone w ramach archiwum jest odtwarzany podczas testowania.
* [Oparty na adresach URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; umożliwia określenie adresów URL można załadować testu, typy żądań, nagłówki i ciągi zapytań. Uruchamianie, ustawianie parametrów, np. czas trwania, wzorca obciążenia oraz liczbę użytkowników, można skonfigurować.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure Portal

[Portal systemu Azure pozwala konfigurowania i uruchamiania, testowanie obciążeniowe aplikacji sieci web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z **wydajności** kartę usługi App Service w witrynie Azure portal.

![Usługa Azure App Service w witrynie Azure portal](./load-tests/_static/azure-appservice-perf-test.png)

Test może być testu ręcznego z określonym adresem URL lub plik sieci Web Test programu Visual Studio, który można przetestować wiele adresów URL.

![Nowa strona Test Wydajnościowy w witrynie Azure portal](./load-tests/_static/azure-appservice-perf-test-config.png)

Na końcu testu generowane raporty pokazują charakterystyki wydajności aplikacji. Statystyka przykład zawiera:

* Średni czas odpowiedzi
* Maksymalna przepływność: żądań na sekundę
* Procent niepowodzeń

## <a name="third-party-tools"></a>Narzędzia innych producentów

Poniższa lista zawiera narzędzia wydajności sieci web innych firm, z różnymi zestawami funkcji:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Chleba](https://locust.io/)
* [WebSurge wiatru zachodnie](http://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Serię wpisów w blogu testu obciążenia](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
