---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="30ecb-103">Ochrona danych w podstawowej platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="30ecb-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="30ecb-104">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="30ecb-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="30ecb-105">Wprowadzenie do interfejsów API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="30ecb-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="30ecb-106">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="30ecb-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="30ecb-107">Omówienie interfejsów API przeznaczonych dla klientów</span><span class="sxs-lookup"><span data-stu-id="30ecb-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="30ecb-108">Ciągi celów</span><span class="sxs-lookup"><span data-stu-id="30ecb-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="30ecb-109">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="30ecb-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="30ecb-110">Skrót hasła</span><span class="sxs-lookup"><span data-stu-id="30ecb-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="30ecb-111">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="30ecb-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="30ecb-112">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="30ecb-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="30ecb-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="30ecb-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="30ecb-114">Konfigurowanie ochrony danych platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30ecb-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="30ecb-115">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="30ecb-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="30ecb-116">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="30ecb-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="30ecb-117">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="30ecb-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="30ecb-118">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="30ecb-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="30ecb-119">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="30ecb-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="30ecb-120">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="30ecb-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="30ecb-121">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="30ecb-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="30ecb-122">Implementacja</span><span class="sxs-lookup"><span data-stu-id="30ecb-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="30ecb-123">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="30ecb-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="30ecb-124">Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie</span><span class="sxs-lookup"><span data-stu-id="30ecb-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="30ecb-125">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="30ecb-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="30ecb-126">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="30ecb-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="30ecb-127">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="30ecb-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="30ecb-128">Szyfrowanie kluczy podczas magazynowania</span><span class="sxs-lookup"><span data-stu-id="30ecb-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="30ecb-129">Immutability klucza i ustawienia</span><span class="sxs-lookup"><span data-stu-id="30ecb-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="30ecb-130">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="30ecb-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="30ecb-131">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="30ecb-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="30ecb-132">Zgodność</span><span class="sxs-lookup"><span data-stu-id="30ecb-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="30ecb-133">Zastępowanie ASP.NET <machineKey> w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30ecb-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
