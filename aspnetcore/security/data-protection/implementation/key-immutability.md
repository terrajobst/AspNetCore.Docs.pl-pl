---
title: "Immutability klucza i zmienianie ustawień"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core danych ochrony klucza immutability interfejsów API."
keywords: Immutability klucza platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="9e6d9-104">Klucz Immutability i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="9e6d9-104">Key Immutability and changing settings</span></span>

<span data-ttu-id="9e6d9-105">Gdy obiekt jest utrwalone w magazynie zapasowy, jego reprezentacja zawsze jest stała.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-105">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="9e6d9-106">Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie mogą ulegać mutacjom.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-106">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="9e6d9-107">Głównym celem to zachowanie jest aby zapobiec uszkodzeniu danych.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-107">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="9e6d9-108">W wyniku tego zachowania po klucz są zapisywane jako magazynu zapasowego, jest niezmienialny.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-108">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="9e6d9-109">Jego daty utworzenia, aktywacji i wygaśnięcia można nigdy zmieniać, chociaż może on odwołany za pomocą `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-109">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="9e6d9-110">Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowanie właściwości rest również są niezmienne.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-110">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="9e6d9-111">Deweloper zmiana każdego ustawienia, które ma wpływ na trwałości klucza te zmiany nie zostaną zaczynają obowiązywać po ponownym wygenerowaniu klucza, albo za pośrednictwem jawnym wywołaniem `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](key-management.md#data-protection-implementation-key-management) zachowanie.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-111">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="9e6d9-112">Dostępne są następujące ustawienia, które mają wpływ na trwałości klucza:</span><span class="sxs-lookup"><span data-stu-id="9e6d9-112">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="9e6d9-113">Domyślny okres istnienia klucza</span><span class="sxs-lookup"><span data-stu-id="9e6d9-113">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="9e6d9-114">Szyfrowanie klucza w mechanizm rest</span><span class="sxs-lookup"><span data-stu-id="9e6d9-114">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="9e6d9-115">Algorytmicznego informacje zawarte w kluczu</span><span class="sxs-lookup"><span data-stu-id="9e6d9-115">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="9e6d9-116">Te ustawienia do wcześniejszej niż klawisza Następna automatyczne stopniowych czasu zaczną działać, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` wymusić Tworzenie nowego klucza.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-116">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="9e6d9-117">Pamiętaj, aby określić datę aktywację jawnym ({teraz + 2 dni} jest regułą umożliwia czasu, dla której zmiana obejmie) oraz datę wygaśnięcia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-117">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="9e6d9-118">Wszystkie aplikacje dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-118">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="9e6d9-119">W przeciwnym razie wartość właściwości klucza utrwalonego będzie zależna od określonej aplikacji, która wywołała procedury generowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="9e6d9-119">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
