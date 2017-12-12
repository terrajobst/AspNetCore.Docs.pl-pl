---
title: "Tworzenia skrótu hasła"
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak skrótu hasła przy użyciu platformy ASP.NET Core interfejsy API ochrony danych."
keywords: "Platformy ASP.NET Core, ochrony danych, skrót hasła"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: da9f505f58f18f7ab3b93753bae079eb976b3939
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="password-hashing"></a>Tworzenia skrótu hasła

Bazy danych ochrony kodu zawiera pakiet *Microsoft.AspNetCore.Cryptography.KeyDerivation* zawierającą funkcje kryptograficzne klucza pochodnego. Ten pakiet jest składnikiem autonomiczny, nie ma żadnych zależności w pozostałej części systemu ochrony danych. Można całkowicie niezależnie. Źródło istnieje równolegle z podstawowej jako udogodnienie kod ochrony danych.

Pakiet zawiera obecnie metody `KeyDerivation.Pbkdf2` umożliwiającą tworzenie skrótu hasła przy użyciu [algorytm PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobne do istniejących .NET Framework [Rfc2898DeriveBytes typu](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale istnieją trzy ważne różnice:

1. `KeyDerivation.Pbkdf2` Metoda obsługuje korzystanie z wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`, i `HMACSHA512`), podczas gdy `Rfc2898DeriveBytes` obsługuje tylko typ `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Metody wykrywa bieżący system operacyjny i próbuje wybierz najbardziej zoptymalizowanego wdrożenia standardowego, zapewniając dużo wyższą wydajność w pewnych przypadkach. (W systemie Windows 8 zapewnia około 10 x przepływność `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Metoda wymaga obiekt wywołujący, aby określić wszystkie parametry (soli, PRF i liczby iteracji). `Rfc2898DeriveBytes` Typu zapewnia te wartości domyślne.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

Zobacz kod źródłowy dla tożsamości ASP.NET Core `PasswordHasher` typ, do rzeczywistych przypadek użycia.
