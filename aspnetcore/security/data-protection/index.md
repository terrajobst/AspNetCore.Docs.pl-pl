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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="0b31a-104">Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie</span><span class="sxs-lookup"><span data-stu-id="0b31a-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="0b31a-105">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="0b31a-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="0b31a-106">Rozpoczynanie pracy z interfejsami API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="0b31a-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="0b31a-107">Interfejsy API klienta</span><span class="sxs-lookup"><span data-stu-id="0b31a-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="0b31a-108">Omówienie interfejsów API klienta</span><span class="sxs-lookup"><span data-stu-id="0b31a-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="0b31a-109">Cel ciągów</span><span class="sxs-lookup"><span data-stu-id="0b31a-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="0b31a-110">Cel hierarchii i obsługi wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="0b31a-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="0b31a-111">Tworzenia skrótu hasła</span><span class="sxs-lookup"><span data-stu-id="0b31a-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="0b31a-112">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="0b31a-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="0b31a-113">Wyłączanie ochrony ładunków, której klucze zostały odwołane.</span><span class="sxs-lookup"><span data-stu-id="0b31a-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="0b31a-114">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="0b31a-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="0b31a-115">Konfigurowanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="0b31a-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="0b31a-116">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="0b31a-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="0b31a-117">Międzynarodowe zasad komputera</span><span class="sxs-lookup"><span data-stu-id="0b31a-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="0b31a-118">Scenariusze pamiętać z systemem innym niż Podpisane</span><span class="sxs-lookup"><span data-stu-id="0b31a-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="0b31a-119">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="0b31a-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="0b31a-120">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="0b31a-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="0b31a-121">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="0b31a-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="0b31a-122">Dodatkowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="0b31a-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="0b31a-123">Implementacja</span><span class="sxs-lookup"><span data-stu-id="0b31a-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="0b31a-124">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="0b31a-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="0b31a-125">Pochodnym podkluczy i uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="0b31a-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="0b31a-126">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="0b31a-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="0b31a-127">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="0b31a-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="0b31a-128">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="0b31a-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="0b31a-129">Szyfrowanie klucza magazynowane</span><span class="sxs-lookup"><span data-stu-id="0b31a-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="0b31a-130">Immutability klucza i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="0b31a-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="0b31a-131">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="0b31a-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="0b31a-132">Dostawców ochrony danych tymczasowych</span><span class="sxs-lookup"><span data-stu-id="0b31a-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="0b31a-133">Zgodność</span><span class="sxs-lookup"><span data-stu-id="0b31a-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="0b31a-134">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="0b31a-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="0b31a-135">Zastępowanie <machineKey> w programie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0b31a-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
