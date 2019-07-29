---
title: Hasła skrótu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skrócić hasła przy użyciu interfejsów API ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602454"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hasła skrótu w ASP.NET Core

Baza kodu ochrony danych zawiera pakiet *Microsoft. AspNetCore. Cryptography.* Key, który zawiera funkcje wyprowadzania klucza kryptograficznego. Ten pakiet jest składnikiem autonomicznym i nie ma żadnych zależności w pozostałej części systemu ochrony danych. Może być używany całkowicie niezależnie. Źródło istnieje wraz z bazą kodu ochrony danych jako wygoda.

Pakiet oferuje obecnie metodę `KeyDerivation.Pbkdf2` , która umożliwia mieszanie hasła przy użyciu [algorytmu PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobny do istniejącego [typu Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes).NET Framework, ale istnieją trzy ważne rozróżnienia:

1. `HMACSHA512` `HMACSHA1` `HMACSHA1` `Rfc2898DeriveBytes` Metoda obsługuje zużywanie wielu PRFs (obecnie, `HMACSHA256`i), podczas gdy typ obsługuje tylko. `KeyDerivation.Pbkdf2`

2. `KeyDerivation.Pbkdf2` Metoda wykrywa bieżący system operacyjny i próbuje wybrać najbardziej zoptymalizowaną implementację procedury, zapewniając znacznie lepszą wydajność w niektórych przypadkach. (W systemie Windows 8 oferuje około 10X przepływności `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Metoda wymaga, aby obiekt wywołujący określił wszystkie parametry (sól, PRF i liczba iteracji). `Rfc2898DeriveBytes` Typ zawiera wartości domyślne dla tych.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Zobacz [kod źródłowy](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) dla `PasswordHasher` typu tożsamości ASP.NET Core dla rzeczywistego przypadku użycia.
