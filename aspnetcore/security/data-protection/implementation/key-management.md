---
title: Zarządzanie kluczami w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji interfejsów API zarządzania kluczami ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219254"
---
# <a name="key-management-in-aspnet-core"></a>Zarządzanie kluczami w programie ASP.NET Core

<a name="data-protection-implementation-key-management"></a>

System ochrony danych automatycznie zarządza czasem istnienia kluczy głównych używany do włączania i wyłączania ochrony ładunków. Każdy klucz może znajdować się w jednej z czterech etapów:

* Utworzony — klucz istnieje w pierścienia klucza, ale nie został jeszcze aktywowany. Klucz nie powinien służyć do nowych operacji ochrony przed upływem wystarczająco dużo czasu, klucz miała szansę zostanie rozpropagowany do wszystkich maszyn, które zużywają ten pierścień klucza.

* Aktywny - klucz istnieje w pierścieniu klucza i powinien być używany dla wszystkich nowych operacji ochrony.

* Wygasłe — klucz zostało uruchomione istnienia naturalnych i nie będzie używać dla nowych operacji ochrony.

* Odwołane - klucz zostanie naruszony i nie może być używany dla nowych operacji ochrony.

Utworzone, aktywnych i wygasłe klucze może być używane do wyłączanie ochrony ładunków, przychodzących. Odwołane kluczy domyślnie nie mogą służyć do wyłączanie ochrony ładunków, ale Deweloper aplikacji może [zastąpienia tego zachowania](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) w razie potrzeby.

>[!WARNING]
> Deweloper może być uznasz, że można usunąć klucza z kluczem pierścienia (np. poprzez usunięcie odpowiedniego pliku z systemu plików). W tym momencie wszystkich danych chronionych za pomocą klucza jest trwale zapisane, a istnieje żadne przesłonięcie awaryjnego, tak jak ma to przy użyciu kluczy odwołane. Usuwanie klucza jest naprawdę destrukcyjne zachowanie i w związku z tym system ochrony danych uwidacznia żadnych najwyższej klasy interfejsów API do wykonania tej operacji.

## <a name="default-key-selection"></a>Domyślny wybór klucza

Gdy system ochrony danych odczytuje pierścień klucza z repozytorium zapasowy, jej będzie podejmować próby zlokalizowania klucza "default" z pierścienia klucza. Domyślny klucz jest używany dla nowych operacji ochrony.

Ogólny Algorytm heurystyczny jest, że system ochrony danych wybierze klucz z najbardziej aktualną datę aktywacji jako domyślnego klucza. (Brak jest współczynnik małych grę fudge, aby umożliwić zegara serwer serwer pochylenia). Jeśli klucz jest wygasłe lub odwołane, i jeśli aplikacja nie została wyłączona automatyczna klucza generacji, a następnie zostanie wygenerowany nowy klucz za pomocą natychmiastowej aktywacji na [klucza wygaśnięcia i wycofania](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) zasad poniżej.

Przyczyna system ochrony danych natychmiast generuje nowy klucz, a nie nastąpi powrót do innego klucza jest, że wygenerowanie nowego klucza powinny być traktowane jako niejawne okresu wszystkie klucze, które zostały aktywowane przed nowego klucza. Ogólne chodzi o to, że nowe klucze zostały skonfigurowane przy użyciu różnych algorytmów lub mechanizmów szyfrowania podczas spoczynku niż starych kluczy, a system powinien Preferuj bieżącej konfiguracji przed powrotem.

Występuje wyjątek. Jeśli Deweloper aplikacji ma [wyłączyć automatyczne generowanie klucza](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), a następnie system ochrony danych, musisz wybrać coś jako domyślnego klucza. W tym scenariuszu rezerwowy system wybierze klucz odwołać się najbardziej aktualną datę aktywacji, z preferencją kluczy, których czas propagacji dla innych maszyn w klastrze. System rezerwowy może pozostać w wyniku wybór domyślny wygasły klucz. System rezerwowy nigdy nie wybierze odwołanych klucza jako klucza domyślne, a pierścień klucza jest pusta lub za każdy klucz został odwołany. następnie systemu powoduje wygenerowanie błędu po zainicjowaniu.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Data wygaśnięcia klucza i wycofania

Gdy klucz jest tworzony, automatycznie dało się data aktywacji {teraz + 2 dni} i {teraz + 90 dni} datę wygaśnięcia. 2-dniowym opóźnieniem aktywacji zapewnia klucza czasu propagowane przez system. Oznacza to, że umożliwia innym aplikacjom wskazującej na magazyn zapasowy przestrzegać klucz ich następnego okresu automatyczne odświeżanie, maksymalizując prawdopodobieństwo, że po klucz cyklicznego czy stanie się aktywny, który jej zakończeniem propagacji do wszystkich aplikacji, które może być konieczne jej używać.

Domyślny klucz wygaśnie w ciągu 2 dni i pierścień klucz jeszcze jej nie ma klucza, który będzie aktywny po upływie domyślnego klucza, w system ochrony danych będzie automatycznie umieszczony nowy klucz do pierścienia klucza. Ten nowy klucz ma Data aktywacji {Data wygaśnięcia domyślny klucz} i {teraz + 90 dni} datę wygaśnięcia. Dzięki temu system automatycznie przeprowadzić kluczy na bieżąco z nie przerw w działaniu usług.

Mogą istnieć okoliczności gdzie klucz zostanie utworzony za pomocą natychmiastowej aktywacji. Jeden przykładem może być, gdy aplikacja nie zostało uruchomione przez czas wszystkie klucze w pierścienia klucz wygasł. W takim przypadku klucza jest podana data aktywacji {teraz} niezwłocznie normalne 2-dniowych aktywacji.

Domyślny okres istnienia klucza jest 90 dni, chociaż jest to można skonfigurować, jak w poniższym przykładzie.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Administrator można również zmienić domyślne systemowe, chociaż jawnym wywołaniem `SetDefaultKeyLifetime` zastąpią wszelkie zasady systemowe. Domyślny okres istnienia klucza nie może być krótszy niż 7 dni.

## <a name="automatic-key-ring-refresh"></a>Odświeżanie automatyczne pierścień klucza

Gdy system ochrony danych, odczytuje pierścień klucza podstawowego repozytorium i zapisuje go w pamięci podręcznej w pamięci. Ta pamięć podręczna umożliwia wykonywanie operacji Chroń i Unprotect kontynuować bez osiągnięcia magazyn zapasowy. System automatycznie sprawdzi magazyn zapasowy zmiany mniej więcej co 24 godziny lub po upływie bieżącego klucza domyślne osiągnięta jako pierwsza.

>[!WARNING]
> Deweloperzy powinien bardzo rzadko (Jeśli w ogóle) trzeba korzystać bezpośrednio zarządzanie kluczami interfejsów API. System ochrony danych będzie wykonywać automatyczne zarządzanie kluczami, zgodnie z powyższym opisem.

System ochrony danych udostępnia interfejs `IKeyManager` który może służyć do sprawdzania i wprowadzić zmiany do pierścienia klucza. System DI podanym wystąpienie `IDataProtectionProvider` oferuje również wystąpienie `IKeyManager` za użycie. Alternatywnie możesz ściągnąć `IKeyManager` bezpośrednio z `IServiceProvider` tak jak w poniższym przykładzie.

Każdej operacji, która modyfikuje pierścień klucza (Tworzenie nowego klucza jawnie lub odwołania) spowoduje unieważnienie w pamięci podręcznej. Następne wywołanie `Protect` lub `Unprotect` spowoduje, że system ochrony danych, odświeżanie pierścień klucza i ponowne utworzenie pamięci podręcznej.

Poniższy przykład demonstruje użycie `IKeyManager` interfejs do sprawdzania i manipulowania ring klucza, w tym odwołaniem istniejących kluczy i ręczne Generowanie nowego klucza.

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Magazyn kluczy

System ochrony danych ma heurystyki, według którego próbuje automatycznie wywnioskować lokalizacji odpowiedniego magazynu kluczy i mechanizm szyfrowania podczas spoczynku. Mechanizm trwałość klucza jest także można skonfigurować przez dewelopera aplikacji. Następujące dokumenty omawia implementacje skrzynkach odbiorczych tych mechanizmów:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
