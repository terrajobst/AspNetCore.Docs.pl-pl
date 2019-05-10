---
title: Wprowadzenie do autoryzacji w programie ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe zasady autoryzacji i działanie autoryzacji w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902845"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Wprowadzenie do autoryzacji w programie ASP.NET Core

<a name="security-authorization-introduction"></a>

Autoryzacja Określa, do procesu, który określa, co użytkownik będzie mógł robić. Na przykład użytkownik administracyjny może utworzyć bibliotekę dokumentów, dodawanie dokumentów, edytowanie dokumentów i je usunąć. Użytkownik niebędący administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.

Autoryzacja jest prostopadły i niezależny od uwierzytelniania. Autoryzacja wymaga jednak mechanizm uwierzytelniania. Uwierzytelnianie to proces upewnieniu się, kim jest użytkownik. Uwierzytelnianie może utworzyć jedną lub więcej tożsamości dla bieżącego użytkownika.

## <a name="authorization-types"></a>Typy autoryzacji

Autoryzacji platformy ASP.NET Core zawiera prostego, deklaratywnego [roli](xref:security/authorization/roles) i zaawansowane [oparte na zasadach](xref:security/authorization/policies) modelu. Autoryzacji jest będzie wyrażana wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań. Imperatywne sprawdzanie może bazować na proste lub zasady, które ocenić tożsamości użytkownika i właściwości zasobu, który użytkownik próbuje uzyskać dostęp.

## <a name="namespaces"></a>Namespaces

Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybutów, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.

Zapoznaj się z dokumentacją na [autoryzacja prosta](xref:security/authorization/simple).
