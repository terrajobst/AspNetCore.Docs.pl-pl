---
title: Wprowadzenie do interfejsów API ochrony danych w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać platformy ASP.NET Core interfejsy API ochrony danych do ochrony i wyłączenie ochrony danych w aplikacji.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087635"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="c5757-103">Wprowadzenie do interfejsów API ochrony danych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5757-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="c5757-104">W jego najprostszy i ochronę danych składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c5757-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="c5757-105">Utwórz funkcję ochrony danych na podstawie dostawca ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c5757-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="c5757-106">Wywołaj `Protect` metody z danymi, które chcesz chronić.</span><span class="sxs-lookup"><span data-stu-id="c5757-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="c5757-107">Wywołaj `Unprotect` metody przy użyciu danych chcesz przekształcić zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="c5757-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="c5757-108">Większość struktur i modele aplikacji, takich jak ASP.NET Core lub SignalR, już skonfigurować system ochrony danych i dodać go do kontenera usługi, których dostęp przy użyciu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="c5757-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="c5757-109">W poniższym przykładzie pokazano konfigurowania usługi kontenera do wstrzykiwania zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem DI, tworzenia danych ochronę, a następnie nie jest chroniony i ochrony.</span><span class="sxs-lookup"><span data-stu-id="c5757-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="c5757-110">Po utworzeniu funkcję ochrony należy podać co najmniej jeden [ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="c5757-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="c5757-111">Ciąg przeznaczenia zapewnia izolację między konsumentów.</span><span class="sxs-lookup"><span data-stu-id="c5757-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="c5757-112">Na przykład funkcję ochrony utworzonych za pomocą ciągu cel "green" nie można wyłączyć ochronę danych udostępnionych przez funkcję ochrony z celem "purpurowy".</span><span class="sxs-lookup"><span data-stu-id="c5757-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="c5757-113">Wystąpienia elementu `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="c5757-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="c5757-114">Ma zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` poprzez wywołanie `CreateProtector`, użyje tego odwołania dla wielu wywołań `Protect` i `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="c5757-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="c5757-115">Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane.</span><span class="sxs-lookup"><span data-stu-id="c5757-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="c5757-116">Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który odczytuje pliki cookie uwierzytelniania może obsługiwać ten błąd i traktować żądania tak, jakby był w ogóle pliki cookie nie zamiast zwraca Niepowodzenie żądania od razu wykupić.</span><span class="sxs-lookup"><span data-stu-id="c5757-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="c5757-117">Składniki, które chcesz tego zachowania, należy w szczególności złapać cryptographicexception — zamiast powodu spożycia wszystkie wyjątki.</span><span class="sxs-lookup"><span data-stu-id="c5757-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
