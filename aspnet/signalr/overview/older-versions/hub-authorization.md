---
uid: signalr/overview/older-versions/hub-authorization
title: "Uwierzytelnianie i autoryzacja koncentratorów SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora."
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
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Uwierzytelnianie i autoryzacja koncentratorów SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano, jak ograniczyć, których użytkowników lub ról można uzyskać dostępu do metody koncentratora.


## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Autoryzowanie atrybutu](#authorizeattribute)
- [Wymagaj uwierzytelniania dla wszystkich koncentratorów.](#requireauth)
- [Dostosowane autoryzacji](#custom)
- [Przekaż informacje dotyczące uwierzytelniania dla klientów](#passauth)
- [Opcje uwierzytelniania dla klientów platformy .NET](#authoptions)

    - [Plik cookie uwierzytelniania formularzy](#cookie)
    - [Uwierzytelnianie systemu Windows](#windows)
    - [Nagłówek połączenia](#header)
    - [Certyfikat](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autoryzowanie atrybutu

Biblioteka SignalR udostępnia [autoryzacji](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, których użytkowników lub role mają dostęp do koncentratora lub metody. Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw. Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora. Po zastosowaniu `Authorize` atrybut do klasy koncentratora, wymaganie autoryzacji określony jest stosowane do wszystkich metod koncentratora. Poniżej przedstawiono wymagania autoryzacji, które można stosować różne typy. Bez `Authorize` atrybutu, wszystkie metody publiczne koncentratora są dostępne dla klienta, który jest podłączony do koncentratora.

Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Administrator", można określić, że tylko użytkownicy w tej roli mają dostęp do Centrum następującym kodem.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Alternatywnie można określić, że koncentrator zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Poniższe przykłady dotyczą autoryzacji różnych scenariuszy:

- `[Authorize]`— tylko do uwierzytelnionych użytkowników
- `[Authorize(Roles = "Admin,Manager")]`— tylko do uwierzytelnionych użytkowników w określonych ról
- `[Authorize(Users = "user1,user2")]`— tylko do uwierzytelnionych użytkowników z określone nazwy użytkownika
- `[Authorize(RequireOutgoing=false)]`— tylko do uwierzytelnionych użytkowników może wywołać koncentratora, ale połączenia z serwera do klientów nie są ograniczone przez autoryzacji, takich jak podczas tylko określonym użytkownikom można wysłać komunikatu, ale wszystkie inne mogą odbierać wiadomości. Właściwość niej można stosować do całego elementu hub, nie na metod osób w ramach koncentratora. Po niej nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są nazywane z serwera.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Wymagaj uwierzytelniania dla wszystkich koncentratorów.

Może być wymagane uwierzytelnienie dla wszystkich koncentratorów i metod koncentratorów w aplikacji przez wywołanie metody [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metody podczas uruchamiania aplikacji. Gdy ma wiele koncentratorów i mają być egzekwowane wymaganie uwierzytelniania dla wszystkich z nich można użyć tej metody. Przy użyciu tej metody nie można określić roli, użytkowników lub wychodzących autoryzacji. Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych. Można jednak nadal stosować atrybutu autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania. Wszystkie wymagania określone w atrybutach jest stosowany oprócz podstawowych wymagań uwierzytelniania.

W poniższym przykładzie przedstawiono plik Global.asax, który ogranicza wszystkich metod koncentratora użytkownikom uwierzytelnionym.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Jeśli należy wywołać `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku. Zgłoszenia tego wyjątku, ponieważ nie można dodać moduł do metodę HubPipeline po potok został wywołany. W poprzednim przykładzie przedstawiono wywoływania `RequireAuthentication` metody w `Application_Start` metodę, która jest wykonywana raz przed obsługi pierwszego żądania.

<a id="custom"></a>

## <a name="customized-authorization"></a>Dostosowane autoryzacji

Jeśli musisz dostosować jak Autoryzacja jest określona, można utworzyć klasy, która jest pochodną `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody. Ta metoda jest wywoływana dla każdego żądania w celu określenia, czy użytkownik jest autoryzowany do wykonania żądania. Przeciążonej musisz podać logiki niezbędne dla danego scenariusza autoryzacji. Poniższy przykład pokazuje, jak wymusić autoryzacji za pomocą tożsamości opartej na oświadczeniach.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Przekaż informacje dotyczące uwierzytelniania dla klientów

Może być konieczne używanie informacji uwierzytelniania w kodzie, który działa na kliencie. Należy przekazać wymaganych informacji podczas wywoływania metody na kliencie. Na przykład metoda aplikacji rozmów można przekazać jako parametr nazwę osoby publikowanie komunikatów, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Alternatywnie można utworzyć obiektu reprezentuje informacje dotyczące uwierzytelniania i przekazywanie obiektu jako parametru, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Nigdy nie należy przekazać jeden klient identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladować żądania z tego klienta.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opcje uwierzytelniania dla klientów platformy .NET

Jeśli masz klienta programu .NET, na przykład aplikacji konsoli, która współdziała z koncentratora, który jest ograniczone do uwierzytelnionych użytkowników, należy przekazać poświadczenia uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikatu. Przykłady w tej sekcji pokazano, jak użyć tych innych metod uwierzytelniania użytkownika. Nie są one aplikacji SignalR w pełni funkcjonalne. Aby uzyskać więcej informacji dotyczących klientów platformy .NET z SignalR, zobacz [Podręcznik interfejsu API koncentratory - klienta .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Gdy klient .NET użyje koncentratora, który korzysta z uwierzytelniania formularzy ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania w połączeniu. Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu. W poniższym przykładzie przedstawiono aplikacji konsoli, która pobiera pliku cookie uwierzytelniania ze strony sieci web i dodaje ten plik cookie dla połączenia. Adres URL `https://www.contoso.com/RemoteLogin` w punktach przykład do strony sieci web, którą należy utworzyć. Strona może pobrać nazwy oczekujących na opublikowanie użytkownika i hasła i prób zalogowania użytkownika przy użyciu poświadczeń.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Aplikacja konsoli wpisów poświadczenia www.contoso.com/RemoteLogin którego można odwoływać się do pustej strony, która zawiera następujący plik CodeBehind.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Podczas korzystania z uwierzytelniania systemu Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości. Poświadczenia dla połączenia zostanie ustawiona wartość DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Nagłówek połączenia

Jeśli aplikacja nie używa plików cookie, należy przekazać informacje o użytkowniku w nagłówku połączenia. Na przykład można przekazać tokenu w nagłówku połączenia.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Następnie w Centrum, możesz Zweryfikuj token użytkownika.

<a id="certificate"></a>

### <a name="certificate"></a>certyfikat

Można przekazać certyfikatu klienta w celu weryfikacji użytkownika. Możesz dodać certyfikat, podczas tworzenia połączenia. Poniższy przykład przedstawia tylko sposób dodawania certyfikat klienta do połączenia; nie są wyświetlane w aplikacji konsoli pełna. Używa [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasy, która zapewnia kilka różnych sposobów, aby utworzyć certyfikat.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
