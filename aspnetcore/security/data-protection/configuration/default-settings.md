---
title: "Zarządzanie kluczami ochrony danych i okres istnienia w ASP.NET Core"
author: rick-anderson
description: "Więcej informacji na temat zarządzania kluczami ochrony danych i okres istnienia w ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Zarządzanie kluczami ochrony danych i okres istnienia w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Zarządzanie kluczami

Aplikacja próbuje wykryć jego środowisku operacyjnym i obsługiwać klucza konfiguracji samodzielnie.

1. Jeśli aplikacja jest hostowana w [aplikacji Azure](https://azure.microsoft.com/services/app-service/), klucze są utrwalane w *%HOME%\ASP.NET\DataProtection-Keys* folderu. Ten folder nie jest obsługiwana przez sieć magazynu i jest synchronizowane na wszystkich komputerach hosting aplikacji.
   * Klucze chronione nie są w stanie spoczynku.
   * *DataProtection klucze* folderu dostarcza pierścień klucza do wszystkich wystąpień aplikacji w miejscu pojedynczego wdrożenia.
   * Miejsc oddzielne wdrożenia, takich jak przemieszczania i produkcji, nie należy współużytkować pierścień klucza. Podczas wymiany między miejscami wdrożenia, na przykład wymiany przemieszczania do środowiska produkcyjnego lub za pomocą / B, testowanie, dowolną aplikację przy użyciu ochrony danych nie można odszyfrować przechowywanych danych przy użyciu pierścień klucza w poprzednim miejsca. Prowadzi to do użytkowników rejestrowane poza aplikacji, która używa standardowego uwierzytelniania plików cookie platformy ASP.NET Core, ponieważ używa ona ochrony danych do ochrony jego plików cookie. Jeśli konieczna jest niezależny od miejsca klucza sygnałów, używa dostawcy zewnętrznego pierścień klucza, takie jak magazyn obiektów Blob Azure, usługa Azure Key Vault magazynu SQL, lub pamięci podręcznej Redis.

1. Jeśli profil użytkownika jest dostępna, klucze są utrwalane w *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* folderu. Jeśli system operacyjny Windows, klucze są szyfrowane, gdy przy użyciu DPAPI.

1. Jeśli aplikacja jest obsługiwana w usługach IIS, klucze są zachowywane w rejestrze HKLM w kluczu rejestru specjalne, który ma ACLed tylko konta procesu roboczego. Klucze są szyfrowane, gdy przy użyciu DPAPI.

1. Jeśli żadna z tych warunków, klucze nie są trwałe poza bieżącym procesie. Podczas procesu zamykania, wszystkie wygenerowane klucze zostaną utracone.

Deweloper zawsze ma pełną kontrolę, można zmienić, jak i gdzie są przechowywane klucze. Pierwsze trzy opcje powyżej powinno zapewniać dobrej wartości domyślne dla większości aplikacji, podobnie jak ASP.NET  **\<machineKey >** działał automatycznego generowania procedur. Opcja końcowym, rezerwowego jest tylko scenariusz, który wymaga developer określić [konfiguracji](xref:security/data-protection/configuration/overview) góry, jeśli chcą trwałości klucza, ale ta powrotu występuje tylko w rzadkich przypadkach.

Odnośnie do hostowania w kontenerze Docker, kluczy powinien utrwalone w folderze, który jest woluminem Docker (udostępnionego woluminu lub wolumin zainstalowany w hoście będzie się powtarzać, poza okres istnienia kontenera) lub zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/). Zewnętrznego dostawcy jest również przydatne w scenariuszach kolektywu serwerów sieci web, jeśli aplikacje nie może uzyskać dostęp do woluminu udostępnionego sieci (zobacz [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) Aby uzyskać więcej informacji).

> [!WARNING]
> Jeśli dewelopera reguł opisanych powyżej zastąpień i punktów systemu ochrony danych w określonym repozytorium klucza, automatycznego szyfrowania tych kluczy w stanie spoczynku jest wyłączone. W pozostałych ochrony można ją ponownie włączyć za pomocą [konfiguracji](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Okres istnienia klucza

Domyślnie klucze mają 90-dniowy okres istnienia. Po wygaśnięciu klucz aplikacji automatycznie generuje nowy klucz i ustawia nowy klucz jako aktywnego klucza. Tak długo, jak klucze wycofane pozostają w systemie, aplikacja może odszyfrować wszystkie dane chronione za pomocą ich. Zobacz [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) Aby uzyskać więcej informacji.

## <a name="default-algorithms"></a>Algorytmy domyślne

Domyślnym algorytmem ochrony ładunku używany jest AES-256-CBC poufności oraz HMACSHA256 autentyczności. Klucz główny 512-bitowe, zmienić co 90 dni, jest używany do uzyskania dwa klucze podrzędne używane dla tych algorytmów na podstawie na ładunku. Zobacz [podkluczy pochodnym](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) Aby uzyskać więcej informacji.

## <a name="see-also"></a>Zobacz także

* [Rozszerzalność zarządzania kluczami](xref:security/data-protection/extensibility/key-management)
