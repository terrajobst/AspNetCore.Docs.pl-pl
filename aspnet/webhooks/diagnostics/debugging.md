---
uid: webhooks/diagnostics/debugging
title: Elementy Webhook ASP.NET debugowanie | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak debugować elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753972"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="3d982-103">Elementy Webhook ASP.NET debugowania</span><span class="sxs-lookup"><span data-stu-id="3d982-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="3d982-104">Debugowanie w systemie Azure</span><span class="sxs-lookup"><span data-stu-id="3d982-104">Debugging in Azure</span></span>

<span data-ttu-id="3d982-105">Aby debugować aplikację sieci Web podczas uruchamiania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="3d982-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="3d982-106">Debugowanie przy użyciu źródła i symboli</span><span class="sxs-lookup"><span data-stu-id="3d982-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="3d982-107">Oprócz debugowania własnego kodu, jest możliwe debugowanie bezpośrednio do programu Microsoft ASP.NET WebHooks, a w rzeczywistości wszystkie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3d982-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="3d982-108">To działa, niezależnie od tego, czy debugujesz lokalnie lub zdalnie.</span><span class="sxs-lookup"><span data-stu-id="3d982-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="3d982-109">Najpierw należy skonfigurować program Visual Studio można znaleźć, przechodząc do źródła i symboli **debugowania** i następnie **opcje i ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="3d982-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="3d982-110">Ustaw opcje następująco:</span><span class="sxs-lookup"><span data-stu-id="3d982-110">Set the options like this:</span></span>

![Opcje i ustawienia](_static/SourceSymbols.png)

<span data-ttu-id="3d982-112">Następnie dodaj link do [symbolsource.org](http://symbolsource.org) pobierania źródła i symboli.</span><span class="sxs-lookup"><span data-stu-id="3d982-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="3d982-113">Przejdź do **symbole** kartę menu powyżej i Dodaj następujący kod jako lokalizację symboli:</span><span class="sxs-lookup"><span data-stu-id="3d982-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="3d982-114">Ponadto upewnij się, że katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików można uzyskać za długa co spowoduje symboli, które nie były ładowane.</span><span class="sxs-lookup"><span data-stu-id="3d982-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="3d982-115">Ścieżka próbki jest:</span><span class="sxs-lookup"><span data-stu-id="3d982-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="3d982-116">Ustawienia powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3d982-116">The settings should look similar to this:</span></span>

![Przykład lokalizacji pliku symboli opcje](_static/SymSource.png)
