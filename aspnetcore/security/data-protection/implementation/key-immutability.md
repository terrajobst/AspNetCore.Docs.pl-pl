---
title: "Immutability klucza i zmienianie ustawień"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core danych ochrony klucza immutability interfejsów API."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="99fbc-103">Klucz Immutability i zmienianie ustawień</span><span class="sxs-lookup"><span data-stu-id="99fbc-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="99fbc-104">Gdy obiekt jest utrwalone w magazynie zapasowy, jego reprezentacja zawsze jest stała.</span><span class="sxs-lookup"><span data-stu-id="99fbc-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="99fbc-105">Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie mogą ulegać mutacjom.</span><span class="sxs-lookup"><span data-stu-id="99fbc-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="99fbc-106">Głównym celem to zachowanie jest aby zapobiec uszkodzeniu danych.</span><span class="sxs-lookup"><span data-stu-id="99fbc-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="99fbc-107">W wyniku tego zachowania po klucz są zapisywane jako magazynu zapasowego, jest niezmienialny.</span><span class="sxs-lookup"><span data-stu-id="99fbc-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="99fbc-108">Jego daty utworzenia, aktywacji i wygaśnięcia można nigdy zmieniać, chociaż może on odwołany za pomocą `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="99fbc-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="99fbc-109">Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowanie właściwości rest również są niezmienne.</span><span class="sxs-lookup"><span data-stu-id="99fbc-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="99fbc-110">Zmiana każdego ustawienia, które ma wpływ na klucza trwałości, deweloper tych zmian nie zaczynają obowiązywać po ponownym wygenerowaniu klucza, albo za pomocą jawnego wywołania `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](key-management.md#data-protection-implementation-key-management) zachowanie.</span><span class="sxs-lookup"><span data-stu-id="99fbc-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="99fbc-111">Dostępne są następujące ustawienia, które mają wpływ na trwałości klucza:</span><span class="sxs-lookup"><span data-stu-id="99fbc-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="99fbc-112">Domyślny okres istnienia klucza</span><span class="sxs-lookup"><span data-stu-id="99fbc-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="99fbc-113">Szyfrowanie klucza w mechanizm rest</span><span class="sxs-lookup"><span data-stu-id="99fbc-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="99fbc-114">Algorytmicznego informacje zawarte w kluczu</span><span class="sxs-lookup"><span data-stu-id="99fbc-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="99fbc-115">Te ustawienia do wcześniejszej niż klawisza Następna automatyczne stopniowych czasu zaczną działać, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` wymusić Tworzenie nowego klucza.</span><span class="sxs-lookup"><span data-stu-id="99fbc-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="99fbc-116">Pamiętaj, aby określić datę aktywację jawnym ({teraz + 2 dni} jest regułą umożliwia czasu, dla której zmiana obejmie) oraz datę wygaśnięcia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="99fbc-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="99fbc-117">Wszystkie aplikacje dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="99fbc-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="99fbc-118">W przeciwnym razie wartość właściwości klucza utrwalonego będzie zależna od określonej aplikacji, która wywołała procedury generowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="99fbc-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
