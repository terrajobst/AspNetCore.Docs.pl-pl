---
title: "Omówienie interfejsów API klienta"
author: rick-anderson
description: "Ten dokument zawiera krótki przegląd różnych konsumenta interfejsami API dostępnymi w bibliotece platformy ASP.NET Core ochrony danych."
keywords: "Platformy ASP.NET Core, ochrony danych konsumentów interfejsów API"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: c80ed22776b0dbbaccb686f8c2ea534e6f5d9a74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="consumer-apis-overview"></a>Omówienie interfejsów API klienta

`IDataProtectionProvider` i `IDataProtector` interfejsy są podstawowe interfejsy, za pomocą których użytkowników na korzystanie z systemu ochrony danych. Znajdują się one w [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pakietu.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Interfejs dostawcy reprezentuje katalog główny systemu ochrony danych. Nie można go bezpośrednio służyć do Włączanie lub wyłączanie ochrony danych. Zamiast tego klient musi pobrać odwołanie do `IDataProtector` przez wywołanie metody `IDataProtectionProvider.CreateProtector(purpose)`, której celem jest ciąg, który opisuje zamierzone konsumenta przypadek użycia. Zobacz [ciągów w celu](purpose-strings.md) znacznie więcej informacji na celem tego parametru i jak wybrać odpowiednią wartość.

## <a name="idataprotector"></a>Interfejsu IDataProtector

Interfejs ochrony jest zwracany przez wywołanie do `CreateProtector`, i jest to interfejs, który użytkowników można używać do wykonywania ustawiania i usuwania ochrony operacji.

Aby chronić element danych, należy przekazać dane do `Protect` metody. Podstawowy interfejs definiuje metody, które byte [] konwertuje -> byte [], ale jest także przeciążenia (pod warunkiem jako metodę rozszerzenie) konwertuje ciągu -> ciągu. Zabezpieczenia oferowane przez te dwie metody są identyczne; Deweloper powinien wybrać, niezależnie od przeciążenia jest najbardziej odpowiednim ich przypadek użycia. Niezależnie od przeciążenia wybrane, wartość zwracana przez Chroń metody są teraz chronione (enciphered oraz potwierdzone odporne na próby), a aplikacja może przesyłać je do niezaufanego klienta.

Aby wyłączyć ochronę element poprzednio chronionych danych, należy przekazać chronionych danych `Unprotect` metody. (Brak byte [] — na podstawie i na podstawie ciągu przeciążenia dla wygody deweloperów.) Jeśli chronione ładunek został wygenerowany przez wywołanie wcześniejszych `Protect` na tym samym `IDataProtector`, `Unprotect` metoda zwróci oryginalnego ładunku niechronione. Jeśli chroniony ładunku została naruszona lub został utworzony przez inną `IDataProtector`, `Unprotect` metoda zgłosi cryptographicexception —.

Pojęcie sam a inną `IDataProtector` powiązań z powrotem do koncepcji cel. Jeśli dwa `IDataProtector` wystąpienia zostały wygenerowane z tego samego głównego `IDataProtectionProvider` , ale za pomocą innego celu ciągów w wywołaniu `IDataProtectionProvider.CreateProtector`, następnie są one uznawane za [różne funkcje ochrony](purpose-strings.md), i jedną nie będzie można wyłączyć ochrony ładunki generowane przez innych.

## <a name="consuming-these-interfaces"></a>Korzystanie z tych interfejsów

Składnik obsługujący Podpisane zamierzonego użycia jest, że składnik stosować `IDataProtectionProvider` parametr w swoich konstruktorach i czy system Podpisane automatycznie udostępnia tę usługę, gdy składnik zostanie uruchomiony.

> [!NOTE]
> Niektóre aplikacje (np. aplikacje konsoli lub aplikacji ASP.NET 4.x) może nie być Podpisane obsługujący, nie można używać mechanizmu opisane w tym miejscu. Dla tych scenariuszy, zapoznaj się [z systemem innym niż scenariusze pamiętać Podpisane](../configuration/non-di-scenarios.md) dokumentu, aby uzyskać więcej informacji na temat pobierania wystąpienia `IDataProtection` dostawcy bez pośrednictwa Podpisane.

W poniższym przykładzie pokazano trzy pojęcia:

1. [Dodawanie systemu ochrony danych](../configuration/overview.md) do kontenera usługi

2. Przy użyciu Podpisane do odbierania wystąpienia `IDataProtectionProvider`, i

3. Tworzenie `IDataProtector` z `IDataProtectionProvider` i używanie go do zabezpieczania i odbezpieczania danych.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Pakiet Microsoft.AspNetCore.DataProtection.Abstractions zawiera metody rozszerzenia `IServiceProvider.GetDataProtector` dla wygody deweloperów. Hermetyzuje jako jedna operacja pobierania zarówno `IDataProtectionProvider` od dostawcy usług i wywoływania `IDataProtectionProvider.CreateProtector`. W poniższym przykładzie pokazano sposób jego użycia.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań. Jest zamierzone, który po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`. Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich. Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.
