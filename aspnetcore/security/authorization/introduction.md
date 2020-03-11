---
title: Wprowadzenie do autoryzacji w ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe informacje dotyczące autoryzacji i sposobu działania autoryzacji w aplikacjach ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660714"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="7f78b-103">Wprowadzenie do autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f78b-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="7f78b-104">Autoryzacja odnosi się do procesu, który określa, co użytkownik może zrobić.</span><span class="sxs-lookup"><span data-stu-id="7f78b-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="7f78b-105">Na przykład użytkownik administracyjny może utworzyć bibliotekę dokumentów, dodać dokumenty, edytować dokumenty i je usunąć.</span><span class="sxs-lookup"><span data-stu-id="7f78b-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="7f78b-106">Użytkownik niebędący użytkownikiem administracyjnym, który współpracuje z biblioteką, ma uprawnienia tylko do odczytu dokumentów.</span><span class="sxs-lookup"><span data-stu-id="7f78b-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="7f78b-107">Autoryzacja jest prostopadła i niezależna od uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7f78b-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="7f78b-108">Autoryzacja wymaga jednak mechanizmu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7f78b-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="7f78b-109">Uwierzytelnianie to proces ustalania, kto jest użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="7f78b-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="7f78b-110">Uwierzytelnianie może utworzyć jedną lub więcej tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7f78b-110">Authentication may create one or more identities for the current user.</span></span>

<span data-ttu-id="7f78b-111">Aby uzyskać więcej informacji na temat uwierzytelniania w ASP.NET Core, zobacz <xref:security/authentication/index>.</span><span class="sxs-lookup"><span data-stu-id="7f78b-111">For more information about authentication in ASP.NET Core, see <xref:security/authentication/index>.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="7f78b-112">Typy autoryzacji</span><span class="sxs-lookup"><span data-stu-id="7f78b-112">Authorization types</span></span>

<span data-ttu-id="7f78b-113">Autoryzacja ASP.NET Core zapewnia prostą, deklaratywną [rolę](xref:security/authorization/roles) i bogaty model [oparty na zasadach](xref:security/authorization/policies) .</span><span class="sxs-lookup"><span data-stu-id="7f78b-113">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="7f78b-114">Autoryzacja jest wyrażona w wymaganiach, a programy obsługi szacują oświadczenia użytkownika względem wymagań.</span><span class="sxs-lookup"><span data-stu-id="7f78b-114">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="7f78b-115">Bezwzględne kontrole mogą opierać się na prostych zasadach lub zasadach, które obliczają zarówno tożsamość użytkownika, jak i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="7f78b-115">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="7f78b-116">Przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="7f78b-116">Namespaces</span></span>

<span data-ttu-id="7f78b-117">Składniki autoryzacji, w tym atrybuty `AuthorizeAttribute` i `AllowAnonymousAttribute`, znajdują się w przestrzeni nazw `Microsoft.AspNetCore.Authorization`.</span><span class="sxs-lookup"><span data-stu-id="7f78b-117">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="7f78b-118">Zapoznaj się z dokumentacją dotyczącą [prostej autoryzacji](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="7f78b-118">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
