---
title: Ciągi celów w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ciągi celów są używane w interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087550"
---
# <a name="purpose-strings-in-aspnet-core"></a>Ciągi celów w programie ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

Składniki, które zużywają `IDataProtectionProvider` należy przekazać unikatowy *celów* parametr `CreateProtector` metody. Celów *parametru* jest związane z zabezpieczeniami systemu ochrony danych, ponieważ umożliwia izolację między kryptograficznych konsumentów, nawet jeśli kryptograficzne klucze główne są takie same.

Gdy użytkownik określa cel, ciąg przeznaczenia służy wraz z głównym kluczy kryptograficznych wyprowadzanie podkluczy kryptograficznych unikatowy tego użytkownika. W ten sposób izoluje konsumenta przed innymi konsumentami kryptograficznych w aplikacji: składnik nie może odczytywać ładunki jego, a nie może odczytać ładunków jakikolwiek inny składnik. Izolacja Ponadto renderuje niewykonalne całych kategorii ataku składnika.

![Przykład diagramu cel](purpose-strings/_static/purposes.png)

W powyższym diagramie `IDataProtector` wystąpień A i B **nie** odczytywać siebie nawzajem ładunki, tylko ich własnych.

Ciąg cel nie musi być wpisu tajnego. Po prostu powinny być unikatowe w tym sensie, inny składnik dobrze behaved nigdy nie udostępni te same parametry przeznaczenia.

>[!TIP]
> Przy użyciu nazwy przestrzeni nazw i typ składnika, korzystanie z interfejsami API ochrony danych jest regułą, tak jak praktyk, które te informacje nigdy nie spowoduje konflikt.
>
>Składnik opracowanych przez firmę Contoso, która jest odpowiedzialna za minting tokenów elementu nośnego może używać Contoso.Security.BearerToken jako ciąg jego przeznaczenia. Lub — jeszcze lepsze - Contoso.Security.BearerToken.v1 może zostać użyty jako ciąg jego przeznaczenia. Dołączać numer wersji umożliwia przyszłej wersji do użycia Contoso.Security.BearerToken.v2 jako jej przeznaczenie i różne wersje będzie całkowicie odizolowane od siebie nawzajem, jeśli chodzi o ładunków Przejdź.

Od celów parametr `CreateProtector` jest tablicą ciągów z powyższych może już został zamiast tego określony jako `[ "Contoso.Security.BearerToken", "v1" ]`. Zezwala na tworzenie hierarchii celów i otwiera możliwość obsługi wielu dzierżawców scenariusze obejmujące usługę systemu ochrony danych.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Składniki nie zezwalać na niezaufane dane użytkowników jako jedynego źródła danych wejściowych dla celów łańcucha.
>
>Na przykład należy wziąć pod uwagę składnika Contoso.Messaging.SecureMessage, który jest odpowiedzialny za przechowywanie zabezpieczonych wiadomości. Gdyby bezpiecznych składników obsługi wiadomości do wywołania `CreateProtector([ username ])`, a następnie złośliwy użytkownik może utworzyć konto przy użyciu nazwy użytkownika "Contoso.Security.BearerToken" w celu podjęcia próby Uzyskaj składnik do wywołania `CreateProtector([ "Contoso.Security.BearerToken" ])`, przypadkowo powodując bezpiecznej wymiany komunikatów system do ładunków mennic, które mogą być uważane za tokeny uwierzytelniania.
>
>Byłoby lepsze łańcucha celów dla składnika obsługi wiadomości `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, który zapewnia izolację właściwe.

Izolacja zapewniana przez i zachowań `IDataProtectionProvider`, `IDataProtector`, i celów w następujący sposób:

* Dla danego `IDataProtectionProvider` obiektu `CreateProtector` utworzy metoda `IDataProtector` jednoznacznie powiązane zarówno obiekt `IDataProtectionProvider` obiektu, który go utworzył oraz parametr do celów, który został przekazany do metody.

* Parametr cel nie może być zerowy. (Jeśli celów jest określony jako tablicę, oznacza to, że tablicy nie może być o zerowej długości, a wszystkie elementy tablicy muszą mieć wartości null.) Cel pusty ciąg z technicznego punktu widzenia jest dozwolone, ale jest odradzane.

* Argumenty dwa cele są równoważne tylko wtedy, gdy zawierają one ten sam ciągów (przy użyciu porównania porządkowego) w tej samej kolejności. Argument jednozadaniowe jest równoważna do odpowiedniej tablicy Jednoelementowy celów.

* Dwa `IDataProtector` obiekty są równoważne tylko wtedy, gdy zostały one utworzone na podstawie odpowiednik `IDataProtectionProvider` obiektów z parametrami celów równoważnych.

* Dla danego `IDataProtector` obiektu, wywołanie `Unprotect(protectedData)` zwróci oryginalny `unprotectedData` tylko wtedy, gdy `protectedData := Protect(unprotectedData)` dla odpowiednik `IDataProtector` obiektu.

> [!NOTE]
> Nie rozważamy przypadek, gdzie niektóre składnika celowo wybiera ciągu cel, który jest znany w konflikcie z innego składnika. Takiego składnika zasadniczo jest traktowane jako złośliwe, a ten system nie mają na celu dostarczenie gwarancje bezpieczeństwa, w przypadku, gdy złośliwy kod jest już uruchomiona wewnątrz procesu roboczego.
