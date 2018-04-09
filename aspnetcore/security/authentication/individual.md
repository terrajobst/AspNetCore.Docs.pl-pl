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
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".

Szablony uwierzytelniania są dostępne w .NET Core interfejsu wiersza polecenia z `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Następujące artykuły pokazują, jak używać kod wygenerowany w szablonach platformy ASP.NET Core, które używają indywidualne konta użytkowników:

* [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
* [Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji](xref:security/authorization/secure-data)
