---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="c1025-104">Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="c1025-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="c1025-105">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c1025-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="c1025-106">Rozpoczynanie pracy z interfejsami API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c1025-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="c1025-107">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="c1025-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="c1025-108">Omówienie interfejsów API klienta</span><span class="sxs-lookup"><span data-stu-id="c1025-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="c1025-109">Cel ciągów</span><span class="sxs-lookup"><span data-stu-id="c1025-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="c1025-110">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="c1025-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="c1025-111">Tworzenia skrótu hasła</span><span class="sxs-lookup"><span data-stu-id="c1025-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="c1025-112">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="c1025-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="c1025-113">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="c1025-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="c1025-114">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="c1025-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="c1025-115">Konfigurowanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c1025-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="c1025-116">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="c1025-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="c1025-117">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="c1025-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="c1025-118">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="c1025-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="c1025-119">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="c1025-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="c1025-120">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="c1025-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="c1025-121">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="c1025-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="c1025-122">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="c1025-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="c1025-123">Implementacja</span><span class="sxs-lookup"><span data-stu-id="c1025-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="c1025-124">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="c1025-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="c1025-125">Pochodnym podkluczy i uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="c1025-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="c1025-126">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="c1025-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="c1025-127">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="c1025-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="c1025-128">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="c1025-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="c1025-129">Szyfrowanie klucza magazynowane</span><span class="sxs-lookup"><span data-stu-id="c1025-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="c1025-130">Immutability klucza i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="c1025-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="c1025-131">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="c1025-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="c1025-132">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="c1025-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="c1025-133">Zgodność</span><span class="sxs-lookup"><span data-stu-id="c1025-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="c1025-134">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="c1025-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="c1025-135">Zastępowanie <machineKey> na platformie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c1025-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
