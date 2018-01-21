---
title: "Cel ciągów"
author: rick-anderson
description: "Ten dokument zawiera szczegóły dotyczące używania ciągów cel w interfejsy API ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b1e95c9d0aa8195aa73fddfb97a4079e67a351bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-strings"></a>Cel ciągów

<a name="data-protection-consumer-apis-purposes"></a>

Składniki, które zużywają `IDataProtectionProvider` należy przekazać unikatowy *celów* parametr `CreateProtector` — metoda. Celów *parametru* jest związane z zabezpieczeniami systemu ochrony danych, ponieważ zapewnia izolację między kryptograficznych konsumentów, nawet jeśli kluczy kryptograficznych głównych są takie same.

Konsument określa cel, ciąg cel jest używana wraz z głównym kluczy kryptograficznych pochodzić kryptograficznych podkluczy unikatowy tego użytkownika. Zidentyfikowany konsumenta z innym klientom kryptograficznych w aplikacji: składnik nie może odczytywać ładunki jego i nie można odczytać ładunków żadnych innych składników. Izolacja renderuje również praktyce całych kategorii atak składnika.

![Przykład diagramu cel](purpose-strings/_static/purposes.png)

W powyższym diagramie `IDataProtector` wystąpień A i B **nie** siebie nawzajem ładunki, tylko do odczytu własne.

Ciąg cel nie ma poufne. Po prostu powinny być unikatowe w tym sensie, brak dobrze behaved składnika kiedykolwiek udostępni te same parametry w celu.

>[!TIP]
> Używanie przestrzeni nazw i wpisz nazwę składnika korzystanie z interfejsami API ochrony danych jest regułą, jak w praktyce te informacje nie są zgodne.
>
>Składnik utworzone firmy Contoso, której jest odpowiedzialne za minting tokenów elementu nośnego może używać Contoso.Security.BearerToken jako ciąg jego przeznaczenia. Lub - lepszy - Contoso.Security.BearerToken.v1 może zostać użyty jako ciąg jego przeznaczenia. Dołączanie numer wersji umożliwia przyszłych wersji do użycia jako jej celem Contoso.Security.BearerToken.v2 i różne wersje będzie całkowicie odizolowane od siebie, ile ładunków Przejdź.

Od parametru celów `CreateProtector` jest tablicą ciągów powyższych można zamiast tego określono jako `[ "Contoso.Security.BearerToken", "v1" ]`. Umożliwia tworzenie hierarchii celów i otwiera możliwości scenariusze obsługi wielu dzierżawców przy użyciu systemu ochrony danych.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Składniki nie należy zezwalać niezaufanym użytkownikiem jako jedyne źródło danych wejściowych dla celów łańcucha.
>
>Rozważmy na przykład składnik Contoso.Messaging.SecureMessage, która jest odpowiedzialna za zapisywanie zabezpieczonych wiadomości. Gdyby bezpiecznych składników obsługi wiadomości do wywołania `CreateProtector([ username ])`, a następnie złośliwy użytkownik może utworzyć konto z nazwą użytkownika "Contoso.Security.BearerToken" w celu pobrania składników do wywołania `CreateProtector([ "Contoso.Security.BearerToken" ])`, przypadkowo powodując bezpiecznej wymiany komunikatów system do ładunków mennic, które mogą być uważane za tokeny uwierzytelniania.
>
>Lepsze łańcucha celów dla składnika obsługi komunikatów będą `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, który zapewnia izolację właściwe.

Izolacja podał i zachowania `IDataProtectionProvider`, `IDataProtector`, i do celów są następujące:

* Dla danego `IDataProtectionProvider` obiektu, `CreateProtector` utworzy metody `IDataProtector` jednoznacznie powiązany zarówno obiekt `IDataProtectionProvider` obiektu, który go utworzył oraz parametru celów, który został przekazany do metody.

* Parametr celu nie może mieć wartości null. (Jeśli celów jest określony jako tablica, oznacza to, że tablicy nie może być o zerowej długości i wszystkie elementy tablicy muszą być inne niż null.) Cel pustego ciągu technicznie jest dozwolone, ale jest niezalecane.

* Argumenty dwa cele są równoważne tylko wtedy, gdy zawierają tej samej ciągi ({numer porządkowy porównania przy użyciu) w tej samej kolejności. Argument jednozadaniowe jest odpowiednikiem odpowiednich celów pojedynczego elementu tablicy.

* Dwa `IDataProtector` obiekty są równoważne tylko wtedy, gdy są one tworzone na podstawie równoważne `IDataProtectionProvider` obiektów z parametrami celów równoważnych.

* Dla danego `IDataProtector` obiektu, wywołanie `Unprotect(protectedData)` zwróci oryginalnej `unprotectedData` tylko wtedy, gdy `protectedData := Protect(unprotectedData)` dla równoważnej `IDataProtector` obiektu.

> [!NOTE]
> Firma Microsoft nie jest uwzględnieniu przypadek, w którym niektórych składników celowo wybiera ciąg cel, który jest znana powodują konflikt z innym składnikiem. Takie składnika zasadniczo zostanie uznane za złośliwego, a ten system nie mają na celu dostarczenie gwarancje bezpieczeństwa, w przypadku, gdy złośliwy kod jest już działają w ramach procesu roboczego.
