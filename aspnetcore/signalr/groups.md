---
title: Zarządzanie użytkownikami i grupami w SignalR
author: rachelappel
description: Omówienie platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402528"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="3689e-103">Zarządzanie użytkownikami i grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="3689e-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="3689e-104">Przez [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3689e-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="3689e-105">SignalR umożliwia wiadomości do wysłania do wszystkie połączenia skojarzone z określonym użytkownikiem, a także do nazwanych grup połączeń.</span><span class="sxs-lookup"><span data-stu-id="3689e-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="3689e-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3689e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="3689e-107">Użytkownicy w SignalR</span><span class="sxs-lookup"><span data-stu-id="3689e-107">Users in SignalR</span></span>

<span data-ttu-id="3689e-108">SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń, skojarzone z określonym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="3689e-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="3689e-109">Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3689e-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="3689e-110">Jeden użytkownik może mieć wiele połączeń aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="3689e-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="3689e-111">Można na przykład połączenia użytkownika, na pulpicie, a także swojego telefonu.</span><span class="sxs-lookup"><span data-stu-id="3689e-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="3689e-112">Każde urządzenie ma oddzielne połączenia SignalR, ale są one wszystkie skojarzone z tym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="3689e-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="3689e-113">Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem komunikat.</span><span class="sxs-lookup"><span data-stu-id="3689e-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="3689e-114">Identyfikator użytkownika dla połączenia może zostać oceniony przez `Context.UserIdentifier` właściwości Centrum.</span><span class="sxs-lookup"><span data-stu-id="3689e-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="3689e-115">Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodzie Centrum, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3689e-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="3689e-116">Identyfikator użytkownika jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="3689e-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="3689e-117">Identyfikator użytkownika, można dostosowywać, tworząc `IUserIdProvider`i zarejestrowanie go w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3689e-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="3689e-118">AddSignalR musi zostać wywołana przed zarejestrowaniem niestandardowych usług SignalR.</span><span class="sxs-lookup"><span data-stu-id="3689e-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="3689e-119">Grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="3689e-119">Groups in SignalR</span></span>

<span data-ttu-id="3689e-120">Grupy to zbiór połączenia skojarzone z nazwą.</span><span class="sxs-lookup"><span data-stu-id="3689e-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="3689e-121">Komunikaty mogą być wysyłane do wszystkich połączeń w grupie.</span><span class="sxs-lookup"><span data-stu-id="3689e-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="3689e-122">Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ te grupy są zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3689e-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="3689e-123">Połączenie może należeć do wielu grup.</span><span class="sxs-lookup"><span data-stu-id="3689e-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="3689e-124">Dzięki temu grupy idealne podobny aplikację do obsługi rozmów, gdzie każdego pomieszczenia mogą być reprezentowane jako grupa.</span><span class="sxs-lookup"><span data-stu-id="3689e-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="3689e-125">Połączenia, które mogą być dodawane do lub usunięte z grup za pośrednictwem `AddToGroupAsync` i `RemoveFromGroupAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="3689e-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="3689e-126">Członkostwo w grupie nie są zachowywane podczas ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="3689e-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="3689e-127">Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="3689e-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="3689e-128">Nie jest możliwe do zliczenia członkowie danej grupy, ponieważ te informacje nie są dostępne, jeśli aplikacja będzie skalowana na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="3689e-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="3689e-129">Nazwy grup jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="3689e-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3689e-130">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="3689e-130">Related resources</span></span>

* [<span data-ttu-id="3689e-131">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="3689e-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="3689e-132">Centra</span><span class="sxs-lookup"><span data-stu-id="3689e-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3689e-133">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="3689e-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
