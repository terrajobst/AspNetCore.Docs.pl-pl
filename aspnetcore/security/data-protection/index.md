---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="219e8-103">Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="219e8-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="219e8-104">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="219e8-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="219e8-105">Wprowadzenie do interfejsów API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="219e8-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="219e8-106">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="219e8-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="219e8-107">Omówienie interfejsów API przeznaczonych dla klientów</span><span class="sxs-lookup"><span data-stu-id="219e8-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="219e8-108">Ciągi celów</span><span class="sxs-lookup"><span data-stu-id="219e8-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="219e8-109">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="219e8-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="219e8-110">Tworzenia skrótów haseł</span><span class="sxs-lookup"><span data-stu-id="219e8-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="219e8-111">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="219e8-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="219e8-112">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="219e8-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="219e8-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="219e8-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="219e8-114">Konfigurowanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="219e8-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="219e8-115">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="219e8-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="219e8-116">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="219e8-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="219e8-117">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="219e8-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="219e8-118">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="219e8-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="219e8-119">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="219e8-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="219e8-120">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="219e8-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="219e8-121">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="219e8-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="219e8-122">Implementacja</span><span class="sxs-lookup"><span data-stu-id="219e8-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="219e8-123">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="219e8-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="219e8-124">Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie</span><span class="sxs-lookup"><span data-stu-id="219e8-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="219e8-125">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="219e8-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="219e8-126">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="219e8-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="219e8-127">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="219e8-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="219e8-128">Szyfrowanie kluczy podczas magazynowania</span><span class="sxs-lookup"><span data-stu-id="219e8-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="219e8-129">Niezmienność kluczy i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="219e8-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="219e8-130">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="219e8-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="219e8-131">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="219e8-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="219e8-132">Zgodność</span><span class="sxs-lookup"><span data-stu-id="219e8-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="219e8-133">Zastępowanie <machineKey> na platformie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="219e8-133">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
