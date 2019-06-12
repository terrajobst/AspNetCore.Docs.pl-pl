---
title: Omówienie interfejsów API przeznaczonych dla klientów dla platformy ASP.NET Core
author: rick-anderson
description: Odbierać krótkie omówienie różnych odbiorców interfejsami API dostępnymi w bibliotece programu ASP.NET Core ochrony danych.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837376"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Omówienie interfejsów API przeznaczonych dla klientów dla platformy ASP.NET Core

`IDataProtectionProvider` i `IDataProtector` interfejsy są podstawowe interfejsy, za pomocą których użytkownicy korzystają z systemu ochrony danych. Znajdują się one w [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pakietu.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Interfejs dostawcy reprezentuje katalog główny system ochrony danych. Nie można go bezpośrednio służyć do ochrony lub wyłączanie ochrony danych. Zamiast tego użytkownik musi uzyskać odwołanie do `IDataProtector` przez wywołanie metody `IDataProtectionProvider.CreateProtector(purpose)`, której celem jest ciąg, który opisuje przypadek użycia zamierzonych odbiorców. Zobacz [ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings) uzyskać znacznie więcej informacji na temat celem tego parametru i jak wybrać odpowiednią wartość.

## <a name="idataprotector"></a>IDataProtector

Interfejs ochrony jest zwracany przez wywołanie `CreateProtector`, a jego ten interfejs, który użytkownicy mogą używać do wykonywania włączania i wyłączania ochrony operacji.

Aby chronić część danych, należy przekazać dane do `Protect` metody. Podstawowy interfejs definiuje metody, które byte [] konwertuje -> byte [], ale ma również przeciążenia (udostępniane jako metodę rozszerzenia), która konwertuje ciąg -> ciągu. Zabezpieczenia oferowanych przez te dwie metody jest identyczna; Deweloper należy wybrać, niezależnie od przeciążenia jest najbardziej odpowiednim ich przypadkach użycia. Niezależnie od przeciążenia wybrane, wartość zwracana przez ochrony jest teraz chroniony (enciphered i potwierdzone odporne), a aplikacja może wysłać ją do niezaufanego klienta.

Aby wyłączyć ochronę elementu poprzednio chronionych danych, należy przekazać chronionych danych `Unprotect` metody. (Brak byte [] — przeciążenia opartego na ciąg i zależności dla wygody deweloperów.) Jeśli chronione ładunek został wygenerowany przez podczas wcześniejszego wywołania `Protect` na tym samym `IDataProtector`, `Unprotect` metoda zwróci oryginalnego ładunku niechronione. Jeśli chronione ładunku została naruszona lub został utworzony przez inną `IDataProtector`, `Unprotect` metoda zgłosi cryptographicexception —.

Pojęcie tego samego a inną `IDataProtector` więzi z powrotem do koncepcji przeznaczenia. Dwóch `IDataProtector` wystąpienia zostały wygenerowane z takim samym certyfikatem głównym `IDataProtectionProvider` za pośrednictwem innego celu ciągów w wywołaniu, ale `IDataProtectionProvider.CreateProtector`, a następnie były uważane za [różne funkcje ochrony kluczy](xref:security/data-protection/consumer-apis/purpose-strings), i nie będzie mogła zostać wyłączona ochrona ładunki generowane przez drugi.

## <a name="consuming-these-interfaces"></a>Korzystanie z tych interfejsów

DI aware składnika, że składnik przyjmowana jest zamierzone użycie `IDataProtectionProvider` parametru w jego konstruktorze i czy DI system automatycznie udostępnia tę usługę, podczas tworzenia wystąpienia składnika.

> [!NOTE]
> Niektóre aplikacje (na przykład aplikacje konsoli lub aplikacji programu ASP.NET 4.x) może nie być DI-aware, więc nie można użyć mechanizm opisane w tym miejscu. Dla tych scenariuszy, zapoznaj się z [innych scenariuszy pamiętać DI](xref:security/data-protection/configuration/non-di-scenarios) dokumentu, aby uzyskać więcej informacji na temat pobierania wystąpienia `IDataProtection` dostawcy bez pośrednictwa DI.

W poniższym przykładzie pokazano trzy pojęcia:

1. [Dodaj system ochrony danych](xref:security/data-protection/configuration/overview) do kontenera usługi

2. Za pomocą DI do odbierania wystąpienia `IDataProtectionProvider`, i

3. Tworzenie `IDataProtector` z `IDataProtectionProvider` i używanie ich do włączania i wyłączania ochrony danych.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Pakiet Microsoft.AspNetCore.DataProtection.Abstractions zawiera metodę rozszerzającą `IServiceProvider.GetDataProtector` jako udogodnienie dla deweloperów. Hermetyzuje jako pojedyncza operacja zarówno pobieranie `IDataProtectionProvider` od dostawcy usług i wywoływania `IDataProtectionProvider.CreateProtector`. W poniższym przykładzie pokazano jej użycie.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Wystąpienia elementu `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu obiektów wywołujących. Ma zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` poprzez wywołanie `CreateProtector`, użyje tego odwołania dla wielu wywołań `Protect` i `Unprotect`. Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który odczytuje pliki cookie uwierzytelniania może obsługiwać ten błąd i traktować żądania tak, jakby był w ogóle pliki cookie nie zamiast zwraca Niepowodzenie żądania od razu wykupić. Składniki, które chcesz tego zachowania, należy w szczególności złapać cryptographicexception — zamiast powodu spożycia wszystkie wyjątki.
