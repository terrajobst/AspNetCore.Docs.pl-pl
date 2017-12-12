---
title: "Zarządzanie kluczami"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core danych ochrony zarządzanie kluczami interfejsów API."
keywords: "Zarządzanie kluczami platformy ASP.NET Core, ochrony danych"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fb9b807a-d143-4861-9ddb-005d8796afa3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: d9e38fd5c8de2b10ad24fe557aa6e3063e40236e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="key-management"></a>Zarządzanie kluczami

<a name="data-protection-implementation-key-management"></a>

System ochrony danych automatycznie zarządza czasem istnienia kluczy głównych używany do zabezpieczania i odbezpieczania ładunków. Każdy klucz może istnieć w jednym z czterech etapów:

* Utworzone — klucz istnieje w kręgu klucza, ale nie zostało jeszcze uaktywnione. Klucz nie powinien służyć do nowych operacji Chroń przed upływem wystarczającą ilość czasu czy klucz miał możliwość obejmie wszystkie maszyny korzystające pierścień tego klucza.

* Aktywny - klucz istnieje w kręgu klucza i należy jej używać do wszystkich nowych operacji Chroń.

* Ważność — klucz jest wykonywane jego fizyczną okres istnienia i już używanego dla nowych operacji Chroń.

* Odwołany — klucz zostanie naruszony i nie mogą być używane dla nowej operacji Chroń.

Utworzony, aktywne i wygasłe klucze mogą używane do usunięcia ochrony ładunków przychodzących. Odwołania kluczy domyślnie nie można używać do usunięcia ochrony ładunków, ale Deweloper aplikacji może [zastąpienia tego zachowania](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) w razie potrzeby.

>[!WARNING]
> Deweloper może być wydawać się usuwanie klucza z pierścienia klucz (np. przez usunięcie odpowiedniego pliku z systemu plików). W tym momencie wszystkie dane chronione za pomocą klucza jest trwale niemożliwe do odczytania, a nie zastąpień awaryjnego, tak jak w przypadku kluczy odwołane. Usuwanie klucza jest rzeczywiście destrukcyjnego zachowanie, i w związku z tym system ochrony danych udostępnia żadnego pierwszej klasy interfejsu API do wykonania tej operacji.

## <a name="default-key-selection"></a>Domyślny wybór klucza

Gdy system ochrony danych odczytuje pierścień klucza z repozytorium zapasowy, spróbuje zlokalizować "domyślny" klucz z pierścienia klucza. Domyślny klucz służy do nowych operacji Chroń.

Ogólne heurystyki jest wybierze klucz przy użyciu najnowszych danych aktywacji jako domyślny klucz w systemu ochrony danych. (Brak współczynnik małych fudge umożliwiające zegar serwera serwera pochylenia.) Jeśli klucz jest wygasłe lub odwołane, i jeśli aplikacja nie została wyłączona automatycznego generowania klucza, a następnie zostanie wygenerowany nowy klucz o natychmiastowej aktywacji na [kluczy wygaśnięcia i stopniowych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) zasad poniżej.

Przyczyna systemu ochrony danych natychmiast generuje nowy klucz, a nie nastąpi powrót do innego klucza jest, że wygenerowanie nowego klucza powinien być traktowany jako niejawne ważności wszystkich kluczy, które zostały aktywowane przed nowego klucza. Ogólne informacje o tym jest nowe klucze zostały skonfigurowane z różnych algorytmów lub mechanizmów szyfrowania w rest niż stare klucze, czy system powinien Preferuj bieżącej konfiguracji przed powrotem.

Brak Wystąpił wyjątek. Jeśli Deweloper aplikacji ma [wyłączona, automatyczne generowanie klucza](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), a następnie systemu ochrony danych należy wybrać element jako domyślny klucz. W tym scenariuszu rezerwowy systemu wybierze klucza odwołane przy użyciu najnowszych danych aktywacji, z preferencją klucze, których czas na propagację do innych komputerów w klastrze. System rezerwowy może zakończyć się w związku z tym wybór domyślny wygasły klucz. System rezerwowy nigdy nie wybierze klucz odwołane jako domyślny klucz, a pierścień klucza jest pusta lub został odwołany każdego klucza następnie systemu powoduje wygenerowanie wystąpił błąd podczas inicjowania.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Wygaśnięcie klucza i wdrażania

Po utworzeniu klucza automatycznie znajduje się data aktywacji {teraz + 2 dni} i {teraz + 90 dni} daty wygaśnięcia. 2-dniowego opóźnienie aktywacji daje klucza czas na propagację za pośrednictwem systemu. Oznacza to umożliwia innym aplikacjom wskazując magazynu zapasowego przestrzegać klucz ich następnego okresu automatycznego odświeżania, w związku z tym maksymalizacja prawdopodobieństwo, że po klucz pierścienia ma stają się aktywne, którą ma on propagowane do wszystkich aplikacji, które może być konieczne jej używać.

Domyślny klucz wygaśnie w ciągu 2 dni i pierścień klucza nie ma już klucz, który będzie aktywny po wygaśnięciu domyślny klucz, w systemu ochrony danych będą automatycznie utrwalić nowy klucz do pierścień klucza. Ten nowy klucz ma Data aktywacji {Data wygaśnięcia domyślny klucz} i {teraz + 90 dni} daty wygaśnięcia. Dzięki temu system automatycznie wycofanie kluczy na bieżąco z przerwy w świadczeniu usług.

Mogą wystąpić okoliczności gdzie klucz zostanie utworzony z natychmiastowej aktywacji. Przykładem może dochodzić do aplikacji nie zostało uruchomione przez czas i wygasły wszystkie klucze w kręgu klucza. W takim przypadku klucz znajduje się data aktywacji {teraz} niezwłocznie normalne 2-dniowego aktywacji.

Domyślny okres istnienia klucza jest 90 dni, ale można skonfigurować, jak w poniższym przykładzie.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Administrator może zmienić również domyślny całego systemu, chociaż jawnym wywołaniem `SetDefaultKeyLifetime` spowoduje zastąpienie wszelkich zasady systemowe. Domyślny okres istnienia klucza nie może być krótszy niż 7 dni.

## <a name="automatic-key-ring-refresh"></a>Pierścień klucz automatycznego odświeżania

Gdy system ochrony danych, odczytuje pierścień klucz podstawowy repozytorium i buforuje ją w pamięci. Ta pamięć podręczna umożliwia operacji Chroń i Unprotect kontynuować bez naciśnięcie magazynu zapasowego. System automatycznie sprawdza magazynu zapasowego zmiany mniej więcej co 24 godziny lub po wygaśnięciu bieżący domyślny klucz zależności zostanie osiągnięty jako pierwszy.

>[!WARNING]
> Deweloperzy powinien bardzo rzadko (Jeśli kiedykolwiek) trzeba korzystać bezpośrednio zarządzania kluczami interfejsów API. Automatyczne zarządzanie kluczami będzie wykonywać systemu ochrony danych, zgodnie z powyższym opisem.

System ochrony danych udostępnia interfejsem `IKeyManager` który można sprawdzić i zmienić pierścień klucza. System Podpisane podane wystąpienie `IDataProtectionProvider` może także udostępnić wystąpienia `IKeyManager` do użycia. Alternatywnie można ściągnąć `IKeyManager` bezpośrednio z `IServiceProvider` tak jak w poniższym przykładzie.

Wszystkie działania, które modyfikuje pierścień klucza (Tworzenie nowego klucza jawnie lub odwołania) spowoduje unieważnienie w pamięci podręcznej. Następne wywołanie `Protect` lub `Unprotect` spowoduje, że system ochrony danych odczytać pierścień klucza i ponowne utworzenie pamięci podręcznej.

Poniższy przykład pokazuje, przy użyciu `IKeyManager` interfejs do przeglądania i modyfikowania pierścień klucz, w tym uchylająca istniejących kluczy i ręcznie generowania klucza.

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Magazyn kluczy

System ochrony danych ma heurystycznego, zgodnie z którymi spróbuje automatycznie wywnioskować lokalizacji odpowiedniego magazynu kluczy i szyfrowania w mechanizm rest. Dotyczy to również konfigurowane przez dewelopera aplikacji. Poniższe dokumenty omówiono implementacje pola w tych mechanizmów:

* [W polu dostawcy magazynu kluczy](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [W polu klucza szyfrowania dostawców rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
