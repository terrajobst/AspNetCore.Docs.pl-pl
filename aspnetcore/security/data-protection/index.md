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
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="2a387-103">Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="2a387-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="2a387-104">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="2a387-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="2a387-105">Wprowadzenie do interfejsów API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="2a387-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="2a387-106">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="2a387-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="2a387-107">Omówienie interfejsów API przeznaczonych dla klientów</span><span class="sxs-lookup"><span data-stu-id="2a387-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="2a387-108">Ciągi celów</span><span class="sxs-lookup"><span data-stu-id="2a387-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="2a387-109">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="2a387-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="2a387-110">Tworzenia skrótów haseł</span><span class="sxs-lookup"><span data-stu-id="2a387-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="2a387-111">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="2a387-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="2a387-112">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="2a387-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="2a387-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="2a387-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="2a387-114">Konfigurowanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="2a387-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="2a387-115">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="2a387-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="2a387-116">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="2a387-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="2a387-117">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="2a387-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="2a387-118">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="2a387-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="2a387-119">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="2a387-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="2a387-120">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="2a387-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="2a387-121">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="2a387-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="2a387-122">Implementacja</span><span class="sxs-lookup"><span data-stu-id="2a387-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="2a387-123">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="2a387-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="2a387-124">Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie</span><span class="sxs-lookup"><span data-stu-id="2a387-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="2a387-125">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="2a387-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="2a387-126">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="2a387-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="2a387-127">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="2a387-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="2a387-128">Szyfrowanie kluczy podczas magazynowania</span><span class="sxs-lookup"><span data-stu-id="2a387-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="2a387-129">Niezmienność kluczy i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="2a387-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="2a387-130">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="2a387-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="2a387-131">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="2a387-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="2a387-132">Zgodność</span><span class="sxs-lookup"><span data-stu-id="2a387-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="2a387-133">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="2a387-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="2a387-134">Zastępowanie <machineKey> na platformie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2a387-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
