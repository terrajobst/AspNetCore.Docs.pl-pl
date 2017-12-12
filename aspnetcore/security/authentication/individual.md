---
title: "Artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników"
author: rick-anderson
description: "Ten dokument zawiera listę artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników."
keywords: Platformy ASP.NET Core autoryzacji, IAuthorizationService
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="ddc92-104">Artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników</span><span class="sxs-lookup"><span data-stu-id="ddc92-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="ddc92-105">Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".</span><span class="sxs-lookup"><span data-stu-id="ddc92-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="ddc92-106">Szablony uwierzytelniania są dostępne w .NET Core interfejsu wiersza polecenia z `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="ddc92-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="ddc92-107">Następujące artykuły pokazują, jak używać kod wygenerowany w szablonach platformy ASP.NET Core, które używają indywidualne konta użytkowników:</span><span class="sxs-lookup"><span data-stu-id="ddc92-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="ddc92-108">Uwierzytelnianie dwuskładnikowe z programem SMS</span><span class="sxs-lookup"><span data-stu-id="ddc92-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="ddc92-109">Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddc92-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ddc92-110">Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ddc92-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)