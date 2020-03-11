---
title: Key niezmienności i ustawienia klucza w ASP.NET Core
author: rick-anderson
description: Zapoznaj się ze szczegółami implementacji interfejsów API niezmienności w kluczach ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664718"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="a6ac7-103">Key niezmienności i ustawienia klucza w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6ac7-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="a6ac7-104">Gdy obiekt zostanie utrwalony w magazynie zapasowym, jego reprezentacja jest trwała.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="a6ac7-105">Nowe dane można dodać do magazynu zapasowego, ale nie można zmodyfikować istniejących danych.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="a6ac7-106">Głównym celem tego zachowania jest uniknięcie uszkodzenia danych.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="a6ac7-107">Jedną z konsekwencji tego zachowania jest to, że po zapisaniu klucza do magazynu zapasowego jest on niezmienny.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="a6ac7-108">Nie można zmienić daty jego utworzenia, aktywacji i wygaśnięcia, chociaż można ją odwołać przy użyciu `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="a6ac7-109">Ponadto zmienne podstawowe informacje o algorytmach, główny materiał do obsługi kluczy i szyfrowanie we właściwościach REST są również niemodyfikowalne.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="a6ac7-110">Jeśli deweloper zmieni wszystkie ustawienia, które mają wpływ na trwałość klucza, te zmiany nie będą obowiązywać do momentu następnego wygenerowania klucza — za pomocą jawnego wywołania do `IKeyManager.CreateNewKey` lub przy użyciu własnego [automatycznego generowania klucza](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) w systemie ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="a6ac7-111">Ustawienia wpływające na trwałość klucza są następujące:</span><span class="sxs-lookup"><span data-stu-id="a6ac7-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="a6ac7-112">Domyślny okres istnienia klucza</span><span class="sxs-lookup"><span data-stu-id="a6ac7-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="a6ac7-113">Szyfrowanie klucza w mechanizmie REST</span><span class="sxs-lookup"><span data-stu-id="a6ac7-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

* [<span data-ttu-id="a6ac7-114">Informacje o algorytmach zawarte w kluczu</span><span class="sxs-lookup"><span data-stu-id="a6ac7-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="a6ac7-115">Jeśli te ustawienia są potrzebne, aby uruchomić je wcześniej niż w następnym automatycznym czasie wycofywania, rozważ jawne wywołanie `IKeyManager.CreateNewKey`, aby wymusić utworzenie nowego klucza.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="a6ac7-116">Pamiętaj, aby podać jawną datę aktywacji ({Now + 2 dni} to lepsza reguła przycisku przewijania, która pozwala na przekroczenie czasu zmiany) i datę wygaśnięcia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="a6ac7-117">Wszystkie aplikacje dotykające repozytorium powinny określać te same ustawienia przy użyciu metod rozszerzenia `IDataProtectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="a6ac7-118">W przeciwnym razie właściwości trwałego klucza będą zależne od konkretnej aplikacji, która wywołała procedury generowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="a6ac7-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
