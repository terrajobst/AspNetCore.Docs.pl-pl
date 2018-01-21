---
title: "Obsługuje zasady komputera ochrony danych w ASP.NET Core"
author: rick-anderson
description: "Więcej informacji na temat obsługują ustawiania domyślnych zasad komputera dla wszystkich aplikacji używających platformy ASP.NET Core do ochrony danych."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 4c3ae3b628ebe17c7926c71f1fad664d719d1706
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Obsługuje zasady komputera ochrony danych w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Podczas uruchamiania w systemie Windows, system ochrony danych ma ograniczoną obsługę ustawienie domyślne zasady komputera dla wszystkich aplikacji używających platformy ASP.NET Core Data Protection. Ogólne informacje o tym jest administrator może chcieć zmienić ustawienie domyślne, takie jak używane algorytmy lub okres istnienia klucza, bez konieczności ręcznej aktualizacji każdej aplikacji na maszynie.

> [!WARNING]
> Administrator systemu może ustawić domyślną zasadę, ale nie mogą jej wymusić. Deweloper aplikacji zawsze można zmienić wartości z swój wybór. Domyślna zasada dotyczy tylko aplikacji, gdzie dewelopera nie określono jawnej wartości dla ustawień.

## <a name="setting-default-policy"></a>Ustawienie domyślne zasady

Aby ustawić domyślną zasadę, administrator może ustawić znane wartości w rejestrze systemu w następującym kluczu rejestru:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Jeśli komputer znajduje się w 64-bitowym systemie operacyjnym i mają wpływ na działanie aplikacji 32-bitowych, pamiętaj, aby skonfigurować odpowiednikiem Wow6432Node klucz powyżej.

Poniżej przedstawiono obsługiwane wartości.

| Wartość              | Typ   | Opis |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Określa, jakie algorytmy powinny być używane do ochrony danych. Wartość musi być CNG CBC, CNG GCM lub zarządzane i jest opisany bardziej szczegółowo poniżej. |
| DefaultKeyLifetime | DWORD  | Określa okres istnienia dla nowo wygenerować kluczy. Wartość jest określona w dni i musi być > = 7. |
| KeyEscrowSinks     | string | Określa typy, które są używane do depozytu klucza. Wartość jest rozdzielana średnikami lista wychwytywanie kluczy depozytu, gdzie każdy element na liście jest nazwa kwalifikowana zestawu typu, który implementuje [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Typy szyfrowania

Jeśli EncryptionType CNG CBC, system jest skonfigurowany do używania szyfrowania symetrycznego bloku trybie CBC poufności oraz HMAC autentyczności z usług świadczonych przez CNG systemu Windows (zobacz [Określanie niestandardowych algorytmów Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) dla więcej szczegółów). Obsługiwane są następujące dodatkowe wartości, z których każdy odpowiada właściwości w typie CngCbcAuthenticatedEncryptionSettings.

| Wartość                       | Typ   | Opis |
| --------------------------- | :----: | ----------- |
| Algorytm szyfrowania         | string | Nazwa algorytmu szyfrowania symetrycznego bloku zrozumiałe CNG. Ten algorytm jest otwarty w trybie CBC. |
| EncryptionAlgorithmProvider | string | Nazwa implementacji dostawcy CNG, które mogą wytwarzać algorytmu EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Długość (w bitach) klucza pochodzić dla algorytmu szyfrowania symetrycznego bloku. |
| HashAlgorithm               | string | Nazwa algorytmu wyznaczania wartości skrótu, zrozumiałe CNG. Ten algorytm jest otwarty w trybie HMAC. |
| HashAlgorithmProvider       | string | Nazwa implementacja dostawcy CNG, które mogą wytwarzać algorytm algorytm skrótu. |

Jeśli EncryptionType CNG GCM, system został skonfigurowany do użycia szyfrowania symetrycznego bloku liczników Galois trybu poufności oraz autentyczności z usług świadczonych przez CNG systemu Windows (zobacz [Określanie niestandardowych algorytmów Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Aby uzyskać więcej informacji). Obsługiwane są następujące dodatkowe wartości, z których każdy odpowiada właściwości w typie CngGcmAuthenticatedEncryptionSettings.

| Wartość                       | Typ   | Opis |
| --------------------------- | :----: | ----------- |
| Algorytm szyfrowania         | string | Nazwa algorytmu szyfrowania symetrycznego bloku zrozumiałe CNG. Ten algorytm jest otwarty w trybie Galois liczników. |
| EncryptionAlgorithmProvider | string | Nazwa implementacji dostawcy CNG, które mogą wytwarzać algorytmu EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Długość (w bitach) klucza pochodzić dla algorytmu szyfrowania symetrycznego bloku. |

Jeśli EncryptionType jest zarządzany, system jest skonfigurowany do używania zarządzanych SymmetricAlgorithm poufności oraz KeyedHashAlgorithm autentyczności (zobacz [Określanie niestandardowych zarządzane algorytmów](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) więcej szczegółów). Obsługiwane są następujące dodatkowe wartości, z których każdy odpowiada właściwości w typie ManagedAuthenticatedEncryptionSettings.

| Wartość                      | Typ   | Opis |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | Nazwa kwalifikowana zestawu typu, który implementuje SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | Długość (w bitach) klucza pochodzić dla algorytmu szyfrowania symetrycznego. |
| ValidationAlgorithmType    | string | Nazwa kwalifikowana zestawu typu, który implementuje KeyedHashAlgorithm. |

Jeśli EncryptionType ma inne wartości innych niż null lub jest pusty, system ochrony danych zgłasza wyjątek podczas uruchamiania.

> [!WARNING]
> Podczas konfigurowania domyślne ustawienie zasad, które wymaga nazwy typu (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), typy muszą być dostępne dla aplikacji. Oznacza to, że dla aplikacji działających na Desktop CLR, zestawy, które zawierają te typy powinien znajdować się w globalnej pamięci podręcznej zestawów (GAC). Platformy ASP.NET Core aplikacji działających na [.NET Core](https://www.microsoft.com/net/core), należy zainstalować pakiety, które zawierają te typy.
