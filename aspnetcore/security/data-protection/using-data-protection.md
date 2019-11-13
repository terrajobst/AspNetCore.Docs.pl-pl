---
title: Wprowadzenie do interfejsów API ochrony danych w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać interfejsów API ochrony danych ASP.NET Core do ochrony i nieochrony danych w aplikacji.
ms.author: riande
ms.date: 11/12/2019
no-loc:
- SignalR
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 8c3f3c7fb21434cf335591c41741f0ce868df33e
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963865"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Wprowadzenie do interfejsów API ochrony danych w ASP.NET Core

<a name="security-data-protection-getting-started"></a>

W najprostszym zakresie Ochrona danych obejmuje następujące kroki:

1. Utwórz funkcję ochrony danych z poziomu dostawcy ochrony danych.

2. Wywołaj metodę `Protect` z danymi, które mają być chronione.

3. Wywołaj metodę `Unprotect` z danymi, które chcesz wrócić do zwykłego tekstu.

Większość platform i modeli aplikacji, takich jak ASP.NET Core lub SignalR, już konfiguruje system ochrony danych i dodaje go do kontenera usługi, do którego uzyskuje dostęp za pośrednictwem iniekcji zależności. Poniższy przykład demonstruje skonfigurowanie kontenera usługi pod kątem iniekcji zależności i rejestrowanie stosu ochrony danych, otrzymywanie dostawcy ochrony danych za pośrednictwem programu DI, utworzenie funkcji ochrony i ochronę danych.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Podczas tworzenia funkcji ochrony należy podać co najmniej jeden [ciąg przeznaczenia](xref:security/data-protection/consumer-apis/purpose-strings). Ciąg celu zapewnia izolację między użytkownikami. Na przykład funkcja ochrony utworzona z przeznaczeniem ciągu "Green" nie może wyłączyć ochrony danych dostarczonych przez funkcję ochrony w celu "purpurowego".

>[!TIP]
> Wystąpienia `IDataProtectionProvider` i `IDataProtector` są bezpieczne wątkowo dla wielu wywołań. Jest to zamierzone, gdy składnik pobiera odwołanie do `IDataProtector` za pośrednictwem wywołania do `CreateProtector`, będzie używać tego odwołania dla wielu wywołań `Protect` i `Unprotect`.
>
>Wywołanie `Unprotect` spowoduje zgłoszenie CryptographicException, jeśli nie można zweryfikować ani deszyfrowanie chronionego ładunku. Niektóre składniki mogą chcieć zignorować błędy podczas operacji usunięcia ochrony; składnik, który odczytuje pliki cookie uwierzytelniania, może obsłużyć ten błąd i traktować żądanie tak, jakby nie zawierało w ogóle pliku cookie, a nie niepowodzeniem. Składniki, które chcą tego zachowania, powinny zwrócić uwagę na CryptographicException zamiast połknięcia wszystkich wyjątków.
