---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210109"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Jak kompilacji/uruchomić próbki danych bezpiecznego użytkownika

* Ustaw hasło, za pomocą narzędzia menedżera klucz tajny:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aktualizacja bazy danych:

  `dotnet ef database update`

* Włączanie obsługi protokołu HTTPS w projekcie
