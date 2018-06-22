---
title: Wprowadzenie do autoryzacji w ASP.NET Core
author: rick-anderson
description: Poznaj podstawy autoryzacji i działanie autoryzacji w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276870"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Wprowadzenie do autoryzacji w ASP.NET Core

<a name="security-authorization-introduction"></a>

Autoryzacji odwołuje się do proces, który określa, jakie użytkownik jest w stanie wykonać. Na przykład użytkownik administracyjny jest możliwość utworzenia biblioteki dokumentów, dodawać dokumenty edycji dokumentów i usuń je. Użytkownika niebędącego administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.

Autoryzacja jest prostopadły i niezależnie od uwierzytelniania. Mechanizm uwierzytelniania wymaga jednak autoryzacji. Uwierzytelnianie to proces upewnieniu się, kto jest użytkownikiem. Uwierzytelnianie może utworzyć co najmniej jeden tożsamości dla bieżącego użytkownika.

## <a name="authorization-types"></a>Typy autoryzacji

Platformy ASP.NET Core autoryzacji udostępnia prostą, deklaratywne [roli](xref:security/authorization/roles) i wzbogaconej [opartych na zasadach](xref:security/authorization/policies) modelu. Autoryzacji jest wyrażona w wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań. Kontrole konieczne może bazować na prostego lub zasady, których ocena tożsamości użytkownika i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.

## <a name="namespaces"></a>Namespaces

Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybuty, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.

Zapoznaj się z dokumentacją na [proste autoryzacji](xref:security/authorization/simple).
