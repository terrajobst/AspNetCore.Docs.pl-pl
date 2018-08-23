---
uid: webhooks/source
title: Kod źródłowy elementów Webhook programu ASP.NET i pakiety NuGet | Dokumentacja firmy Microsoft
author: rick-anderson
description: Łącza do elementów Webhook programu ASP.NET w kodzie źródłowym oraz pakietów NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752692"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="4f2af-103">Kod źródłowy elementów Webhook programu ASP.NET i pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="4f2af-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="4f2af-104">Microsoft ASP.NET WebHooks jest częścią rodziny Microsoft ASP.NET, modułów i jest hostowany jako [Open Source Project w serwisie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="4f2af-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="4f2af-105">To oznacza, że firma Microsoft akceptuje wkładów, ale można znaleźć w [wytyczne dotyczące zamieszczania treści](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="4f2af-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="4f2af-106">Tej dokumentacji online, w którym czytasz teraz również znajduje się jako [typu Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) oraz akceptuje wkładów.</span><span class="sxs-lookup"><span data-stu-id="4f2af-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4f2af-107">Pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="4f2af-107">NuGet packages</span></span>

<span data-ttu-id="4f2af-108">[Pakiety NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:</span><span class="sxs-lookup"><span data-stu-id="4f2af-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="4f2af-109">[Typowe](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): typowe pakiet, który jest udostępniany między nadawcami a odbiornikami.</span><span class="sxs-lookup"><span data-stu-id="4f2af-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="4f2af-110">[Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): zestaw pakietów obsługi wysyłania własne elementy Webhook dla innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4f2af-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="4f2af-111">Funkcja wysyłania elementów Webhook jest opisany bardziej szczegółowo w [wysyłania elementów Webhook](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="4f2af-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="4f2af-112">[Odbiorniki](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): zestaw pakietów obsługi odbieranie elementów Webhook od innych.</span><span class="sxs-lookup"><span data-stu-id="4f2af-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="4f2af-113">Funkcje do odbierania elementów Webhook jest opisany bardziej szczegółowo w [odbieranie elementów Webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="4f2af-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
