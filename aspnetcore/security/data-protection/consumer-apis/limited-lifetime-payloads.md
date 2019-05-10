---
title: Ograniczanie okresu istnienia ładunków chronionych z platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczanie okresu istnienia ładunek chronionych za pomocą interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902911"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Ograniczanie okresu istnienia ładunków chronionych z platformy ASP.NET Core

Istnieją scenariusze, w którym chce utworzyć chronionych ładunek, która wygaśnie po upływie czasu określonego Ustaw dewelopera aplikacji. Na przykład chronione ładunku może reprezentować token resetowania hasła, który powinien składać się wyłącznie prawidłową godzinę. Bez obaw można dla deweloperów do tworzenia własnych format ładunku, która zawiera datę wygaśnięcia osadzonego i zaawansowanych deweloperów mogą chcieć czy mimo to zrobić, ale dla większości deweloperów zarządzanie tymi wygaśnięcia można powiększać niewygodna.

Aby to ułatwić dla naszych użytkowników dla deweloperów pakietu [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera narzędzia interfejsów API do tworzenia ładunków, które automatycznie wygasają po upływie pewien okres czasu. Te interfejsy API zawieszanie się wylogować się z `ITimeLimitedDataProtector` typu.

## <a name="api-usage"></a>Użycie interfejsu API

`ITimeLimitedDataProtector` Interfejs jest interfejsem core do ochrony i wyłączenie ochrony ograniczonej czasowo / własnym wygasające ładunków. Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, musisz najpierw wystąpienie zwykły [interfejsu IDataProtector](xref:security/data-protection/consumer-apis/overview) skonstruowany z określonym przeznaczeniem. Gdy `IDataProtector` wystąpienia jest dostępny, wywołaj `IDataProtector.ToTimeLimitedDataProtector` metodę rozszerzenia, aby wrócić funkcję ochrony za pomocą funkcji wbudowanych wygaśnięcia.

`ITimeLimitedDataProtector` udostępnia następujące metody rozszerzenie i powierzchni interfejsu API:

* CreateProtector (ciąg cel): ITimeLimitedDataProtector — ten interfejs API jest podobne do istniejących `IDataProtectionProvider.CreateProtector` , może służyć do tworzenia [zastosowania łańcuchów](xref:security/data-protection/consumer-apis/purpose-strings) z ochrony ograniczonej czasowo głównego.

* Ochrona (byte [] zwykłego tekstu, DateTimeOffset wygaśnięcia): byte]

* Ochrona (byte [] postaci zwykłego tekstu, przedział czasu istnienia): byte]

* Ochrona (byte [] zwykły tekst): byte]

* Ochrona (ciągu zwykłego tekstu, DateTimeOffset wygaśnięcia): ciąg

* Ochrona (ciągu zwykłego tekstu, przedział czasu istnienia): ciąg

* Ochrona (ciąg zwykły tekst): ciąg

Oprócz podstawowych `Protect` metod, które zająć tylko zwykły tekst, istnieją nowe przeciążenia, które pozwala na stosowanie datę wygaśnięcia w ładunku. Data wygaśnięcia, można określić jako Data bezwzględna (za pośrednictwem `DateTimeOffset`) lub jako względne czasu (z obecną systemową godzina, za pośrednictwem `TimeSpan`). Przeciążenia, które nie przyjmuje wygaśnięcia nosi nazwę, ładunek jest traktowana jako nigdy nie wygasa.

* Wyłącz ochronę (byte [] protectedData, out DateTimeOffset wygaśnięcia): byte]

* Wyłącz ochronę (protectedData byte []): byte]

* Wyłącz ochronę (out wygaśnięcia DateTimeOffset protectedData ciągu): ciąg

* Wyłącz ochronę (ciąg protectedData): ciąg

`Unprotect` Metody zwracają oryginalne dane niechronione. Jeśli jeszcze nie upłynął ładunek, bezwzględnych wygaśnięcia jest zwracana jako opcjonalny parametr wraz z oryginalnym dane niechronione out. Jeżeli obciążenie jest uznawane za wygasłe, wszystkie przeciążenia metody Unprotect zgłosi cryptographicexception —.

>[!WARNING]
> Zalecane jest nie ochrony ładunków, wymagających długoterminowe lub nieokreślony trwałości za pomocą tych interfejsów API. "Można pozwolić sobie na ładunków chronionych trwale nie da się po upływie miesiąca?" może służyć jako regułą; Jeśli odpowiedź jest nie następnie deweloperzy należy rozważyć alternatywne interfejsy API.

Przykładowe poniżej został użyty [ścieżek kodu-DI](xref:security/data-protection/configuration/non-di-scenarios) podczas tworzenia wystąpienia system ochrony danych. Aby uruchomić ten przykład, upewnij się, że najpierw zostały dodane odwołanie do pakietu Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
