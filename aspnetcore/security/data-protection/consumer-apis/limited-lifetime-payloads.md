---
title: Ogranicz okres istnienia chronionych ładunków w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczyć okres istnienia chronionego ładunku przy użyciu interfejsów API ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656059"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Ogranicz okres istnienia chronionych ładunków w ASP.NET Core

Istnieją scenariusze, w których Deweloper aplikacji chce utworzyć chroniony ładunek, który wygaśnie po upływie określonego czasu. Na przykład, chroniony ładunek może reprezentować token resetowania hasła, który powinien być prawidłowy tylko przez jedną godzinę. W przypadku deweloperów istnieje możliwość utworzenia własnego formatu ładunku zawierającego osadzoną datę wygaśnięcia, a zaawansowani deweloperzy mogą chcieć to zrobić mimo to, ale w przypadku większości deweloperów, którzy zarządzają tymi wygasami, mogą wzrosnąć żmudnym.

Aby ułatwić naszym użytkownikom deweloperów, pakiet [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) zawiera interfejsy API narzędzi do tworzenia ładunków, które automatycznie wygasną po upływie określonego czasu. Te interfejsy API zawieszają się `ITimeLimitedDataProtector` typu.

## <a name="api-usage"></a>Użycie interfejsu API

Interfejs `ITimeLimitedDataProtector` jest podstawowym interfejsem do ochrony i nieochrony ładunków z ograniczeniami czasowymi/z własnym wygaśnięciem. Aby utworzyć wystąpienie `ITimeLimitedDataProtector`, najpierw będzie potrzebne wystąpienie regularnego [IDataProtector](xref:security/data-protection/consumer-apis/overview) skonstruowane z określonym przeznaczeniem. Po udostępnieniu wystąpienia `IDataProtector` Wywołaj metodę rozszerzenia `IDataProtector.ToTimeLimitedDataProtector`, aby odzyskać funkcję ochrony z wbudowanymi funkcjami wygaśnięcia.

`ITimeLimitedDataProtector` udostępnia następujące metody:

* Funkcja onprotecter (przeznaczenie ciągu): ITimeLimitedDataProtector — ten interfejs API jest podobny do istniejącego `IDataProtectionProvider.CreateProtector` w tym, że może służyć do tworzenia [łańcuchów celów](xref:security/data-protection/consumer-apis/purpose-strings) z poziomu głównego ochrony ograniczonego czasowo.

* Ochrona (Byte [] — zwykły tekst, wygaśnięcie DateTimeOffset): Byte []

* Ochrona (Byte [] — zwykły tekst, okres istnienia przedziału czasu): Byte []

* Chroń (Byte [] zwykły tekst): Byte []

* Ochrona (tekst tekstowy, wygaśnięcie DateTimeOffset): ciąg

* Ochrona (tekst tekstowy, okres istnienia przedziału): ciąg

* Ochrona (ciąg tekstowy): ciąg

Oprócz podstawowych metod `Protect`, które pobierają tylko zwykły tekst, istnieją nowe przeciążenia, które umożliwiają określenie daty wygaśnięcia ładunku. Datę wygaśnięcia można określić jako datę bezwzględną (za pośrednictwem `DateTimeOffset`) lub jako względny czas (od bieżącego czasu systemowego za pośrednictwem `TimeSpan`). Jeśli zostanie wywołane Przeciążenie, które nie przyjmuje czasu wygaśnięcia, ładunek jest założono, że nigdy nie wygaśnie.

* Unprotected (Byte [] protectedData, out — wygaśnięcie DateTimeOffset): Byte []

* Unprotected (Byte [] protectedData): Byte []

* Wyłącz ochronę (ciąg protectedData, out — wygaśnięcie DateTimeOffset): ciąg

* Unprotected (String protectedData): ciąg

Metody `Unprotect` zwracają oryginalne niechronione dane. Jeśli ładunek jeszcze nie wygasł, bezwzględne wygaśnięcie jest zwracane jako opcjonalny parametr out wraz z oryginalnymi danymi niechronionymi. Jeśli ładunek wygasł, wszystkie przeciążenia metody unprotected będą generować CryptographicException.

>[!WARNING]
> Nie zaleca się używania tych interfejsów API do ochrony ładunków, które wymagają długoterminowej lub nieograniczonej trwałości. "Czy mogę zapewnić trwałe nieodwracalne odzyskanie chronionych ładunków po miesiącu?" może działać jako dobra zasada dla kciuka; Jeśli odpowiedź nie jest, deweloperzy powinni rozważyć alternatywne interfejsy API.

W poniższym przykładzie są stosowane [ścieżki kodu inne niż di](xref:security/data-protection/configuration/non-di-scenarios) do tworzenia wystąpienia systemu ochrony danych. Aby uruchomić ten przykład, upewnij się, że najpierw Dodano odwołanie do pakietu Microsoft. AspNetCore. dataprotection. Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
