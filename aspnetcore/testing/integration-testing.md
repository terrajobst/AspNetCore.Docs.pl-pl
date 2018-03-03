---
title: Testowanie w ASP.NET Core integracji
author: ardalis
description: "Jak korzystać z integracji platformy ASP.NET Core testy w celu zapewnienia poprawnego działania składników aplikacji."
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 8c28f1b4f66433eaebd9e474e784ecf3f1ac271b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="01ccf-103">Testowanie w ASP.NET Core integracji</span><span class="sxs-lookup"><span data-stu-id="01ccf-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="01ccf-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="01ccf-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="01ccf-105">Testowanie integracji gwarantuje, że składniki aplikacji działać prawidłowo, jeśli połączone ze sobą.</span><span class="sxs-lookup"><span data-stu-id="01ccf-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="01ccf-106">Integracja obsługuje platformy ASP.NET Core testowania przy użyciu platform testów jednostkowych i wbudowane testu hosta sieci web, który może służyć do obsługi żądań bez obciążenie sieci.</span><span class="sxs-lookup"><span data-stu-id="01ccf-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="01ccf-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01ccf-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="01ccf-108">Wprowadzenie do integracji testowania</span><span class="sxs-lookup"><span data-stu-id="01ccf-108">Introduction to integration testing</span></span>

<span data-ttu-id="01ccf-109">Testy integracji Sprawdź, czy różne części aplikacji poprawnie współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="01ccf-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="01ccf-110">W odróżnieniu od [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integracja testy obejmują często dotyczy infrastruktury aplikacji, takich jak bazy danych, system plików, zasobów sieciowych lub żądania sieci web i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01ccf-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="01ccf-111">Testy jednostkowe używać elementów sztucznych lub zasymulować obiektów zamiast tych problemów, ale testy integracji ma na celu potwierdzenie, że system działa zgodnie z oczekiwaniami do tych systemów.</span><span class="sxs-lookup"><span data-stu-id="01ccf-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="01ccf-112">Testy integracji, ponieważ korzystają oni większych segmenty kodu i opierają się na elementy infrastruktury zazwyczaj rzędów wolniej niż testy jednostkowe można.</span><span class="sxs-lookup"><span data-stu-id="01ccf-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="01ccf-113">W związku z tym jest dobrym rozwiązaniem jest ograniczenie ile testów integracji zapisu, zwłaszcza, jeśli takie samo zachowanie można przetestować z testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="01ccf-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="01ccf-114">Jeśli niektóre zachowanie można przetestować przy użyciu testu jednostkowego lub testu integracji, preferowane testu jednostkowego, ponieważ będzie prawie zawsze można szybciej.</span><span class="sxs-lookup"><span data-stu-id="01ccf-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="01ccf-115">Konieczne może być dziesiątki lub setki testy jednostek z wielu różnych danych wejściowych, ale tylko kilka najważniejsze scenariusze obejmujące testów integracji.</span><span class="sxs-lookup"><span data-stu-id="01ccf-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="01ccf-116">Testowanie logiki w ramach własnego metod jest zwykle domeny testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="01ccf-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="01ccf-117">Testowanie, jak aplikacja działa w ramach, na przykład z platformy ASP.NET Core lub z bazą danych jest gdzie testów integracji wchodzić w grę.</span><span class="sxs-lookup"><span data-stu-id="01ccf-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="01ccf-118">Nie wymaga zbyt wiele testów integracji, aby upewnić się, że jesteś w stanie zapisanie wiersza do bazy danych i ponownie go odczytać.</span><span class="sxs-lookup"><span data-stu-id="01ccf-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="01ccf-119">Nie należy przetestować każdej permutacji możliwe kodu dostępu do danych — należy przetestować za mało, aby mieć pewność, że aplikacja działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="01ccf-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="01ccf-120">Testowanie integracji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01ccf-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="01ccf-121">Aby uzyskać ustawione do integracji wykonywania testów, należy utworzyć projektu testowego, Dodaj odwołanie do projektu sieci web platformy ASP.NET Core i zainstaluj moduł uruchamiający.</span><span class="sxs-lookup"><span data-stu-id="01ccf-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="01ccf-122">Ten proces jest opisany w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentacji wraz z bardziej szczegółowe instrukcje na temat uruchamiania testów i zalecenia dotyczące nazewnictwa sieci testy i klasy testowe.</span><span class="sxs-lookup"><span data-stu-id="01ccf-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="01ccf-123">Oddzielanie testy jednostkowe i integracji testów przy użyciu różnych projektów.</span><span class="sxs-lookup"><span data-stu-id="01ccf-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="01ccf-124">To pomaga zapewnić, że nie przypadkowego wprowadzenia zastrzeżenia co do infrastruktury do testy jednostkowe i pozwala łatwo wybrać zestaw testów do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="01ccf-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="01ccf-125">Host testów</span><span class="sxs-lookup"><span data-stu-id="01ccf-125">The Test Host</span></span>

<span data-ttu-id="01ccf-126">Platformy ASP.NET Core obejmuje hosta testów, który można dodać do integracji projekty testowe, a używany do hostowania platformy ASP.NET Core aplikacji, test obsługujących żądania bez konieczności hosta sieci web prawdziwe.</span><span class="sxs-lookup"><span data-stu-id="01ccf-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="01ccf-127">Dostarczona próbka zawiera projekt testowy integracji, który został skonfigurowany do używania [xUnit](https://xunit.github.io) i hosta testów.</span><span class="sxs-lookup"><span data-stu-id="01ccf-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="01ccf-128">Używa `Microsoft.AspNetCore.TestHost` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="01ccf-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="01ccf-129">Raz `Microsoft.AspNetCore.TestHost` pakietu jest dołączony do projektu, można tworzyć i konfigurować `TestServer` w testach.</span><span class="sxs-lookup"><span data-stu-id="01ccf-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="01ccf-130">Następującego testu przedstawiono sposób sprawdzania, czy żądania skierowane do katalogu głównego witryny zwraca "Witaj świecie!"</span><span class="sxs-lookup"><span data-stu-id="01ccf-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="01ccf-131">i uruchamiać pomyślnie przed domyślnie sieci Web platformy ASP.NET Core pusty szablon utworzony przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01ccf-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="01ccf-132">Ten test jest przy użyciu wzorca Assert Act Rozmieść.</span><span class="sxs-lookup"><span data-stu-id="01ccf-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="01ccf-133">Krok Rozmieść odbywa się w konstruktorze, która tworzy wystąpienie `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="01ccf-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="01ccf-134">Skonfigurowany `WebHostBuilder` będzie używane do tworzenia `TestHost`; w tym przykładzie `Configure` metody z systemu w ramach testu (SUT) `Startup` klasy jest przekazywana do `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="01ccf-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="01ccf-135">Ta metoda będzie służyć do konfigurowania potoku żądania z `TestServer` do tak samo jak serwer SUT może być skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="01ccf-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="01ccf-136">W części Act testu, żądań do `TestServer` wystąpienia dla ścieżki "/" i odpowiedź jest do odczytu do ciągu.</span><span class="sxs-lookup"><span data-stu-id="01ccf-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="01ccf-137">Ten ciąg jest porównywana z oczekiwany ciąg "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="01ccf-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="01ccf-138">Jeśli są zgodne, test przechodzi poprawnie; w przeciwnym razie awarii.</span><span class="sxs-lookup"><span data-stu-id="01ccf-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="01ccf-139">Teraz można dodać kilka testy dodatkowe integracji, aby upewnić się, że pierwsze sprawdzanie funkcji działa za pośrednictwem aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="01ccf-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="01ccf-140">Należy pamiętać, że nie naprawdę próbujesz testu poprawności sprawdzania liczba pierwsza z tych testów, ale zamiast aplikacji sieci web wykonuje oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="01ccf-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="01ccf-141">Masz już pokrycie testu jednostki, który daje pewność działania w `PrimeService`, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="01ccf-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Eksplorator testów](integration-testing/_static/test-explorer.png)

<span data-ttu-id="01ccf-143">Dowiedz się więcej o testów jednostkowych w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) artykułu.</span><span class="sxs-lookup"><span data-stu-id="01ccf-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="01ccf-144">Testowanie Mvc i Razor integracji</span><span class="sxs-lookup"><span data-stu-id="01ccf-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="01ccf-145">Projekty testowe, które zawierają widokami Razor wymagają `<PreserveCompilationContext>` można ustawić wartości true w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="01ccf-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="01ccf-146">Projekty tego elementu zostanie wygenerowany błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="01ccf-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="01ccf-147">Refaktoryzacja do używania oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="01ccf-147">Refactoring to use middleware</span></span>

<span data-ttu-id="01ccf-148">Refaktoryzacja jest proces zmiany kodu aplikacji, aby zwiększyć jego projekt bez zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="01ccf-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="01ccf-149">W idealnym przypadku należy zrobić po mechanizm przekazywania testów, ponieważ te upewnij się, że zachowanie systemu nie zmienia się przed i po zmianie.</span><span class="sxs-lookup"><span data-stu-id="01ccf-149">It should ideally be done when there's a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="01ccf-150">Trwa wyszukiwanie w taki sposób, w którym zaimplementowano pierwsze sprawdzanie logikę w aplikacji sieci web `Configure` metody, zobacz:</span><span class="sxs-lookup"><span data-stu-id="01ccf-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="01ccf-151">Ten kod działa, ale jest daleko od sposób wykonania tego rodzaju funkcji w aplikacji platformy ASP.NET Core nawet tak proste, jak jest to.</span><span class="sxs-lookup"><span data-stu-id="01ccf-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="01ccf-152">Wyobraź sobie co `Configure` metoda będzie wyglądać w razie potrzeby można dodać do niego tyle kodu, za każdym razem Dodaj inny adres URL punktu końcowego!</span><span class="sxs-lookup"><span data-stu-id="01ccf-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="01ccf-153">Co należy rozważyć możliwość polega na dodaniu [MVC](xref:mvc/overview) do aplikacji i Tworzenie kontrolera w celu obsługi podstawowe sprawdzanie.</span><span class="sxs-lookup"><span data-stu-id="01ccf-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="01ccf-154">Jednak przy założeniu, że nie zostanie muszą inne funkcje MVC, które są nieco overkill.</span><span class="sxs-lookup"><span data-stu-id="01ccf-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="01ccf-155">Możesz można jednak korzystać z platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index), która pomoże nam Hermetyzowanie pierwsze sprawdzanie logikę w jej własnej klasy i osiągnięcia lepszego [separacji](http://deviq.com/separation-of-concerns/) w `Configure` Metoda.</span><span class="sxs-lookup"><span data-stu-id="01ccf-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware/index), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="01ccf-156">Chcesz zezwolić na ścieżce używa oprogramowania pośredniczącego należy określić jako parametru, więc oczekuje klasy oprogramowanie pośredniczące `RequestDelegate` i `PrimeCheckerOptions` wystąpienie w jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="01ccf-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="01ccf-157">Jeśli ścieżka żądania nie pasuje to oprogramowanie pośredniczące jest skonfigurowany, można oczekiwać, wystarczy wywołać następne oprogramowanie pośredniczące w łańcuchu i nic nie rób dalszych.</span><span class="sxs-lookup"><span data-stu-id="01ccf-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="01ccf-158">Pozostała część kod implementacji, który znajdował się w `Configure` znajduje się teraz w `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="01ccf-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="01ccf-159">Ponieważ zależy od oprogramowania pośredniczącego `PrimeService` usługi, jest również żąda wystąpienie tej usługi z konstruktora.</span><span class="sxs-lookup"><span data-stu-id="01ccf-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="01ccf-160">Platformę zapewni tej usługi za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection), zakładając, że został on skonfigurowany, na przykład w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01ccf-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="01ccf-161">Ponieważ to oprogramowanie pośredniczące działa jako punkt końcowy w łańcuchu delegata żądania, gdy jest zgodna z jego ścieżki, nie jest Brak wywołania `_next.Invoke` podczas tego oprogramowania pośredniczącego obsługuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="01ccf-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there's no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="01ccf-162">Z tego oprogramowania pośredniczącego w miejscu i niektóre przydatne metody rozszerzenia utworzone, aby ułatwić jego konfigurowania, refactored `Configure` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="01ccf-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="01ccf-163">Po tym refaktoryzacji masz pewność, że aplikacja sieci web wciąż działa jak poprzednio, ponieważ testy integracji wszystkich przekazywanie.</span><span class="sxs-lookup"><span data-stu-id="01ccf-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="01ccf-164">Jest dobrym pomysłem jest Zatwierdź zmiany do kontroli źródła, po zakończeniu refaktoryzacji i Twoje testy zostały zaliczone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="01ccf-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="01ccf-165">Jeśli jest ćwiczenia testu Driven Development [należy rozważyć dodanie zatwierdzania do cykl czerwony-zielony-Zrefaktoryzuj](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="01ccf-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="01ccf-166">Resources</span><span class="sxs-lookup"><span data-stu-id="01ccf-166">Resources</span></span>

* [<span data-ttu-id="01ccf-167">Testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="01ccf-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="01ccf-168">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="01ccf-168">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="01ccf-169">Kontrolery testów</span><span class="sxs-lookup"><span data-stu-id="01ccf-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
