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
ms.openlocfilehash: 63348da144e84d614f274b5d816cbecb020dcab4
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="hash-passwords-in-aspnet-core"></a>Skrót hasła w ASP.NET Core

Bazy danych ochrony kodu zawiera pakiet *Microsoft.AspNetCore.Cryptography.KeyDerivation* zawierającą funkcje kryptograficzne klucza pochodnego. Ten pakiet jest składnikiem autonomiczny, nie ma żadnych zależności w pozostałej części systemu ochrony danych. Można całkowicie niezależnie. Źródło istnieje równolegle z podstawowej jako udogodnienie kod ochrony danych.

Pakiet zawiera obecnie metody `KeyDerivation.Pbkdf2` umożliwiającą tworzenie skrótu hasła przy użyciu [algorytm PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobne do istniejących .NET Framework [Rfc2898DeriveBytes typu](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ale istnieją trzy ważne różnice:

1. `KeyDerivation.Pbkdf2` Metoda obsługuje korzystanie z wielu PRFs (obecnie `HMACSHA1`, `HMACSHA256`, i `HMACSHA512`), podczas gdy `Rfc2898DeriveBytes` obsługuje tylko typ `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Metody wykrywa bieżący system operacyjny i próbuje wybierz najbardziej zoptymalizowanego wdrożenia standardowego, zapewniając dużo wyższą wydajność w pewnych przypadkach. (W systemie Windows 8 zapewnia około 10 x przepływność `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Metoda wymaga obiekt wywołujący, aby określić wszystkie parametry (soli, PRF i liczby iteracji). `Rfc2898DeriveBytes` Typu zapewnia te wartości domyślne.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Zobacz kod źródłowy dla tożsamości ASP.NET Core `PasswordHasher` typ, do rzeczywistych przypadek użycia.
