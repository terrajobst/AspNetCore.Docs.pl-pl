---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665739"
---
# <a name="error-handling-sample-application"></a>Błąd podczas obsługi przykładowej aplikacji

Ta przykładowa aplikacja demonstruje scenariuszy opisanych w temacie [obsługi błędów w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) tematu.

## <a name="configure-a-custom-exception-handling-page"></a>Konfigurowanie niestandardowych wyjątków, Obsługa strony

Gdy aplikacja nie jest uruchomiony w środowisku deweloperskim, obsługa wyjątków w oprogramowaniu pośredniczącym:

* Przechwytuje wyjątki.
* Rejestruje wyjątki.
* Ponownie wykonuje żądanie w potoku alternatywne w podanej ścieżce.

## <a name="configure-custom-exception-handling-code"></a>Konfigurowanie niestandardowego kodu obsługi wyjątków

Alternatywa dla obsługująca punktu końcowego dla błędów za pomocą strony Obsługa niestandardowy wyjątek ma na celu dostarczenie wyrażenia lambda `UseExceptionHandler`. Używanie wyrażenia lambda z `UseExceptionHandler` zezwala na dostęp do błędu przed zwróceniem odpowiedzi.

Przykładowa aplikacja pokazuje niestandardowy wyjątek, Obsługa kodu w `Startup.Configure`. Postępuj zgodnie z instrukcjami w górnej części *Startup.cs* pliku (`LambdaErrorHandler`). Po uruchomieniu aplikacji, wywołanie wyjątku z **zgłosić wyjątek** łącze na stronie indeksu.
