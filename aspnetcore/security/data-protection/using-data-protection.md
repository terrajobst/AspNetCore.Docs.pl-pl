---
title: Rozpoczynanie pracy z interfejsami API ochrony danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Dowiedz się, jak używać interfejsy API ochrony danych platformy ASP.NET Core dla ochrony i wyłączenie ochrony danych w aplikacji.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 3a69abd2b58e02f87ccaf2317b0a8a2a7e9d7b4a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Rozpoczynanie pracy z interfejsami API ochrony danych w podstawowej platformy ASP.NET

<a name="security-data-protection-getting-started"></a>

W najprostszy, ochrona danych obejmuje następujące kroki:

1. Utwórz funkcji ochrony danych na podstawie dostawcę ochrony danych.

2. Wywołanie `Protect` metody z danymi, które chcesz chronić.

3. Wywołanie `Unprotect` metody z danymi chcesz włączyć ponownie w postaci zwykłego tekstu.

Większość platform i modeli aplikacji, takich jak ASP.NET lub SignalR, już konfiguracji systemu ochrony danych i dodaj go do kontenera usług, których dostęp za pomocą iniekcji zależności. W poniższym przykładzie pokazano Konfigurowanie usługi kontenera iniekcji zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem Podpisane, tworzenie ochrony i ochrona, a następnie wyłączanie ochrony danych

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Podczas tworzenia ochrony należy podać co najmniej jeden [ciągów w celu](xref:security/data-protection/consumer-apis/purpose-strings). Ciąg przeznaczenia zapewnia izolację między konsumentów. Na przykład utworzone za pomocą ciąg "green" cel ochrony nie będą mogli nie Chroń dane dostarczone przez funkcję ochrony z celem "purpurowy".

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań. Ma ona która po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`.
>
>Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich. Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.
