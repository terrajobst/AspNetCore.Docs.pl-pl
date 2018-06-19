---
title: Omówienie interfejsów API klienta dla platformy ASP.NET Core
author: rick-anderson
description: Odbieranie krótki przegląd różnych konsumenta interfejsami API dostępnymi w bibliotece platformy ASP.NET Core ochrony danych.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 5d161ed8fbc39bcf4a970644480b4e909810b555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
ms.locfileid: "30076010"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Omówienie interfejsów API klienta dla platformy ASP.NET Core

`IDataProtectionProvider` i `IDataProtector` interfejsy są podstawowe interfejsy, za pomocą których użytkowników na korzystanie z systemu ochrony danych. W przypadku się one w [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pakietu.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Interfejs dostawcy reprezentuje katalog główny systemu ochrony danych. Nie można go bezpośrednio służyć do Włączanie lub wyłączanie ochrony danych. Zamiast tego klient musi pobrać odwołanie do `IDataProtector` przez wywołanie metody `IDataProtectionProvider.CreateProtector(purpose)`, której celem jest ciąg, który opisuje zamierzone konsumenta przypadek użycia. Zobacz [ciągów w celu](xref:security/data-protection/consumer-apis/purpose-strings) znacznie więcej informacji na celem tego parametru i jak wybrać odpowiednią wartość.

## <a name="idataprotector"></a>IDataProtector

Interfejs ochrony jest zwracany przez wywołanie do `CreateProtector`, a jego użytkowników można używać do wykonywania tego interfejsu ustawiania i usuwania ochrony operacji.

Aby chronić element danych, należy przekazać dane do `Protect` metody. Podstawowy interfejs definiuje metody, które byte [] konwertuje -> byte [], ale jest także przeciążenia (pod warunkiem jako metodę rozszerzenie) konwertuje ciągu -> ciągu. Zabezpieczenia oferowane przez te dwie metody są identyczne; Deweloper powinien wybrać, niezależnie od przeciążenia jest najbardziej odpowiednim ich przypadek użycia. Niezależnie od przeciążenia wybrane, wartość zwracana przez Chroń metody są teraz chronione (enciphered oraz potwierdzone odporne na próby), a aplikacja może przesyłać je do niezaufanego klienta.

Aby wyłączyć ochronę element poprzednio chronionych danych, należy przekazać chronionych danych `Unprotect` metody. (Brak byte [] — na podstawie i na podstawie ciągu przeciążenia dla wygody deweloperów.) Jeśli chronione ładunek został wygenerowany przez wywołanie wcześniejszych `Protect` na tym samym `IDataProtector`, `Unprotect` metoda zwróci oryginalnego ładunku niechronione. Jeśli chroniony ładunku została naruszona lub został utworzony przez inną `IDataProtector`, `Unprotect` metoda zgłosi cryptographicexception —.

Pojęcie sam a inną `IDataProtector` powiązań z powrotem do koncepcji cel. Jeśli dwa `IDataProtector` wystąpienia zostały wygenerowane z tego samego głównego `IDataProtectionProvider` , ale za pomocą innego celu ciągów w wywołaniu `IDataProtectionProvider.CreateProtector`, następnie jest uznawany za [różne funkcje ochrony](xref:security/data-protection/consumer-apis/purpose-strings), i nie będzie mógł wyłączyć ochronę ładunki generowane przez innych.

## <a name="consuming-these-interfaces"></a>Korzystanie z tych interfejsów

Składnik obsługujący Podpisane zamierzonego użycia jest, że składnik stosować `IDataProtectionProvider` parametr w swoich konstruktorach i czy system Podpisane automatycznie udostępnia tę usługę, gdy składnik zostanie uruchomiony.

> [!NOTE]
> Niektóre aplikacje (np. aplikacje konsoli lub aplikacji ASP.NET 4.x) może nie być Podpisane obsługujący, nie można używać mechanizmu opisane w tym miejscu. Dla tych scenariuszy, zapoznaj się [z systemem innym niż scenariusze pamiętać Podpisane](xref:security/data-protection/configuration/non-di-scenarios) dokumentu, aby uzyskać więcej informacji na temat pobierania wystąpienia `IDataProtection` dostawcy bez pośrednictwa Podpisane.

W poniższym przykładzie pokazano trzy pojęcia:

1. [Dodaj systemu ochrony danych](xref:security/data-protection/configuration/overview) do kontenera usługi

2. Przy użyciu Podpisane do odbierania wystąpienia `IDataProtectionProvider`, i

3. Tworzenie `IDataProtector` z `IDataProtectionProvider` i używanie go do zabezpieczania i odbezpieczania danych.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Pakiet Microsoft.AspNetCore.DataProtection.Abstractions zawiera metody rozszerzenia `IServiceProvider.GetDataProtector` dla wygody deweloperów. Hermetyzuje jako jedna operacja pobierania zarówno `IDataProtectionProvider` od dostawcy usług i wywoływania `IDataProtectionProvider.CreateProtector`. W poniższym przykładzie pokazano sposób jego użycia.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań. Ma ona która po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`. Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich. Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.
