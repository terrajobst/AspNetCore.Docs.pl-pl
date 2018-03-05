---
title: "Wyłączanie ochrony ładunków, której klucze zostały odwołane."
author: rick-anderson
description: "Tym dokumencie wyjaśniono, jak wyłączyć ochronę danych i chronione przy użyciu kluczy, które od chwili zostały odwołane, w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 37332dda794f898fb866424b38394f5d4441e166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Wyłączanie ochrony ładunków, której klucze zostały odwołane.

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Interfejsy API ochrony danych platformy ASP.NET Core nie są głównie przeznaczone do nieograniczonego trwałość ładunki poufne. Innych technologii, takich jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczonego magazynu i mają możliwości odpowiednio silne zarządzania kluczami. Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu interfejsy API ochrony danych platformy ASP.NET Core dla długoterminowej ochrony poufnych danych. Klucze są nigdy nie usuwane z pierścienia klucza, więc `IDataProtector.Unprotect` zawsze można odzyskać istniejących ładunków tak długo, jak klucze są dostępne i prawidłowe.

Jednak problemy występują, gdy developer podejmie próbę wyłączania ochrony danych, który jest chroniony za pomocą klucza odwołane, jako `IDataProtector.Unprotect` spowoduje zgłoszenie wyjątku w takim przypadku. Może to być poprawnie dla ładunków krótkim okresie lub przejściowy (na przykład tokeny uwierzytelniania), tego rodzaju ładunki można łatwo odtworzyć przez system, a w najgorszym przypadku użytkownik może być konieczne ponowne zalogowanie. Nawet w przypadku utrwalonego ładunków, o `Unprotect` throw może prowadzić do utraty danych można zaakceptować.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Na potrzeby scenariusza umożliwienia ładunków do usunięcia ochrony nawet w wypadku klucze odwołane, system ochrony danych zawiera `IPersistedDataProtector` typu. Można pobrać wystąpienia `IPersistedDataProtector`, wystarczy pobrać wystąpienia `IDataProtector` w sposób normalne i spróbuj wykonać rzutowanie `IDataProtector` do `IPersistedDataProtector`.

> [!NOTE]
> Nie wszystkie `IDataProtector` wystąpienia mogą być rzutowane na `IPersistedDataProtector`. Programiści powinni używać języka C# jako operator lub podobne, aby uniknąć wyjątki środowiska uruchomieniowego spowodowane przez nieprawidłowy rzutowania i powinny być przygotowane do odpowiednią obsługę w przypadku awarii.

`IPersistedDataProtector` udostępnia następujące powierzchni interfejsu API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Ten interfejs API przyjmuje chronione obciążenie (w postaci tablicy bajtów) i zwraca niechronione ładunku. Nie ma żadnych przeciążenia opartego na ciągach. Dostępne są następujące dwa parametry wyjściowe.

* `requiresMigration`: ustawionej na wartość true, jeśli klucz używany do ochrony tego ładunku nie jest już aktywne domyślny klucz, np. klucz używany do ochrony tego ładunku jest stary i ma klucz Wycofanie operacji ponieważ nastąpi miejsce. Obiekt wywołujący możesz rozważyć ponownej ochrony ładunku w zależności od ich potrzeb biznesowych.

* `wasRevoked`: zostanie ustawiona na true, jeśli klucz używany do ochrony tego ładunku został odwołany.

>[!WARNING]
> Zachować wyjątkową ostrożność podczas przekazywania `ignoreRevocationErrors: true` do `DangerousUnprotect` metody. Jeśli po wywołaniu tej metody `wasRevoked` ma wartość true, a następnie klucz używany do ochrony tego ładunku został odwołany i autentyczności ładunku powinny być traktowane jako podejrzana. W takim przypadku tylko nadal działających na niechronione ładunku, jeśli masz niektórych oddzielne gwarancji jest autentyczna, np. czy jest pochodzi z bezpiecznej bazie danych nie są wysyłane przez klienta niezaufanych sieci web.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
