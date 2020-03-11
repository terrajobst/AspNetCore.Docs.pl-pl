---
title: Omówienie interfejsów API odbiorców dla ASP.NET Core
author: rick-anderson
description: Otrzymuj krótkie omówienie różnych interfejsów API odbiorców dostępnych w ramach biblioteki ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666587"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Omówienie interfejsów API odbiorców dla ASP.NET Core

Interfejsy `IDataProtectionProvider` i `IDataProtector` są podstawowymi interfejsami, za pomocą których konsumenci używają systemu ochrony danych. Znajdują się one w pakiecie [Microsoft. AspNetCore. dataprotection. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) .

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Interfejs dostawcy reprezentuje główny system ochrony danych. Nie może być on bezpośrednio używany do ochrony lub wyochronowania danych. Zamiast tego konsument musi uzyskać odwołanie do `IDataProtector` przez wywołanie `IDataProtectionProvider.CreateProtector(purpose)`, gdzie cel jest ciągiem opisującym zamierzony przypadek użycia konsumenta. Zobacz [ciągi przeznaczenia](xref:security/data-protection/consumer-apis/purpose-strings) , aby uzyskać więcej informacji na temat zamiaru tego parametru i sposobu wybierania odpowiedniej wartości.

## <a name="idataprotector"></a>IDataProtector

Interfejs funkcji ochrony jest zwracany przez wywołanie do `CreateProtector`i jest to interfejs, za pomocą którego klienci mogą wykonywać operacje ochrony i nieochrony.

Aby chronić dane, przekaż je do metody `Protect`. Basic Interface definiuje metodę, która konwertuje bajt []-> Byte [], ale istnieje również Przeciążenie (dostarczone jako Metoda rozszerzenia), które konwertuje ciąg > ciągu. Zabezpieczenia oferowane przez dwie metody są identyczne. Deweloper powinien wybrać dowolne Przeciążenie, które jest najwygodniejsze dla przypadków użycia. Niezależnie od wybranego przeciążenia, wartość zwrócona przez metodę ochrony jest teraz chroniona (ENCIPHERED i nieautoryzowane), a aplikacja może wysłać ją do niezaufanego klienta.

Aby wyłączyć ochronę wcześniej chronionego fragmentu danych, Przekaż chronione dane do metody `Unprotect`. (Istnieją oparte na bajtach [] i przeciążenia oparte na ciągach dla wygody dla deweloperów). Jeśli chroniony ładunek został wygenerowany przez wcześniejsze wywołanie do `Protect` na tym samym `IDataProtector`, Metoda `Unprotect` zwróci oryginalny niechroniony ładunek. Jeśli chroniony ładunek został naruszony lub został wyprodukowany przez inny `IDataProtector`, Metoda `Unprotect` zwróci CryptographicException.

Koncepcja tego samego a różne `IDataProtector` wiąże się z koncepcją celu. Jeśli dwa wystąpienia `IDataProtector` zostały wygenerowane z tego samego katalogu głównego `IDataProtectionProvider` ale przy użyciu różnych ciągów przeznaczenie w wywołaniu `IDataProtectionProvider.CreateProtector`, są one uznawane za [różne funkcje ochrony](xref:security/data-protection/consumer-apis/purpose-strings), a nikt nie będzie w stanie wyłączyć ochrony ładunków wygenerowanych przez inne.

## <a name="consuming-these-interfaces"></a>Korzystanie z tych interfejsów

W przypadku składnika o podwójnej próbie zamierzone użycie polega na tym, że składnik przyjmuje `IDataProtectionProvider` parametr w konstruktorze, a system DI automatycznie udostępnia tę usługę podczas tworzenia wystąpienia składnika.

> [!NOTE]
> Niektóre aplikacje (takie jak aplikacje konsolowe lub aplikacje ASP.NET 4. x) mogą nie być rozróżniane, więc nie można użyć mechanizmu opisanego w tym miejscu. W tych scenariuszach zapoznaj się z dokumentem [scenariusze bez nieznanej wiedzy](xref:security/data-protection/configuration/non-di-scenarios) , aby uzyskać więcej informacji na temat uzyskiwania wystąpienia dostawcy `IDataProtection` bez przechodzenia przez di.

Poniższy przykład ilustruje trzy koncepcje:

1. [Dodaj system ochrony danych](xref:security/data-protection/configuration/overview) do kontenera usługi,

2. Przy użyciu DI do odbierania wystąpienia `IDataProtectionProvider`i

3. Tworzenie `IDataProtector` z `IDataProtectionProvider` i używanie go do ochrony i wyochronowania danych.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Pakiet Microsoft. AspNetCore. dataprotection. Abstracts zawiera metodę rozszerzenia `IServiceProvider.GetDataProtector` jako wygoda dewelopera. Jest on hermetyzowany jako pojedyncza operacja pobierająca `IDataProtectionProvider` od dostawcy usług i wywołujący `IDataProtectionProvider.CreateProtector`. Poniższy przykład demonstruje użycie.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są bezpieczne wątkowo dla wielu wywołań. Jest to zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania dla wielu wywołań `Protect` i `Unprotect`. Wywołanie `Unprotect` spowoduje zgłoszenie CryptographicException, jeśli nie można zweryfikować ani deszyfrowanie chronionego ładunku. Niektóre składniki mogą chcieć zignorować błędy podczas operacji usunięcia ochrony; składnik, który odczytuje pliki cookie uwierzytelniania, może obsłużyć ten błąd i traktować żądanie tak, jakby nie zawierało w ogóle pliku cookie, a nie niepowodzeniem. Składniki, które chcą tego zachowania, powinny zwrócić uwagę na CryptographicException zamiast połknięcia wszystkich wyjątków.
