---
title: Niezmienność kluczy i kluczy ustawień w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji systemu niezmienność kluczy ochrony danych programu ASP.NET Core interfejsów API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898765"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="96082-103">Niezmienność kluczy i kluczy ustawień w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96082-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="96082-104">Gdy obiekt jest utrwalone w magazynie pomocniczym, jego reprezentację w postaci zawsze jest stała.</span><span class="sxs-lookup"><span data-stu-id="96082-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="96082-105">Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie można zmutować.</span><span class="sxs-lookup"><span data-stu-id="96082-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="96082-106">Głównym celem to zachowanie jest, aby zapobiec uszkodzeniu danych.</span><span class="sxs-lookup"><span data-stu-id="96082-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="96082-107">W wyniku tego zachowania, jest napisana klucz do magazynu zapasowego niezmienne.</span><span class="sxs-lookup"><span data-stu-id="96082-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="96082-108">Jego daty utworzenia, aktywacji i wygaśnięcia nigdy nie można zmienić, chociaż mogą odwoływane za pomocą `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="96082-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="96082-109">Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowania we właściwościach rest również są niezmienne.</span><span class="sxs-lookup"><span data-stu-id="96082-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="96082-110">Jeśli Deweloper zmienia każdego ustawienia, które ma wpływ na trwałość klucza, te zmiany nie zaczną obowiązywać po ponownym wygenerowaniu klucza, za pośrednictwem jawnym wywołaniem `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zachowanie.</span><span class="sxs-lookup"><span data-stu-id="96082-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="96082-111">Dostępne są następujące ustawienia, które wpływają na trwałość klucza:</span><span class="sxs-lookup"><span data-stu-id="96082-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="96082-112">Domyślny okres istnienia klucza</span><span class="sxs-lookup"><span data-stu-id="96082-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="96082-113">Szyfrowanie kluczy podczas mechanizm rest</span><span class="sxs-lookup"><span data-stu-id="96082-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="96082-114">Konsolidatorze informacji zawartych w kluczu</span><span class="sxs-lookup"><span data-stu-id="96082-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="96082-115">Jeśli potrzebujesz tych ustawień, aby uruchamiać starszych niż kluczowi automatyczne stopniowe czasu, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` Aby wymusić utworzenie nowego klucza.</span><span class="sxs-lookup"><span data-stu-id="96082-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="96082-116">Pamiętaj, aby określić datę aktywacji jawne ({teraz + 2 dni} jest regułą aby był czas na której zmiana obejmie) i datę wygaśnięcia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="96082-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="96082-117">Wszystkie aplikacje, dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="96082-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="96082-118">W przeciwnym razie właściwości klucza utrwalonych będzie zależny od określonej aplikacji, która wywołała procedury generowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="96082-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
