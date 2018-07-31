---
title: Dostawcy magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji o dostawcy magazynu kluczy na platformie ASP.NET Core i jak skonfigurować lokalizacje magazynu kluczy.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e712ff09b5306bc4481c4cc105448d7cbfa39f3a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356769"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="0b3c2-103">Dostawcy magazynu kluczy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b3c2-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="0b3c2-104">System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, gdzie utrwalone kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="0b3c2-105">Deweloper można zastąpić domyślny mechanizm odnajdywania i ręcznie określ lokalizację.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="0b3c2-106">Jeśli określisz lokalizacji jawne trwałość klucza, system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizmu rest, więc klucze nie są szyfrowane w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="0b3c2-107">Zalecamy, aby można dodatkowo [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="0b3c2-108">System plików</span><span class="sxs-lookup"><span data-stu-id="0b3c2-108">File system</span></span>

<span data-ttu-id="0b3c2-109">Aby skonfigurować repozytorium kluczy opartych na systemie plików, należy wywołać [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) procedury konfiguracji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="0b3c2-110">Podaj [DirectoryInfo](/dotnet/api/system.io.directoryinfo) wskazującego repozytorium, gdzie można przechowywać kluczy:</span><span class="sxs-lookup"><span data-stu-id="0b3c2-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="0b3c2-111">`DirectoryInfo` Może wskazywać katalog na komputerze lokalnym lub może wskazywać do folderu w udziale sieciowym.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="0b3c2-112">Jeśli wskazuje katalog na komputerze lokalnym i scenariusza jest to, że tylko aplikacje na komputerze lokalnym wymaga dostępu do używania tego repozytorium, należy rozważyć użycie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (na Windows) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="0b3c2-113">W przeciwnym razie należy wziąć pod uwagę przy użyciu [certyfikat X.509](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="0b3c2-114">Platforma Azure i usługi Redis</span><span class="sxs-lookup"><span data-stu-id="0b3c2-114">Azure and Redis</span></span>

<span data-ttu-id="0b3c2-115">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) i [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) pakietów zezwolić na przechowywanie kluczy ochrony danych w usłudze Azure Storage lub usługi Redis cache.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="0b3c2-116">Klucze mogą być współużytkowane przez wiele wystąpień aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="0b3c2-117">Aplikacje mogą udostępniać pliki cookie uwierzytelniania lub CSRF ochronę na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="0b3c2-118">Aby skonfigurować dostawcę usługi Azure Blob Storage, należy wywołać jedną z [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="0b3c2-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="0b3c2-119">Aby skonfigurować w pamięci podręcznej Redis, należy wywołać jedną z [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="0b3c2-119">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

<span data-ttu-id="0b3c2-120">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="0b3c2-120">For more information, see the following topics:</span></span>

* [<span data-ttu-id="0b3c2-121">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="0b3c2-121">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="0b3c2-122">Usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="0b3c2-122">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="0b3c2-123">Przykłady ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="0b3c2-123">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a><span data-ttu-id="0b3c2-124">Rejestr</span><span class="sxs-lookup"><span data-stu-id="0b3c2-124">Registry</span></span>

<span data-ttu-id="0b3c2-125">**Dotyczy tylko wdrożenia Windows.**</span><span class="sxs-lookup"><span data-stu-id="0b3c2-125">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="0b3c2-126">Czasami aplikacja ma uprawnienia do zapisu w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-126">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="0b3c2-127">Rozważmy scenariusz, w którym aplikacja jest uruchomiona jako konto usługi wirtualnej (takie jak *w3wp.exe*dla tożsamości puli aplikacji).</span><span class="sxs-lookup"><span data-stu-id="0b3c2-127">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="0b3c2-128">W takich przypadkach administrator może aprowizować klucz rejestru, który jest dostępny za pomocą tożsamości konta usługi.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-128">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="0b3c2-129">Wywołaj [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodę rozszerzenia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-129">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="0b3c2-130">Podaj [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) wskazuje lokalizację przechowywania kluczy kryptograficznych:</span><span class="sxs-lookup"><span data-stu-id="0b3c2-130">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="0b3c2-131">Firma Microsoft zaleca używanie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) do szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="0b3c2-131">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="0b3c2-132">Niestandardowe repozytorium klucza</span><span class="sxs-lookup"><span data-stu-id="0b3c2-132">Custom key repository</span></span>

<span data-ttu-id="0b3c2-133">Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm trwałość klucza, podając własne [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="0b3c2-133">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
