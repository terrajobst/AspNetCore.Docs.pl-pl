---
title: Limit ważności chronionych ładunków w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczyć okres istnienia ładunku chronionych przy użyciu interfejsów API platformy ASP.NET Core danych ochrony.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278060"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limit ważności chronionych ładunków w ASP.NET Core

Istnieją scenariusze, w których chce utworzyć ładunku chronionych, która wygaśnie po upływie ustaw czas Deweloper aplikacji. Na przykład chronione obciążenie może reprezentować token resetowania hasła, który tylko powinna być prawidłową godzinę. Pewno można dla deweloperów do tworzenia własnych format ładunku, zawierającą datę wygaśnięcia osadzonych i zaawansowane deweloperzy mogą chcieć mimo to zrobić, ale dla większości deweloperzy zarządzanie tymi wygasanie może zwiększyć nużące.

Aby ułatwić naszym odbiorców developer, pakiet dla [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera narzędzie interfejsów API do tworzenia ładunków, które automatycznie wygasają po okres czasu. Te interfejsy API wykraczać poza z `ITimeLimitedDataProtector` typu.

## <a name="api-usage"></a>Użycie interfejsu API

`ITimeLimitedDataProtector` Interfejs jest interfejsem core do ochrony i wyłączenie ograniczone czasowo / własnym wygasające ładunków. Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, musisz najpierw wystąpienia zwykły [interfejsu IDataProtector](xref:security/data-protection/consumer-apis/overview) wykonane z określonym przeznaczeniem. Raz `IDataProtector` wystąpienia jest dostępna, należy wywołać `IDataProtector.ToTimeLimitedDataProtector` — metoda rozszerzenia odzyskać z funkcji ochrony z możliwości wbudowanego wygaśnięcia.

`ITimeLimitedDataProtector` udostępnia następujące metody powierzchni i rozszerzenia interfejsu API:

* CreateProtector (ciąg cel): ITimeLimitedDataProtector — ten interfejs API jest podobne do istniejących `IDataProtectionProvider.CreateProtector` w tym może służyć do tworzenia [cel łańcuchów](xref:security/data-protection/consumer-apis/purpose-strings) z funkcji ochrony kluczem głównym ograniczone czasowo.

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
> Nie ma on zalecane jest użycie te interfejsy API ochrony ładunków, które wymaga trwałości długoterminowe lub nieokreślona. "I pozwolić sobie na chronionych ładunków być trwale nieodwracalny po miesiąca?" może służyć jako regułą; Jeśli odpowiedź jest nie deweloperzy następnie rozważyć alternatywnych interfejsów API.

Przykładowe poniżej używa [ścieżki kodu nie są Podpisane](xref:security/data-protection/configuration/non-di-scenarios) dla wystąpienia systemu ochrony danych. Do uruchomienia tego przykładu, upewnij się, że najpierw zostały dodane odwołanie do pakietu Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
