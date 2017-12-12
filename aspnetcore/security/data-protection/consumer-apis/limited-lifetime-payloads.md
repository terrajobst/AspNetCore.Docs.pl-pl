---
title: "Ograniczanie okresu istnienia ładunków chronionych"
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak ograniczyć okres istnienia ładunku chronionych przy użyciu platformy ASP.NET Core interfejsy API ochrony danych."
keywords: "Platformy ASP.NET Core, ochrony danych, okres istnienia ładunku"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Ograniczanie okresu istnienia ładunków chronionych

Istnieją scenariusze, w których chce utworzyć ładunku chronionych, która wygaśnie po upływie ustaw czas Deweloper aplikacji. Na przykład chronione obciążenie może reprezentować token resetowania hasła, który tylko powinna być prawidłową godzinę. Pewno można dla deweloperów do tworzenia własnych format ładunku, zawierającą datę wygaśnięcia osadzonych i zaawansowane deweloperzy mogą chcieć mimo to zrobić, ale dla większości deweloperzy zarządzanie tymi wygasanie może zwiększyć nużące.

Aby ułatwić naszym odbiorców developer, pakiet dla [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera narzędzie interfejsów API do tworzenia ładunków, które automatycznie wygasają po okres czasu. Te interfejsy API wykraczać poza z `ITimeLimitedDataProtector` typu.

## <a name="api-usage"></a>Użycie interfejsu API

`ITimeLimitedDataProtector` Interfejs jest interfejsem core do ochrony i wyłączenie ograniczone czasowo / własnym wygasające ładunków. Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, musisz najpierw wystąpienia zwykły [interfejsu IDataProtector](overview.md) wykonane z określonym przeznaczeniem. Raz `IDataProtector` wystąpienia jest dostępna, należy wywołać `IDataProtector.ToTimeLimitedDataProtector` — metoda rozszerzenia odzyskać z funkcji ochrony z możliwości wbudowanego wygaśnięcia.

`ITimeLimitedDataProtector`udostępnia następujące metody powierzchni i rozszerzenia interfejsu API:

* CreateProtector (ciąg cel): ITimeLimitedDataProtector — ten interfejs API jest podobne do istniejących `IDataProtectionProvider.CreateProtector` w tym może służyć do tworzenia [cel łańcuchów](purpose-strings.md) z funkcji ochrony kluczem głównym ograniczone czasowo.

* Ochrona (byte [] w postaci zwykłego tekstu, wygaśnięcia DateTimeOffset): byte]

* Ochrona (byte [] w postaci zwykłego tekstu, okres istnienia TimeSpan): byte]

* Ochrona (byte [] zwykłym tekstem): byte]

* Ochrona (ciągu zwykłego tekstu, wygaśnięcia DateTimeOffset): ciąg

* Ochrona (ciąg w postaci zwykłego tekstu, okres istnienia TimeSpan): ciąg

* Ochrona (ciąg zwykłym tekstem): ciąg

Oprócz podstawowe `Protect` metod, które biorą tylko zwykły tekst, istnieją nowe przeciążenia, które umożliwiają określenie daty wygaśnięcia ładunku. Data wygaśnięcia można określić jako Data bezwzględna (za pośrednictwem `DateTimeOffset`) lub jako względne czasu (z bieżącym systemem czasu za pomocą `TimeSpan`). Po wywołaniu przepełnienia, które nie przyjmuje wygaśnięcia ładunku jest traktowana jako nigdy nie wygasa.

* Wyłącz ochronę (byte [] protectedData, limit wygaśnięcia DateTimeOffset): byte]

* Wyłącz ochronę (protectedData byte []): byte]

* Wyłącz ochronę (limit wygaśnięcia DateTimeOffset protectedData ciągu): ciąg

* Wyłącz ochronę (ciąg protectedData): ciąg

`Unprotect` Metody zwracają oryginalne dane niechronione. Jeśli jeszcze nie wygasł ładunku, wygaśnięcia bezwzględne jest zwracana jako opcjonalny parametr wraz z danymi niechronionych wyjściowy. Jeśli wygasł ładunku wszystkie przeciążenia metody Unprotect zgłosi cryptographicexception —.

>[!WARNING]
> Nie zaleca się przy użyciu tych interfejsów API ochrony ładunków, które wymaga trwałości długoterminowe lub nieokreślona. "I pozwolić sobie na chronionych ładunków być trwale nieodwracalny po miesiąca?" może służyć jako regułą; Jeśli odpowiedź jest nie deweloperzy następnie rozważyć alternatywnych interfejsów API.

Przykładowe poniżej używa [ścieżki kodu nie są Podpisane](../configuration/non-di-scenarios.md) dla wystąpienia systemu ochrony danych. Do uruchomienia tego przykładu, upewnij się, że najpierw zostały dodane odwołanie do pakietu Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
