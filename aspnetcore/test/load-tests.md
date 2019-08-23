---
title: ASP.NET Core testowanie obciążenia/obciążeniowego
author: Jeremy-Meng
description: Poznaj kilka istotnych narzędzi i metod testowania obciążeniowego i testowania obciążeniowego ASP.NET Core aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975248"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core testowanie obciążenia/obciążeniowego

Testowanie obciążeniowe i testowanie obciążeniowe są ważne, aby zapewnić, że aplikacja sieci Web jest wydajna i skalowalna. Ich cele są różne, chociaż często korzystają z podobnych testów.

**Testy obciążenia** &ndash; Sprawdź, czy aplikacja może obsłużyć określone obciążenie użytkownikami w pewnym scenariuszu, zachowując jednocześnie cel odpowiedzi. Aplikacja jest uruchamiana w normalnych warunkach.

**Testy obciążeniowe** &ndash; Przetestuj stabilność aplikacji w przypadku długotrwałych warunków, często przez długi czas. Testy zapewniają duże obciążenie użytkownikami, przerastają lub stopniowo zwiększają obciążenie, w aplikacji lub ograniczają zasoby obliczeniowe aplikacji.

Testy obciążeniowe określają, czy aplikacja poddawana obciążeniom może odzyskać sprawność po awarii i bezpiecznie wrócić do oczekiwanego zachowania. W obszarze naprężenie aplikacja nie jest uruchamiana w normalnych warunkach.

Program Visual Studio 2019 to Ostatnia wersja programu Visual Studio z funkcjami testów obciążenia. W przypadku klientów wymagających narzędzi testowania obciążenia w przyszłości zalecamy używanie alternatywnych narzędzi, takich jak Apache JMeter, Akamai CloudTest i BlazeMeter. Aby uzyskać więcej informacji, zobacz [Informacje o wersji programu Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

Usługa testowania obciążenia w usłudze Azure DevOps kończy się w 2020. Aby uzyskać więcej informacji, zobacz temat [koniec okresu istnienia usługi testowania obciążenia opartego na chmurze](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Narzędzia programu Visual Studio

Program Visual Studio umożliwia użytkownikom tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci Web. Dostępna jest opcja tworzenia testów przez rejestrowanie akcji w przeglądarce internetowej.

Aby uzyskać informacje na temat sposobu tworzenia, konfigurowania i uruchamiania projektów testów obciążenia przy użyciu programu Visual Studio 2017, [zobacz Szybki Start: Utwórz projekt](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)testu obciążenia.

Testy obciążenia można skonfigurować do uruchamiania lokalnego lub uruchamiania w chmurze przy użyciu usługi Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Uruchomienia testów obciążenia można rozpocząć przy użyciu usługi [Azure DevOps test Plans](/azure/devops/test/load-test/index?view=vsts) .

![Strona docelowa testowania obciążenia usługi Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

Usługa obsługuje następujące formaty testów:

* Test sieci &ndash; Web programu Visual Studio utworzony w programie Visual Studio.
* &ndash; Przechwycony ruch HTTP w archiwum http w archiwum jest odtwarzany podczas testowania.
* [Na podstawie adresu URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Pozwala określić adresy URL do testu obciążenia, typy żądań, nagłówki i ciągi zapytań. Można skonfigurować parametry ustawień, takie jak czas trwania, wzorzec obciążenia i liczba użytkowników.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure Portal

[Azure Portal umożliwia konfigurowanie i uruchamianie testów obciążenia aplikacji sieci Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z karty **wydajność** App Service w Azure Portal.

![Azure App Service w Azure Portal](./load-tests/_static/azure-appservice-perf-test.png)

Test może być testem ręcznym z określonym adresem URL lub plikiem testu sieci Web programu Visual Studio, który umożliwia przetestowanie wielu adresów URL.

![Nowa strona testu wydajności na Azure Portal](./load-tests/_static/azure-appservice-perf-test-config.png)

Na końcu testu wygenerowane raporty pokazują charakterystykę wydajności aplikacji. Przykładowe Statystyki obejmują:

* Średni czas odpowiedzi
* Maksymalna przepływność: żądania na sekundę
* Procent niepowodzeń

## <a name="third-party-tools"></a>Narzędzia innych firm

Poniższa lista zawiera narzędzia do oceny wydajności sieci Web innych firm z różnymi zestawami funkcji:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Chleb](https://locust.io/)
* [Moje przepięcio zachodnie wiatru](https://websurge.west-wind.com/)
* [W ramach](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
