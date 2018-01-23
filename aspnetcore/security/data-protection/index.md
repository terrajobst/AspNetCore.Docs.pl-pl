---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="3315b-103">Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="3315b-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="3315b-104">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="3315b-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="3315b-105">Wprowadzenie do interfejsów API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="3315b-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="3315b-106">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="3315b-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="3315b-107">Omówienie interfejsów API przeznaczonych dla klientów</span><span class="sxs-lookup"><span data-stu-id="3315b-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="3315b-108">Ciągi celów</span><span class="sxs-lookup"><span data-stu-id="3315b-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="3315b-109">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="3315b-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="3315b-110">Tworzenia skrótów haseł</span><span class="sxs-lookup"><span data-stu-id="3315b-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="3315b-111">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="3315b-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="3315b-112">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="3315b-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="3315b-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="3315b-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="3315b-114">Konfigurowanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="3315b-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="3315b-115">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="3315b-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="3315b-116">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="3315b-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="3315b-117">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="3315b-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="3315b-118">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="3315b-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="3315b-119">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="3315b-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="3315b-120">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="3315b-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="3315b-121">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="3315b-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="3315b-122">Implementacja</span><span class="sxs-lookup"><span data-stu-id="3315b-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="3315b-123">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="3315b-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="3315b-124">Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie</span><span class="sxs-lookup"><span data-stu-id="3315b-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="3315b-125">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="3315b-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="3315b-126">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="3315b-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="3315b-127">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="3315b-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="3315b-128">Szyfrowanie kluczy podczas magazynowania</span><span class="sxs-lookup"><span data-stu-id="3315b-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="3315b-129">Niezmienność kluczy i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="3315b-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="3315b-130">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="3315b-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="3315b-131">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="3315b-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="3315b-132">Zgodność</span><span class="sxs-lookup"><span data-stu-id="3315b-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="3315b-133">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="3315b-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="3315b-134">Zastępowanie <machineKey> na platformie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3315b-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
