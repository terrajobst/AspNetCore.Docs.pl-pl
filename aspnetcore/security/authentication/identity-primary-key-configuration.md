---
title: Konfigurowanie tożsamości typu danych kluczy podstawowych w ASP.NET Core
author: AdrienTorris
description: Więcej informacji na temat kroki związane z konfigurowaniem typ żądanych danych, używany dla tożsamości ASP.NET Core klucza podstawowego.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 49d5ef94abeb5bd616c5ddbcdd4358a58a8e63a4
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="4a970-103">Konfigurowanie tożsamości typu danych kluczy podstawowych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a970-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="4a970-104">Tożsamość platformy ASP.NET Core pozwala na skonfigurowanie typ danych używany do reprezentowania klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4a970-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="4a970-105">Używa tożsamości `string` domyślnie — typ danych.</span><span class="sxs-lookup"><span data-stu-id="4a970-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="4a970-106">Możesz zmienić to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="4a970-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="4a970-107">Dostosowanie typu danych kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="4a970-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="4a970-108">Utwórz niestandardową implementację [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) klasy.</span><span class="sxs-lookup"><span data-stu-id="4a970-108">Create a custom implementation of the [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="4a970-109">Reprezentuje typ służący do tworzenia obiektów użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a970-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="4a970-110">W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.</span><span class="sxs-lookup"><span data-stu-id="4a970-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="4a970-111">Utwórz niestandardową implementację [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) klasy.</span><span class="sxs-lookup"><span data-stu-id="4a970-111">Create a custom implementation of the [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="4a970-112">Reprezentuje typ służący do tworzenia obiektów roli.</span><span class="sxs-lookup"><span data-stu-id="4a970-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="4a970-113">W poniższym przykładzie, domyślnie `string` typu jest zastępowany `Guid`.</span><span class="sxs-lookup"><span data-stu-id="4a970-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="4a970-114">Utwórz klasę kontekstu w niestandardowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4a970-114">Create a custom database context class.</span></span> <span data-ttu-id="4a970-115">Dziedziczy z klasy kontekstu bazy danych programu Entity Framework używany dla tożsamości.</span><span class="sxs-lookup"><span data-stu-id="4a970-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="4a970-116">`TUser` i `TRole` argumenty odwołania klas niestandardowych użytkownika i roli odpowiednio utworzony w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="4a970-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="4a970-117">`Guid` — Typ danych jest zdefiniowany dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="4a970-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="4a970-118">Podczas dodawania usługa tożsamości w klasie uruchamiania aplikacji, należy zarejestrować klasy kontekstu w niestandardowej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4a970-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a970-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a970-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   <span data-ttu-id="4a970-120">`AddEntityFrameworkStores` Nie obsługuje metody `TKey` argumentu w postaci, w jakiej zostały w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4a970-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="4a970-121">Klucz podstawowy typ danych jest wywnioskowany analizując `DbContext` obiektu.</span><span class="sxs-lookup"><span data-stu-id="4a970-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a970-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a970-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   <span data-ttu-id="4a970-123">`AddEntityFrameworkStores` Metoda przyjmuje `TKey` wskazuje klucz podstawowy typ danych argumentu.</span><span class="sxs-lookup"><span data-stu-id="4a970-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a><span data-ttu-id="4a970-124">Testowanie zmian</span><span class="sxs-lookup"><span data-stu-id="4a970-124">Test the changes</span></span>

<span data-ttu-id="4a970-125">Po zakończeniu zmiany konfiguracji właściwość reprezentujące klucz podstawowy odzwierciedla nowy typ danych.</span><span class="sxs-lookup"><span data-stu-id="4a970-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="4a970-126">W poniższym przykładzie pokazano, uzyskiwanie dostępu do właściwości w kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="4a970-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
