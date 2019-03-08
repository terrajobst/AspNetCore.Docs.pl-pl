---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665739"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="22226-101">Błąd podczas obsługi przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="22226-101">Error Handling Sample Application</span></span>

<span data-ttu-id="22226-102">Ta przykładowa aplikacja demonstruje scenariuszy opisanych w temacie [obsługi błędów w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) tematu.</span><span class="sxs-lookup"><span data-stu-id="22226-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="22226-103">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="22226-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="22226-104">Gdy aplikacja nie jest uruchomiony w środowisku deweloperskim, obsługa wyjątków w oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="22226-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="22226-105">Przechwytuje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="22226-105">Catches exceptions.</span></span>
* <span data-ttu-id="22226-106">Rejestruje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="22226-106">Logs exceptions.</span></span>
* <span data-ttu-id="22226-107">Ponownie wykonuje żądanie w potoku alternatywne w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="22226-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="22226-108">Konfigurowanie niestandardowego kodu obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="22226-108">Configure custom exception handling code</span></span>

<span data-ttu-id="22226-109">Alternatywa dla obsługująca punktu końcowego dla błędów za pomocą strony Obsługa niestandardowy wyjątek ma na celu dostarczenie wyrażenia lambda `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="22226-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="22226-110">Używanie wyrażenia lambda z `UseExceptionHandler` zezwala na dostęp do błędu przed zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="22226-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="22226-111">Przykładowa aplikacja pokazuje niestandardowy wyjątek, Obsługa kodu w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="22226-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="22226-112">Postępuj zgodnie z instrukcjami w górnej części *Startup.cs* pliku (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="22226-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="22226-113">Po uruchomieniu aplikacji, wywołanie wyjątku z **zgłosić wyjątek** łącze na stronie indeksu.</span><span class="sxs-lookup"><span data-stu-id="22226-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
