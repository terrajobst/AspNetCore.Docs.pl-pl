---
title: Zarządzaj użytkownikami i grupami w SignalR
author: rachelappel
description: Przegląd platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358465"
---
# <a name="manage-users-and-groups-in-signalr"></a>Zarządzaj użytkownikami i grupami w SignalR

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR temu wiadomości wysłane do wszystkich połączeń skojarzone z określonym użytkownikiem, a także o nazwie grupy połączeń.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Użytkownicy w SignalR

SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzone z określonym użytkownikiem. Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzone z połączeniem jako identyfikator użytkownika. Jeden użytkownik może mieć wiele połączeń z aplikacji SignalR. Na przykład użytkownik może być połączony na pulpicie, a także telefon. Każde urządzenie ma oddzielnego połączenia SignalR, ale są wszystkie skojarzone z tym użytkownikiem. Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z użytkownikiem zostanie wyświetlony komunikat.

Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodę koncentratora, jak pokazano w poniższym przykładzie:

> [!NOTE]
> Identyfikator użytkownika jest rozróżniana wielkość liter.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Identyfikator użytkownika można dostosować, tworząc `IUserIdProvider`i rejestrując ją w `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR musi zostać wywołana przed zarejestrowaniem niestandardowych usług SignalR.

## <a name="groups-in-signalr"></a>Grupy w SignalR

Grupa jest kolekcją połączenia skojarzone z nazwą. Wiadomości mogą być wysyłane do wszystkich połączeń w grupie. Grupy są zalecanym sposobem wysłać do połączenia lub wiele połączeń, ponieważ grupy są zarządzane przez aplikację. Połączenie może należeć do wielu grup. Dzięki temu grupy idealne przypominać aplikacji rozmów, gdzie każdy miejsca może być reprezentowany jako grupa. Dodawać lub usunięte z grup za pośrednictwem połączenia `AddToGroupAsync` i `RemoveFromGroupAsync` metody.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Członkostwo w grupie nie jest zachowywane podczas ponownego nawiązania połączenia. Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia. Nie jest możliwe liczba członków grupy, ponieważ te informacje nie są dostępne w przypadku skalowania aplikacji na wielu serwerach.

> [!NOTE]
> Nazwy grup jest rozróżniana wielkość liter.

## <a name="related-resources"></a>Zasoby pokrewne

* [Wprowadzenie](xref:signalr/get-started)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
