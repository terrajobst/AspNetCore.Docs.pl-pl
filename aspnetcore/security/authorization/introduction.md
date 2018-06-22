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
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="6fde8-103">Wprowadzenie do autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6fde8-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="6fde8-104">Autoryzacji odwołuje się do proces, który określa, jakie użytkownik jest w stanie wykonać.</span><span class="sxs-lookup"><span data-stu-id="6fde8-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="6fde8-105">Na przykład użytkownik administracyjny jest możliwość utworzenia biblioteki dokumentów, dodawać dokumenty edycji dokumentów i usuń je.</span><span class="sxs-lookup"><span data-stu-id="6fde8-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="6fde8-106">Użytkownika niebędącego administratorem Praca z biblioteką tylko jest upoważniony do odczytu w dokumentach.</span><span class="sxs-lookup"><span data-stu-id="6fde8-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="6fde8-107">Autoryzacja jest prostopadły i niezależnie od uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="6fde8-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="6fde8-108">Mechanizm uwierzytelniania wymaga jednak autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="6fde8-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="6fde8-109">Uwierzytelnianie to proces upewnieniu się, kto jest użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="6fde8-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="6fde8-110">Uwierzytelnianie może utworzyć co najmniej jeden tożsamości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6fde8-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="6fde8-111">Typy autoryzacji</span><span class="sxs-lookup"><span data-stu-id="6fde8-111">Authorization types</span></span>

<span data-ttu-id="6fde8-112">Platformy ASP.NET Core autoryzacji udostępnia prostą, deklaratywne [roli](xref:security/authorization/roles) i wzbogaconej [opartych na zasadach](xref:security/authorization/policies) modelu.</span><span class="sxs-lookup"><span data-stu-id="6fde8-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="6fde8-113">Autoryzacji jest wyrażona w wymagania i procedury obsługi ocenić oświadczenia użytkownika względem wymagań.</span><span class="sxs-lookup"><span data-stu-id="6fde8-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="6fde8-114">Kontrole konieczne może bazować na prostego lub zasady, których ocena tożsamości użytkownika i właściwości zasobu, do którego użytkownik próbuje uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="6fde8-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="6fde8-115">Namespaces</span><span class="sxs-lookup"><span data-stu-id="6fde8-115">Namespaces</span></span>

<span data-ttu-id="6fde8-116">Składniki autoryzacji, w tym `AuthorizeAttribute` i `AllowAnonymousAttribute` atrybuty, znajdują się w `Microsoft.AspNetCore.Authorization` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6fde8-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="6fde8-117">Zapoznaj się z dokumentacją na [proste autoryzacji](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="6fde8-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
