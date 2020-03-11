---
title: Ciągi przeznaczenie w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ciągi przeznaczenia są używane w interfejsach API ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664725"
---
# <a name="purpose-strings-in-aspnet-core"></a>Ciągi przeznaczenie w ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

Składniki, które zużywają `IDataProtectionProvider` muszą przekazać *unikatowy parametr* do metody `CreateProtector`. *Parametr* cele jest związany z bezpieczeństwem systemu ochrony danych, ponieważ zapewnia on izolację między konsumentami kryptograficznymi, nawet jeśli główne klucze kryptograficzne są takie same.

Gdy odbiorca określi przeznaczenie, ciąg celu jest używany razem z głównymi kluczami kryptograficznymi w celu uzyskania kluczy kryptograficznych, które są unikatowe dla tego klienta. Spowoduje to oddzielenie konsumenta od wszystkich innych użytkowników kryptograficznych w aplikacji: żaden inny składnik nie może odczytywać swoich ładunków i nie może odczytać żadnych innych ładunków składnika. Ta izolacja również renderuje w sposób niewykonalny cały poziom ataku na składnik.

![Przykład diagramu przeznaczenie](purpose-strings/_static/purposes.png)

Na powyższym diagramie `IDataProtector` wystąpienia A i B **nie mogą** odczytywać wszystkich ładunków, tylko ich własnymi.

Ciąg celu nie musi być tajny. Powinien być po prostu unikatowy w sensie, że żaden inny dobrze działający składnik nie będzie miał tego samego ciągu celu.

>[!TIP]
> Użycie przestrzeni nazw i nazwy typu składnika korzystającego z interfejsów API ochrony danych jest dobrą regułą kciuka, jak w przypadku te informacje nigdy nie będą powodować konfliktów.
>
>Składnik firmy Contoso, który jest odpowiedzialny za tokeny okaziciela minting, może używać contoso. Security. BearerToken jako ciągu celu. Lub — jeszcze lepsze — może używać contoso. Security. BearerToken. v1 jako ciągu celu. Dołączenie numeru wersji umożliwia użycie w przyszłości firmy Contoso. Security. BearerToken. v2, a różne wersje są całkowicie odizolowane od siebie.

Ponieważ parametr cele do `CreateProtector` jest tablicą ciągów, zamiast tego można określić powyższe polecenie jako `[ "Contoso.Security.BearerToken", "v1" ]`. Pozwala to na utworzenie hierarchii celów i otwarcie możliwości scenariuszy wielodostępnych z systemem ochrony danych.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Składniki nie powinny zezwalać na dostęp niezaufanych użytkowników jako jedyne źródło danych wejściowych dla łańcucha celów.
>
>Rozważmy na przykład składnik contoso. Messaging. SecureMessage, który jest odpowiedzialny za przechowywanie bezpiecznych komunikatów. Jeśli składnik zabezpieczonych komunikatów był do wywołania `CreateProtector([ username ])`, złośliwy użytkownik może utworzyć konto z nazwą użytkownika "contoso. Security. BearerToken" w celu uzyskania składnika, który wywoła `CreateProtector([ "Contoso.Security.BearerToken" ])`, a tym samym przypadkowo powoduje, że bezpieczny system obsługi komunikatów mennic ładunki, które mogą być postrzegane jako tokeny uwierzytelniania.
>
>Łańcuch lepszych celów dla składnika obsługi komunikatów mógłby być `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, który zapewnia właściwą izolację.

Izolacja zapewniana przez program oraz zachowania `IDataProtectionProvider`, `IDataProtector`i cele są następujące:

* Dla danego obiektu `IDataProtectionProvider` Metoda `CreateProtector` utworzy obiekt `IDataProtector`, który został jednoznacznie powiązany zarówno z obiektem `IDataProtectionProvider`, który go utworzył, jak i parametrem cele, który został przekazany do metody.

* Parametr celu nie może mieć wartości null. (Jeśli cele są określone jako tablica, oznacza to, że tablica nie może mieć zerowej długości, a wszystkie elementy tablicy muszą być inne niż null.) Pusty cel ciągu jest technicznie dozwolony, ale nie jest odradzany.

* Dwa cele argumentów są równoważne, jeśli i tylko wtedy, gdy zawierają te same ciągi (przy użyciu porównującej porządkowej) w tej samej kolejności. Argument pojedynczego celu jest równoważny z odpowiadającą mu tablicą celów pojedynczego elementu.

* Dwa obiekty `IDataProtector` są równoważne i tylko wtedy, gdy są tworzone z odpowiedników obiektów `IDataProtectionProvider` z równoważnymi parametrami celów.

* Dla danego obiektu `IDataProtector` wywołanie `Unprotect(protectedData)` zwróci oryginalny `unprotectedData` Jeśli i tylko wtedy, gdy `protectedData := Protect(unprotectedData)` dla równoważnego obiektu `IDataProtector`.

> [!NOTE]
> Nie rozważamy przypadków, w których jakiś składnik celowo wybiera ciąg celu, który jest znany w konflikcie z innym składnikiem. Taki składnik powinien być uważany za złośliwy i ten system nie jest przeznaczony do zapewnienia gwarancji bezpieczeństwa w przypadku, gdy złośliwy kod jest już uruchomiony w procesie roboczym.
