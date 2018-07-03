---
uid: signalr/overview/security/hub-authorization
title: Uwierzytelnianie i autoryzacja dla centrów SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak ograniczyć, które użytkownicy lub ról dostęp do metod koncentratora. Wersje oprogramowania używaną w tym temacie program Visual Studio 2013 .NET 4.5 SignalR ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6d351542a3238cbb8168ac20bcba559551837351
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361946"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="5542a-104">Uwierzytelnianie i autoryzacja dla centrów SignalR</span><span class="sxs-lookup"><span data-stu-id="5542a-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="5542a-105">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5542a-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5542a-106">W tym temacie opisano, jak ograniczyć, które użytkownicy lub ról dostęp do metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5542a-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5542a-107">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="5542a-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5542a-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5542a-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5542a-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5542a-109">.NET 4.5</span></span>
> - <span data-ttu-id="5542a-110">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="5542a-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5542a-111">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="5542a-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5542a-112">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5542a-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5542a-113">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="5542a-113">Questions and comments</span></span>
> 
> <span data-ttu-id="5542a-114">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="5542a-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5542a-115">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5542a-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="5542a-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5542a-116">Overview</span></span>

<span data-ttu-id="5542a-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="5542a-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5542a-118">Autoryzuj atrybutu</span><span class="sxs-lookup"><span data-stu-id="5542a-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="5542a-119">Wymagaj uwierzytelniania do wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="5542a-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="5542a-120">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="5542a-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="5542a-121">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="5542a-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="5542a-122">Opcje uwierzytelniania dla klientów programu .NET</span><span class="sxs-lookup"><span data-stu-id="5542a-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="5542a-123">Plik cookie uwierzytelniania formularzy</span><span class="sxs-lookup"><span data-stu-id="5542a-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="5542a-124">Uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="5542a-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="5542a-125">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="5542a-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="5542a-126">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="5542a-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="5542a-127">Autoryzuj atrybutu</span><span class="sxs-lookup"><span data-stu-id="5542a-127">Authorize attribute</span></span>

<span data-ttu-id="5542a-128">Biblioteka SignalR udostępnia [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, którzy użytkownicy lub które role mają dostęp do Centrum lub metody.</span><span class="sxs-lookup"><span data-stu-id="5542a-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="5542a-129">Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="5542a-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="5542a-130">Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5542a-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="5542a-131">Po zastosowaniu `Authorize` do klasy koncentratora, wymóg autoryzacji określony jest stosowany do wszystkich metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5542a-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="5542a-132">Ten temat zawiera przykłady różnych typów wymagań autoryzacji, które można zastosować.</span><span class="sxs-lookup"><span data-stu-id="5542a-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="5542a-133">Bez `Authorize` atrybutu połączonej klient może uzyskać dostęp do wszelkich publicznej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5542a-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="5542a-134">Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Admin", można określić, że tylko użytkownicy w tej roli dostęp do Centrum z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="5542a-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="5542a-135">Lub można określić, czy Centrum zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="5542a-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="5542a-136">Poniższe przykłady scenariuszy różnych autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="5542a-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="5542a-137">`[Authorize]` — tylko do uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="5542a-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="5542a-138">`[Authorize(Roles = "Admin,Manager")]` — tylko do uwierzytelnionych użytkowników w określonych ról</span><span class="sxs-lookup"><span data-stu-id="5542a-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="5542a-139">`[Authorize(Users = "user1,user2")]` — tylko do uwierzytelnionych użytkowników przy użyciu nazwy określonego użytkownika</span><span class="sxs-lookup"><span data-stu-id="5542a-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="5542a-140">`[Authorize(RequireOutgoing=false)]` — tylko do uwierzytelnionych użytkowników można wywołać koncentratora, ale wywołania z serwera do klientów nie są ograniczone przez autoryzacji, takich jak, gdy tylko w przypadku niektórych użytkowników może także wysłać komunikat, ale wszystkie inne pojawia się komunikat o.</span><span class="sxs-lookup"><span data-stu-id="5542a-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="5542a-141">Właściwość RequireOutgoing może zostać zastosowana tylko do całego centrum hub, nie na metody osób w ramach Centrum.</span><span class="sxs-lookup"><span data-stu-id="5542a-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="5542a-142">Gdy RequireOutgoing nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="5542a-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="5542a-143">Wymagaj uwierzytelniania do wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="5542a-143">Require authentication for all hubs</span></span>

<span data-ttu-id="5542a-144">Można wymagać uwierzytelniania do wszystkich koncentratorów i metod koncentratorów w aplikacji, wywołując [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5542a-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="5542a-145">Ta metoda jest przydatna po wiele centrów i chcesz wymuszać wymogu uwierzytelniania dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="5542a-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="5542a-146">Przy użyciu tej metody nie można określić wymagania dla roli, użytkowników lub wychodzących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="5542a-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="5542a-147">Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="5542a-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="5542a-148">Można jednak stosować atrybut autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="5542a-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="5542a-149">Wszelkie wymagania, którą określasz w atrybucie jest dodawany do podstawowych wymagań uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="5542a-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="5542a-150">Poniższy przykład pokazuje plik startowy, który ogranicza możliwość użycia wszystkich metod koncentratora do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="5542a-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="5542a-151">Jeśli wywołasz `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku.</span><span class="sxs-lookup"><span data-stu-id="5542a-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="5542a-152">SignalR zgłasza ten wyjątek, ponieważ nie można dodać modułu do HubPipeline, po wywołaniu potoku.</span><span class="sxs-lookup"><span data-stu-id="5542a-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="5542a-153">W poprzednim przykładzie pokazano wywołanie `RequireAuthentication` method in Class metoda `Configuration` metodę, która jest wykonywana jeden raz przed pierwszego żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="5542a-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="5542a-154">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="5542a-154">Customized authorization</span></span>

<span data-ttu-id="5542a-155">Jeśli trzeba dostosować, jak Autoryzacja jest określona, możesz utworzyć klasę, która pochodzi od klasy `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="5542a-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="5542a-156">Dla każdego żądania SignalR wywołuje tę metodę w celu określenia, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="5542a-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="5542a-157">W metodzie zgodnym z przesłoniętą podasz logikę potrzebną dla danego scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="5542a-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="5542a-158">Poniższy przykład pokazuje, jak wymusić autoryzację za pomocą tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="5542a-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="5542a-159">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="5542a-159">Pass authentication information to clients</span></span>

<span data-ttu-id="5542a-160">Może być konieczne używanie informacji uwierzytelniania w kodzie, który jest uruchamiany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5542a-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="5542a-161">Wymagane informacje są przekazywane podczas wywoływania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5542a-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="5542a-162">Na przykład metoda aplikacji rozmów można przekazać jako parametr Nazwa użytkownika osoby, publikując wiadomość, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="5542a-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="5542a-163">Alternatywnie można utworzyć obiektu reprezentują informacje dotyczące uwierzytelniania, a następnie przekazać ten obiekt jako parametr, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="5542a-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="5542a-164">Nigdy nie należy przekazywać jednego klienta identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania tego klienta.</span><span class="sxs-lookup"><span data-stu-id="5542a-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="5542a-165">Opcje uwierzytelniania dla klientów programu .NET</span><span class="sxs-lookup"><span data-stu-id="5542a-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="5542a-166">Jeśli masz klienta platformy .NET, takie jak aplikacja konsolowa, która współdziała z koncentratora, który jest ograniczony do użytkowników uwierzytelnionych, można przekazać poświadczeń uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikat.</span><span class="sxs-lookup"><span data-stu-id="5542a-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="5542a-167">Przykłady w tej sekcji pokazano, jak używać tych różnych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5542a-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="5542a-168">Nie są one w pełni funkcjonalne aplikacje SignalR.</span><span class="sxs-lookup"><span data-stu-id="5542a-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="5542a-169">Aby uzyskać więcej informacji na temat klientów programu .NET przy użyciu SignalR, zobacz [Podręcznik interfejsu API centrów — klient modelu .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="5542a-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="5542a-170">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="5542a-170">Cookie</span></span>

<span data-ttu-id="5542a-171">Twój klient .NET wchodzi w interakcję z koncentratora, który korzysta z uwierzytelniania formularzy programu ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="5542a-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="5542a-172">Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="5542a-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="5542a-173">Poniższy przykład pokazuje aplikacja konsolowa, która pobiera pliku cookie uwierzytelniania ze strony sieci web, a następnie dodaje ten plik cookie dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="5542a-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="5542a-174">Aplikacja konsoli opublikuje poświadczenia, aby <strong>www.contoso.com/RemoteLogin</strong> której mogłoby się odwoływać pusta strona, zawierający następujący plik CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="5542a-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="5542a-175">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="5542a-175">Windows authentication</span></span>

<span data-ttu-id="5542a-176">Korzystając z uwierzytelniania Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości.</span><span class="sxs-lookup"><span data-stu-id="5542a-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="5542a-177">Poświadczenia dla połączenia jest ustawiona na wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="5542a-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="5542a-178">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="5542a-178">Connection header</span></span>

<span data-ttu-id="5542a-179">Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="5542a-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="5542a-180">Na przykład można przekazać token w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="5542a-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="5542a-181">Następnie w Centrum, możesz sprawdzić tokenu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5542a-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="5542a-182">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="5542a-182">Certificate</span></span>

<span data-ttu-id="5542a-183">Można przekazać certyfikatu klienta w celu zweryfikowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5542a-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="5542a-184">Dodaj certyfikat, podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="5542a-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="5542a-185">Poniższy przykład pokazuje tylko sposób dodawania certyfikatu klienta dla połączenia; Aplikacja konsoli pełne nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="5542a-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="5542a-186">Używa ona [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasę, która oferuje kilka sposobów, aby utworzyć certyfikat.</span><span class="sxs-lookup"><span data-stu-id="5542a-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
