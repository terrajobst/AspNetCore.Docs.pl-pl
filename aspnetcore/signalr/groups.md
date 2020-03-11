---
title: Zarządzanie użytkownikami i grupami w SignalR
author: bradygaster
description: Omówienie ASP.NET Core SignalR zarządzanie użytkownikami i grupami.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665866"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="89d54-103">Zarządzanie użytkownikami i grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="89d54-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="89d54-104">Autor [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="89d54-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="89d54-105"> umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzonych z określonym użytkownikiem, a także do nazwanych grup połączeń.</span><span class="sxs-lookup"><span data-stu-id="89d54-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="89d54-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="89d54-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="89d54-107">Użytkownicy w SignalR</span><span class="sxs-lookup"><span data-stu-id="89d54-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="89d54-108"> umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzonych z określonym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="89d54-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="89d54-109">Domyślnie SignalR używa `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonego z połączeniem jako identyfikatorem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="89d54-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="89d54-110">Pojedynczy użytkownik może mieć wiele połączeń z aplikacją SignalR.</span><span class="sxs-lookup"><span data-stu-id="89d54-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="89d54-111">Na przykład użytkownik może być połączony na swoim komputerze, a także na telefonie.</span><span class="sxs-lookup"><span data-stu-id="89d54-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="89d54-112">Każde urządzenie ma oddzielne połączenie SignalR, ale wszystkie są skojarzone z tym samym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="89d54-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="89d54-113">Jeśli wiadomość jest wysyłana do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem otrzymują komunikat.</span><span class="sxs-lookup"><span data-stu-id="89d54-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="89d54-114">Do identyfikatora użytkownika dla połączenia można uzyskać dostęp za pomocą właściwości `Context.UserIdentifier` w centrum.</span><span class="sxs-lookup"><span data-stu-id="89d54-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="89d54-115">Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika do funkcji `User` w metodzie centrum, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="89d54-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="89d54-116">W identyfikatorze użytkownika jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="89d54-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="89d54-117">Grupy w SignalR</span><span class="sxs-lookup"><span data-stu-id="89d54-117">Groups in SignalR</span></span>

<span data-ttu-id="89d54-118">Grupa jest kolekcją połączeń skojarzonych z nazwą.</span><span class="sxs-lookup"><span data-stu-id="89d54-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="89d54-119">Komunikaty mogą być wysyłane do wszystkich połączeń w grupie.</span><span class="sxs-lookup"><span data-stu-id="89d54-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="89d54-120">Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ grupy są zarządzane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="89d54-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="89d54-121">Połączenie może być członkiem wielu grup.</span><span class="sxs-lookup"><span data-stu-id="89d54-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="89d54-122">Sprawia to, że grupy są idealnym rozwiązaniem, takim jak aplikacja czatu, gdzie każde pomieszczenie może być reprezentowane jako Grupa.</span><span class="sxs-lookup"><span data-stu-id="89d54-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="89d54-123">Połączenia mogą być dodawane lub usuwane z grup za pośrednictwem metod `AddToGroupAsync` i `RemoveFromGroupAsync`.</span><span class="sxs-lookup"><span data-stu-id="89d54-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="89d54-124">Członkostwo w grupie nie jest zachowywane po ponownym nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="89d54-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="89d54-125">Połączenie musi ponownie dołączyć do grupy po jej ponownym ustanowieniu.</span><span class="sxs-lookup"><span data-stu-id="89d54-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="89d54-126">Nie jest możliwe liczenie członków grupy, ponieważ te informacje są niedostępne, jeśli aplikacja jest skalowana na wielu serwerach.</span><span class="sxs-lookup"><span data-stu-id="89d54-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="89d54-127">Aby chronić dostęp do zasobów przy użyciu grup, użyj funkcji [uwierzytelniania i autoryzacji](xref:signalr/authn-and-authz) w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="89d54-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="89d54-128">W przypadku dodawania użytkowników do grupy tylko wtedy, gdy poświadczenia są prawidłowe dla tej grupy, wiadomości wysyłane do tej grupy będą przekazywane tylko autoryzowanym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="89d54-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="89d54-129">Jednak grupy nie są funkcją zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="89d54-129">However, groups are not a security feature.</span></span> <span data-ttu-id="89d54-130">Oświadczenia uwierzytelniania mają funkcje, które nie są, takie jak wygaśnięcie i odwoływanie.</span><span class="sxs-lookup"><span data-stu-id="89d54-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="89d54-131">Jeśli uprawnienie użytkownika do uzyskiwania dostępu do grupy zostało odwołane, musisz ręcznie wykryć ten element i usunąć go z grupy.</span><span class="sxs-lookup"><span data-stu-id="89d54-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="89d54-132">W nazwach grup jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="89d54-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="89d54-133">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="89d54-133">Related resources</span></span>

* [<span data-ttu-id="89d54-134">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="89d54-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="89d54-135">Centra</span><span class="sxs-lookup"><span data-stu-id="89d54-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="89d54-136">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="89d54-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
