---
title: Zarządzanie użytkownikami i grupami w SignalR
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 0a4836cfa3cf79136b56da1ff05ce8533b4df16c
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837887"
---
# <a name="manage-users-and-groups-in-signalr"></a>Zarządzanie użytkownikami i grupami w SignalR

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR umożliwia wiadomości do wysłania do wszystkie połączenia skojarzone z określonym użytkownikiem, a także do nazwanych grup połączeń.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Użytkownicy w SignalR

SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń, skojarzone z określonym użytkownikiem. Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika. Jeden użytkownik może mieć wiele połączeń aplikacji SignalR. Można na przykład połączenia użytkownika, na pulpicie, a także swojego telefonu. Każde urządzenie ma oddzielne połączenia SignalR, ale są one wszystkie skojarzone z tym użytkownikiem. Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem komunikat. Identyfikator użytkownika dla połączenia może zostać oceniony przez `Context.UserIdentifier` właściwości Centrum.

Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodzie Centrum, jak pokazano w poniższym przykładzie:

> [!NOTE]
> Identyfikator użytkownika jest rozróżniana wielkość liter.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Identyfikator użytkownika, można dostosowywać, tworząc `IUserIdProvider`i zarejestrowanie go w `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR musi zostać wywołana przed zarejestrowaniem niestandardowych usług SignalR.

## <a name="groups-in-signalr"></a>Grupami w SignalR

Grupy to zbiór połączenia skojarzone z nazwą. Komunikaty mogą być wysyłane do wszystkich połączeń w grupie. Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ te grupy są zarządzane przez aplikację. Połączenie może należeć do wielu grup. Dzięki temu grupy idealne podobny aplikację do obsługi rozmów, gdzie każdego pomieszczenia mogą być reprezentowane jako grupa. Połączenia, które mogą być dodawane do lub usunięte z grup za pośrednictwem `AddToGroupAsync` i `RemoveFromGroupAsync` metody.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Członkostwo w grupie nie są zachowywane podczas ponownego nawiązania połączenia. Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia. Nie jest możliwe do zliczenia członkowie danej grupy, ponieważ te informacje nie są dostępne, jeśli aplikacja będzie skalowana na wielu serwerach.

Aby chronić dostęp do zasobów podczas korzystania z grup, użyj [uwierzytelnianie i autoryzacja](xref:signalr/authn-and-authz) funkcji w programie ASP.NET Core. Tylko dodanie użytkowników do grupy, jeśli poświadczenia są prawidłowe dla tej grupy, komunikaty wysyłane do tej grupy zaczną się tylko dla autoryzowanych użytkowników. Jednak grupy nie są funkcją zabezpieczeń. Uwierzytelniania oświadczeń ma funkcje, których nie grupy, na przykład wygaśnięcia i odwoływania praw dostępu. Uprawnienia użytkownika, aby uzyskać dostęp do grupy jest odwołany, trzeba ręcznie wykryje to i usunąć je z niej.

> [!NOTE]
> Nazwy grup jest rozróżniana wielkość liter.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
