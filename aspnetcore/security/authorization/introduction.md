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
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="b910c-103">Wprowadzenie do autoryzacji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b910c-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="b910c-104">Autoryzacja Określa, do procesu, który określa, co użytkownik będzie mógł robić.</span><span class="sxs-lookup"><span data-stu-id="b910c-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="b910c-105">Na przykład użytkownik administracyjny może utworzyć bibliotekę dokumentów, dodawanie dokumentów, edytowanie dokumentów i je usunąć.</span><span class="sxs-lookup"><span data-stu-id="b910c-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="b910c-106">Użytkownik niebędący administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.</span><span class="sxs-lookup"><span data-stu-id="b910c-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="b910c-107">Autoryzacja jest prostopadły i niezależny od uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b910c-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="b910c-108">Autoryzacja wymaga jednak mechanizm uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b910c-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="b910c-109">Uwierzytelnianie to proces upewnieniu się, kim jest użytkownik.</span><span class="sxs-lookup"><span data-stu-id="b910c-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="b910c-110">Uwierzytelnianie może utworzyć jedną lub więcej tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b910c-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="b910c-111">Typy autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b910c-111">Authorization types</span></span>

<span data-ttu-id="b910c-112">Autoryzacji platformy ASP.NET Core zawiera prostego, deklaratywnego [roli](xref:security/authorization/roles) i zaawansowane [oparte na zasadach](xref:security/authorization/policies) modelu.</span><span class="sxs-lookup"><span data-stu-id="b910c-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="b910c-113">Autoryzacji jest będzie wyrażana wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań.</span><span class="sxs-lookup"><span data-stu-id="b910c-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="b910c-114">Imperatywne sprawdzanie może bazować na proste lub zasady, które ocenić tożsamości użytkownika i właściwości zasobu, który użytkownik próbuje uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="b910c-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="b910c-115">Namespaces</span><span class="sxs-lookup"><span data-stu-id="b910c-115">Namespaces</span></span>

<span data-ttu-id="b910c-116">Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybutów, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="b910c-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="b910c-117">Zapoznaj się z dokumentacją na [autoryzacja prosta](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="b910c-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
