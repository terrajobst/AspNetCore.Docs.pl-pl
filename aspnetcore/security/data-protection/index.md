---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277127"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="84155-103">Ochrona danych w podstawowej platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="84155-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="84155-104">Wprowadzenie do ochrony danych</span><span class="sxs-lookup"><span data-stu-id="84155-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="84155-105">Wprowadzenie do interfejsów API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="84155-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="84155-106">Interfejsy API przeznaczone dla klientów</span><span class="sxs-lookup"><span data-stu-id="84155-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="84155-107">Omówienie interfejsów API przeznaczonych dla klientów</span><span class="sxs-lookup"><span data-stu-id="84155-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="84155-108">Ciągi celów</span><span class="sxs-lookup"><span data-stu-id="84155-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="84155-109">Hierarchia celów i obsługa wielu dzierżawców</span><span class="sxs-lookup"><span data-stu-id="84155-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="84155-110">Wyznaczanie skrótów haseł</span><span class="sxs-lookup"><span data-stu-id="84155-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="84155-111">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="84155-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="84155-112">Wyłączanie ochrony ładunków, których klucze zostały odwołane</span><span class="sxs-lookup"><span data-stu-id="84155-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="84155-113">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="84155-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="84155-114">Konfigurowanie ochrony danych platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84155-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="84155-115">Ustawienia domyślne</span><span class="sxs-lookup"><span data-stu-id="84155-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="84155-116">Zasady dla komputera</span><span class="sxs-lookup"><span data-stu-id="84155-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="84155-117">Scenariusze z systemem innym niż Podpisane aware</span><span class="sxs-lookup"><span data-stu-id="84155-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="84155-118">Interfejsy API rozszerzalności</span><span class="sxs-lookup"><span data-stu-id="84155-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="84155-119">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="84155-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="84155-120">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="84155-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="84155-121">Różne interfejsy API</span><span class="sxs-lookup"><span data-stu-id="84155-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="84155-122">Implementacja</span><span class="sxs-lookup"><span data-stu-id="84155-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="84155-123">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="84155-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="84155-124">Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie</span><span class="sxs-lookup"><span data-stu-id="84155-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="84155-125">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="84155-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="84155-126">Zarządzanie kluczami</span><span class="sxs-lookup"><span data-stu-id="84155-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="84155-127">Dostawcy magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="84155-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="84155-128">Szyfrowanie kluczy podczas magazynowania</span><span class="sxs-lookup"><span data-stu-id="84155-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="84155-129">Ustawienia i niezmienność klucza</span><span class="sxs-lookup"><span data-stu-id="84155-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="84155-130">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="84155-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="84155-131">Dostawcy efemerycznej ochrony danych</span><span class="sxs-lookup"><span data-stu-id="84155-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="84155-132">Zgodność</span><span class="sxs-lookup"><span data-stu-id="84155-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="84155-133">Zastępowanie ASP.NET <machineKey> w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84155-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
