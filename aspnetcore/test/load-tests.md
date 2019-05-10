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
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="b70a6-103">Testowanie obciążenia/obciążeniowe platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b70a6-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="b70a6-104">Testowanie obciążenia i testy obciążenia są ważne, aby upewnić się, że aplikacja sieci web jest wydajne i skalowalne.</span><span class="sxs-lookup"><span data-stu-id="b70a6-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="b70a6-105">Swoje cele są różne, nawet jeśli często mają podobne testów.</span><span class="sxs-lookup"><span data-stu-id="b70a6-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="b70a6-106">**Testy obciążenia** &ndash; sprawdzić, czy aplikacja może obsługiwać określonego obciążenia użytkowników dla niektórych scenariuszy nadal spełniając docelowy odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b70a6-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="b70a6-107">Aplikacja jest uruchamiana w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="b70a6-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="b70a6-108">**Testy obciążeniowe** &ndash; testowych stabilność aplikacji, podczas działania w warunkach extreme często przez dłuższy czas.</span><span class="sxs-lookup"><span data-stu-id="b70a6-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="b70a6-109">Testy umieść duże obciążenie użytkownikami, wzrostów lub stopniowo zwiększa obciążenie dla aplikacji lub ograniczać zasoby obliczeniowe aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b70a6-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="b70a6-110">Testy obciążeniowe określają, czy aplikację przy dużym obciążeniu można dokonać odzyskiwania po awarii i bez problemu zmieniała powrócić do oczekiwane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b70a6-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="b70a6-111">Przy dużym obciążeniu aplikacja nie jest uruchamiana w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="b70a6-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="b70a6-112">Visual Studio 2019 r jest najnowszej wersji programu Visual Studio za pomocą funkcji testów obciążeniowych.</span><span class="sxs-lookup"><span data-stu-id="b70a6-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="b70a6-113">Dla klientów wymagających w przyszłości narzędzia do testowania obciążenia zalecamy alternatywne narzędzi, takich jak Apache JMeter Akamai CloudTest i BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="b70a6-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="b70a6-114">Aby uzyskać więcej informacji, zobacz [Visual Studio 2019 informacje o wersji](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="b70a6-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="b70a6-115">Usługi testowania obciążeniowego w DevOps platformy Azure kończy się 2020 r.</span><span class="sxs-lookup"><span data-stu-id="b70a6-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="b70a6-116">Aby uzyskać więcej informacji, zobacz [usługi koniec cyklu życia testowania obciążenia w chmurze](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="b70a6-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="b70a6-117">Visual Studio tools</span><span class="sxs-lookup"><span data-stu-id="b70a6-117">Visual Studio tools</span></span>

<span data-ttu-id="b70a6-118">Program Visual Studio pozwala użytkownikom na tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci web.</span><span class="sxs-lookup"><span data-stu-id="b70a6-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="b70a6-119">Opcja jest dostępna do utworzenia testów poprzez nagrywanie akcji w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="b70a6-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="b70a6-120">Aby uzyskać informacje na temat sposobu tworzenie, konfigurowanie i uruchamianie testu obciążenia projektów przy użyciu programu Visual Studio 2017, zobacz [Szybki Start: Tworzenie projektu testu obciążeniowego](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="b70a6-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="b70a6-121">Aby uzyskać więcej informacji, zobacz [dodatkowe zasoby](#additional-resources) sekcji.</span><span class="sxs-lookup"><span data-stu-id="b70a6-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="b70a6-122">Testy obciążenia można skonfigurować do uruchamiania w środowisku lokalnym lub uruchamiania w chmurze przy użyciu DevOps platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="b70a6-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="b70a6-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="b70a6-123">Azure DevOps</span></span>

<span data-ttu-id="b70a6-124">Można uruchomić przebiegów testów obciążeniowych przy użyciu [plany testów Azure DevOps](/azure/devops/test/load-test/index?view=vsts) usługi.</span><span class="sxs-lookup"><span data-stu-id="b70a6-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Testowanie strony docelowej obciążenia DevOps platformy Azure](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="b70a6-126">Usługa obsługuje następujące formaty testu:</span><span class="sxs-lookup"><span data-stu-id="b70a6-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="b70a6-127">Program Visual Studio &ndash; testu sieci Web w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b70a6-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="b70a6-128">Archiwum HTTP &ndash; ruch HTTP przechwycone w ramach archiwum jest odtwarzany podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="b70a6-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="b70a6-129">[Oparty na adresach URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; umożliwia określenie adresów URL można załadować testu, typy żądań, nagłówki i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="b70a6-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="b70a6-130">Uruchamianie, ustawianie parametrów, np. czas trwania, wzorca obciążenia oraz liczbę użytkowników, można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="b70a6-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="b70a6-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="b70a6-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="b70a6-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b70a6-132">Azure portal</span></span>

<span data-ttu-id="b70a6-133">[Portal systemu Azure pozwala konfigurowania i uruchamiania, testowanie obciążeniowe aplikacji sieci web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z **wydajności** kartę usługi App Service w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b70a6-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Usługa Azure App Service w witrynie Azure portal](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="b70a6-135">Test może być testu ręcznego z określonym adresem URL lub plik sieci Web Test programu Visual Studio, który można przetestować wiele adresów URL.</span><span class="sxs-lookup"><span data-stu-id="b70a6-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nowa strona Test Wydajnościowy w witrynie Azure portal](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="b70a6-137">Na końcu testu generowane raporty pokazują charakterystyki wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b70a6-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="b70a6-138">Statystyka przykład zawiera:</span><span class="sxs-lookup"><span data-stu-id="b70a6-138">Example statistics include:</span></span>

* <span data-ttu-id="b70a6-139">Średni czas odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="b70a6-139">Average response time</span></span>
* <span data-ttu-id="b70a6-140">Maksymalna przepływność: żądań na sekundę</span><span class="sxs-lookup"><span data-stu-id="b70a6-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="b70a6-141">Procent niepowodzeń</span><span class="sxs-lookup"><span data-stu-id="b70a6-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="b70a6-142">Narzędzia innych producentów</span><span class="sxs-lookup"><span data-stu-id="b70a6-142">Third-party tools</span></span>

<span data-ttu-id="b70a6-143">Poniższa lista zawiera narzędzia wydajności sieci web innych firm, z różnymi zestawami funkcji:</span><span class="sxs-lookup"><span data-stu-id="b70a6-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="b70a6-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="b70a6-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="b70a6-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="b70a6-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="b70a6-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="b70a6-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="b70a6-147">Chleba</span><span class="sxs-lookup"><span data-stu-id="b70a6-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="b70a6-148">WebSurge wiatru zachodnie</span><span class="sxs-lookup"><span data-stu-id="b70a6-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="b70a6-149">Netling</span><span class="sxs-lookup"><span data-stu-id="b70a6-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="b70a6-150">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b70a6-150">Additional resources</span></span>

* [<span data-ttu-id="b70a6-151">Serię wpisów w blogu testu obciążenia</span><span class="sxs-lookup"><span data-stu-id="b70a6-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
