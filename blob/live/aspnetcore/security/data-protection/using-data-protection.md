---
title: Rozpoczynanie pracy z interfejsami API ochrony danych
author: rick-anderson
description: "Tym dokumencie opisano sposób użycia interfejsy API ochrony danych platformy ASP.NET Core dla ochrony i wyłączenie ochrony danych w aplikacji."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 54976a7f2ac13fe445eb2eea204f4f781813030f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="48cd2-103">Rozpoczynanie pracy z interfejsami API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="48cd2-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="48cd2-104">W najprostszy, ochrona danych obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="48cd2-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="48cd2-105">Utwórz funkcji ochrony danych na podstawie dostawcę ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="48cd2-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="48cd2-106">Wywołanie `Protect` metody z danymi, które chcesz chronić.</span><span class="sxs-lookup"><span data-stu-id="48cd2-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="48cd2-107">Wywołanie `Unprotect` metody z danymi chcesz włączyć ponownie w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="48cd2-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="48cd2-108">Większość platform i modeli aplikacji, takich jak ASP.NET lub SignalR, już konfiguracji systemu ochrony danych i dodaj go do kontenera usług, których dostęp za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="48cd2-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="48cd2-109">W poniższym przykładzie pokazano Konfigurowanie usługi kontenera iniekcji zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem Podpisane, tworzenie ochrony i ochrona, a następnie wyłączanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="48cd2-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="48cd2-110">Podczas tworzenia ochrony należy podać co najmniej jeden [ciągów w celu](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="48cd2-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="48cd2-111">Ciąg przeznaczenia zapewnia izolację między konsumentów.</span><span class="sxs-lookup"><span data-stu-id="48cd2-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="48cd2-112">Na przykład utworzone za pomocą ciąg "green" cel ochrony nie będzie mógł wyłączyć ochronę danych dostarczonych przez funkcję ochrony z celem "purpurowy".</span><span class="sxs-lookup"><span data-stu-id="48cd2-112">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="48cd2-113">Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="48cd2-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="48cd2-114">Jest zamierzone, który po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="48cd2-114">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="48cd2-115">Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane.</span><span class="sxs-lookup"><span data-stu-id="48cd2-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="48cd2-116">Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich.</span><span class="sxs-lookup"><span data-stu-id="48cd2-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="48cd2-117">Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.</span><span class="sxs-lookup"><span data-stu-id="48cd2-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
