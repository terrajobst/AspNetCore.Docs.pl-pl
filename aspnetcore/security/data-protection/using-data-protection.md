---
title: Rozpoczynanie pracy z interfejsami API ochrony danych
author: rick-anderson
description: "Tym dokumencie opisano sposób użycia interfejsy API ochrony danych platformy ASP.NET Core dla ochrony i wyłączenie ochrony danych w aplikacji."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: e8c4183f5c47d8ffec8edf163eb1e4d6f2757d9d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a>Rozpoczynanie pracy z interfejsami API ochrony danych

<a name="security-data-protection-getting-started"></a>

W najprostszy, ochrona danych obejmuje następujące kroki:

1. Utwórz funkcji ochrony danych na podstawie dostawcę ochrony danych.

2. Wywołanie `Protect` metody z danymi, które chcesz chronić.

3. Wywołanie `Unprotect` metody z danymi chcesz włączyć ponownie w postaci zwykłego tekstu.

Większość platform i modeli aplikacji, takich jak ASP.NET lub SignalR, już konfiguracji systemu ochrony danych i dodaj go do kontenera usług, których dostęp za pomocą iniekcji zależności. W poniższym przykładzie pokazano Konfigurowanie usługi kontenera iniekcji zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem Podpisane, tworzenie ochrony i ochrona, a następnie wyłączanie ochrony danych

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Podczas tworzenia ochrony należy podać co najmniej jeden [ciągów w celu](consumer-apis/purpose-strings.md). Ciąg przeznaczenia zapewnia izolację między konsumentów. Na przykład utworzone za pomocą ciąg "green" cel ochrony nie będą mogli nie Chroń dane dostarczone przez funkcję ochrony z celem "purpurowy".

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań. Ma ona która po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`.
>
>Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich. Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.
