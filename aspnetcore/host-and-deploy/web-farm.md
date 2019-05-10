---
title: Host platformy ASP.NET Core w ramach farmy sieci web
author: guardrex
description: Dowiedz się, jak hostować wiele wystąpień aplikacji ASP.NET Core z udostępnionymi zasobami w środowisku farmy sieci web.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/web-farm
ms.openlocfilehash: 4873665e6174a6acf885e1ebb41fb005d646bd1f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899245"
---
# <a name="host-aspnet-core-in-a-web-farm"></a>Host platformy ASP.NET Core w ramach farmy sieci web

Przez [Luke Latham](https://github.com/guardrex) i [Chris Ross](https://github.com/Tratcher)

A *kolektywu serwerów sieci web* jest grupą dwóch lub więcej serwerów sieci web (lub *węzłów*) które hostują wiele wystąpień aplikacji. W przypadku żądań od użytkowników dotrze do farmy sieci web *moduł równoważenia obciążenia* rozdziela żądania do węzłów kolektywu serwerów sieci web. Poprawa farmy serwerów sieci Web:

* **Niezawodność/dostępności** &ndash; przy co najmniej jeden węzeł ulegnie awarii, moduł równoważenia obciążenia może kierować żądania do innych węzłów działa, aby kontynuować przetwarzanie żądań.
* **Pojemności i wydajności** &ndash; wielu węzłach może przetwarzać żądania więcej niż jednym serwerze. Moduł równoważenia obciążenia równoważy obciążenie przez dystrybucję żądań do węzłów.
* **Skalowalność** &ndash; gdy wymagana jest więcej lub mniej pojemności, liczba aktywnych węzłów można zwiększyć lub zmniejszyć do dopasowania obciążenia. Sieci Web farmy technologie platformy, takie jak [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), automatycznie dodawać lub usuwać węzły na żądanie przez administratora systemu lub automatycznie bez interwencji człowieka.
* **Łatwość konserwacji** &ndash; węzły farmy sieci web może polegać na zbiór usług udostępnionych, co skutkuje łatwiejsze zarządzanie w systemie. Na przykład polegać węzły farmy sieci web od pojedynczego serwera bazy danych i wspólnej lokalizacji sieciowej dla zasobów statycznych, takich jak obrazy i pliki do pobrania.

W tym temacie opisano, konfiguracji i zależności dla aplikacji ASP.NET core hostowana na farmie sieci web, które korzystają z zasobów udostępnionych.

## <a name="general-configuration"></a>Konfiguracja ogólna

<xref:host-and-deploy/index>  
Dowiedz się, jak skonfigurować środowiskach hostingu i wdrażanie aplikacji platformy ASP.NET Core. Skonfiguruj menedżera procesów w każdym węźle kolektywu serwerów sieci web do zautomatyzowania aplikacja uruchamia się i ponownego uruchomienia. Każdy węzeł wymaga środowiska uruchomieniowego platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz Tematy w [hosta i wdrażanie](xref:host-and-deploy/index) obszaru dokumentacji.

<xref:host-and-deploy/proxy-load-balancer>  
Więcej informacji o konfiguracji dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia, które często zasłaniać żądanie ważnych informacji.

<xref:host-and-deploy/azure-apps/index>  
[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) jest [chmury obliczeniowej platformy usługi firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, łącznie z platformą ASP.NET Core. App Service to w pełni zarządzana platforma, która umożliwia automatyczne skalowanie, równoważenie obciążenia sieciowego, poprawek i ciągłego wdrażania.

## <a name="app-data"></a>Dane aplikacji

Gdy aplikacja jest skalowana do wielu wystąpień, może to być stan aplikacji, który wymaga udostępnianie między węzłami. Jeśli stan jest przejściowe, rozważ udostępnienie [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). Jeśli udostępniony stan wymaga trwałości, należy wziąć pod uwagę przechowywania udostępnionego stanu w bazie danych.

## <a name="required-configuration"></a>Wymaganej konfiguracji

Ochrona danych i pamięć podręczna wymagają konfiguracji dla aplikacji wdrożonych na farmie sieci web.

### <a name="data-protection"></a>Ochrona danych

[System ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction) jest używane przez aplikacje do ochrony danych. Ochrona danych opiera się na zestaw klucze szyfrowania są przechowywane w *pierścienia klucz*. Po zainicjowaniu system ochrony danych dotyczy [domyślne ustawienia](xref:security/data-protection/configuration/default-settings) , zapisać pierścienia klucz lokalnie. W obszarze Konfiguracja domyślna pierścień Unikatowy klucz znajduje się w każdym węźle kolektywu serwerów sieci web. W związku z tym każdy węzeł farmy sieci web nie może odszyfrować danych, które są szyfrowane przez aplikację w innym węźle. Domyślna konfiguracja nie jest zazwyczaj odpowiedni do hostowania aplikacji w ramach farmy sieci web. Alternatywa Implementowanie udostępnionego pierścień klucza jest zawsze Przekierowywanie żądań użytkowników do tego samego węzła. Aby uzyskać więcej informacji na temat konfiguracji systemu ochrony danych w przypadku wdrożeń kolektywu serwerów sieci web, zobacz <xref:security/data-protection/configuration/overview>.

### <a name="caching"></a>Buforowanie

W środowisku farmy sieci web mechanizm buforowania muszą współużytkować elementów pamięci podręcznej w węzłach kolektywu serwerów sieci web. Buforowanie musi albo korzystają z typowych pamięci podręcznej redis cache, udostępnionej bazy danych programu SQL Server lub niestandardowych implementacji buforowania, która udostępnia elementy pamięci podręcznej w całej farmie sieci web. Aby uzyskać więcej informacji, zobacz <xref:performance/caching/distributed>.

## <a name="dependent-components"></a>Składniki zależne

Następujące scenariusze nie wymaga dodatkowych czynności konfiguracyjnych, ale są one zależne od technologii, które wymagają konfiguracji dla farmy serwerów sieci web.

| Scenariusz | Zależy od &hellip; |
| -------- | ------------------- |
| Uwierzytelnianie | Ochrona danych (zobacz <xref:security/data-protection/configuration/overview>).<br><br>Aby uzyskać więcej informacji, zobacz <xref:security/authentication/cookie> i <xref:security/cookie-sharing>. |
| Tożsamość | Konfiguracja uwierzytelniania i bazy danych.<br><br>Aby uzyskać więcej informacji, zobacz <xref:security/authentication/identity>. |
| Sesja | Ochrona danych (zaszyfrowanych plików cookie) (zobacz <xref:security/data-protection/configuration/overview>) i pamięć podręczna (zobacz <xref:performance/caching/distributed>).<br><br>Aby uzyskać więcej informacji, zobacz [stan sesji i aplikacji: Stan sesji](xref:fundamentals/app-state#session-state). |
| TempData | Ochrona danych (zaszyfrowanych plików cookie) (zobacz <xref:security/data-protection/configuration/overview>) lub sesji (zobacz [stan sesji i aplikacji: Stan sesji](xref:fundamentals/app-state#session-state)).<br><br>Aby uzyskać więcej informacji, zobacz [stan sesji i aplikacji: TempData](xref:fundamentals/app-state#tempdata). |
| Zabezpieczający przed sfałszowaniem | Ochrona danych (zobacz <xref:security/data-protection/configuration/overview>).<br><br>Aby uzyskać więcej informacji, zobacz <xref:security/anti-request-forgery>. |

## <a name="troubleshoot"></a>Rozwiązywanie problemów

### <a name="data-protection-and-caching"></a>Ochrona danych i buforowanie

Podczas ochrony danych lub pamięci podręcznej nie jest skonfigurowany w środowisku kolektywu serwerów sieci web, sporadyczne błędy występują podczas przetwarzania żądania. Dzieje się tak, ponieważ węzły nie współużytkują te same zasoby, a żądania użytkowników nie są zawsze kierowane do tego samego węzła.

Należy wziąć pod uwagę użytkownika, który zaloguje się do aplikacji przy użyciu uwierzytelniania plików cookie. Zalogowaniu się użytkownika do aplikacji na węzeł farmy jedną witrynę sieci web. Jeśli ich kolejne żądanie zostanie odebrana na tym samym węźle, gdzie one podpisane, aplikacja jest w stanie odszyfrować pliku cookie uwierzytelniania i umożliwia dostęp do zasobów aplikacji. Jeśli ich kolejne żądanie dociera do innego węzła, aplikacja nie można odszyfrować pliku cookie uwierzytelniania z poziomu węzła, gdzie użytkownik jest zalogowany i autoryzacji dla żądanego zasobu nie powiodło się.

Gdy wystąpienia któregokolwiek z następujących objawów **sporadycznie**, problem jest zazwyczaj śledzone niewłaściwa ochrona danych lub konfiguracji buforowania w środowisku farmy sieci web:

* Podziały uwierzytelniania &ndash; pliku cookie uwierzytelniania jest nieprawidłowo skonfigurowana lub nie można odszyfrować. OAuth (Facebook, Microsoft, Twitter) lub OpenIdConnect logowania się nie powieść z powodu błędu "Korelacji nie powiodło się."
* Podziały autoryzacji &ndash; tożsamości zostaną utracone.
* Stan sesji powoduje utratę danych.
* Elementy pamięci podręcznej znikną.
* TempData kończy się niepowodzeniem.
* Publikuje niepowodzenie &ndash; sprawdzenie zabezpieczający przed sfałszowaniem nie powiedzie się.

Aby uzyskać więcej informacji na temat konfigurowania ochrony danych dla wdrożeń kolektywu serwerów sieci web, zobacz <xref:security/data-protection/configuration/overview>. Aby uzyskać więcej informacji na temat konfiguracji buforowania w przypadku wdrożeń kolektywu serwerów sieci web, zobacz <xref:performance/caching/distributed>.

## <a name="obtain-data-from-apps"></a>Uzyskiwanie danych z aplikacji

Aplikacje sieci web w farmie są w stanie odpowiadać na żądania, z aplikacji za pomocą oprogramowania pośredniczącego terminalu wbudowane uzyskać żądania, połączenia i dodatkowych danych. Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.
