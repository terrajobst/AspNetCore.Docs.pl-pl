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
# <a name="introduction"></a><span data-ttu-id="4e910-103">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="4e910-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="4e910-104">Autoryzacji odwołuje się do proces, który określa, jakie użytkownik jest w stanie wykonać.</span><span class="sxs-lookup"><span data-stu-id="4e910-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="4e910-105">Na przykład użytkownik administracyjny jest możliwość utworzenia biblioteki dokumentów, dodawać dokumenty edycji dokumentów i usuń je.</span><span class="sxs-lookup"><span data-stu-id="4e910-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="4e910-106">Użytkownika niebędącego administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.</span><span class="sxs-lookup"><span data-stu-id="4e910-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="4e910-107">Autoryzacja jest prostopadły i niezależnie od uwierzytelniania, które polega na upewnieniu się, kto jest użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="4e910-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="4e910-108">Uwierzytelnianie może utworzyć co najmniej jeden tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4e910-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="4e910-109">Typy autoryzacji</span><span class="sxs-lookup"><span data-stu-id="4e910-109">Authorization types</span></span>

<span data-ttu-id="4e910-110">Platformy ASP.NET Core autoryzacji udostępnia prostą, deklaratywne [roli](roles.md) i wzbogaconej [opartych na zasadach](policies.md) modelu.</span><span class="sxs-lookup"><span data-stu-id="4e910-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="4e910-111">Autoryzacji jest wyrażona w wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań.</span><span class="sxs-lookup"><span data-stu-id="4e910-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="4e910-112">Kontrole konieczne może bazować na prostego lub zasady, których ocena tożsamości użytkownika i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="4e910-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="4e910-113">Namespaces</span><span class="sxs-lookup"><span data-stu-id="4e910-113">Namespaces</span></span>

<span data-ttu-id="4e910-114">Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybuty, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="4e910-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="4e910-115">Zapoznaj się z dokumentacją na [proste autoryzacji](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="4e910-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
