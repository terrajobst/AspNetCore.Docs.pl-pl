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
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Artykułów opartych na projektów utworzonych za pomocą indywidualne konta użytkowników

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".

Szablony uwierzytelniania są dostępne w .NET Core interfejsu wiersza polecenia z `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Następujące artykuły pokazują, jak używać kod wygenerowany w szablonach platformy ASP.NET Core, które używają indywidualne konta użytkowników:

* [Uwierzytelnianie dwuskładnikowe z programem SMS](xref:security/authentication/2fa)
* [Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji](xref:security/authorization/secure-data)