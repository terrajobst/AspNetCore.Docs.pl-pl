---
title: Wyznaczanie skrótów haseł w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak i wyznaczania wartości skrótu hasła, przy użyciu interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902671"
---
# <a name="hash-passwords-in-aspnet-core"></a>Wyznaczanie skrótów haseł w programie ASP.NET Core

Podstawowy kod ochrony danych zawiera pakiet *Microsoft.AspNetCore.Cryptography.KeyDerivation* zawierający funkcje wyprowadzania klucza kryptograficznego. Ten pakiet jest składnikiem autonomiczne i nie ma zależności dla reszty systemu ochrony danych. Może służyć całkowicie niezależne. Źródło istnieje wraz z kodem ochrony danych podstawowych dla wygody.

Pakiet oferuje obecnie metody `KeyDerivation.Pbkdf2` pozwalający wyznaczania wartości skrótu hasła przy użyciu [algorytm PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobny do istniejącego środowiska .NET Framework [typu Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale istnieją trzy ważne różnice:

1. `KeyDerivation.Pbkdf2` Metoda obsługuje korzystanie z wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`, i `HMACSHA512`), podczas gdy `Rfc2898DeriveBytes` obsługuje tylko typ `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Metoda wykrywa bieżący system operacyjny i próbuje wybierz najbardziej zoptymalizowane wdrożenia standardowego, zapewniając znacznie lepszej wydajności w niektórych przypadkach. (W systemie Windows 8 oferuje około 10 razy przepływności `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Wymaga, aby obiekt wywołujący, aby określić wszystkie parametry (soli, PRF i liczba iteracji). `Rfc2898DeriveBytes` Typu zawiera wartości domyślne dla tych.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Zobacz [kod źródłowy](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) dla produktu ASP.NET Core Identity `PasswordHasher` przypadek użycia typu dla rzeczywistych warunkach.
