---
title: Wprowadzenie do interfejsów API ochrony danych w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać platformy ASP.NET Core interfejsy API ochrony danych do ochrony i wyłączenie ochrony danych w aplikacji.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755955"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Wprowadzenie do interfejsów API ochrony danych w programie ASP.NET Core

<a name="security-data-protection-getting-started"></a>

W jego najprostszy i ochronę danych składa się z następujących czynności:

1. Utwórz funkcję ochrony danych na podstawie dostawca ochrony danych.

2. Wywołaj `Protect` metody z danymi, które chcesz chronić.

3. Wywołaj `Unprotect` metody przy użyciu danych chcesz przekształcić zwykły tekst.

Większość struktur i modele aplikacji, takich jak ASP.NET Core lub SignalR, już skonfigurować system ochrony danych i dodać go do kontenera usługi, których dostęp przy użyciu iniekcji zależności. W poniższym przykładzie pokazano konfigurowania usługi kontenera do wstrzykiwania zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem DI, tworzenia danych ochronę, a następnie nie jest chroniony i ochrony.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Po utworzeniu funkcję ochrony należy podać co najmniej jeden [ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings). Ciąg przeznaczenia zapewnia izolację między konsumentów. Na przykład funkcję ochrony utworzonych za pomocą ciągu cel "green" nie można wyłączyć ochronę danych udostępnionych przez funkcję ochrony z celem "purpurowy".

>[!TIP]
> Wystąpienia elementu `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu obiektów wywołujących. Ma zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` poprzez wywołanie `CreateProtector`, użyje tego odwołania dla wielu wywołań `Protect` i `Unprotect`.
>
>Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który odczytuje pliki cookie uwierzytelniania może obsługiwać ten błąd i traktować żądania tak, jakby był w ogóle pliki cookie nie zamiast zwraca Niepowodzenie żądania od razu wykupić. Składniki, które chcesz tego zachowania, należy w szczególności złapać cryptographicexception — zamiast powodu spożycia wszystkie wyjątki.
