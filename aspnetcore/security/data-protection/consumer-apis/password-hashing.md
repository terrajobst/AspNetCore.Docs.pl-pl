---
title: Skrót hasła w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skrótów haseł przy użyciu interfejsów API platformy ASP.NET Core danych ochrony.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: f44e66789bf348ef6d99f6d862fb34c2d943a0b2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="c9d4b-103">Skrót hasła w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9d4b-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="c9d4b-104">Bazy danych ochrony kodu zawiera pakiet *Microsoft.AspNetCore.Cryptography.KeyDerivation* zawierającą funkcje kryptograficzne klucza pochodnego.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="c9d4b-105">Ten pakiet jest składnikiem autonomiczny, nie ma żadnych zależności w pozostałej części systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="c9d4b-106">Można całkowicie niezależnie.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-106">It can be used completely independently.</span></span> <span data-ttu-id="c9d4b-107">Źródło istnieje równolegle z podstawowej jako udogodnienie kod ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="c9d4b-108">Pakiet zawiera obecnie metody `KeyDerivation.Pbkdf2` umożliwiającą tworzenie skrótu hasła przy użyciu [algorytm PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="c9d4b-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="c9d4b-109">Ten interfejs API jest bardzo podobne do istniejących .NET Framework [Rfc2898DeriveBytes typu](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale istnieją trzy ważne różnice:</span><span class="sxs-lookup"><span data-stu-id="c9d4b-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="c9d4b-110">`KeyDerivation.Pbkdf2` Metoda obsługuje korzystanie z wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`, i `HMACSHA512`), podczas gdy `Rfc2898DeriveBytes` obsługuje tylko typ `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="c9d4b-111">`KeyDerivation.Pbkdf2` Metody wykrywa bieżący system operacyjny i próbuje wybierz najbardziej zoptymalizowanego wdrożenia standardowego, zapewniając dużo wyższą wydajność w pewnych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="c9d4b-112">(W systemie Windows 8 zapewnia około 10 x przepływność `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="c9d4b-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="c9d4b-113">`KeyDerivation.Pbkdf2` Metoda wymaga obiekt wywołujący, aby określić wszystkie parametry (soli, PRF i liczby iteracji).</span><span class="sxs-lookup"><span data-stu-id="c9d4b-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="c9d4b-114">`Rfc2898DeriveBytes` Typu zapewnia te wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="c9d4b-115">Zobacz kod źródłowy dla tożsamości ASP.NET Core `PasswordHasher` typ, do rzeczywistych przypadek użycia.</span><span class="sxs-lookup"><span data-stu-id="c9d4b-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
