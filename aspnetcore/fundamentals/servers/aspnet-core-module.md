---
title: Moduł ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak modułu ASP.NET Core zezwala na serwerze sieci web Kestrel do używania usług IIS lub IIS Express jako serwera zwrotny serwer proxy.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910892"
---
# <a name="aspnet-core-module"></a>Moduł ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), i [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

Modułu ASP.NET Core umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w proces roboczy usług IIS (*w trakcie*) lub za usług IIS w konfiguracji zwrotny serwer proxy (*spoza procesu*). Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Modułu ASP.NET Core umożliwia platformy ASP.NET Core uruchamiania za usług IIS w konfiguracji zwrotny serwer proxy aplikacji. Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.

::: moniker-end

Obsługiwane wersje Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

::: moniker range=">= aspnetcore-2.2"

W przypadku hostowania w procesie, moduł ma własną implementację serwera `IISHttpServer`.

Gdy w hostingu poza procesem, moduł działa tylko z Kestrel. Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Moduł działa tylko z Kestrel. Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>Opis podstawowych modułu platformy ASP.NET

::: moniker range=">= aspnetcore-2.2"

Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:

* Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).

* Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).

### <a name="in-process-hosting-model"></a>W trakcie modelu hostingu

Za pomocą hostingu platformy ASP.NET Core w trakcie aplikacja jest uruchamiana w tym samym procesie co jej proces roboczy usług IIS. Spowoduje to usunięcie spadek wydajności buforowania żądań za pośrednictwem karty sprzężenia zwrotnego korzystając z modelu hostingu poza procesem. Usługi IIS obsługuje zarządzanie procesami przy użyciu [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Moduł ASP.NET Core:

* Wykonuje inicjowania aplikacji.
  * Ładunki [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Wywołania `Program.Main`.
* Obsługuje okres istnienia żądanie natywnych usług IIS.

Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core i aplikacji obsługiwanych w procesie:

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

Żądanie dociera z sieci web do sterownik HTTP.sys trybu jądra. Sterownik kieruje żądanie macierzystego w usługach IIS na porcie skonfigurowanym witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł odbiera żądanie natywnych i przekazuje sterowanie do `IISHttpServer`, czyli co konwertuje żądanie z natywnego na zarządzane.

Po `IISHttpServer` przejmuje żądania, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core. Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki. Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.

### <a name="out-of-process-hosting-model"></a>Model hostingu poza procesem

Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem. Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii. Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys. Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.

Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`. Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane. Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.

Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core. Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki. Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel. Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.

Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem. Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii. Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys. Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.

Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`. Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane. Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.

Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core. Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki. Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel. Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.

::: moniker-end

Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne. Aby dowiedzieć się, jak aktywne moduły usług IIS za pomocą modułu, zobacz <xref:host-and-deploy/iis/modules>.

Modułu ASP.NET Core ma kilka innych funkcji. Moduł wykonywać następujące czynności:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.
* Przekazuj tokeny uwierzytelniania Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować i używać modułu ASP.NET Core

Aby uzyskać szczegółowe instrukcje dotyczące sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>. Aby uzyskać informacje na temat konfigurowania modułu, zobacz <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [Repozytorium GitHub modułów Core ASP.NET (kodu źródłowego)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
