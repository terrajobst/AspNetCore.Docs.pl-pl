---
title: Konfigurowanie tożsamości typu danych kluczy podstawowych w ASP.NET Core
author: AdrienTorris
description: Więcej informacji na temat kroki związane z konfigurowaniem typ żądanych danych, używany dla tożsamości ASP.NET Core klucza podstawowego.
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274359"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Konfigurowanie tożsamości typu danych kluczy podstawowych w ASP.NET Core

Tożsamość platformy ASP.NET Core pozwala na skonfigurowanie typ danych używany do reprezentowania klucza podstawowego. Używa tożsamości `string` domyślnie — typ danych. Możesz zmienić to zachowanie.

## <a name="customize-the-primary-key-data-type"></a>Dostosowanie typu danych kluczy podstawowych

1. Utwórz niestandardową implementację [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) klasy. Reprezentuje typ służący do tworzenia obiektów użytkownika. W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. Utwórz niestandardową implementację [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) klasy. Reprezentuje typ służący do tworzenia obiektów roli. W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. Utwórz klasę kontekstu w niestandardowej bazie danych. Dziedziczy z klasy kontekstu bazy danych programu Entity Framework używany dla tożsamości. `TUser` i `TRole` argumenty odwołania klas niestandardowych użytkownika i roli odpowiednio utworzony w poprzednim kroku. `Guid` — Typ danych jest zdefiniowany dla klucza podstawowego.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. Podczas dodawania usługa tożsamości w klasie uruchamiania aplikacji, należy zarejestrować klasy kontekstu w niestandardowej bazie danych.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   `AddEntityFrameworkStores` Nie obsługuje metody `TKey` argumentu w postaci, w jakiej zostały w ASP.NET Core 1.x. Klucz podstawowy typ danych jest wywnioskowany analizując `DbContext` obiektu.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   `AddEntityFrameworkStores` Metoda przyjmuje `TKey` wskazuje klucz podstawowy typ danych argumentu.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a>Testowanie zmian

Po zakończeniu zmiany konfiguracji właściwość reprezentujące klucz podstawowy odzwierciedla nowy typ danych. W poniższym przykładzie pokazano, uzyskiwanie dostępu do właściwości w kontroler MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
