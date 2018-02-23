---
title: Wprowadzenie do autoryzacji
author: rick-anderson
description: "Ten dokument zawiera podstawowe informacje dotyczące autoryzacji i wyjaśniono, jak autoryzacji odnosi się do platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Wprowadzenie

<a name="security-authorization-introduction"></a>

Autoryzacji odwołuje się do proces, który określa, jakie użytkownik jest w stanie wykonać. Na przykład użytkownik administracyjny jest możliwość utworzenia biblioteki dokumentów, dodawać dokumenty edycji dokumentów i usuń je. Użytkownika niebędącego administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.

Autoryzacja jest prostopadły i niezależnie od uwierzytelniania, które polega na upewnieniu się, kto jest użytkownikiem. Uwierzytelnianie może utworzyć co najmniej jeden tożsamości dla bieżącego użytkownika.

## <a name="authorization-types"></a>Typy autoryzacji

Platformy ASP.NET Core autoryzacji udostępnia prostą, deklaratywne [roli](roles.md) i wzbogaconej [opartych na zasadach](policies.md) modelu. Autoryzacji jest wyrażona w wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań. Kontrole konieczne może bazować na prostego lub zasady, których ocena tożsamości użytkownika i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.

## <a name="namespaces"></a>Namespaces

Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybuty, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.

Zapoznaj się z dokumentacją na [proste autoryzacji](xref:security/authorization/simple).
