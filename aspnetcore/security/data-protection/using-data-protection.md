---
title: Rozpoczynanie pracy z interfejsami API ochrony danych
author: rick-anderson
description: "Tym dokumencie opisano sposób użycia interfejsy API ochrony danych platformy ASP.NET Core dla ochrony i wyłączenie ochrony danych w aplikacji."
keywords: Platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="7c8bd-104">Rozpoczynanie pracy z interfejsami API ochrony danych</span><span class="sxs-lookup"><span data-stu-id="7c8bd-104">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="7c8bd-105">W najprostszy, ochrona danych obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="7c8bd-105">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="7c8bd-106">Utwórz funkcji ochrony danych na podstawie dostawcę ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-106">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="7c8bd-107">Wywołanie `Protect` metody z danymi, które chcesz chronić.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-107">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="7c8bd-108">Wywołanie `Unprotect` metody z danymi chcesz włączyć ponownie w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-108">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="7c8bd-109">Większość platform i modeli aplikacji, takich jak ASP.NET lub SignalR, już konfiguracji systemu ochrony danych i dodaj go do kontenera usług, których dostęp za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-109">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="7c8bd-110">W poniższym przykładzie pokazano Konfigurowanie usługi kontenera iniekcji zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem Podpisane, tworzenie ochrony i ochrona, a następnie wyłączanie ochrony danych</span><span class="sxs-lookup"><span data-stu-id="7c8bd-110">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="7c8bd-111">Podczas tworzenia ochrony należy podać co najmniej jeden [ciągów w celu](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="7c8bd-111">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="7c8bd-112">Ciąg przeznaczenia zapewnia izolację między konsumentów.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-112">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="7c8bd-113">Na przykład utworzone za pomocą ciąg "green" cel ochrony nie będzie mógł wyłączyć ochronę danych dostarczonych przez funkcję ochrony z celem "purpurowy".</span><span class="sxs-lookup"><span data-stu-id="7c8bd-113">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="7c8bd-114">Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-114">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="7c8bd-115">Jest zamierzone, który po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-115">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="7c8bd-116">Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-116">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="7c8bd-117">Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-117">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="7c8bd-118">Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.</span><span class="sxs-lookup"><span data-stu-id="7c8bd-118">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
