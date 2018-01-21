---
title: Wprowadzenie do autoryzacji
author: rick-anderson
description: "Ten dokument zawiera podstawowe informacje dotyczące autoryzacji i wyjaśniono, jak autoryzacji odnosi się do platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 6f4f1fb4f2776db10a1640049885e31e9a54011a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction"></a>Wprowadzenie

<a name="security-authorization-introduction"></a>

Autoryzacji odwołuje się do proces, który określa, jakie użytkownik jest w stanie wykonać. Na przykład użytkownik administracyjny jest możliwość utworzenia biblioteki dokumentów, dodawać dokumenty edycji dokumentów i usuń je. Użytkownika niebędącego administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.

Autoryzacja jest prostopadły i niezależnie od uwierzytelniania, które polega na upewnieniu się, kto jest użytkownikiem. Uwierzytelnianie może utworzyć co najmniej jeden tożsamości dla bieżącego użytkownika.

## <a name="authorization-types"></a>Typy autoryzacji

Platformy ASP.NET Core autoryzacji udostępnia prostą deklaratywne [roli](roles.md) i [sformatowanego opartej na zasadach](policies.md) modelu. Autoryzacji jest wyrażona w wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań. Kontrole konieczne może bazować na prostego lub zasady, których ocena tożsamości użytkownika i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.

## <a name="namespaces"></a>Namespaces

Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybutów znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.
