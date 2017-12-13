---
uid: webhooks/source
title: "Kod źródłowy elementów Webhook ASP.NET i pakiety NuGet | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Łącza do kodu źródłowego elementów Webhook ASP.NET i pakiety NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="2f056-103">Kod źródłowy elementów Webhook ASP.NET i pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="2f056-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="2f056-104">Microsoft ASP.NET WebHooks jest częścią rodziny Microsoft ASP.NET modułów i jest obsługiwany jako [Otwórz projekt źródłowy w serwisie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="2f056-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="2f056-105">To oznacza, że możemy zaakceptować wkładów, ale można znaleźć w [wytyczne wkład](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="2f056-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="2f056-106">Ten dokumentację w trybie online, które są odczytywanie teraz jest również obsługiwane jako [typu Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , a także akceptuje udziały.</span><span class="sxs-lookup"><span data-stu-id="2f056-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="2f056-107">Pakiety Nuget</span><span class="sxs-lookup"><span data-stu-id="2f056-107">Nuget Packages</span></span>

<span data-ttu-id="2f056-108">Microsoft ASP.NET WebHooks jest również dostępne, ponieważ Podgląd pakietów Nuget, które oznacza, że należy wybrać flagi wersji zapoznawczej programu Visual Studio w celu zapoznania się z nimi.</span><span class="sxs-lookup"><span data-stu-id="2f056-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="2f056-109">[Pakietów Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są devided na trzy części:</span><span class="sxs-lookup"><span data-stu-id="2f056-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="2f056-110">[Typowe](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): typowe pakietu, który jest udostępniana między nadawcami a odbiornikami.</span><span class="sxs-lookup"><span data-stu-id="2f056-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="2f056-111">[Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): zestaw pakietów obsługi wysyłania własnych elementów Webhook do innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2f056-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="2f056-112">Funkcja wysyłania elementów Webhook jest opisany bardziej szczegółowo w [wysyłania elementów Webhook](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f056-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="2f056-113">[Odbiorniki](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): zestaw pakietów obsługujący odbieranie elementów Webhook od innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2f056-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="2f056-114">Funkcja odbierania elementów Webhook jest opisany bardziej szczegółowo w [odbieranie elementów Webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f056-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
