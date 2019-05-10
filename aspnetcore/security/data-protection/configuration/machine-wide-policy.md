---
title: Zasady komputera ochrony danych obsługi w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat pomocy technicznej do ustawiania domyślnych zasad komputera dla wszystkich aplikacji korzystających z ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902980"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Zasady komputera ochrony danych obsługi w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Gdy w systemie Windows, system ochrony danych ma ograniczoną obsługę ustawienie domyślnych zasad komputera dla wszystkich aplikacji korzystających z ochrony danych programu ASP.NET Core. Ogólne chodzi o to, że administrator może chcieć zmienić ustawienie domyślne, takie jak używane algorytmy lub okres istnienia klucza, bez konieczności ręcznie zaktualizować wszystkie aplikacje na maszynie.

> [!WARNING]
> Administrator systemu może ustawić domyślną zasadę, ale nie mogą jej wymusić. Deweloper aplikacji zawsze można zmienić dowolną wartość z swój wybór. Domyślne zasady dotyczy tylko aplikacji, w których deweloper nie zostało określone jawną wartość dla ustawienia.

## <a name="setting-default-policy"></a>Ustawienie domyślne zasady

Aby ustawić domyślną zasadę, administrator może ustawić znane wartości w rejestrze systemowym w następującym kluczu rejestru:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Jeśli jesteś na 64-bitowym systemie operacyjnym i chcesz mieć wpływ na zachowanie aplikacji 32-bitowych, pamiętaj, aby skonfigurować Wow6432Node wielokrotność powyżej klucza.

Obsługiwane wartości zostały wymienione poniżej.

| Wartość              | Typ   | Opis |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Określa, jakie algorytmy powinna być używana do ochrony danych. Wartość musi być CNG CBC, CNG GCM lub zarządzane i jest opisany bardziej szczegółowo poniżej. |
| DefaultKeyLifetime | DWORD  | Określa okres istnienia dla nowo wygenerowane klucze. Wartość określona w dniach, musi być > = 7. |
| KeyEscrowSinks     | string | Określa typy, które są używane na potrzeby kluczy depozytu. Wartość jest rozdzieloną średnikami listę kluczy depozytu ujścia, gdzie każdy element na liście jest nazwą kwalifikowaną dla zestawu typu, który implementuje [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Typy szyfrowania

Jeśli EncryptionType CNG CBC, system jest skonfigurowany za pomocą szyfrowania symetrycznego bloku trybie CBC poufności oraz HMAC autentyczności usługi świadczone przez Windows CNG (zobacz [określania niestandardowych algorytmy Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) dla więcej szczegółów). Obsługiwane są następujące dodatkowe wartości, z których każdy odnosi się do właściwości w typie CngCbcAuthenticatedEncryptionSettings.

| Wartość                       | Typ   | Opis |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Nazwa algorytmu szyfrowania symetrycznego bloku zrozumiałe CNG. Ten algorytm jest otwarty w trybie CBC. |
| EncryptionAlgorithmProvider | string | Nazwa implementacja dostawcy CNG, które mogą wytwarzać algorytm usuwają elementy EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Długość (w bitach) klucz do wyprowadzenia dla algorytmu szyfrowania symetrycznego bloku. |
| Algorytm               | string | Nazwa algorytmu wyznaczania wartości skrótu, zrozumiałym CNG. Ten algorytm jest otwarty w trybie HMAC. |
| HashAlgorithmProvider       | string | Nazwa implementacja dostawcy CNG, które mogą wytwarzać algorytm algorytm. |

Jeśli EncryptionType CNG GCM, system jest skonfigurowany za pomocą szyfrowania symetrycznego bloku liczników Galois trybu poufności oraz autentyczności usługi świadczone przez Windows CNG (zobacz [określania niestandardowych algorytmy Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Aby uzyskać więcej informacji). Obsługiwane są następujące dodatkowe wartości, z których każdy odnosi się do właściwości w typie CngGcmAuthenticatedEncryptionSettings.

| Wartość                       | Typ   | Opis |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Nazwa algorytmu szyfrowania symetrycznego bloku zrozumiałe CNG. Ten algorytm jest otwarty w trybie Galois liczników. |
| EncryptionAlgorithmProvider | string | Nazwa implementacja dostawcy CNG, które mogą wytwarzać algorytm usuwają elementy EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | Długość (w bitach) klucz do wyprowadzenia dla algorytmu szyfrowania symetrycznego bloku. |

Jeśli EncryptionType jest zarządzany, system został skonfigurowany na potrzeby zarządzanych SymmetricAlgorithm poufności i KeyedHashAlgorithm autentyczności (zobacz [Określanie niestandardowego zarządzane algorytmy](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) Aby uzyskać więcej informacji). Obsługiwane są następujące dodatkowe wartości, z których każdy odnosi się do właściwości w typie ManagedAuthenticatedEncryptionSettings.

| Wartość                      | Typ   | Opis |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | Nazwa kwalifikowanego dla zestawu typu, który implementuje SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | Długość (w bitach) klucz do wyprowadzenia dla algorytmu szyfrowania symetrycznego. |
| ValidationAlgorithmType    | string | Nazwa kwalifikowanego dla zestawu typu, który implementuje KeyedHashAlgorithm. |

Jeśli EncryptionType ma inne wartości innych niż null lub jest pusta, system ochrony danych zgłasza wyjątek podczas uruchamiania.

> [!WARNING]
> Podczas konfigurowania domyślne ustawienie zasad, która obejmuje nazwy typów (EncryptionAlgorithmType ValidationAlgorithmType, KeyEscrowSinks), typy muszą być dostępne dla aplikacji. Oznacza to, że dla aplikacji działających w Desktop CLR, zestawy, które zawierają te typy powinien być obecny w globalnej pamięci podręcznej zestawów (GAC). W przypadku aplikacji platformy ASP.NET Core uruchomiony na platformie .NET Core należy zainstalować pakiety, które zawierają te typy.
