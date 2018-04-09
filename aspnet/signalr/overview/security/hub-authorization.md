---
uid: signalr/overview/security/hub-authorization
title: Uwierzytelnianie i autoryzacja koncentratorów SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora. Wersje oprogramowania używane w tym temacie kolejnych Visual Studio 2013 .NET 4.5 SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8e3bc8889efb1be80c57084fb04dc8030b386601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="b3e15-104">Uwierzytelnianie i autoryzacja koncentratorów SignalR</span><span class="sxs-lookup"><span data-stu-id="b3e15-104">Authentication and Authorization for SignalR Hubs</span></span>
====================
<span data-ttu-id="b3e15-105">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b3e15-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b3e15-106">W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b3e15-106">This topic describes how to restrict which users or roles can access hub methods.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b3e15-107">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="b3e15-107">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="b3e15-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b3e15-108">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b3e15-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b3e15-109">.NET 4.5</span></span>
> - <span data-ttu-id="b3e15-110">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="b3e15-110">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b3e15-111">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="b3e15-111">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="b3e15-112">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b3e15-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b3e15-113">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="b3e15-113">Questions and comments</span></span>
> 
> <span data-ttu-id="b3e15-114">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="b3e15-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b3e15-115">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b3e15-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b3e15-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b3e15-116">Overview</span></span>

<span data-ttu-id="b3e15-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="b3e15-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b3e15-118">Autoryzowanie atrybutu</span><span class="sxs-lookup"><span data-stu-id="b3e15-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="b3e15-119">Wymagaj uwierzytelniania dla wszystkich koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="b3e15-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="b3e15-120">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b3e15-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="b3e15-121">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="b3e15-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="b3e15-122">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="b3e15-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="b3e15-123">Plik cookie uwierzytelniania formularzy</span><span class="sxs-lookup"><span data-stu-id="b3e15-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="b3e15-124">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b3e15-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="b3e15-125">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="b3e15-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="b3e15-126">certyfikat</span><span class="sxs-lookup"><span data-stu-id="b3e15-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="b3e15-127">Autoryzowanie atrybutu</span><span class="sxs-lookup"><span data-stu-id="b3e15-127">Authorize attribute</span></span>

<span data-ttu-id="b3e15-128">Biblioteka SignalR udostępnia [autoryzacji](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, których użytkowników lub role mają dostęp do koncentratora lub metody.</span><span class="sxs-lookup"><span data-stu-id="b3e15-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="b3e15-129">Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="b3e15-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="b3e15-130">Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b3e15-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="b3e15-131">Po zastosowaniu `Authorize` atrybut do klasy koncentratora, wymaganie autoryzacji określony jest stosowane do wszystkich metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b3e15-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="b3e15-132">W tym temacie przedstawiono przykłady różnych typów wymagań autoryzacji, które można zastosować.</span><span class="sxs-lookup"><span data-stu-id="b3e15-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="b3e15-133">Bez `Authorize` atrybutu połączony klient ma dostęp do wszelkich publicznej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b3e15-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="b3e15-134">Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Administrator", można określić, że tylko użytkownicy w tej roli mają dostęp do Centrum następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="b3e15-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="b3e15-135">Alternatywnie można określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3e15-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="b3e15-136">Poniższe przykłady dotyczą autoryzacji różnych scenariuszy:</span><span class="sxs-lookup"><span data-stu-id="b3e15-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="b3e15-137">`[Authorize]` — tylko do uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="b3e15-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="b3e15-138">`[Authorize(Roles = "Admin,Manager")]` — tylko do uwierzytelnionych użytkowników w określonych ról</span><span class="sxs-lookup"><span data-stu-id="b3e15-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="b3e15-139">`[Authorize(Users = "user1,user2")]` — tylko do uwierzytelnionych użytkowników z określone nazwy użytkownika</span><span class="sxs-lookup"><span data-stu-id="b3e15-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="b3e15-140">`[Authorize(RequireOutgoing=false)]` — tylko do uwierzytelnionych użytkowników może wywołać koncentratora, ale połączenia z serwera do klientów nie są ograniczone przez autoryzacji, takich jak podczas tylko określonym użytkownikom można wysłać komunikatu, ale wszystkie inne mogą odbierać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="b3e15-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="b3e15-141">Właściwość niej można stosować do całego elementu hub, nie na metod osób w ramach koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b3e15-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="b3e15-142">Po niej nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są nazywane z serwera.</span><span class="sxs-lookup"><span data-stu-id="b3e15-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="b3e15-143">Wymagaj uwierzytelniania dla wszystkich koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="b3e15-143">Require authentication for all hubs</span></span>

<span data-ttu-id="b3e15-144">Może być wymagane uwierzytelnienie dla wszystkich koncentratorów i metod koncentratorów w aplikacji przez wywołanie metody [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metody podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b3e15-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="b3e15-145">Gdy ma wiele koncentratorów i mają być egzekwowane wymaganie uwierzytelniania dla wszystkich z nich można użyć tej metody.</span><span class="sxs-lookup"><span data-stu-id="b3e15-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="b3e15-146">Przy użyciu tej metody nie można określić wymagania dotyczące roli, użytkowników lub wychodzących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="b3e15-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="b3e15-147">Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="b3e15-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="b3e15-148">Można jednak nadal stosować atrybutu autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="b3e15-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="b3e15-149">Wszystkie wymagania określone w atrybucie jest dodawany do podstawowych wymagań uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b3e15-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="b3e15-150">W poniższym przykładzie przedstawiono plik uruchamiania, który ogranicza wszystkich metod koncentratora użytkownikom uwierzytelnionym.</span><span class="sxs-lookup"><span data-stu-id="b3e15-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="b3e15-151">Jeśli należy wywołać `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku.</span><span class="sxs-lookup"><span data-stu-id="b3e15-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="b3e15-152">SignalR zgłasza tego wyjątku, ponieważ nie można dodać moduł do metodę HubPipeline po potok został wywołany.</span><span class="sxs-lookup"><span data-stu-id="b3e15-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="b3e15-153">W poprzednim przykładzie przedstawiono wywoływania `RequireAuthentication` metody w `Configuration` metodę, która jest wykonywana raz przed obsługi pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="b3e15-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="b3e15-154">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b3e15-154">Customized authorization</span></span>

<span data-ttu-id="b3e15-155">Jeśli musisz dostosować jak Autoryzacja jest określona, można utworzyć klasy, która jest pochodną `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="b3e15-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="b3e15-156">Dla każdego żądania SignalR wywołuje tę metodę w celu określenia, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="b3e15-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="b3e15-157">Przeciążonej musisz podać logiki niezbędne dla danego scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="b3e15-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="b3e15-158">Poniższy przykład pokazuje, jak wymusić autoryzacji za pomocą tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="b3e15-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="b3e15-159">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="b3e15-159">Pass authentication information to clients</span></span>

<span data-ttu-id="b3e15-160">Może być konieczne używanie informacji uwierzytelniania w kodzie, który działa na kliencie.</span><span class="sxs-lookup"><span data-stu-id="b3e15-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="b3e15-161">Należy przekazać wymaganych informacji podczas wywoływania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="b3e15-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="b3e15-162">Na przykład metoda aplikacji rozmów można przekazać jako parametr nazwę osoby publikowanie komunikatów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3e15-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="b3e15-163">Alternatywnie można utworzyć obiektu reprezentuje informacje dotyczące uwierzytelniania i przekazywanie obiektu jako parametru, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3e15-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="b3e15-164">Nigdy nie należy przekazać jeden klient identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladować żądania z tego klienta.</span><span class="sxs-lookup"><span data-stu-id="b3e15-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="b3e15-165">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="b3e15-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="b3e15-166">Jeśli masz klienta programu .NET, na przykład aplikacji konsoli, która współdziała z koncentratora, który jest ograniczone do uwierzytelnionych użytkowników, należy przekazać poświadczenia uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="b3e15-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="b3e15-167">Przykłady w tej sekcji pokazano, jak użyć tych innych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b3e15-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="b3e15-168">Nie są one aplikacji SignalR w pełni funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b3e15-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="b3e15-169">Aby uzyskać więcej informacji dotyczących klientów platformy .NET z SignalR, zobacz [Podręcznik interfejsu API koncentratory - klienta .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="b3e15-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="b3e15-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="b3e15-170">Cookie</span></span>

<span data-ttu-id="b3e15-171">Gdy klient .NET użyje koncentratora, który korzysta z uwierzytelniania formularzy ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="b3e15-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="b3e15-172">Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="b3e15-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="b3e15-173">W poniższym przykładzie przedstawiono aplikacji konsoli, która pobiera pliku cookie uwierzytelniania ze strony sieci web i dodaje ten plik cookie dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="b3e15-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="b3e15-174">Aplikacji konsoli ogłoszeń poświadczenia <strong>www.contoso.com/RemoteLogin</strong> którego można odwoływać się do pustej strony, która zawiera następujący plik CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="b3e15-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="b3e15-175">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="b3e15-175">Windows authentication</span></span>

<span data-ttu-id="b3e15-176">Podczas korzystania z uwierzytelniania systemu Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości.</span><span class="sxs-lookup"><span data-stu-id="b3e15-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="b3e15-177">Poświadczenia dla połączenia zostanie ustawiona wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="b3e15-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="b3e15-178">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="b3e15-178">Connection header</span></span>

<span data-ttu-id="b3e15-179">Jeśli aplikacja nie używa plików cookie, należy przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="b3e15-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="b3e15-180">Na przykład można przekazać tokenu w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="b3e15-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="b3e15-181">Następnie w Centrum, możesz Zweryfikuj token użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b3e15-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="b3e15-182">certyfikat</span><span class="sxs-lookup"><span data-stu-id="b3e15-182">Certificate</span></span>

<span data-ttu-id="b3e15-183">Można przekazać certyfikatu klienta w celu weryfikacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b3e15-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="b3e15-184">Możesz dodać certyfikat, podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="b3e15-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="b3e15-185">Poniższy przykład przedstawia tylko sposób dodawania certyfikat klienta do połączenia; nie są wyświetlane w aplikacji konsoli pełna.</span><span class="sxs-lookup"><span data-stu-id="b3e15-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="b3e15-186">Używa [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasy, która zapewnia kilka różnych sposobów, aby utworzyć certyfikat.</span><span class="sxs-lookup"><span data-stu-id="b3e15-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
