---
title: Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 7cfe90115b0710fba196693efd309f7c914f0ad4
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234543"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Uwierzytelnianie i autoryzacja w biblioteki SignalR platformy ASP.NET Core

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Uwierzytelnianie użytkowników łączących się z Centrum SignalR

SignalR mogą być używane z [uwierzytelnianie programu ASP.NET Core](xref:security/authentication/index) do skojarzenia użytkownika z każdego połączenia. W centrum danych uwierzytelniania są dostępne z [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) właściwości. Uwierzytelnianie umożliwia piaście aby wywoływać metody na wszystkie połączenia skojarzone z użytkownikiem (zobacz [zarządzania użytkownikami i grupami w SignalR](xref:signalr/groups) Aby uzyskać więcej informacji). Wiele połączeń mogą być skojarzone z żadnym użytkownikiem.

### <a name="cookie-authentication"></a>Plik cookie uwierzytelniania

W aplikacji opartej na przeglądarce uwierzytelniania plików cookie umożliwia istniejących poświadczeń użytkownika w taki sposób, aby automatycznie przekazywane z połączeniami SignalR. Podczas korzystania z klienta przeglądarki, dodatkowa konfiguracja nie jest potrzebna. Jeśli użytkownik jest zalogowany do aplikacji, połączenia SignalR automatycznie dziedziczy tego uwierzytelnienia.

Pliki cookie są specyficzne dla przeglądarki sposób przesyłania tokenów dostępu, ale klientów nie korzystających z przeglądarki mogą wysyłać je. Korzystając z [klient modelu .NET](xref:signalr/dotnet-client), `Cookies` właściwości można skonfigurować w `.WithUrl` wywołanie w celu zapewnienia pliku cookie. Jednak przy użyciu uwierzytelniania plików cookie z klienta .NET wymaga aplikacji, aby dostarczać interfejs API w celu wymiany danych uwierzytelniania dla pliku cookie.

### <a name="bearer-token-authentication"></a>Uwierzytelnianie przy użyciu tokenów elementu nośnego

Klient może dostarczyć token dostępu, zamiast korzystać z pliku cookie. Serwer sprawdza poprawność tokenu i używa ich do identyfikacji użytkownika. To jest sprawdzana tylko wtedy, gdy połączenie zostanie nawiązane. W okresie obowiązywania połączenia serwera nie automatycznie ponownie sprawdź poprawność pod kątem tokenu odwołania.

Na serwerze uwierzytelniania tokenu elementu nośnego jest skonfigurowany przy użyciu [oprogramowanie pośredniczące elementu nośnego JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

W kliencie JavaScript, można określić token za pomocą [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opcji.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

W kliencie programu .NET jest podobny [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) właściwość, która może służyć do konfigurowania tokenu:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> Funkcja tokenu dostępu, należy podać jest wywoływana przed **co** żądania HTTP przez SignalR. Jeśli musisz odnowić token, aby zachować połączenie jako aktywne (ponieważ mogą stracić ważność podczas nawiązywania połączenia), to zrobić w ramach tej funkcji i zwróć zaktualizowanego tokenu.

W standardowych interfejsów API sieci web tokenów elementu nośnego, są wysyłane w nagłówku HTTP. Nie można ustawić tych nagłówków w przeglądarkach, korzystając z niektórych transportu jest jednak SignalR. Korzystając z funkcji WebSockets i zdarzenia Server-Sent, token jest przekazywany jako parametr ciągu zapytania. Aby to umożliwić, na serwerze, wymagana jest dodatkowa konfiguracja:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Pliki cookie, a tokenów elementu nośnego 

Ponieważ pliki cookie są specyficzne dla przeglądarek, wysyłając je od innych rodzajów klientów dodaje złożoności w porównaniu do wysyłania tokenów elementu nośnego. Z tego powodu uwierzytelniania plików cookie nie jest zalecane, chyba że aplikacja musi się tylko do uwierzytelniania użytkowników z klienta przeglądarki. Uwierzytelnianie przy użyciu tokenów elementu nośnego zaleca się podczas korzystania z klientów innych niż klient przeglądarki.

### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Jeśli [uwierzytelniania Windows](xref:security/authentication/windowsauth) jest skonfigurowana w Twojej aplikacji SignalR można użyć tej tożsamości do zabezpieczania koncentratorów. Jednak aby można było wysyłać komunikaty do poszczególnych użytkowników, należy dodać niestandardowego dostawcy Identyfikatora użytkownika. Jest to spowodowane uwierzytelniania systemu Windows nie zapewnia roszczenia "Identyfikator nazwy", która korzysta z biblioteki SignalR można ustalić nazwy użytkownika.

Dodaj nową klasę, która implementuje `IUserIdProvider` i pobieranie jedno z oświadczeń użytkownika do wykorzystania jako identyfikator. Na przykład, aby użyć oświadczenia "Name" (czyli nazwę użytkownika Windows w postaci `[Domain]\[Username]`), utworzyć następujące klasy:

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

Zamiast `ClaimTypes.Name`, możesz użyć dowolnej wartości z `User` (np. identyfikator Windows SID itd.).

> [!NOTE]
> Wartość, którą wybierzesz musi być unikatowa wśród wszystkich użytkowników w Twoim systemie. W przeciwnym razie komunikat przeznaczony dla jednego użytkownika może się okazać, przechodząc do innego użytkownika.

Zarejestruj ten składnik w usługi `Startup.ConfigureServices` metoda **po** wywołania `.AddSignalR`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

W kliencie programu .NET, należy włączyć uwierzytelnianie Windows, ustawiając [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) właściwości:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

Uwierzytelnianie Windows tylko jest obsługiwany przez klienta przeglądarki, korzystając z programu Microsoft Internet Explorer lub Microsoft Edge.

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Zezwolić użytkownikom na dostęp do koncentratorów i metod koncentratorów

Domyślnie wszystkie metody w Centrum można wywoływać za pomocą nieuwierzytelniony użytkownik. Aby wymagać uwierzytelniania, należy zastosować [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) atrybutu do Centrum:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Można użyć w argumentach konstruktora i właściwości działania `[Authorize]` atrybutu, aby ograniczyć dostęp do użytkowników tylko pasujących do określonych [zasady autoryzacji](xref:security/authorization/policies). Na przykład, jeśli masz niestandardowych zasad autoryzacji o nazwie `MyAuthorizationPolicy` upewnij się, tylko użytkownicy dopasowania tych zasad mają dostęp do Centrum, używając następującego kodu:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Może mieć metod koncentratora poszczególnych `[Authorize]` zastosowany także. Jeśli bieżący użytkownik nie jest zgodny zasady zastosowane do metody, błąd jest zwracany do obiektu wywołującego:

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
