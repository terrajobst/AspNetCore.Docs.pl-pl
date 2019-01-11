---
title: Testowanie obciążenia/obciążeniowe platformy ASP.NET Core
author: Jeremy-Meng
description: W tym artykule opisano kilka istotnych narzędzi i podejścia do testowania obciążenia i aplikacje platformy ASP.NET Core testowanie obciążeniowe.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: d989bc841a372bed7ebf2c84c6abe1a57762ad04
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207359"
---
# <a name="load-and-stress-testing-aspnet-core"></a>Obciążeniowe testowania platformy ASP.NET Core

Testowanie obciążenia i testy obciążenia są ważne, aby upewnić się, że aplikacja sieci web jest wydajne i skalowalne. Swoje cele są inne nawet często mają podobne testów.

**Testy obciążenia**: Sprawdza, czy aplikacja może obsługiwać określonego obciążenia użytkowników dla niektórych scenariuszy spełniając nadal docelowy odpowiedzi. Aplikacja jest uruchamiana w normalnych warunkach.

**Testy obciążeniowe**: Testy stabilność aplikacji podczas uruchamiania w ekstremalnych warunków i często dłuższy czas:

* Duże obciążenie użytkownikami — wzrostów lub stopniowo zwiększając.
* Ograniczone zasoby obliczeniowe.  

Przy dużym obciążeniu można ją odzyskiwanie po awarii i bez problemu zmieniała powrócić do oczekiwane zachowanie? Przy dużym obciążeniu, aplikacja jest *nie* Uruchom w normalnych warunkach.

## <a name="visual-studio-tools"></a>Narzędzia programu Visual Studio

Program Visual Studio pozwala użytkownikom na tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci web. Opcja jest dostępna do utworzenia testów poprzez nagrywanie akcji w przeglądarce sieci web.

[Szybki start: Tworzenie projektu testu obciążeniowego](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) pokazuje, jak tworzenie, konfigurowanie i uruchamianie testu obciążenia projektów przy użyciu programu Visual Studio 2017.

Zobacz [dodatkowe zasoby](#add) Aby uzyskać więcej informacji.

Testy obciążenia można skonfigurować do uruchamiania w lokalnych lub działają w chmurze przy użyciu DevOps platformy Azure.

## <a name="azure-devops"></a>Usługa Azure DevOps

Można uruchomić przebiegów testów obciążeniowych przy użyciu [plany testów Azure DevOps](/azure/devops/test/load-test/index?view=vsts) usługi.

![](./load-tests/_static/azure-devops-load-test.png)

Usługa obsługuje następujące typy format testu:

- Test programu Visual Studio — testu sieci web w programie Visual Studio.
- Archiwum HTTP testami — przechwyconych ruch HTTP w ramach archiwum jest odtwarzany podczas testowania.
- [Test oparty na adresach URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) — umożliwia określenie adresów URL można załadować testu, typy żądań, nagłówki i ciągi zapytań. Uruchamianie, ustawianie parametrów, np. czas trwania, wzorca obciążenia, liczbę użytkowników itp., można skonfigurować.
- [Apache JMeter](https://jmeter.apache.org/) testu.

## <a name="azure-portal"></a>Azure Portal

[Portal systemu Azure pozwala konfigurowania i uruchamiania, testowanie obciążeniowe aplikacji sieci Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z karty wydajność usługi App Service w witrynie Azure portal.

![](./load-tests/_static/azure-appservice-perf-test.png)

Test może być testu ręcznego z określonego adresu URL lub plik sieci Web Test programu Visual Studio, który można przetestować wiele adresów URL.

![](./load-tests/_static/azure-appservice-perf-test-config.png)

Na końcu testu raporty są generowane, aby pokazać charakterystyki wydajności aplikacji. Statystyka przykład zawiera:

- Średni czas odpowiedzi
- Maksymalna przepływność: żądań na sekundę
- Procent niepowodzeń

## <a name="third-party-tools"></a>Narzędzia innych producentów

Poniższa lista zawiera narzędzia wydajności sieci web innych firm, z różnymi zestawami funkcji:

- [Apache JMeter](https://jmeter.apache.org/) : Pełny zestaw polecanych narzędziom do testowania obciążenia. Powiązane z wątku: muszą jeden wątek na użytkownika.
- [AB — Apache HTTP server, narzędzia do testów porównawczych](https://httpd.apache.org/docs/2.4/programs/ab.html)
- [Gatling](https://gatling.io/) : Narzędzia pulpitu za pomocą urządzenia graficznego interfejsu użytkownika i testowania. Wydajniej niż JMeter.
- [Locust.IO](https://locust.io/) : Nie są ograniczone przez wątków.

<a name="add"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Ładowanie serię wpisów w blogu testu](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Autor: Charles Sterling. Z dnia, ale nadal dotyczą większości tematów.
