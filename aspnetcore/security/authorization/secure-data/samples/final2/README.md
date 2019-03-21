---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210109"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="8cf90-101">Jak kompilacji/uruchomić próbki danych bezpiecznego użytkownika</span><span class="sxs-lookup"><span data-stu-id="8cf90-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="8cf90-102">Ustaw hasło, za pomocą narzędzia menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="8cf90-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="8cf90-103">Aktualizacja bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8cf90-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="8cf90-104">Włączanie obsługi protokołu HTTPS w projekcie</span><span class="sxs-lookup"><span data-stu-id="8cf90-104">Enable HTTPS in the project</span></span>
