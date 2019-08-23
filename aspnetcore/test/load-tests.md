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
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="0239c-103">ASP.NET Core testowanie obciążenia/obciążeniowego</span><span class="sxs-lookup"><span data-stu-id="0239c-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="0239c-104">Testowanie obciążeniowe i testowanie obciążeniowe są ważne, aby zapewnić, że aplikacja sieci Web jest wydajna i skalowalna.</span><span class="sxs-lookup"><span data-stu-id="0239c-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="0239c-105">Ich cele są różne, chociaż często korzystają z podobnych testów.</span><span class="sxs-lookup"><span data-stu-id="0239c-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="0239c-106">**Testy obciążenia** &ndash; Sprawdź, czy aplikacja może obsłużyć określone obciążenie użytkownikami w pewnym scenariuszu, zachowując jednocześnie cel odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0239c-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="0239c-107">Aplikacja jest uruchamiana w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="0239c-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="0239c-108">**Testy obciążeniowe** &ndash; Przetestuj stabilność aplikacji w przypadku długotrwałych warunków, często przez długi czas.</span><span class="sxs-lookup"><span data-stu-id="0239c-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="0239c-109">Testy zapewniają duże obciążenie użytkownikami, przerastają lub stopniowo zwiększają obciążenie, w aplikacji lub ograniczają zasoby obliczeniowe aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0239c-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="0239c-110">Testy obciążeniowe określają, czy aplikacja poddawana obciążeniom może odzyskać sprawność po awarii i bezpiecznie wrócić do oczekiwanego zachowania.</span><span class="sxs-lookup"><span data-stu-id="0239c-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="0239c-111">W obszarze naprężenie aplikacja nie jest uruchamiana w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="0239c-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="0239c-112">Program Visual Studio 2019 to Ostatnia wersja programu Visual Studio z funkcjami testów obciążenia.</span><span class="sxs-lookup"><span data-stu-id="0239c-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="0239c-113">W przypadku klientów wymagających narzędzi testowania obciążenia w przyszłości zalecamy używanie alternatywnych narzędzi, takich jak Apache JMeter, Akamai CloudTest i BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="0239c-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="0239c-114">Aby uzyskać więcej informacji, zobacz [Informacje o wersji programu Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="0239c-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="0239c-115">Usługa testowania obciążenia w usłudze Azure DevOps kończy się w 2020.</span><span class="sxs-lookup"><span data-stu-id="0239c-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="0239c-116">Aby uzyskać więcej informacji, zobacz temat [koniec okresu istnienia usługi testowania obciążenia opartego na chmurze](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="0239c-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="0239c-117">Narzędzia programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0239c-117">Visual Studio tools</span></span>

<span data-ttu-id="0239c-118">Program Visual Studio umożliwia użytkownikom tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0239c-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="0239c-119">Dostępna jest opcja tworzenia testów przez rejestrowanie akcji w przeglądarce internetowej.</span><span class="sxs-lookup"><span data-stu-id="0239c-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="0239c-120">Aby uzyskać informacje na temat sposobu tworzenia, konfigurowania i uruchamiania projektów testów obciążenia przy użyciu programu Visual Studio 2017, [zobacz Szybki Start: Utwórz projekt](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)testu obciążenia.</span><span class="sxs-lookup"><span data-stu-id="0239c-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="0239c-121">Testy obciążenia można skonfigurować do uruchamiania lokalnego lub uruchamiania w chmurze przy użyciu usługi Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="0239c-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="0239c-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="0239c-122">Azure DevOps</span></span>

<span data-ttu-id="0239c-123">Uruchomienia testów obciążenia można rozpocząć przy użyciu usługi [Azure DevOps test Plans](/azure/devops/test/load-test/index?view=vsts) .</span><span class="sxs-lookup"><span data-stu-id="0239c-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Strona docelowa testowania obciążenia usługi Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="0239c-125">Usługa obsługuje następujące formaty testów:</span><span class="sxs-lookup"><span data-stu-id="0239c-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="0239c-126">Test sieci &ndash; Web programu Visual Studio utworzony w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0239c-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="0239c-127">&ndash; Przechwycony ruch HTTP w archiwum http w archiwum jest odtwarzany podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="0239c-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="0239c-128">[Na podstawie adresu URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Pozwala określić adresy URL do testu obciążenia, typy żądań, nagłówki i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="0239c-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="0239c-129">Można skonfigurować parametry ustawień, takie jak czas trwania, wzorzec obciążenia i liczba użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0239c-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="0239c-130">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0239c-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="0239c-131">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0239c-131">Azure portal</span></span>

<span data-ttu-id="0239c-132">[Azure Portal umożliwia konfigurowanie i uruchamianie testów obciążenia aplikacji sieci Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z karty **wydajność** App Service w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0239c-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure App Service w Azure Portal](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="0239c-134">Test może być testem ręcznym z określonym adresem URL lub plikiem testu sieci Web programu Visual Studio, który umożliwia przetestowanie wielu adresów URL.</span><span class="sxs-lookup"><span data-stu-id="0239c-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nowa strona testu wydajności na Azure Portal](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="0239c-136">Na końcu testu wygenerowane raporty pokazują charakterystykę wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0239c-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="0239c-137">Przykładowe Statystyki obejmują:</span><span class="sxs-lookup"><span data-stu-id="0239c-137">Example statistics include:</span></span>

* <span data-ttu-id="0239c-138">Średni czas odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="0239c-138">Average response time</span></span>
* <span data-ttu-id="0239c-139">Maksymalna przepływność: żądania na sekundę</span><span class="sxs-lookup"><span data-stu-id="0239c-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="0239c-140">Procent niepowodzeń</span><span class="sxs-lookup"><span data-stu-id="0239c-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="0239c-141">Narzędzia innych firm</span><span class="sxs-lookup"><span data-stu-id="0239c-141">Third-party tools</span></span>

<span data-ttu-id="0239c-142">Poniższa lista zawiera narzędzia do oceny wydajności sieci Web innych firm z różnymi zestawami funkcji:</span><span class="sxs-lookup"><span data-stu-id="0239c-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="0239c-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="0239c-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="0239c-144">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="0239c-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="0239c-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="0239c-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="0239c-146">Chleb</span><span class="sxs-lookup"><span data-stu-id="0239c-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="0239c-147">Moje przepięcio zachodnie wiatru</span><span class="sxs-lookup"><span data-stu-id="0239c-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="0239c-148">W ramach</span><span class="sxs-lookup"><span data-stu-id="0239c-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="0239c-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="0239c-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
