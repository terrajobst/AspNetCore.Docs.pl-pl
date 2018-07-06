---
uid: webhooks/diagnostics/debugging
title: Elementy Webhook ASP.NET debugowanie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak debugować elementów Webhook programu ASP.NET.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801991"
---
# <a name="aspnet-webhooks-debugging"></a>Elementy Webhook ASP.NET debugowania  

## <a name="debugging-in-azure"></a>Debugowanie w systemie Azure

Aby debugować aplikację sieci Web podczas uruchamiania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debugowanie przy użyciu źródła i symboli

Oprócz debugowania własnego kodu, jest możliwe debugowanie bezpośrednio do programu Microsoft ASP.NET WebHooks, a w rzeczywistości wszystkie platformy .NET. To działa, niezależnie od tego, czy debugujesz lokalnie lub zdalnie. Najpierw należy skonfigurować program Visual Studio można znaleźć, przechodząc do źródła i symboli **debugowania** i następnie **opcje i ustawienia**. Ustaw opcje następująco:

![Opcje i ustawienia](_static/SourceSymbols.png)

Następnie dodaj link do [symbolsource.org](http://symbolsource.org) pobierania źródła i symboli. Przejdź do **symbole** kartę menu powyżej i Dodaj następujący kod jako lokalizację symboli:

```
http://srv.symbolsource.org/pdb/Public
```

Ponadto upewnij się, że katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików można uzyskać za długa co spowoduje symboli, które nie były ładowane. Ścieżka próbki jest:

```
C:\SymCache
```

Ustawienia powinny wyglądać następująco:

![Przykład lokalizacji pliku symboli opcje](_static/SymSource.png)
