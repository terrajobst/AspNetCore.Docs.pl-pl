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
ms.openlocfilehash: 59e90042ecbaf936602643bbdc3965e036426b26
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963809"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a>Zarządzanie użytkownikami i grupami w SignalR

Autor [Brennan Conroy](https://github.com/BrennanConroy)

SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzonych z określonym użytkownikiem, a także do nazwanych grup połączeń.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="users-in-opno-locsignalr"></a>Użytkownicy w SignalR

SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń skojarzonych z określonym użytkownikiem. Domyślnie SignalR używa `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonego z połączeniem jako identyfikatorem użytkownika. Pojedynczy użytkownik może mieć wiele połączeń z aplikacją SignalR. Na przykład użytkownik może być połączony na swoim komputerze, a także na telefonie. Każde urządzenie ma oddzielne połączenie SignalR, ale wszystkie są skojarzone z tym samym użytkownikiem. Jeśli wiadomość jest wysyłana do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem otrzymują komunikat. Do identyfikatora użytkownika dla połączenia można uzyskać dostęp za pomocą właściwości `Context.UserIdentifier` w centrum.

Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika do funkcji `User` w metodzie centrum, jak pokazano w następującym przykładzie:

> [!NOTE]
> W identyfikatorze użytkownika jest rozróżniana wielkość liter.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a>Grupy w SignalR

Grupa jest kolekcją połączeń skojarzonych z nazwą. Komunikaty mogą być wysyłane do wszystkich połączeń w grupie. Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ grupy są zarządzane przez aplikację. Połączenie może być członkiem wielu grup. Sprawia to, że grupy są idealnym rozwiązaniem, takim jak aplikacja czatu, gdzie każde pomieszczenie może być reprezentowane jako Grupa. Połączenia mogą być dodawane lub usuwane z grup za pośrednictwem metod `AddToGroupAsync` i `RemoveFromGroupAsync`.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Członkostwo w grupie nie jest zachowywane po ponownym nawiązaniu połączenia. Połączenie musi ponownie dołączyć do grupy po jej ponownym ustanowieniu. Nie jest możliwe liczenie członków grupy, ponieważ te informacje są niedostępne, jeśli aplikacja jest skalowana na wielu serwerach.

Aby chronić dostęp do zasobów przy użyciu grup, użyj funkcji [uwierzytelniania i autoryzacji](xref:signalr/authn-and-authz) w ASP.NET Core. W przypadku dodawania użytkowników do grupy tylko wtedy, gdy poświadczenia są prawidłowe dla tej grupy, wiadomości wysyłane do tej grupy będą przekazywane tylko autoryzowanym użytkownikom. Jednak grupy nie są funkcją zabezpieczeń. Oświadczenia uwierzytelniania mają funkcje, które nie są, takie jak wygaśnięcie i odwoływanie. Jeśli uprawnienie użytkownika do uzyskiwania dostępu do grupy zostało odwołane, musisz ręcznie wykryć ten element i usunąć go z grupy.

> [!NOTE]
> W nazwach grup jest rozróżniana wielkość liter.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
