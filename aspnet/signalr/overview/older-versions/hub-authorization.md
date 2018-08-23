---
uid: signalr/overview/older-versions/hub-authorization
title: Uwierzytelnianie i autoryzacja dla centrów SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak ograniczyć, które użytkownicy lub ról dostęp do metod koncentratora.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6600e63371d54f14615e4c9af4c572e73464c2e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756256"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="ecca4-103">Uwierzytelnianie i autoryzacja dla centrów SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ecca4-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ecca4-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ecca4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ecca4-105">W tym temacie opisano, jak ograniczyć, które użytkownicy lub ról dostęp do metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ecca4-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="ecca4-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ecca4-106">Overview</span></span>

<span data-ttu-id="ecca4-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="ecca4-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="ecca4-108">Autoryzuj atrybutu</span><span class="sxs-lookup"><span data-stu-id="ecca4-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="ecca4-109">Wymagaj uwierzytelniania do wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="ecca4-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="ecca4-110">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ecca4-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="ecca4-111">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="ecca4-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="ecca4-112">Opcje uwierzytelniania dla klientów programu .NET</span><span class="sxs-lookup"><span data-stu-id="ecca4-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="ecca4-113">Plik cookie uwierzytelniania formularzy</span><span class="sxs-lookup"><span data-stu-id="ecca4-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="ecca4-114">Uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="ecca4-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="ecca4-115">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="ecca4-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="ecca4-116">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="ecca4-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="ecca4-117">Autoryzuj atrybutu</span><span class="sxs-lookup"><span data-stu-id="ecca4-117">Authorize attribute</span></span>

<span data-ttu-id="ecca4-118">Biblioteka SignalR udostępnia [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, którzy użytkownicy lub które role mają dostęp do Centrum lub metody.</span><span class="sxs-lookup"><span data-stu-id="ecca4-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="ecca4-119">Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="ecca4-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="ecca4-120">Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ecca4-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="ecca4-121">Po zastosowaniu `Authorize` do klasy koncentratora, wymóg autoryzacji określony jest stosowany do wszystkich metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ecca4-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="ecca4-122">Poniżej przedstawiono różne rodzaje wymagań autoryzacji, które można zastosować.</span><span class="sxs-lookup"><span data-stu-id="ecca4-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="ecca4-123">Bez `Authorize` atrybutu, wszystkie publiczne metody koncentratora są dostępne dla klienta, który jest podłączony do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ecca4-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="ecca4-124">Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Admin", można określić, że tylko użytkownicy w tej roli dostęp do Centrum z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="ecca4-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="ecca4-125">Lub można określić, czy Centrum zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ecca4-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="ecca4-126">Poniższe przykłady scenariuszy różnych autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="ecca4-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="ecca4-127">`[Authorize]` — tylko do uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="ecca4-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="ecca4-128">`[Authorize(Roles = "Admin,Manager")]` — tylko do uwierzytelnionych użytkowników w określonych ról</span><span class="sxs-lookup"><span data-stu-id="ecca4-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="ecca4-129">`[Authorize(Users = "user1,user2")]` — tylko do uwierzytelnionych użytkowników przy użyciu nazwy określonego użytkownika</span><span class="sxs-lookup"><span data-stu-id="ecca4-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="ecca4-130">`[Authorize(RequireOutgoing=false)]` — tylko do uwierzytelnionych użytkowników można wywołać koncentratora, ale wywołania z serwera do klientów nie są ograniczone przez autoryzacji, takich jak, gdy tylko w przypadku niektórych użytkowników może także wysłać komunikat, ale wszystkie inne pojawia się komunikat o.</span><span class="sxs-lookup"><span data-stu-id="ecca4-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="ecca4-131">Właściwość RequireOutgoing może zostać zastosowana tylko do całego centrum hub, nie na metody osób w ramach Centrum.</span><span class="sxs-lookup"><span data-stu-id="ecca4-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="ecca4-132">Gdy RequireOutgoing nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="ecca4-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="ecca4-133">Wymagaj uwierzytelniania do wszystkich centrów</span><span class="sxs-lookup"><span data-stu-id="ecca4-133">Require authentication for all hubs</span></span>

<span data-ttu-id="ecca4-134">Można wymagać uwierzytelniania do wszystkich koncentratorów i metod koncentratorów w aplikacji, wywołując [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ecca4-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="ecca4-135">Ta metoda jest przydatna po wiele centrów i chcesz wymuszać wymogu uwierzytelniania dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="ecca4-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="ecca4-136">Przy użyciu tej metody nie można określić roli, użytkowników lub wychodzących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ecca4-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="ecca4-137">Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="ecca4-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="ecca4-138">Można jednak stosować atrybut autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="ecca4-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="ecca4-139">Wszelkie wymagania, którą określasz w atrybutach są stosowane oprócz podstawowych wymagań uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ecca4-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="ecca4-140">Poniższy przykład pokazuje plik Global.asax, który ogranicza możliwość użycia wszystkich metod koncentratora do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ecca4-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="ecca4-141">Jeśli wywołasz `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku.</span><span class="sxs-lookup"><span data-stu-id="ecca4-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="ecca4-142">Ten wyjątek jest generowany, ponieważ nie można dodać modułu do HubPipeline, po wywołaniu potoku.</span><span class="sxs-lookup"><span data-stu-id="ecca4-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="ecca4-143">W poprzednim przykładzie pokazano wywołanie `RequireAuthentication` method in Class metoda `Application_Start` metodę, która jest wykonywana jeden raz przed pierwszego żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="ecca4-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="ecca4-144">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ecca4-144">Customized authorization</span></span>

<span data-ttu-id="ecca4-145">Jeśli trzeba dostosować, jak Autoryzacja jest określona, możesz utworzyć klasę, która pochodzi od klasy `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="ecca4-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="ecca4-146">Ta metoda jest wywoływana dla każdego żądania ustalić, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="ecca4-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="ecca4-147">W metodzie zgodnym z przesłoniętą podasz logikę potrzebną dla danego scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ecca4-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="ecca4-148">Poniższy przykład pokazuje, jak wymusić autoryzację za pomocą tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="ecca4-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="ecca4-149">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="ecca4-149">Pass authentication information to clients</span></span>

<span data-ttu-id="ecca4-150">Może być konieczne używanie informacji uwierzytelniania w kodzie, który jest uruchamiany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ecca4-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="ecca4-151">Wymagane informacje są przekazywane podczas wywoływania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ecca4-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="ecca4-152">Na przykład metoda aplikacji rozmów można przekazać jako parametr Nazwa użytkownika osoby, publikując wiadomość, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ecca4-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="ecca4-153">Alternatywnie można utworzyć obiektu reprezentują informacje dotyczące uwierzytelniania, a następnie przekazać ten obiekt jako parametr, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ecca4-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="ecca4-154">Nigdy nie należy przekazywać jednego klienta identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania tego klienta.</span><span class="sxs-lookup"><span data-stu-id="ecca4-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="ecca4-155">Opcje uwierzytelniania dla klientów programu .NET</span><span class="sxs-lookup"><span data-stu-id="ecca4-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="ecca4-156">Jeśli masz klienta platformy .NET, takie jak aplikacja konsolowa, która współdziała z koncentratora, który jest ograniczony do użytkowników uwierzytelnionych, można przekazać poświadczeń uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikat.</span><span class="sxs-lookup"><span data-stu-id="ecca4-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="ecca4-157">Przykłady w tej sekcji pokazano, jak używać tych różnych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ecca4-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="ecca4-158">Nie są one w pełni funkcjonalne aplikacje SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecca4-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="ecca4-159">Aby uzyskać więcej informacji na temat klientów programu .NET przy użyciu SignalR, zobacz [Podręcznik interfejsu API centrów — klient modelu .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="ecca4-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="ecca4-160">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="ecca4-160">Cookie</span></span>

<span data-ttu-id="ecca4-161">Twój klient .NET wchodzi w interakcję z koncentratora, który korzysta z uwierzytelniania formularzy programu ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecca4-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="ecca4-162">Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="ecca4-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="ecca4-163">Poniższy przykład pokazuje aplikacja konsolowa, która pobiera pliku cookie uwierzytelniania ze strony sieci web, a następnie dodaje ten plik cookie dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecca4-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="ecca4-164">Adres URL `https://www.contoso.com/RemoteLogin` w przykładzie wskazuje do strony sieci web, którą należy utworzyć.</span><span class="sxs-lookup"><span data-stu-id="ecca4-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="ecca4-165">Strona pobierania nazwy przesłanych użytkownika i hasła i prób zalogowania użytkownika przy użyciu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="ecca4-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="ecca4-166">Aplikacja konsoli opublikuje poświadczenia, aby www.contoso.com/RemoteLogin której mogłoby się odwoływać pusta strona, zawierający następujący plik CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="ecca4-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="ecca4-167">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="ecca4-167">Windows authentication</span></span>

<span data-ttu-id="ecca4-168">Korzystając z uwierzytelniania Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości.</span><span class="sxs-lookup"><span data-stu-id="ecca4-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="ecca4-169">Poświadczenia dla połączenia jest ustawiona na wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="ecca4-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="ecca4-170">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="ecca4-170">Connection header</span></span>

<span data-ttu-id="ecca4-171">Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecca4-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="ecca4-172">Na przykład można przekazać token w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecca4-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="ecca4-173">Następnie w Centrum, możesz sprawdzić tokenu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ecca4-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="ecca4-174">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="ecca4-174">Certificate</span></span>

<span data-ttu-id="ecca4-175">Można przekazać certyfikatu klienta w celu zweryfikowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ecca4-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="ecca4-176">Dodaj certyfikat, podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="ecca4-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="ecca4-177">Poniższy przykład pokazuje tylko sposób dodawania certyfikatu klienta dla połączenia; Aplikacja konsoli pełne nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="ecca4-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="ecca4-178">Używa ona [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasę, która oferuje kilka sposobów, aby utworzyć certyfikat.</span><span class="sxs-lookup"><span data-stu-id="ecca4-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
