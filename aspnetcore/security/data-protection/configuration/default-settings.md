---
title: Zarządzanie kluczami ochrony danych i okres istnienia w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat ochrony danych zarządzania kluczami i okresem istnienia w programie ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902983"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Zarządzanie kluczami ochrony danych i okres istnienia w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Zarządzanie kluczami

Aplikacja próbuje wykrywanie jego środowisku operacyjnym i usuwanie konfiguracji klucza samodzielnie.

1. Jeśli aplikacja jest hostowana w [Azure Apps](https://azure.microsoft.com/services/app-service/), kluczy zostaną utrwalone w *%HOME%\ASP.NET\DataProtection-Keys* folderu. Ten folder jest wspierana przez sieć, Magazyn, które jest synchronizowane na wszystkich maszynach hostingu aplikacji.
   * Klucze nie są chronione w stanie spoczynku.
   * *DataProtection klucze* folderu dostarcza pierścień klucza do wszystkich wystąpień aplikacji w gnieździe pojedynczego wdrożenia.
   * Gniazda wdrażane pojedynczo, takich jak przejściowe i produkcyjne, nie udostępniaj klucza pierścień. Podczas zamiany między miejscami wdrożenia, na przykład zamianę przejściowe i produkcyjne lub za pomocą / B, testowanie do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza nie będzie można dowolną aplikację przy użyciu ochrony danych. Prowadzi to do użytkowników rejestrowane poza aplikację, która używa standardowego uwierzytelniania plików cookie programu ASP.NET Core, ponieważ używa ona ochrony danych, aby chronić swoje pliki cookie. W razie potrzeby pierścieni klucz niezależnie od miejsca, użyj dostawcę zewnętrznego pierścienia klucza, takich jak Azure Blob Storage, Azure Key Vault, magazynu SQL, lub pamięci podręcznej redis Cache.

1. Jeśli profil użytkownika jest dostępna, kluczy zostaną utrwalone w *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folderu. W przypadku systemu operacyjnego Windows kluczy są szyfrowane za pomocą DPAPI.

   Pula aplikacji [atrybut setProfileEnvironment](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) musi być także włączona. Wartość domyślna `setProfileEnvironment` jest `true`. W niektórych przypadkach (na przykład Windows System operacyjny) `setProfileEnvironment` ustawiono `false`. Jeśli klucze nie są przechowywane w katalogu profilu użytkownika, co Oczekiwano:

   1. Przejdź do *%windir%/system32/inetsrv/config* folderu.
   1. Otwórz *applicationHost.config* pliku.
   1. Znajdź `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` elementu.
   1. Upewnij się, że `setProfileEnvironment` atrybut nie jest obecny, które domyślnie używa wartości do `true`, lub jawnie ustawić wartość atrybutu `true`.

1. Jeśli aplikacja jest hostowana w usługach IIS, klucze są zachowywane do rejestru HKLM w kluczu rejestru specjalne, który ma ACLed tylko proces roboczy. Klucze są szyfrowane za pomocą DPAPI.

1. Jeśli żadna z tych warunków, klucze nie są zachowywane poza bieżącym procesie. Podczas procesu zamykania, wszystkich wygenerowanych kluczy zostaną utracone.

Deweloper zawsze ma pełną kontrolę, można zmienić, jak i, w którym są przechowywane klucze. Pierwszych trzech powyższych opcji powinny zapewnić dobre wartości domyślne to większość aplikacji, podobnie jak ASP.NET  **\<machineKey >** procedury Autogenerowanie działały w przeszłości. Opcja ostateczną, rezerwowy jest tylko scenariusz, który wymaga dla deweloperów określić [konfiguracji](xref:security/data-protection/configuration/overview) ponoszonych z góry, jeśli chcą trwałość klucza, ale ta rezerwowe występuje tylko w rzadkich sytuacjach.

W przypadku hostowania w kontenerze platformy Docker, kluczy powinny zostać utrwalona, w folderze, który jest woluminem platformy Docker (udostępnionego woluminu lub wolumin zainstalowany host będzie się powtarzać, poza okres istnienia kontenera) lub zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/). Zewnętrznego dostawcy jest również przydatne w scenariuszach z farmami internetowymi, jeśli aplikacje nie mogą uzyskać dostęp do woluminu udostępnioną w sieci (zobacz [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) Aby uzyskać więcej informacji).

> [!WARNING]
> Jeśli Deweloper zastępuje zasadom opisanym powyżej i wskazuje system ochrony danych w określonym repozytorium klucza, automatycznego szyfrowania kluczy w stanie spoczynku jest wyłączone. Ochrona magazynowanych można ją ponownie włączyć, za pośrednictwem [konfiguracji](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Okres istnienia klucza

Domyślnie klucze mają 90-dniowy okres istnienia. Po wygaśnięciu klucza, aplikacja automatycznie generuje nowy klucz i ustawia nowego klucza jako aktywnego klucza. Tak długo, jak wycofane klucze pozostają w systemie, aplikacja może odszyfrować dowolne dane, chronione przy użyciu ich. Zobacz [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) Aby uzyskać więcej informacji.

## <a name="default-algorithms"></a>Domyślne algorytmy

Domyślny ładunek ochrony algorytm to AES-256-CBC poufności oraz HMACSHA256 autentyczności. 512-bitowy klucz główny, zmienić co 90 dni, jest używany do uzyskania dwa klucze podrzędne używane dla tych algorytmów na podstawie poszczególnych ładunku. Zobacz [podklucza pochodnym](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) Aby uzyskać więcej informacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
