---
title: "Ograniczanie okresu istnienia ładunków chronionych"
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak ograniczyć okres istnienia ładunku chronionych przy użyciu platformy ASP.NET Core interfejsy API ochrony danych."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 7909f21057f22e78c03b41464a19a18ce0908216
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="2ba3a-103">Ograniczanie okresu istnienia ładunków chronionych</span><span class="sxs-lookup"><span data-stu-id="2ba3a-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="2ba3a-104">Istnieją scenariusze, w których chce utworzyć ładunku chronionych, która wygaśnie po upływie ustaw czas Deweloper aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="2ba3a-105">Na przykład chronione obciążenie może reprezentować token resetowania hasła, który tylko powinna być prawidłową godzinę.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="2ba3a-106">Pewno można dla deweloperów do tworzenia własnych format ładunku, zawierającą datę wygaśnięcia osadzonych i zaawansowane deweloperzy mogą chcieć mimo to zrobić, ale dla większości deweloperzy zarządzanie tymi wygasanie może zwiększyć nużące.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="2ba3a-107">Aby ułatwić naszym odbiorców developer, pakiet dla [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera narzędzie interfejsów API do tworzenia ładunków, które automatycznie wygasają po okres czasu.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="2ba3a-108">Te interfejsy API wykraczać poza z `ITimeLimitedDataProtector` typu.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="2ba3a-109">Użycie interfejsu API</span><span class="sxs-lookup"><span data-stu-id="2ba3a-109">API usage</span></span>

<span data-ttu-id="2ba3a-110">`ITimeLimitedDataProtector` Interfejs jest interfejsem core do ochrony i wyłączenie ograniczone czasowo / własnym wygasające ładunków.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="2ba3a-111">Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, musisz najpierw wystąpienia zwykły [interfejsu IDataProtector](overview.md) wykonane z określonym przeznaczeniem.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="2ba3a-112">Raz `IDataProtector` wystąpienia jest dostępna, należy wywołać `IDataProtector.ToTimeLimitedDataProtector` — metoda rozszerzenia odzyskać z funkcji ochrony z możliwości wbudowanego wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="2ba3a-113">`ITimeLimitedDataProtector`udostępnia następujące metody powierzchni i rozszerzenia interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="2ba3a-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="2ba3a-114">CreateProtector (ciąg cel): ITimeLimitedDataProtector — ten interfejs API jest podobne do istniejących `IDataProtectionProvider.CreateProtector` w tym może służyć do tworzenia [cel łańcuchów](purpose-strings.md) z funkcji ochrony kluczem głównym ograniczone czasowo.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="2ba3a-115">Ochrona (byte [] w postaci zwykłego tekstu, wygaśnięcia DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="2ba3a-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="2ba3a-116">Ochrona (byte [] w postaci zwykłego tekstu, okres istnienia TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="2ba3a-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="2ba3a-117">Ochrona (byte [] zwykłym tekstem): byte]</span><span class="sxs-lookup"><span data-stu-id="2ba3a-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="2ba3a-118">Ochrona (ciągu zwykłego tekstu, wygaśnięcia DateTimeOffset): ciąg</span><span class="sxs-lookup"><span data-stu-id="2ba3a-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="2ba3a-119">Ochrona (ciąg w postaci zwykłego tekstu, okres istnienia TimeSpan): ciąg</span><span class="sxs-lookup"><span data-stu-id="2ba3a-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="2ba3a-120">Ochrona (ciąg zwykłym tekstem): ciąg</span><span class="sxs-lookup"><span data-stu-id="2ba3a-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="2ba3a-121">Oprócz podstawowe `Protect` metod, które biorą tylko zwykły tekst, istnieją nowe przeciążenia, które umożliwiają określenie daty wygaśnięcia ładunku.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="2ba3a-122">Data wygaśnięcia można określić jako Data bezwzględna (za pośrednictwem `DateTimeOffset`) lub jako względne czasu (z bieżącym systemem czasu za pomocą `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="2ba3a-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="2ba3a-123">Po wywołaniu przepełnienia, które nie przyjmuje wygaśnięcia ładunku jest traktowana jako nigdy nie wygasa.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="2ba3a-124">Wyłącz ochronę (byte [] protectedData, limit wygaśnięcia DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="2ba3a-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="2ba3a-125">Wyłącz ochronę (protectedData byte []): byte]</span><span class="sxs-lookup"><span data-stu-id="2ba3a-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="2ba3a-126">Wyłącz ochronę (limit wygaśnięcia DateTimeOffset protectedData ciągu): ciąg</span><span class="sxs-lookup"><span data-stu-id="2ba3a-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="2ba3a-127">Wyłącz ochronę (ciąg protectedData): ciąg</span><span class="sxs-lookup"><span data-stu-id="2ba3a-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="2ba3a-128">`Unprotect` Metody zwracają oryginalne dane niechronione.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="2ba3a-129">Jeśli jeszcze nie wygasł ładunku, wygaśnięcia bezwzględne jest zwracana jako opcjonalny parametr wraz z danymi niechronionych wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="2ba3a-130">Jeśli wygasł ładunku wszystkie przeciążenia metody Unprotect zgłosi cryptographicexception —.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="2ba3a-131">Nie ma on zalecane jest użycie te interfejsy API ochrony ładunków, które wymaga trwałości długoterminowe lub nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="2ba3a-132">"I pozwolić sobie na chronionych ładunków być trwale nieodwracalny po miesiąca?"</span><span class="sxs-lookup"><span data-stu-id="2ba3a-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="2ba3a-133">może służyć jako regułą; Jeśli odpowiedź jest nie deweloperzy następnie rozważyć alternatywnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="2ba3a-134">Przykładowe poniżej używa [ścieżki kodu nie są Podpisane](../configuration/non-di-scenarios.md) dla wystąpienia systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="2ba3a-135">Do uruchomienia tego przykładu, upewnij się, że najpierw zostały dodane odwołanie do pakietu Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="2ba3a-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
