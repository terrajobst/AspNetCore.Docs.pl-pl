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
ms.openlocfilehash: 1bd61b8faff22db3337e9d9c1ed2532d3e64440d
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287471"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Uwierzytelnianie i autoryzacja centrów usługi SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano, jak ograniczyć, które użytkownicy lub ról dostęp do metod koncentratora.


## <a name="overview"></a>Omówienie

Ten temat zawiera następujące sekcje:

- [Autoryzuj atrybutu](#authorizeattribute)
- [Wymagaj uwierzytelniania do wszystkich centrów](#requireauth)
- [Dostosowane autoryzacji](#custom)
- [Przekaż informacje dotyczące uwierzytelniania dla klientów](#passauth)
- [Opcje uwierzytelniania dla klientów programu .NET](#authoptions)

    - [Plik cookie uwierzytelniania formularzy](#cookie)
    - [Uwierzytelnianie Windows](#windows)
    - [Nagłówek połączenia](#header)
    - [Certyfikat](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autoryzuj atrybutu

Biblioteka SignalR udostępnia [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, którzy użytkownicy lub które role mają dostęp do Centrum lub metody. Ten atrybut znajduje się w `Microsoft.AspNet.SignalR` przestrzeni nazw. Należy zastosować `Authorize` atrybutu koncentratora lub konkretnej metody koncentratora. Po zastosowaniu `Authorize` do klasy koncentratora, wymóg autoryzacji określony jest stosowany do wszystkich metod koncentratora. Poniżej przedstawiono różne rodzaje wymagań autoryzacji, które można zastosować. Bez `Authorize` atrybutu, wszystkie publiczne metody koncentratora są dostępne dla klienta, który jest podłączony do koncentratora.

Jeśli zdefiniowano roli w aplikacji sieci web o nazwie "Admin", można określić, że tylko użytkownicy w tej roli dostęp do Centrum z następującym kodem.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Lub można określić, czy Centrum zawiera jedną metodę, która jest dostępna dla wszystkich użytkowników, a druga metoda, która jest dostępna tylko dla uwierzytelnionych użytkowników, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Poniższe przykłady scenariuszy różnych autoryzacji:

- `[Authorize]` — tylko do uwierzytelnionych użytkowników
- `[Authorize(Roles = "Admin,Manager")]` — tylko do uwierzytelnionych użytkowników w określonych ról
- `[Authorize(Users = "user1,user2")]` — tylko do uwierzytelnionych użytkowników przy użyciu nazwy określonego użytkownika
- `[Authorize(RequireOutgoing=false)]` — tylko do uwierzytelnionych użytkowników można wywołać koncentratora, ale wywołania z serwera do klientów nie są ograniczone przez autoryzacji, takich jak, gdy tylko w przypadku niektórych użytkowników może także wysłać komunikat, ale wszystkie inne pojawia się komunikat o. Właściwość RequireOutgoing może zostać zastosowana tylko do całego centrum hub, nie na metody osób w ramach Centrum. Gdy RequireOutgoing nie jest ustawiona na wartość false, tylko użytkownicy, którzy spełniają wymagania autoryzacji są wywoływane z serwera.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Wymagaj uwierzytelniania do wszystkich centrów

Można wymagać uwierzytelniania do wszystkich koncentratorów i metod koncentratorów w aplikacji, wywołując [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metoda podczas uruchamiania aplikacji. Ta metoda jest przydatna po wiele centrów i chcesz wymuszać wymogu uwierzytelniania dla wszystkich z nich. Przy użyciu tej metody nie można określić roli, użytkowników lub wychodzących autoryzacji. Można tylko określić, że dostęp do metod koncentratora jest ograniczony do użytkowników uwierzytelnionych. Można jednak stosować atrybut autoryzacji do koncentratorów i metod, aby określić dodatkowe wymagania. Wszelkie wymagania, którą określasz w atrybutach są stosowane oprócz podstawowych wymagań uwierzytelniania.

Poniższy przykład pokazuje plik Global.asax, który ogranicza możliwość użycia wszystkich metod koncentratora do uwierzytelnionych użytkowników.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Jeśli wywołasz `RequireAuthentication()` metody po przetworzeniu żądania SignalR, SignalR zgłosi `InvalidOperationException` wyjątku. Ten wyjątek jest generowany, ponieważ nie można dodać modułu do HubPipeline, po wywołaniu potoku. W poprzednim przykładzie pokazano wywołanie `RequireAuthentication` method in Class metoda `Application_Start` metodę, która jest wykonywana jeden raz przed pierwszego żądania obsługi.

<a id="custom"></a>

## <a name="customized-authorization"></a>Dostosowane autoryzacji

Jeśli trzeba dostosować, jak Autoryzacja jest określona, możesz utworzyć klasę, która pochodzi od klasy `AuthorizeAttribute` i zastąpić [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metody. Ta metoda jest wywoływana dla każdego żądania ustalić, czy użytkownik jest autoryzowany do wykonania żądania. W metodzie zgodnym z przesłoniętą podasz logikę potrzebną dla danego scenariusza autoryzacji. Poniższy przykład pokazuje, jak wymusić autoryzację za pomocą tożsamości opartej na oświadczeniach.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Przekaż informacje dotyczące uwierzytelniania dla klientów

Może być konieczne używanie informacji uwierzytelniania w kodzie, który jest uruchamiany na kliencie. Wymagane informacje są przekazywane podczas wywoływania metody na kliencie. Na przykład metoda aplikacji rozmów można przekazać jako parametr Nazwa użytkownika osoby, publikując wiadomość, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Alternatywnie można utworzyć obiektu reprezentują informacje dotyczące uwierzytelniania, a następnie przekazać ten obiekt jako parametr, jak pokazano poniżej.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Nigdy nie należy przekazywać jednego klienta identyfikator połączenia dla innych klientów, ponieważ złośliwy użytkownik może użyć go do naśladowania żądania tego klienta.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opcje uwierzytelniania dla klientów programu .NET

Jeśli masz klienta platformy .NET, takie jak aplikacja konsolowa, która współdziała z koncentratora, który jest ograniczony do użytkowników uwierzytelnionych, można przekazać poświadczeń uwierzytelniania w pliku cookie, nagłówek połączenia lub certyfikat. Przykłady w tej sekcji pokazano, jak używać tych różnych metod uwierzytelniania użytkownika. Nie są one w pełni funkcjonalne aplikacje SignalR. Aby uzyskać więcej informacji na temat klientów programu .NET przy użyciu SignalR, zobacz [Podręcznik interfejsu API centrów — klient modelu .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Plik cookie

Twój klient .NET wchodzi w interakcję z koncentratora, który korzysta z uwierzytelniania formularzy programu ASP.NET, należy ręcznie ustawić pliku cookie uwierzytelniania dla połączenia. Dodawanie pliku cookie do `CookieContainer` właściwość [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) obiektu. Poniższy przykład pokazuje aplikacja konsolowa, która pobiera pliku cookie uwierzytelniania ze strony sieci web, a następnie dodaje ten plik cookie dla połączenia. Adres URL `https://www.contoso.com/RemoteLogin` w przykładzie wskazuje do strony sieci web, którą należy utworzyć. Strona pobierania nazwy przesłanych użytkownika i hasła i prób zalogowania użytkownika przy użyciu poświadczeń.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Aplikacja konsoli opublikuje poświadczenia, aby www.contoso.com/RemoteLogin której mogłoby się odwoływać pusta strona, zawierający następujący plik CodeBehind.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Korzystając z uwierzytelniania Windows, można przekazać poświadczeń bieżącego użytkownika za pomocą [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) właściwości. Poświadczenia dla połączenia jest ustawiona na wartość DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Nagłówek połączenia

Jeśli aplikacja nie używa plików cookie, można przekazać informacje o użytkowniku w nagłówku połączenia. Na przykład można przekazać token w nagłówku połączenia.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Następnie w Centrum, możesz sprawdzić tokenu użytkownika.

<a id="certificate"></a>

### <a name="certificate"></a>Certyfikat

Można przekazać certyfikatu klienta w celu zweryfikowania użytkownika. Dodaj certyfikat, podczas tworzenia połączenia. Poniższy przykład pokazuje tylko sposób dodawania certyfikatu klienta dla połączenia; Aplikacja konsoli pełne nie są wyświetlane. Używa ona [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) klasę, która oferuje kilka sposobów, aby utworzyć certyfikat.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
