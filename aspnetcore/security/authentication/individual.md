---
title: Artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników
author: rick-anderson
description: Odnajdywanie artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729553"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="1880b-103">Artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników</span><span class="sxs-lookup"><span data-stu-id="1880b-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="1880b-104">Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".</span><span class="sxs-lookup"><span data-stu-id="1880b-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="1880b-105">Szablony uwierzytelniania są dostępne w .NET Core interfejsu wiersza polecenia z `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="1880b-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="1880b-106">Następujące artykuły pokazują, jak używać kod wygenerowany w szablonach platformy ASP.NET Core, które używają indywidualne konta użytkowników:</span><span class="sxs-lookup"><span data-stu-id="1880b-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="1880b-107">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS</span><span class="sxs-lookup"><span data-stu-id="1880b-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="1880b-108">Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1880b-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1880b-109">Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji</span><span class="sxs-lookup"><span data-stu-id="1880b-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
