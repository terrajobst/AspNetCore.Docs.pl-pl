---
uid: webhooks/diagnostics/debugging
title: Elementów Webhook ASP.NET debugowanie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak można debugować elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044867"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET elementów Webhook debugowania  

## <a name="debugging-in-azure"></a>Debugowanie na platformie Azure

Aby debugować aplikację sieci Web podczas uruchamiania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debugowanie ze źródłem i symboli

Oprócz debugowanie swoim własnym kodem, istnieje możliwość debugowania bezpośrednio do firmy Microsoft ASP.NET WebHooks, a w rzeczywistości wszystkie platformy .NET. Ta metoda działa niezależnie od tego, czy debugowanie lokalnie lub zdalnie. Najpierw należy skonfigurować Visual Studio można znaleźć źródła i symbole, przechodząc do **debugowania** , a następnie **opcje i ustawienia**. Ustaw opcje następująco:

![Opcje i ustawienia](_static/SourceSymbols.png)

Następnie dodaj łącze do [symbolsource.org](http://symbolsource.org) pobierania źródłowe i symboli. Przejdź do **symbole** kartę menu powyżej i dodaj następującą lokalizację symbolu:

```
http://srv.symbolsource.org/pdb/Public
```

Ponadto upewnij się, że katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików można uzyskać za długa co spowoduje wyświetlanie symboli nie załadować. To przykładowe ścieżki:

```
C:\SymCache
```

Ustawienia powinny wyglądać podobnie do poniższego:

![Przykład lokalizację pliku symboli opcje](_static/SymSource.png)
