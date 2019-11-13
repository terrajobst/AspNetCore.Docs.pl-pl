---
title: Wprowadzenie do interfejsów API ochrony danych w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać interfejsów API ochrony danych ASP.NET Core do ochrony i nieochrony danych w aplikacji.
ms.author: riande
ms.date: 11/12/2019
no-loc:
- SignalR
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 8c3f3c7fb21434cf335591c41741f0ce868df33e
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963865"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="2fdd7-103">Wprowadzenie do interfejsów API ochrony danych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fdd7-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="2fdd7-104">W najprostszym zakresie Ochrona danych obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="2fdd7-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="2fdd7-105">Utwórz funkcję ochrony danych z poziomu dostawcy ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="2fdd7-106">Wywołaj metodę `Protect` z danymi, które mają być chronione.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="2fdd7-107">Wywołaj metodę `Unprotect` z danymi, które chcesz wrócić do zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="2fdd7-108">Większość platform i modeli aplikacji, takich jak ASP.NET Core lub SignalR, już konfiguruje system ochrony danych i dodaje go do kontenera usługi, do którego uzyskuje dostęp za pośrednictwem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="2fdd7-109">Poniższy przykład demonstruje skonfigurowanie kontenera usługi pod kątem iniekcji zależności i rejestrowanie stosu ochrony danych, otrzymywanie dostawcy ochrony danych za pośrednictwem programu DI, utworzenie funkcji ochrony i ochronę danych.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="2fdd7-110">Podczas tworzenia funkcji ochrony należy podać co najmniej jeden [ciąg przeznaczenia](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="2fdd7-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="2fdd7-111">Ciąg celu zapewnia izolację między użytkownikami.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="2fdd7-112">Na przykład funkcja ochrony utworzona z przeznaczeniem ciągu "Green" nie może wyłączyć ochrony danych dostarczonych przez funkcję ochrony w celu "purpurowego".</span><span class="sxs-lookup"><span data-stu-id="2fdd7-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="2fdd7-113">Wystąpienia `IDataProtectionProvider` i `IDataProtector` są bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="2fdd7-114">Jest to zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania dla wielu wywołań `Protect` i `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="2fdd7-115">Wywołanie `Unprotect` spowoduje zgłoszenie CryptographicException, jeśli nie można zweryfikować ani deszyfrowanie chronionego ładunku.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="2fdd7-116">Niektóre składniki mogą chcieć zignorować błędy podczas operacji usunięcia ochrony; składnik, który odczytuje pliki cookie uwierzytelniania, może obsłużyć ten błąd i traktować żądanie tak, jakby nie zawierało w ogóle pliku cookie, a nie niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="2fdd7-117">Składniki, które chcą tego zachowania, powinny zwrócić uwagę na CryptographicException zamiast połknięcia wszystkich wyjątków.</span><span class="sxs-lookup"><span data-stu-id="2fdd7-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
