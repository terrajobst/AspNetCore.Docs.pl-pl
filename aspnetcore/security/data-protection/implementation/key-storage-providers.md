---
title: Dostawcy magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji o dostawcy magazynu kluczy na platformie ASP.NET Core i jak skonfigurować lokalizacje magazynu kluczy.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 76f224fb7d322722c3875b734127da4da294a646
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881206"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="7f446-103">Dostawcy magazynu kluczy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f446-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="7f446-104">System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie utrwalone kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="7f446-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="7f446-105">Deweloper można zastąpić domyślny mechanizm odnajdywania i ręcznie określ lokalizację.</span><span class="sxs-lookup"><span data-stu-id="7f446-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="7f446-106">Jeśli określisz lokalizacji jawne trwałość klucza, system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizmu rest, więc klucze nie są szyfrowane w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="7f446-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="7f446-107">Zalecamy, aby można dodatkowo [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="7f446-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="7f446-108">System plików</span><span class="sxs-lookup"><span data-stu-id="7f446-108">File system</span></span>

<span data-ttu-id="7f446-109">Aby skonfigurować repozytorium kluczy opartych na systemie plików, należy wywołać [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="7f446-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="7f446-110">Podaj [DirectoryInfo](/dotnet/api/system.io.directoryinfo) wskazującego repozytorium, gdzie można przechowywać kluczy:</span><span class="sxs-lookup"><span data-stu-id="7f446-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="7f446-111">`DirectoryInfo` Może wskazywać katalog na komputerze lokalnym lub może wskazywać do folderu w udziale sieciowym.</span><span class="sxs-lookup"><span data-stu-id="7f446-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="7f446-112">Jeśli wskazuje katalog na komputerze lokalnym i scenariusza jest to, że tylko aplikacje na komputerze lokalnym wymaga dostępu do używania tego repozytorium, należy rozważyć użycie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (na Windows) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="7f446-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="7f446-113">W przeciwnym razie należy wziąć pod uwagę przy użyciu [certyfikat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="7f446-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="7f446-114">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7f446-114">Azure Storage</span></span>

<span data-ttu-id="7f446-115">Pakiet [Microsoft. AspNetCore. dataprotection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) umożliwia przechowywanie kluczy ochrony danych w usłudze Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7f446-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package allows storing data protection keys in Azure Blob Storage.</span></span> <span data-ttu-id="7f446-116">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7f446-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7f446-117">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="7f446-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

<span data-ttu-id="7f446-118">Aby skonfigurować dostawcę usługi Azure Blob Storage, wywołaj jedno z przeciążeń [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) .</span><span class="sxs-lookup"><span data-stu-id="7f446-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="7f446-119">Jeśli aplikacja sieci Web działa jako usługa platformy Azure, tokeny uwierzytelniania mogą być tworzone automatycznie przy użyciu [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="7f446-119">If the web app is running as an Azure service, authentication tokens can be automatically created using [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span></span>

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

<span data-ttu-id="7f446-120">Zobacz [więcej szczegółów na temat konfigurowania uwierzytelniania](/azure/key-vault/service-to-service-authentication) między usługami.</span><span class="sxs-lookup"><span data-stu-id="7f446-120">See [more details about configuring service-to-service authentication.](/azure/key-vault/service-to-service-authentication)</span></span>

## <a name="redis"></a><span data-ttu-id="7f446-121">Redis</span><span class="sxs-lookup"><span data-stu-id="7f446-121">Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7f446-122">Pakiet [Microsoft. AspNetCore. dataprotection. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) umożliwia przechowywanie kluczy ochrony danych w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="7f446-122">The [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="7f446-123">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7f446-123">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7f446-124">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="7f446-124">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7f446-125">Pakiet [Microsoft. AspNetCore. dataprotection. Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) umożliwia przechowywanie kluczy ochrony danych w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="7f446-125">The [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="7f446-126">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7f446-126">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7f446-127">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="7f446-127">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7f446-128">Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="7f446-128">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

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

<span data-ttu-id="7f446-129">Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="7f446-129">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="7f446-130">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="7f446-130">For more information, see the following topics:</span></span>

* [<span data-ttu-id="7f446-131">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="7f446-131">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="7f446-132">Usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="7f446-132">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="7f446-133">Przykłady ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="7f446-133">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="7f446-134">Rejestr</span><span class="sxs-lookup"><span data-stu-id="7f446-134">Registry</span></span>

<span data-ttu-id="7f446-135">**Dotyczy tylko wdrożenia Windows.**</span><span class="sxs-lookup"><span data-stu-id="7f446-135">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="7f446-136">Czasami aplikacja ma uprawnienia do zapisu w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="7f446-136">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="7f446-137">Rozważmy scenariusz, w którym aplikacja jest uruchomiona jako konto usługi wirtualnej (takie jak *w3wp.exe*dla tożsamości puli aplikacji).</span><span class="sxs-lookup"><span data-stu-id="7f446-137">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="7f446-138">W takich przypadkach administrator może aprowizować klucz rejestru, który jest dostępny za pomocą tożsamości konta usługi.</span><span class="sxs-lookup"><span data-stu-id="7f446-138">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="7f446-139">Wywołaj [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodę rozszerzenia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="7f446-139">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="7f446-140">Podaj [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) wskazuje lokalizację przechowywania kluczy kryptograficznych:</span><span class="sxs-lookup"><span data-stu-id="7f446-140">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="7f446-141">Firma Microsoft zaleca używanie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="7f446-141">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="7f446-142">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7f446-142">Entity Framework Core</span></span>

<span data-ttu-id="7f446-143">[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pakietu udostępnia mechanizm do przechowywania danych ochrony kluczy z bazą danych przy użyciu platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7f446-143">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="7f446-144">`Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` Pakietu NuGet należy dodać do pliku projektu nie jest częścią [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7f446-144">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="7f446-145">Z tego pakietu kluczy mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="7f446-145">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="7f446-146">Aby skonfigurować dostawcę EF Core, wywołaj metodę [PersistKeysToDbContext\<TContext >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) :</span><span class="sxs-lookup"><span data-stu-id="7f446-146">To configure the EF Core provider, call the [PersistKeysToDbContext\<TContext>](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

<span data-ttu-id="7f446-147">Parametr generyczny, `TContext`, musi dziedziczyć z [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) i implementować [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="7f446-147">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and implement [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="7f446-148">Utwórz tabelę `DataProtectionKeys`.</span><span class="sxs-lookup"><span data-stu-id="7f446-148">Create the `DataProtectionKeys` table.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7f446-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f446-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7f446-150">W oknie **konsola Menedżera pakietów** (PMC) wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7f446-150">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7f446-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7f446-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7f446-152">Wykonaj następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="7f446-152">Execute the following commands in a command shell:</span></span>

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="7f446-153">`MyKeysContext` to `DbContext` zdefiniowane w poprzednim przykładzie kodu.</span><span class="sxs-lookup"><span data-stu-id="7f446-153">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="7f446-154">Jeśli używasz `DbContext` z inną nazwą, podstaw `DbContext` nazwę `MyKeysContext`.</span><span class="sxs-lookup"><span data-stu-id="7f446-154">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="7f446-155">Klasa `DataProtectionKeys`/jednostka przyjmuje strukturę pokazaną w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="7f446-155">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="7f446-156">Właściwość/pole</span><span class="sxs-lookup"><span data-stu-id="7f446-156">Property/Field</span></span> | <span data-ttu-id="7f446-157">Typ CLR</span><span class="sxs-lookup"><span data-stu-id="7f446-157">CLR Type</span></span> | <span data-ttu-id="7f446-158">Typ SQL</span><span class="sxs-lookup"><span data-stu-id="7f446-158">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="7f446-159">`int`, PK, not null</span><span class="sxs-lookup"><span data-stu-id="7f446-159">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="7f446-160">`nvarchar(MAX)`, wartość null</span><span class="sxs-lookup"><span data-stu-id="7f446-160">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="7f446-161">`nvarchar(MAX)`, wartość null</span><span class="sxs-lookup"><span data-stu-id="7f446-161">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="7f446-162">Niestandardowe repozytorium klucza</span><span class="sxs-lookup"><span data-stu-id="7f446-162">Custom key repository</span></span>

<span data-ttu-id="7f446-163">Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm trwałość klucza, podając własne [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="7f446-163">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
