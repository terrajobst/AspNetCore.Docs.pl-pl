---
title: Dostawcy magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji o dostawcy magazynu kluczy na platformie ASP.NET Core i jak skonfigurować lokalizacje magazynu kluczy.
ms.author: riande
ms.date: 12/19/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: d6dabc9e4581e0891d1dd14f73e086d50b45bba4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903046"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="ca824-103">Dostawcy magazynu kluczy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca824-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="ca824-104">System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie utrwalone kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="ca824-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="ca824-105">Deweloper można zastąpić domyślny mechanizm odnajdywania i ręcznie określ lokalizację.</span><span class="sxs-lookup"><span data-stu-id="ca824-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="ca824-106">Jeśli określisz lokalizacji jawne trwałość klucza, system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizmu rest, więc klucze nie są szyfrowane w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ca824-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="ca824-107">Zalecamy, aby można dodatkowo [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="ca824-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="ca824-108">System plików</span><span class="sxs-lookup"><span data-stu-id="ca824-108">File system</span></span>

<span data-ttu-id="ca824-109">Aby skonfigurować repozytorium kluczy opartych na systemie plików, należy wywołać [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ca824-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="ca824-110">Podaj [DirectoryInfo](/dotnet/api/system.io.directoryinfo) wskazującego repozytorium, gdzie można przechowywać kluczy:</span><span class="sxs-lookup"><span data-stu-id="ca824-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="ca824-111">`DirectoryInfo` Może wskazywać katalog na komputerze lokalnym lub może wskazywać do folderu w udziale sieciowym.</span><span class="sxs-lookup"><span data-stu-id="ca824-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="ca824-112">Jeśli wskazuje katalog na komputerze lokalnym i scenariusza jest to, że tylko aplikacje na komputerze lokalnym wymaga dostępu do używania tego repozytorium, należy rozważyć użycie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (na Windows) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ca824-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="ca824-113">W przeciwnym razie należy wziąć pod uwagę przy użyciu [certyfikat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ca824-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="ca824-114">Platforma Azure i usługi Redis</span><span class="sxs-lookup"><span data-stu-id="ca824-114">Azure and Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ca824-115">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) i [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) pakietów zezwolić na przechowywanie kluczy ochrony danych w usłudze Azure Storage i Redis pamięć podręczna.</span><span class="sxs-lookup"><span data-stu-id="ca824-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="ca824-116">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca824-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="ca824-117">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="ca824-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ca824-118">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) i [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) pakietów zezwolić na przechowywanie kluczy ochrony danych w usłudze Azure Storage lub usługi Redis cache.</span><span class="sxs-lookup"><span data-stu-id="ca824-118">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="ca824-119">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca824-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="ca824-120">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="ca824-120">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

<span data-ttu-id="ca824-121">Aby skonfigurować dostawcę usługi Azure Blob Storage, należy wywołać jedną z [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="ca824-121">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ca824-122">Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="ca824-122">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ca824-123">Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="ca824-123">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="ca824-124">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="ca824-124">For more information, see the following topics:</span></span>

* [<span data-ttu-id="ca824-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="ca824-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="ca824-126">Usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="ca824-126">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="ca824-127">Przykłady ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="ca824-127">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="ca824-128">Rejestr</span><span class="sxs-lookup"><span data-stu-id="ca824-128">Registry</span></span>

<span data-ttu-id="ca824-129">**Dotyczy tylko wdrożenia Windows.**</span><span class="sxs-lookup"><span data-stu-id="ca824-129">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="ca824-130">Czasami aplikacja ma uprawnienia do zapisu w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="ca824-130">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="ca824-131">Rozważmy scenariusz, w którym aplikacja jest uruchomiona jako konto usługi wirtualnej (takie jak *w3wp.exe*dla tożsamości puli aplikacji).</span><span class="sxs-lookup"><span data-stu-id="ca824-131">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="ca824-132">W takich przypadkach administrator może aprowizować klucz rejestru, który jest dostępny za pomocą tożsamości konta usługi.</span><span class="sxs-lookup"><span data-stu-id="ca824-132">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="ca824-133">Wywołaj [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodę rozszerzenia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ca824-133">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="ca824-134">Podaj [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) wskazuje lokalizację przechowywania kluczy kryptograficznych:</span><span class="sxs-lookup"><span data-stu-id="ca824-134">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="ca824-135">Firma Microsoft zaleca używanie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ca824-135">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="ca824-136">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ca824-136">Entity Framework Core</span></span>

<span data-ttu-id="ca824-137">[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pakietu udostępnia mechanizm do przechowywania danych ochrony kluczy z bazą danych przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ca824-137">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="ca824-138">`Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` Pakietu NuGet należy dodać do pliku projektu nie jest częścią [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ca824-138">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ca824-139">Z tego pakietu kluczy mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca824-139">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="ca824-140">Aby skonfigurować dostawcę programu EF Core, należy wywołać [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metody:</span><span class="sxs-lookup"><span data-stu-id="ca824-140">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="ca824-141">Parametr ogólny, `TContext`, musi dziedziczyć [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="ca824-141">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="ca824-142">Utwórz `DataProtectionKeys` tabeli.</span><span class="sxs-lookup"><span data-stu-id="ca824-142">Create the `DataProtectionKeys` table.</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca824-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca824-143">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ca824-144">Wykonaj następujące polecenia w **Konsola Menedżera pakietów** okna (PMC):</span><span class="sxs-lookup"><span data-stu-id="ca824-144">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ca824-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ca824-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ca824-146">Wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="ca824-146">Execute the following commands in a command shell:</span></span>

```console
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="ca824-147">`MyKeysContext` jest `DbContext` zdefiniowane w poprzednim przykładzie kodu.</span><span class="sxs-lookup"><span data-stu-id="ca824-147">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="ca824-148">Jeśli używasz `DbContext` pod inną nazwą podstaw swoje `DbContext` nazwę `MyKeysContext`.</span><span class="sxs-lookup"><span data-stu-id="ca824-148">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="ca824-149">`DataProtectionKeys` Klasy na jednostkę przyjmuje strukturę pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ca824-149">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="ca824-150">Właściwości/pola</span><span class="sxs-lookup"><span data-stu-id="ca824-150">Property/Field</span></span> | <span data-ttu-id="ca824-151">Typ CLR</span><span class="sxs-lookup"><span data-stu-id="ca824-151">CLR Type</span></span> | <span data-ttu-id="ca824-152">Typ SQL</span><span class="sxs-lookup"><span data-stu-id="ca824-152">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="ca824-153">`int`, Klucz podstawowy, nie ma wartości null</span><span class="sxs-lookup"><span data-stu-id="ca824-153">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="ca824-154">`nvarchar(MAX)`, o wartości null</span><span class="sxs-lookup"><span data-stu-id="ca824-154">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="ca824-155">`nvarchar(MAX)`, o wartości null</span><span class="sxs-lookup"><span data-stu-id="ca824-155">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="ca824-156">Niestandardowe repozytorium klucza</span><span class="sxs-lookup"><span data-stu-id="ca824-156">Custom key repository</span></span>

<span data-ttu-id="ca824-157">Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm trwałość klucza, podając własne [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="ca824-157">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
