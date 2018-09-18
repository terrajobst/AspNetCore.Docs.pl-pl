---
title: Wyznaczanie skrótów haseł w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak i wyznaczania wartości skrótu hasła, przy użyciu interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010965"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="37454-103">Wyznaczanie skrótów haseł w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37454-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="37454-104">Podstawowy kod ochrony danych zawiera pakiet *Microsoft.AspNetCore.Cryptography.KeyDerivation* zawierający funkcje wyprowadzania klucza kryptograficznego.</span><span class="sxs-lookup"><span data-stu-id="37454-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="37454-105">Ten pakiet jest składnikiem autonomiczne i nie ma zależności dla reszty systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="37454-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="37454-106">Może służyć całkowicie niezależne.</span><span class="sxs-lookup"><span data-stu-id="37454-106">It can be used completely independently.</span></span> <span data-ttu-id="37454-107">Źródło istnieje wraz z kodem ochrony danych podstawowych dla wygody.</span><span class="sxs-lookup"><span data-stu-id="37454-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="37454-108">Pakiet oferuje obecnie metody `KeyDerivation.Pbkdf2` pozwalający wyznaczania wartości skrótu hasła przy użyciu [algorytm PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="37454-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="37454-109">Ten interfejs API jest bardzo podobny do istniejącego środowiska .NET Framework [typu Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale istnieją trzy ważne różnice:</span><span class="sxs-lookup"><span data-stu-id="37454-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="37454-110">`KeyDerivation.Pbkdf2` Metoda obsługuje korzystanie z wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`, i `HMACSHA512`), podczas gdy `Rfc2898DeriveBytes` obsługuje tylko typ `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="37454-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="37454-111">`KeyDerivation.Pbkdf2` Metoda wykrywa bieżący system operacyjny i próbuje wybierz najbardziej zoptymalizowane wdrożenia standardowego, zapewniając znacznie lepszej wydajności w niektórych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="37454-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="37454-112">(W systemie Windows 8 oferuje około 10 razy przepływności `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="37454-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="37454-113">`KeyDerivation.Pbkdf2` Wymaga, aby obiekt wywołujący, aby określić wszystkie parametry (soli, PRF i liczba iteracji).</span><span class="sxs-lookup"><span data-stu-id="37454-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="37454-114">`Rfc2898DeriveBytes` Typu zawiera wartości domyślne dla tych.</span><span class="sxs-lookup"><span data-stu-id="37454-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="37454-115">Zobacz [kod źródłowy](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) dla produktu ASP.NET Core Identity `PasswordHasher` przypadek użycia typu dla rzeczywistych warunkach.</span><span class="sxs-lookup"><span data-stu-id="37454-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
