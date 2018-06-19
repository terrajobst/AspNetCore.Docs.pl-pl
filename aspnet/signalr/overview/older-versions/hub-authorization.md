---
uid: signalr/overview/older-versions/hub-authorization
title: Uwierzytelnianie i autoryzacja koncentratorów SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042878"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="d2dba-103">Uwierzytelnianie i autoryzacja koncentratorów SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d2dba-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d2dba-104">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d2dba-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d2dba-105">W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d2dba-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="d2dba-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d2dba-106">Overview</span></span>

<span data-ttu-id="d2dba-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="d2dba-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d2dba-108">Autoryzowanie atrybutu</span><span class="sxs-lookup"><span data-stu-id="d2dba-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="d2dba-109">Wymagaj uwierzytelniania dla wszystkich koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="d2dba-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="d2dba-110">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="d2dba-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="d2dba-111">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="d2dba-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="d2dba-112">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="d2dba-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="d2dba-113">Plik cookie uwierzytelniania formularzy</span><span class="sxs-lookup"><span data-stu-id="d2dba-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="d2dba-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="d2dba-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="d2dba-115">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="d2dba-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="d2dba-116">Certyfikat</span><span class="sxs-lookup"><span data-stu-id="d2dba-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="d2dba-117">Autoryzowanie atrybutu</span><span class="sxs-lookup"><span data-stu-id="d2dba-117">Authorize attribute</span></span>

<span data-ttu-id="d2dba-118">Biblioteka SignalR udostępnia [autoryzacji](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, których użytkowników lub role mają dostęp do koncentratora lub metody.</span><span class="sxs-lookup"><span data-stu-id="d2dba-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="d2dba-119">Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d2dba-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="d2dba-120">Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d2dba-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="d2dba-121">Po zastosowaniu `Authorize` atrybut do klasy koncentratora, wymaganie autoryzacji określony jest stosowane do wszystkich metod koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d2dba-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="d2dba-122">Poniżej przedstawiono wymagania autoryzacji, które można stosować różne typy.</span><span class="sxs-lookup"><span data-stu-id="d2dba-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="d2dba-123">Bez `Authorize` atrybutu, wszystkie metody publiczne koncentratora są dostępne dla klienta, który jest podłączony do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d2dba-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="d2dba-124">Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Administrator", można określić, że tylko użytkownicy w tej roli mają dostęp do Centrum następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="d2dba-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="d2dba-125">Alternatywnie można określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d2dba-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="d2dba-126">Poniższe przykłady dotyczą autoryzacji różnych scenariuszy:</span><span class="sxs-lookup"><span data-stu-id="d2dba-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="d2dba-127">`[Authorize]`— tylko do uwierzytelnionych użytkowników</span><span class="sxs-lookup"><span data-stu-id="d2dba-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="d2dba-128">`[Authorize(Roles = "Admin,Manager")]`— tylko do uwierzytelnionych użytkowników w określonych ról</span><span class="sxs-lookup"><span data-stu-id="d2dba-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="d2dba-129">`[Authorize(Users = "user1,user2")]`— tylko do uwierzytelnionych użytkowników z określone nazwy użytkownika</span><span class="sxs-lookup"><span data-stu-id="d2dba-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="d2dba-130">`[Authorize(RequireOutgoing=false)]`— tylko do uwierzytelnionych użytkowników może wywołać koncentratora, ale połączenia z serwera do klientów nie są ograniczone przez autoryzacji, takich jak podczas tylko określonym użytkownikom można wysłać komunikatu, ale wszystkie inne mogą odbierać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="d2dba-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="d2dba-131">Właściwość niej można stosować do całego elementu hub, nie na metod osób w ramach koncentratora.</span><span class="sxs-lookup"><span data-stu-id="d2dba-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="d2dba-132">Po niej nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są nazywane z serwera.</span><span class="sxs-lookup"><span data-stu-id="d2dba-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="d2dba-133">Wymagaj uwierzytelniania dla wszystkich koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="d2dba-133">Require authentication for all hubs</span></span>

<span data-ttu-id="d2dba-134">Może być wymagane uwierzytelnienie dla wszystkich koncentratorów i metod koncentratorów w aplikacji przez wywołanie metody [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metody podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2dba-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="d2dba-135">Gdy ma wiele koncentratorów i mają być egzekwowane wymaganie uwierzytelniania dla wszystkich z nich można użyć tej metody.</span><span class="sxs-lookup"><span data-stu-id="d2dba-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="d2dba-136">Przy użyciu tej metody nie można określić roli, użytkowników lub wychodzących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="d2dba-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="d2dba-137">Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych.</span><span class="sxs-lookup"><span data-stu-id="d2dba-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="d2dba-138">Można jednak nadal stosować atrybutu autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania.</span><span class="sxs-lookup"><span data-stu-id="d2dba-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="d2dba-139">Wszystkie wymagania określone w atrybutach jest stosowany oprócz podstawowych wymagań uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d2dba-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="d2dba-140">W poniższym przykładzie przedstawiono plik Global.asax, który ogranicza wszystkich metod koncentratora użytkownikom uwierzytelnionym.</span><span class="sxs-lookup"><span data-stu-id="d2dba-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="d2dba-141">Jeśli należy wywołać `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku.</span><span class="sxs-lookup"><span data-stu-id="d2dba-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="d2dba-142">Zgłoszenia tego wyjątku, ponieważ nie można dodać moduł do metodę HubPipeline po potok został wywołany.</span><span class="sxs-lookup"><span data-stu-id="d2dba-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="d2dba-143">W poprzednim przykładzie przedstawiono wywoływania `RequireAuthentication` metody w `Application_Start` metodę, która jest wykonywana raz przed obsługi pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="d2dba-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="d2dba-144">Dostosowane autoryzacji</span><span class="sxs-lookup"><span data-stu-id="d2dba-144">Customized authorization</span></span>

<span data-ttu-id="d2dba-145">Jeśli musisz dostosować jak Autoryzacja jest określona, można utworzyć klasy, która jest pochodną `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody.</span><span class="sxs-lookup"><span data-stu-id="d2dba-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="d2dba-146">Ta metoda jest wywoływana dla każdego żądania w celu określenia, czy użytkownik jest autoryzowany do wykonania żądania.</span><span class="sxs-lookup"><span data-stu-id="d2dba-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="d2dba-147">Przeciążonej musisz podać logiki niezbędne dla danego scenariusza autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="d2dba-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="d2dba-148">Poniższy przykład pokazuje, jak wymusić autoryzacji za pomocą tożsamości opartej na oświadczeniach.</span><span class="sxs-lookup"><span data-stu-id="d2dba-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="d2dba-149">Przekaż informacje dotyczące uwierzytelniania dla klientów</span><span class="sxs-lookup"><span data-stu-id="d2dba-149">Pass authentication information to clients</span></span>

<span data-ttu-id="d2dba-150">Może być konieczne używanie informacji uwierzytelniania w kodzie, który działa na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d2dba-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="d2dba-151">Należy przekazać wymaganych informacji podczas wywoływania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d2dba-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="d2dba-152">Na przykład metoda aplikacji rozmów można przekazać jako parametr nazwę osoby publikowanie komunikatów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d2dba-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="d2dba-153">Alternatywnie można utworzyć obiektu reprezentuje informacje dotyczące uwierzytelniania i przekazywanie obiektu jako parametru, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d2dba-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="d2dba-154">Nigdy nie należy przekazać jeden klient identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladować żądania z tego klienta.</span><span class="sxs-lookup"><span data-stu-id="d2dba-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="d2dba-155">Opcje uwierzytelniania dla klientów platformy .NET</span><span class="sxs-lookup"><span data-stu-id="d2dba-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="d2dba-156">Jeśli masz klienta programu .NET, na przykład aplikacji konsoli, która współdziała z koncentratora, który jest ograniczone do uwierzytelnionych użytkowników, należy przekazać poświadczenia uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="d2dba-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="d2dba-157">Przykłady w tej sekcji pokazano, jak użyć tych innych metod uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2dba-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="d2dba-158">Nie są one aplikacji SignalR w pełni funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="d2dba-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="d2dba-159">Aby uzyskać więcej informacji dotyczących klientów platformy .NET z SignalR, zobacz [Podręcznik interfejsu API koncentratory - klienta .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="d2dba-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="d2dba-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="d2dba-160">Cookie</span></span>

<span data-ttu-id="d2dba-161">Gdy klient .NET użyje koncentratora, który korzysta z uwierzytelniania formularzy ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania w połączeniu.</span><span class="sxs-lookup"><span data-stu-id="d2dba-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="d2dba-162">Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu.</span><span class="sxs-lookup"><span data-stu-id="d2dba-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="d2dba-163">W poniższym przykładzie przedstawiono aplikacji konsoli, która pobiera pliku cookie uwierzytelniania ze strony sieci web i dodaje ten plik cookie dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="d2dba-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="d2dba-164">Adres URL `https://www.contoso.com/RemoteLogin` w punktach przykład do strony sieci web, którą należy utworzyć.</span><span class="sxs-lookup"><span data-stu-id="d2dba-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="d2dba-165">Strona może pobrać nazwy oczekujących na opublikowanie użytkownika i hasła i prób zalogowania użytkownika przy użyciu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="d2dba-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="d2dba-166">Aplikacja konsoli wpisów poświadczenia www.contoso.com/RemoteLogin którego można odwoływać się do pustej strony, która zawiera następujący plik CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="d2dba-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="d2dba-167">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="d2dba-167">Windows authentication</span></span>

<span data-ttu-id="d2dba-168">Podczas korzystania z uwierzytelniania systemu Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości.</span><span class="sxs-lookup"><span data-stu-id="d2dba-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="d2dba-169">Poświadczenia dla połączenia zostanie ustawiona wartość DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="d2dba-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="d2dba-170">Nagłówek połączenia</span><span class="sxs-lookup"><span data-stu-id="d2dba-170">Connection header</span></span>

<span data-ttu-id="d2dba-171">Jeśli aplikacja nie używa plików cookie, należy przekazać informacje o użytkowniku w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="d2dba-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="d2dba-172">Na przykład można przekazać tokenu w nagłówku połączenia.</span><span class="sxs-lookup"><span data-stu-id="d2dba-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="d2dba-173">Następnie w Centrum, możesz Zweryfikuj token użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2dba-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="d2dba-174">certyfikat</span><span class="sxs-lookup"><span data-stu-id="d2dba-174">Certificate</span></span>

<span data-ttu-id="d2dba-175">Można przekazać certyfikatu klienta w celu weryfikacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d2dba-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="d2dba-176">Możesz dodać certyfikat, podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="d2dba-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="d2dba-177">Poniższy przykład przedstawia tylko sposób dodawania certyfikat klienta do połączenia; nie są wyświetlane w aplikacji konsoli pełna.</span><span class="sxs-lookup"><span data-stu-id="d2dba-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="d2dba-178">Używa [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasy, która zapewnia kilka różnych sposobów, aby utworzyć certyfikat.</span><span class="sxs-lookup"><span data-stu-id="d2dba-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
