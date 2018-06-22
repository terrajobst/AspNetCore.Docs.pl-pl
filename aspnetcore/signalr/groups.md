---
title: Zarządzaj użytkownikami i grupami w SignalR
author: rachelappel
description: Przegląd platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272084"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="eb3f9-103">Zarządzaj użytkownikami i grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="eb3f9-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="eb3f9-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="eb3f9-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="eb3f9-105">SignalR temu wiadomości wysłane do wszystkich połączeń skojarzone z określonym użytkownikiem, a także o nazwie grupy połączeń.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="eb3f9-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="eb3f9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="eb3f9-107">Użytkownicy w SignalR</span><span class="sxs-lookup"><span data-stu-id="eb3f9-107">Users in SignalR</span></span>

<span data-ttu-id="eb3f9-108">SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzone z określonym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="eb3f9-109">Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzone z połączeniem jako identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="eb3f9-110">Jeden użytkownik może mieć wiele połączeń z aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="eb3f9-111">Na przykład użytkownik może być połączony na pulpicie, a także telefon.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="eb3f9-112">Każde urządzenie ma oddzielnego połączenia SignalR, ale są wszystkie skojarzone z tym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="eb3f9-113">Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z użytkownikiem zostanie wyświetlony komunikat.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="eb3f9-114">Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodę koncentratora, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="eb3f9-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="eb3f9-115">Identyfikator użytkownika jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="eb3f9-116">Identyfikator użytkownika można dostosować, tworząc `IUserIdProvider`i rejestrując ją w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="eb3f9-117">AddSignalR musi zostać wywołana przed zarejestrowaniem niestandardowych usług SignalR.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="eb3f9-118">Grupy w SignalR</span><span class="sxs-lookup"><span data-stu-id="eb3f9-118">Groups in SignalR</span></span>

<span data-ttu-id="eb3f9-119">Grupa jest kolekcją połączenia skojarzone z nazwą.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="eb3f9-120">Wiadomości mogą być wysyłane do wszystkich połączeń w grupie.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="eb3f9-121">Grupy są zalecanym sposobem wysłać do połączenia lub wiele połączeń, ponieważ grupy są zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="eb3f9-122">Połączenie może należeć do wielu grup.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="eb3f9-123">Dzięki temu grupy idealne przypominać aplikacji rozmów, gdzie każdy miejsca może być reprezentowany jako grupa.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="eb3f9-124">Dodawać lub usunięte z grup za pośrednictwem połączenia `AddToGroupAsync` i `RemoveFromGroupAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="eb3f9-125">Członkostwo w grupie nie jest zachowywane podczas ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="eb3f9-126">Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="eb3f9-127">Nie jest możliwe liczba członków grupy, ponieważ te informacje nie są dostępne w przypadku skalowania aplikacji na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="eb3f9-128">Nazwy grup jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="eb3f9-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="eb3f9-129">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="eb3f9-129">Related resources</span></span>

* [<span data-ttu-id="eb3f9-130">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="eb3f9-130">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="eb3f9-131">Centra</span><span class="sxs-lookup"><span data-stu-id="eb3f9-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="eb3f9-132">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="eb3f9-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
