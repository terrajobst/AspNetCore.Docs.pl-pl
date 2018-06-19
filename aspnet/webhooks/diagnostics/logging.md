---
uid: webhooks/diagnostics/logging
title: ASP.NET elementów Webhook rejestrowania | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak przeprowadzić logowanie elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044828"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET elementów Webhook rejestrowania

Microsoft ASP.NET WebHooks używa rejestrowania sposób zgłaszania problemów i problemy. Domyślnie dzienniki są zapisywane przy użyciu [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) gdzie mogą być zarządzanych za pomocą [obiektów nasłuchujących śledzenia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) innego strumienia dziennika, takich jak.

W przypadku wdrażania aplikacji sieci Web jako aplikacji sieci Web platformy Azure, dzienniki są automatycznie pobierane i mogą być zarządzane wraz z wszystkimi innymi [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) rejestrowania. Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze aplikacji Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Dodatkowo dzienniki można uzyskać bezpośrednio w programie Visual Studio, zgodnie z opisem w [Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Przekierowywanie dzienników

Zamiast zapisywania dzienników [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), istnieje możliwość zapewniać implementację alternatywny rejestrowania, który można rejestrować bezpośrednio do Menedżera dziennika, takich jak [Log4Net](http://logging.apache.org/log4net/) i [NLog ](http://nlog-project.org/). Wystarczy podać implementacja [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) i zarejestrowanie go za pomocą aparatu iniekcji zależności, wybranym i jej będzie pobrać pobrana przez Microsoft ASP.NET WebHooks. Zobacz [iniekcji zależności w ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) szczegółowe informacje.
