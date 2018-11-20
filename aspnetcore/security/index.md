---
title: Przegląd zabezpieczeń platformy ASP.NET Core
author: tdykstra
description: Poznaj podstawowe informacje dotyczące uwierzytelniania, autoryzacji i zabezpieczeń w programie ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 579e472e01efd08bbafe949e37a3b655a42a5b46
ms.sourcegitcommit: 04b55a5ce9d649ff2df926157ec28ae47afe79e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52156922"
---
# <a name="overview-of-aspnet-core-security"></a><span data-ttu-id="39acd-103">Przegląd zabezpieczeń platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39acd-103">Overview of ASP.NET Core Security</span></span>

<span data-ttu-id="39acd-104">Platforma ASP.NET Core umożliwia deweloperom łatwe konfigurowanie i zarządzanie zabezpieczeniami dla swoich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39acd-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="39acd-105">Platforma ASP.NET Core zawiera funkcje zarządzania uwierzytelniania, autoryzacji, ochrony danych, Wymuszanie protokołu SSL, wpisy tajne aplikacji, ochrona przed fałszerstwem żądań ochrony i zarządzanie mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="39acd-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="39acd-106">Te funkcje zabezpieczeń pozwalają na tworzenie niezawodnych, ale zabezpieczanie aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39acd-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span>

## <a name="aspnet-core-security-features"></a><span data-ttu-id="39acd-107">Funkcje zabezpieczeń platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39acd-107">ASP.NET Core security features</span></span>

<span data-ttu-id="39acd-108">Platforma ASP.NET Core udostępnia wiele narzędzi i bibliotek, aby zabezpieczyć swoje aplikacje, w tym Wbudowani dostawcy tożsamości, ale można użyć 3rd usług tożsamości innych firm, takich jak Facebook, Twitter i LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="39acd-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="39acd-109">Platforma ASP.NET Core umożliwia łatwe zarządzanie wpisy tajne aplikacji, które służą do przechowywania i korzystania z poufnych informacji bez ujawniania ich w kodzie.</span><span class="sxs-lookup"><span data-stu-id="39acd-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span>

## <a name="authentication-vs-authorization"></a><span data-ttu-id="39acd-110">Uwierzytelnianie programu vs. Autoryzacja</span><span class="sxs-lookup"><span data-stu-id="39acd-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="39acd-111">Uwierzytelnianie jest procesem, w którym użytkownik podaje poświadczenia, które następnie są porównywane te, przechowywane w systemie operacyjnym, bazy danych, aplikacji lub zasobów.</span><span class="sxs-lookup"><span data-stu-id="39acd-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="39acd-112">Jeśli są zgodne, użytkownikom pomyślnie uwierzytelnione i można wykonać akcje, które otrzymali oni autoryzację pozwalającą, podczas procesu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="39acd-112">If they match, users authenticate successfully, and can then perform actions that they're authorized for, during an authorization process.</span></span> <span data-ttu-id="39acd-113">Autoryzacja odnosi się do procesu, który określa, jakie użytkownik może wykonywać.</span><span class="sxs-lookup"><span data-stu-id="39acd-113">The authorization refers to the process that determines what a user is allowed to do.</span></span>

<span data-ttu-id="39acd-114">Innym sposobem, aby traktować uwierzytelniania jest należy wziąć pod uwagę jako sposób wprowadź spację, np. serwera, bazy danych, aplikacji lub zasobów, podczas gdy autoryzacja to akcje, które może wykonywać użytkownik, do których obiektów wewnątrz tego miejsca (serwer, baza danych lub aplikacji).</span><span class="sxs-lookup"><span data-stu-id="39acd-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="39acd-115">Typowe luk w zabezpieczeniach oprogramowania</span><span class="sxs-lookup"><span data-stu-id="39acd-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="39acd-116">Platforma ASP.NET Core i programem EF zawiera funkcje, które ułatwiają zabezpieczanie aplikacji i zapobieganie naruszeniom bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="39acd-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="39acd-117">Poniższa lista łącza spowoduje przejście do dokumentacji techniki takimi szczegółami, jak uniknąć typowych luk w zabezpieczeniach w usłudze web apps:</span><span class="sxs-lookup"><span data-stu-id="39acd-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="39acd-118">Ataki z użyciem skryptów między witrynami</span><span class="sxs-lookup"><span data-stu-id="39acd-118">Cross-site scripting attacks</span></span>](xref:security/cross-site-scripting)
* [<span data-ttu-id="39acd-119">Ataki polegające na iniekcji SQL</span><span class="sxs-lookup"><span data-stu-id="39acd-119">SQL injection attacks</span></span>](/ef/core/querying/raw-sql)
* [<span data-ttu-id="39acd-120">Cross-Site Request Forgery (CSRF)</span><span class="sxs-lookup"><span data-stu-id="39acd-120">Cross-Site Request Forgery (CSRF)</span></span>](xref:security/anti-request-forgery)
* [<span data-ttu-id="39acd-121">Atakom na otwarte przekierowywanie</span><span class="sxs-lookup"><span data-stu-id="39acd-121">Open redirect attacks</span></span>](xref:security/preventing-open-redirects)

<span data-ttu-id="39acd-122">Ma więcej luk w zabezpieczeniach, które należy wiedzieć.</span><span class="sxs-lookup"><span data-stu-id="39acd-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="39acd-123">Aby uzyskać więcej informacji, zobacz artykuły w **zabezpieczenia i tożsamość** części spisu treści.</span><span class="sxs-lookup"><span data-stu-id="39acd-123">For more information, see the other articles in the **Security and Identity** section of the table of contents.</span></span>
