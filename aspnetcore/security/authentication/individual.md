---
title: "Artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników"
author: rick-anderson
description: "Ten dokument zawiera listę artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników."
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników

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
