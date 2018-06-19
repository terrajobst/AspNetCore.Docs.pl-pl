---
uid: web-api/overview/security/integrated-windows-authentication
title: Zintegrowane uwierzytelnianie systemu Windows | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu zintegrowanego uwierzytelniania systemu Windows.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566753"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="95c1d-103">Zintegrowane uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="95c1d-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="95c1d-104">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="95c1d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="95c1d-105">Zintegrowane uwierzytelnianie systemu Windows umożliwia użytkownikom logowanie się przy użyciu poświadczeń systemu Windows przy użyciu protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="95c1d-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="95c1d-106">Klient wysyła poświadczenia w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="95c1d-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="95c1d-107">Uwierzytelnianie systemu Windows jest najodpowiedniejsze do użytku w środowisku sieci intranet.</span><span class="sxs-lookup"><span data-stu-id="95c1d-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="95c1d-108">Aby uzyskać więcej informacji, zobacz [uwierzytelniania systemu Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="95c1d-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="95c1d-109">Zalety</span><span class="sxs-lookup"><span data-stu-id="95c1d-109">Advantages</span></span> | <span data-ttu-id="95c1d-110">Wady</span><span class="sxs-lookup"><span data-stu-id="95c1d-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="95c1d-111">-Wbudowane w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="95c1d-111">- Built into IIS.</span></span> <span data-ttu-id="95c1d-112">-Nie wysyła poświadczenia użytkownika w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="95c1d-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="95c1d-113">— Jeśli komputer klienta należy do domeny (na przykład aplikacja intranetu), użytkownik musi wprowadzić poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="95c1d-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="95c1d-114">— Nie jest zalecane dla aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="95c1d-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="95c1d-115">-Wymaga obsługi protokołu Kerberos lub NTLM w kliencie.</span><span class="sxs-lookup"><span data-stu-id="95c1d-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="95c1d-116">-Klient musi należeć do domeny usługi Active Directory.</span><span class="sxs-lookup"><span data-stu-id="95c1d-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="95c1d-117">Jeśli aplikacja jest hostowana na platformie Azure i masz lokalnej domeny usługi Active Directory, należy wziąć pod uwagę federowania lokalnej usługi AD z usługą Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="95c1d-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="95c1d-118">W ten sposób użytkownicy mogą Zaloguj się przy użyciu poświadczeń lokalnego, ale uwierzytelnianie jest wykonywane przez usługę Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95c1d-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="95c1d-119">Aby uzyskać więcej informacji, zobacz [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="95c1d-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="95c1d-120">Aby utworzyć aplikację, która używa zintegrowanego uwierzytelniania systemu Windows, wybierz szablon "Intranet aplikacji" w Kreatorze projektu MVC 4.</span><span class="sxs-lookup"><span data-stu-id="95c1d-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="95c1d-121">Ten szablon projektu umieszcza następujące ustawienie w pliku Web.config:</span><span class="sxs-lookup"><span data-stu-id="95c1d-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="95c1d-122">Po stronie klienta zintegrowanego uwierzytelniania systemu Windows współdziała z dowolnej przeglądarki obsługującej [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schemat uwierzytelniania, w tym w większości przeglądarek głównych.</span><span class="sxs-lookup"><span data-stu-id="95c1d-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="95c1d-123">Dla aplikacji klienckich, .NET **HttpClient** klasa obsługuje uwierzytelnianie systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="95c1d-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="95c1d-124">Uwierzytelnianie systemu Windows jest narażony na fałszerstwie żądania (CSRF) między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="95c1d-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="95c1d-125">Zobacz [zapobieganie Fałszerstwie żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="95c1d-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
