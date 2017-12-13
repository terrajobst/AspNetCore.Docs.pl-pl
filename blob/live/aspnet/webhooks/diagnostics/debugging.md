---
uid: webhooks/diagnostics/debugging
title: "Elementów Webhook ASP.NET debugowanie | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Jak można debugować elementów Webhook ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="f0e77-103">ASP.NET elementów Webhook debugowania</span><span class="sxs-lookup"><span data-stu-id="f0e77-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="f0e77-104">Debugowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="f0e77-104">Debugging in Azure</span></span>

<span data-ttu-id="f0e77-105">Aby debugować aplikację sieci Web podczas uruchamiania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="f0e77-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="f0e77-106">Debugowanie ze źródłem i symboli</span><span class="sxs-lookup"><span data-stu-id="f0e77-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="f0e77-107">Oprócz debugowanie swoim własnym kodem, istnieje możliwość debugowania bezpośrednio do firmy Microsoft ASP.NET WebHooks, a w rzeczywistości wszystkie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f0e77-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="f0e77-108">Ta metoda działa niezależnie od tego, czy debugowanie lokalnie lub zdalnie.</span><span class="sxs-lookup"><span data-stu-id="f0e77-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="f0e77-109">Najpierw należy skonfigurować Visual Studio można znaleźć źródła i symbole, przechodząc do **debugowania** , a następnie **opcje i ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="f0e77-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="f0e77-110">Ustaw opcje następująco:</span><span class="sxs-lookup"><span data-stu-id="f0e77-110">Set the options like this:</span></span>

![Opcje i ustawienia](_static/SourceSymbols.png)

<span data-ttu-id="f0e77-112">Następnie dodaj łącze do [symbolsource.org](http://symbolsource.org) pobierania źródłowe i symboli.</span><span class="sxs-lookup"><span data-stu-id="f0e77-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="f0e77-113">Przejdź do **symbole** kartę menu powyżej i dodaj następującą lokalizację symbolu:</span><span class="sxs-lookup"><span data-stu-id="f0e77-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="f0e77-114">Ponadto upewnij się, że katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików można uzyskać za długa co spowoduje wyświetlanie symboli nie załadować.</span><span class="sxs-lookup"><span data-stu-id="f0e77-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="f0e77-115">To przykładowe ścieżki:</span><span class="sxs-lookup"><span data-stu-id="f0e77-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="f0e77-116">Ustawienia powinny wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="f0e77-116">The settings should look similar to this:</span></span>

![Przykład lokalizację pliku symboli opcje](_static/SymSource.png)
