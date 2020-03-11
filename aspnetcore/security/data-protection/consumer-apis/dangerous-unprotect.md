---
title: Wyłącz ochronę ładunków, których klucze zostały odwołane w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wyłączyć ochronę danych, a następnie chronić klucze, które zostały odwołane w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666601"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Wyłącz ochronę ładunków, których klucze zostały odwołane w ASP.NET Core

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Interfejsy API ochrony danych ASP.NET Core nie są przede wszystkim przeznaczone do nieograniczonego trwałości poufnych ładunków. Inne technologie, takie jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [Azure Rights Management](/rights-management/) , są bardziej odpowiednie dla scenariusza nieograniczonego magazynu i mają odpowiadające im silne możliwości zarządzania kluczami. Oznacza to, że nie ma żadnych zakazów używania ASP.NET Core interfejsów API ochrony danych do długoterminowej ochrony poufnych danych. Klucze nigdy nie są usuwane z pierścienia kluczy, więc `IDataProtector.Unprotect` zawsze mogą odzyskać istniejące ładunki, o ile klucze są dostępne i prawidłowe.

Jednak występuje problem, gdy deweloper próbuje wyłączyć ochronę danych chronionych za pomocą odwołanego klucza, ponieważ w takim przypadku `IDataProtector.Unprotect` zgłosi wyjątek. Może to być korzystne w przypadku ładunków krótkoterminowych lub przejściowych (takich jak tokeny uwierzytelniania), ponieważ te rodzaje ładunków mogą być w łatwy sposób odtworzone przez system, a w najgorszym miejscu może być wymagane zalogowanie się do osoby odwiedzającej. Jednak w przypadku utrwalonych ładunków, których `Unprotect` throw może spowodować nieakceptowalną utratę danych.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Aby obsłużyć scenariusz zezwalania na brak ochrony ładunków, nawet w przypadku wycofanych kluczy, system ochrony danych zawiera typ `IPersistedDataProtector`. Aby uzyskać wystąpienie `IPersistedDataProtector`, wystarczy uzyskać wystąpienie `IDataProtector` w normalny sposób i spróbować rzutować `IDataProtector` do `IPersistedDataProtector`.

> [!NOTE]
> Nie wszystkie wystąpienia `IDataProtector` mogą być rzutowane na `IPersistedDataProtector`. Deweloperzy powinni użyć operatora C# AS lub podobnego, aby uniknąć wyjątków środowiska uruchomieniowego spowodowanych przez nieprawidłowe rzutowanie, i powinny być przygotowane do obsługi odpowiedniego przypadku awarii.

`IPersistedDataProtector` uwidacznia następującą powierzchnię interfejsu API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Ten interfejs API pobiera chroniony ładunek (jako tablicę bajtową) i zwraca ładunek niechroniony. Brak przeciążenia opartego na ciągach. Dwa parametry out są następujące.

* `requiresMigration`: zostanie ustawiona na wartość true, jeśli klucz używany do ochrony tego ładunku nie jest już aktywnym kluczem domyślnym, np. klucz używany do ochrony tego ładunku jest stary i przeprowadzono kluczową operację wycofywania. Obiekt wywołujący może chcieć rozważyć ponowne włączenie ochrony ładunku w zależności od potrzeb firmy.

* `wasRevoked`: zostanie ustawiona na wartość true, jeśli klucz używany do ochrony tego ładunku został odwołany.

>[!WARNING]
> Przed przekazaniem `ignoreRevocationErrors: true` do metody `DangerousUnprotect` należy zachować szczególną ostrożność. Jeśli po wywołaniu tej metody `wasRevoked` wartość true, klucz używany do ochrony tego ładunku został odwołany, a autentyczność ładunku powinna być traktowana jako podejrzana. W takim przypadku Kontynuuj działanie w niechronionym ładunku, jeśli masz osobną gwarancję, że jest ona autentyczna, np. pochodzi z bezpiecznej bazy danych, a nie jest wysyłana przez niezaufanego klienta sieci Web.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
