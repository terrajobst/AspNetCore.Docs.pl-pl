---
title: Hasła skrótu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skrócić hasła przy użyciu interfejsów API ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656052"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hasła skrótu w ASP.NET Core

Baza kodu ochrony danych zawiera pakiet *Microsoft. AspNetCore. Cryptography.* Key, który zawiera funkcje wyprowadzania klucza kryptograficznego. Ten pakiet jest składnikiem autonomicznym i nie ma żadnych zależności w pozostałej części systemu ochrony danych. Może być używany całkowicie niezależnie. Źródło istnieje wraz z bazą kodu ochrony danych jako wygoda.

Pakiet oferuje obecnie metodę `KeyDerivation.Pbkdf2`, która umożliwia mieszanie hasła przy użyciu [algorytmu PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobny do istniejącego [typu Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes).NET Framework, ale istnieją trzy ważne rozróżnienia:

1. Metoda `KeyDerivation.Pbkdf2` obsługuje zużywanie wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`i `HMACSHA512`), natomiast typ `Rfc2898DeriveBytes` obsługuje tylko `HMACSHA1`.

2. Metoda `KeyDerivation.Pbkdf2` wykrywa bieżący system operacyjny i próbuje wybrać najbardziej zoptymalizowaną implementację procedury, zapewniając znacznie lepszą wydajność w niektórych przypadkach. (W systemie Windows 8 oferuje około 10X przepływność `Rfc2898DeriveBytes`.)

3. Metoda `KeyDerivation.Pbkdf2` wymaga, aby obiekt wywołujący określił wszystkie parametry (sól, PRF i liczba iteracji). Typ `Rfc2898DeriveBytes` zawiera wartości domyślne dla tych.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Zobacz [kod źródłowy](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) dla typu `PasswordHasher` tożsamości ASP.NET Core dla rzeczywistego przypadku użycia.
