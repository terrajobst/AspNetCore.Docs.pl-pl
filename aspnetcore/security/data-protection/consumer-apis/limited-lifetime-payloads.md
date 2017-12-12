---
title: "Ograniczanie okresu istnienia ładunków chronionych"
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak ograniczyć okres istnienia ładunku chronionych przy użyciu platformy ASP.NET Core interfejsy API ochrony danych."
keywords: "Platformy ASP.NET Core, ochrony danych, okres istnienia ładunku"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="c2d5e-104">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="c2d5e-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="c2d5e-105">Istnieją scenariusze, w których chce utworzyć ładunku chronionych, która wygaśnie po upływie ustaw czas Deweloper aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="c2d5e-106">Na przykład chronione obciążenie może reprezentować token resetowania hasła, który tylko powinna być prawidłową godzinę.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="c2d5e-107">Pewno można dla deweloperów do tworzenia własnych format ładunku, zawierającą datę wygaśnięcia osadzonych i zaawansowane deweloperzy mogą chcieć mimo to zrobić, ale dla większości deweloperzy zarządzanie tymi wygasanie może zwiększyć nużące.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="c2d5e-108">Aby ułatwić naszym odbiorców developer, pakiet dla [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera narzędzie interfejsów API do tworzenia ładunków, które automatycznie wygasają po okres czasu.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="c2d5e-109">Te interfejsy API wykraczać poza z `ITimeLimitedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="c2d5e-110">Użycie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c2d5e-110">API usage</span></span>

<span data-ttu-id="c2d5e-111">`ITimeLimitedDataProtector` Interfejs jest interfejsem core do ochrony i wyłączenie ograniczone czasowo / własnym wygasające ładunków.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="c2d5e-112">Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, musisz najpierw wystąpienia zwykły [interfejsu IDataProtector](overview.md) wykonane z określonym przeznaczeniem.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="c2d5e-113">Raz `IDataProtector` wystąpienia jest dostępna, należy wywołać `IDataProtector.ToTimeLimitedDataProtector` — metoda rozszerzenia odzyskać z funkcji ochrony z możliwości wbudowanego wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="c2d5e-114">`ITimeLimitedDataProtector`udostępnia następujące metody powierzchni i rozszerzenia interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="c2d5e-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="c2d5e-115">CreateProtector (ciąg cel): ITimeLimitedDataProtector — ten interfejs API jest podobne do istniejących `IDataProtectionProvider.CreateProtector` w tym może służyć do tworzenia [cel łańcuchów](purpose-strings.md) z funkcji ochrony kluczem głównym ograniczone czasowo.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="c2d5e-116">Ochrona (byte [] w postaci zwykłego tekstu, wygaśnięcia DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="c2d5e-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="c2d5e-117">Ochrona (byte [] w postaci zwykłego tekstu, okres istnienia TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="c2d5e-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="c2d5e-118">Ochrona (byte [] zwykłym tekstem): byte]</span><span class="sxs-lookup"><span data-stu-id="c2d5e-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="c2d5e-119">Ochrona (ciągu zwykłego tekstu, wygaśnięcia DateTimeOffset): ciąg</span><span class="sxs-lookup"><span data-stu-id="c2d5e-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="c2d5e-120">Ochrona (ciąg w postaci zwykłego tekstu, okres istnienia TimeSpan): ciąg</span><span class="sxs-lookup"><span data-stu-id="c2d5e-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="c2d5e-121">Ochrona (ciąg zwykłym tekstem): ciąg</span><span class="sxs-lookup"><span data-stu-id="c2d5e-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="c2d5e-122">Oprócz podstawowe `Protect` metod, które biorą tylko zwykły tekst, istnieją nowe przeciążenia, które umożliwiają określenie daty wygaśnięcia ładunku.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="c2d5e-123">Data wygaśnięcia można określić jako Data bezwzględna (za pośrednictwem `DateTimeOffset`) lub jako względne czasu (z bieżącym systemem czasu za pomocą `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="c2d5e-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="c2d5e-124">Po wywołaniu przepełnienia, które nie przyjmuje wygaśnięcia ładunku jest traktowana jako nigdy nie wygasa.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="c2d5e-125">Wyłącz ochronę (byte [] protectedData, limit wygaśnięcia DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="c2d5e-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="c2d5e-126">Wyłącz ochronę (protectedData byte []): byte]</span><span class="sxs-lookup"><span data-stu-id="c2d5e-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="c2d5e-127">Wyłącz ochronę (limit wygaśnięcia DateTimeOffset protectedData ciągu): ciąg</span><span class="sxs-lookup"><span data-stu-id="c2d5e-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="c2d5e-128">Wyłącz ochronę (ciąg protectedData): ciąg</span><span class="sxs-lookup"><span data-stu-id="c2d5e-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="c2d5e-129">`Unprotect` Metody zwracają oryginalne dane niechronione.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="c2d5e-130">Jeśli jeszcze nie wygasł ładunku, wygaśnięcia bezwzględne jest zwracana jako opcjonalny parametr wraz z danymi niechronionych wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="c2d5e-131">Jeśli wygasł ładunku wszystkie przeciążenia metody Unprotect zgłosi cryptographicexception —.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="c2d5e-132">Nie zaleca się przy użyciu tych interfejsów API ochrony ładunków, które wymaga trwałości długoterminowe lub nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="c2d5e-133">"I pozwolić sobie na chronionych ładunków być trwale nieodwracalny po miesiąca?"</span><span class="sxs-lookup"><span data-stu-id="c2d5e-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="c2d5e-134">może służyć jako regułą; Jeśli odpowiedź jest nie deweloperzy następnie rozważyć alternatywnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="c2d5e-135">Przykładowe poniżej używa [ścieżki kodu nie są Podpisane](../configuration/non-di-scenarios.md) dla wystąpienia systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="c2d5e-136">Do uruchomienia tego przykładu, upewnij się, że najpierw zostały dodane odwołanie do pakietu Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="c2d5e-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
