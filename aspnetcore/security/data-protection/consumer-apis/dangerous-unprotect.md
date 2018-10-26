---
title: Wyłączanie ochrony ładunków, których klucze zostały odwołane w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wyłączyć ochronę danych, chronione przy użyciu kluczy, które od zostały odwołane, w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: b93ab0fa650041afdfaf1ed5572cc7e081bba244
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089353"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Wyłączanie ochrony ładunków, których klucze zostały odwołane w programie ASP.NET Core


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Interfejsy API ochrony danych programu ASP.NET Core nie są przeznaczone głównie dla nieokreślony stan trwały ładunki poufne. Inne technologie, takie jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](/rights-management/) są bardziej odpowiednie do scenariusza nieograniczony magazyn i mają możliwości odpowiednio silne zarządzania kluczami. Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu platformy ASP.NET Core interfejsy API ochrony danych dla długoterminowej ochrony poufnych danych. Klucze nie są nigdy usuwane z pierścienia klucza, więc `IDataProtector.Unprotect` zawsze można odzyskać istniejących ładunków, tak długo, jak klucze są dostępne i prawidłowy.

Jednak problem pojawia się podczas wyłączania ochrony danych, który został objęty ochroną za pomocą klucza odwołane, jako deweloper `IDataProtector.Unprotect` spowoduje zgłoszenie wyjątku w tym przypadku. Może to być dobrym rozwiązaniem dla ładunków krótkotrwałe lub przejściowe (na przykład tokeny uwierzytelniania), jak można łatwo odtworzyć te rodzaje ładunków w przez system, a w najgorszym przypadku użytkownik może być konieczne ponowne zalogowanie. Ale utrwalonych ładunków, mających `Unprotect` throw może prowadzić do utraty danych nie do przyjęcia.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Aby móc obsługiwać scenariusz umożliwienia ładunki do usunięcia ochrony nawet w przypadku odwołanego kluczy, system ochrony danych zawiera `IPersistedDataProtector` typu. Aby pobrać wystąpienie obiektu `IPersistedDataProtector`, wystarczy pobrać wystąpienie obiektu `IDataProtector` w normalny sposób i spróbuj wykonać rzutowanie `IDataProtector` do `IPersistedDataProtector`.

> [!NOTE]
> Nie wszystkie `IDataProtector` wystąpienia mogą być rzutowane na `IPersistedDataProtector`. Programiści powinni używać C# jako operator lub podobne w celu uniknięcia wyjątki środowiska uruchomieniowego spowodowane przez nieprawidłowy rzutowania i powinna być przygotowana do obsługi w przypadku niepowodzenia odpowiednio.

`IPersistedDataProtector` udostępnia następujące powierzchni interfejsu API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Ten interfejs API przyjmuje chronionych ładunek (w postaci tablicy bajtów) i zwraca ładunek niechronione. Nie istnieje żadne przeciążenie oparte na ciągach. Poniżej znajdują się dwa parametry wyjściowe.

* `requiresMigration`: ustawionej na wartość true, jeśli klucz używany do ochrony ten ładunek nie jest już aktywna domyślny klucz, np. klucz używany do ochrony ten ładunek jest stary i kluczem operację stopniowego od tego czasu, wpłynie miejsce. Obiekt wywołujący może chcieć należy wziąć pod uwagę ponowne włączanie ochrony ładunek, w zależności od swoich potrzeb biznesowych.

* `wasRevoked`: zostanie ustawiony na wartość true, jeśli klucz używany do ochrony ten ładunek został odwołany.

>[!WARNING]
> Zachować wyjątkową ostrożność podczas przekazywania `ignoreRevocationErrors: true` do `DangerousUnprotect` metody. Jeśli po wywołaniu tej metody `wasRevoked` ma wartość true, a następnie klucz używany do ochrony ten ładunek został odwołany i autentyczności ładunku powinny być traktowane jako podejrzana. W tym przypadku tylko kontynuować wykonywanie operacji na niechronionych ładunku w przypadku niektórych oddzielne pewność, że jej są autentyczne, np. że pochodzi z bezpiecznej bazie danych, a nie są wysyłane przez niezaufane sieci web klienta.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
