---
title: Rozpoczynanie pracy z interfejsami API ochrony danych
author: rick-anderson
description: "Tym dokumencie opisano sposób użycia interfejsy API ochrony danych platformy ASP.NET Core dla ochrony i wyłączenie ochrony danych w aplikacji."
keywords: Platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Rozpoczynanie pracy z interfejsami API ochrony danych

<a name="security-data-protection-getting-started"></a>

W najprostszy, ochrona danych obejmuje następujące kroki:

1. Utwórz funkcji ochrony danych na podstawie dostawcę ochrony danych.

2. Wywołanie `Protect` metody z danymi, które chcesz chronić.

3. Wywołanie `Unprotect` metody z danymi chcesz włączyć ponownie w postaci zwykłego tekstu.

Większość platform i modeli aplikacji, takich jak ASP.NET lub SignalR, już konfiguracji systemu ochrony danych i dodaj go do kontenera usług, których dostęp za pomocą iniekcji zależności. W poniższym przykładzie pokazano Konfigurowanie usługi kontenera iniekcji zależności i rejestrowanie stosu ochrony danych, odbieranie dostawca ochrony danych za pośrednictwem Podpisane, tworzenie ochrony i ochrona, a następnie wyłączanie ochrony danych

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Podczas tworzenia ochrony należy podać co najmniej jeden [ciągów w celu](consumer-apis/purpose-strings.md). Ciąg przeznaczenia zapewnia izolację między konsumentów. Na przykład utworzone za pomocą ciąg "green" cel ochrony nie będzie mógł wyłączyć ochronę danych dostarczonych przez funkcję ochrony z celem "purpurowy".

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są wątkowo dla wielu wywołań. Jest zamierzone, który po składnika pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania `CreateProtector`, będzie używać tego odwołania na wiele wywołań `Protect` i `Unprotect`.
>
>Wywołanie `Unprotect` zgłosi cryptographicexception — Jeśli chroniony ładunku nie można zweryfikować lub odszyfrowywane. Niektóre składniki mogą chcieć ignorowanie błędów podczas wyłączania ochrony operacji; składnik, który brzmi plików cookie uwierzytelniania może obsługi tego błędu i traktować żądania tak, jakby zawierał pliki cookie nie na wszystkich zamiast Niepowodzenie żądania bezpośrednich. Składniki, które mają to zachowanie w szczególności powinny catch cryptographicexception — zamiast spożycie wszystkie wyjątki.
