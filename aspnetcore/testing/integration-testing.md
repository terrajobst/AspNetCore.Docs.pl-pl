---
title: Testowanie w ASP.NET Core integracji
author: ardalis
description: "Jak korzystać z integracji platformy ASP.NET Core testy w celu zapewnienia poprawnego działania składników aplikacji."
keywords: Platformy ASP.NET Core integracji testowania Razor
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 155fd2f0663c6225531a4df6f323ebb30ab1ee73
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="40614-104">Testowanie w ASP.NET Core integracji</span><span class="sxs-lookup"><span data-stu-id="40614-104">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="40614-105">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="40614-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="40614-106">Testowanie integracji gwarantuje, że składniki aplikacji działać prawidłowo, jeśli połączone ze sobą.</span><span class="sxs-lookup"><span data-stu-id="40614-106">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="40614-107">Integracja obsługuje platformy ASP.NET Core testowania przy użyciu platform testów jednostkowych i wbudowane testu hosta sieci web, który może służyć do obsługi żądań bez obciążenie sieci.</span><span class="sxs-lookup"><span data-stu-id="40614-107">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="40614-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40614-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="40614-109">Wprowadzenie do integracji testowania</span><span class="sxs-lookup"><span data-stu-id="40614-109">Introduction to integration testing</span></span>

<span data-ttu-id="40614-110">Testy integracji Sprawdź, czy różne części aplikacji poprawnie współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="40614-110">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="40614-111">W odróżnieniu od [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integracja testy obejmują często dotyczy infrastruktury aplikacji, takich jak bazy danych, system plików, zasobów sieciowych lub żądania sieci web i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="40614-111">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="40614-112">Testy jednostkowe używać elementów sztucznych lub zasymulować obiektów zamiast tych problemów, ale testy integracji ma na celu potwierdzenie, że system działa zgodnie z oczekiwaniami do tych systemów.</span><span class="sxs-lookup"><span data-stu-id="40614-112">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="40614-113">Testy integracji, ponieważ korzystają oni większych segmenty kodu i opierają się na elementy infrastruktury zazwyczaj rzędów wolniej niż testy jednostkowe można.</span><span class="sxs-lookup"><span data-stu-id="40614-113">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="40614-114">W związku z tym jest dobrym rozwiązaniem jest ograniczenie ile testów integracji zapisu, zwłaszcza, jeśli takie samo zachowanie można przetestować z testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="40614-114">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="40614-115">Jeśli niektóre zachowanie można przetestować przy użyciu testu jednostkowego lub testu integracji, preferowane testu jednostkowego, ponieważ będzie prawie zawsze można szybciej.</span><span class="sxs-lookup"><span data-stu-id="40614-115">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="40614-116">Konieczne może być dziesiątki lub setki testy jednostek z wielu różnych danych wejściowych, ale tylko kilka najważniejsze scenariusze obejmujące testów integracji.</span><span class="sxs-lookup"><span data-stu-id="40614-116">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="40614-117">Testowanie logiki w ramach własnego metod jest zwykle domeny testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="40614-117">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="40614-118">Testowanie, jak aplikacja działa w ramach, na przykład z platformy ASP.NET Core lub z bazą danych jest gdzie testów integracji wchodzić w grę.</span><span class="sxs-lookup"><span data-stu-id="40614-118">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="40614-119">Nie wymaga zbyt wiele testów integracji, aby upewnić się, że jesteś w stanie zapisanie wiersza do bazy danych i ponownie go odczytać.</span><span class="sxs-lookup"><span data-stu-id="40614-119">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="40614-120">Nie należy przetestować każdej permutacji możliwe kodu dostępu do danych — należy przetestować za mało, aby mieć pewność, że aplikacja działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="40614-120">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="40614-121">Testowanie integracji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40614-121">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="40614-122">Aby uzyskać ustawione do integracji wykonywania testów, należy utworzyć projektu testowego, Dodaj odwołanie do projektu sieci web platformy ASP.NET Core i zainstaluj moduł uruchamiający.</span><span class="sxs-lookup"><span data-stu-id="40614-122">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="40614-123">Ten proces jest opisany w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentacji wraz z bardziej szczegółowe instrukcje na temat uruchamiania testów i zalecenia dotyczące nazewnictwa sieci testy i klasy testowe.</span><span class="sxs-lookup"><span data-stu-id="40614-123">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="40614-124">Oddzielanie testy jednostkowe i integracji testów przy użyciu różnych projektów.</span><span class="sxs-lookup"><span data-stu-id="40614-124">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="40614-125">To pomaga zapewnić, że nie przypadkowego wprowadzenia zastrzeżenia co do infrastruktury do testy jednostkowe i pozwala łatwo wybrać zestaw testów do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="40614-125">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="40614-126">Host testów</span><span class="sxs-lookup"><span data-stu-id="40614-126">The Test Host</span></span>

<span data-ttu-id="40614-127">Platformy ASP.NET Core obejmuje hosta testów, który można dodać do integracji projekty testowe, a używany do hostowania platformy ASP.NET Core aplikacji, test obsługujących żądania bez konieczności hosta sieci web prawdziwe.</span><span class="sxs-lookup"><span data-stu-id="40614-127">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="40614-128">Dostarczona próbka zawiera projekt testowy integracji, który został skonfigurowany do używania [xUnit](https://xunit.github.io) i hosta testów.</span><span class="sxs-lookup"><span data-stu-id="40614-128">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="40614-129">Używa `Microsoft.AspNetCore.TestHost` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="40614-129">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="40614-130">Raz `Microsoft.AspNetCore.TestHost` pakietu jest dołączony do projektu, można tworzyć i konfigurować `TestServer` w testach.</span><span class="sxs-lookup"><span data-stu-id="40614-130">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="40614-131">Następującego testu przedstawiono sposób sprawdzania, czy żądania skierowane do katalogu głównego witryny zwraca "Witaj świecie!"</span><span class="sxs-lookup"><span data-stu-id="40614-131">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="40614-132">i uruchamiać pomyślnie przed domyślnie sieci Web platformy ASP.NET Core pusty szablon utworzony przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40614-132">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="40614-133">Ten test jest przy użyciu wzorca Assert Act Rozmieść.</span><span class="sxs-lookup"><span data-stu-id="40614-133">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="40614-134">Krok Rozmieść odbywa się w konstruktorze, która tworzy wystąpienie `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="40614-134">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="40614-135">Skonfigurowany `WebHostBuilder` będzie używane do tworzenia `TestHost`; w tym przykładzie `Configure` metody z systemu w ramach testu (SUT) `Startup` klasy jest przekazywana do `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="40614-135">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="40614-136">Ta metoda będzie służyć do konfigurowania potoku żądania z `TestServer` do tak samo jak serwer SUT może być skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="40614-136">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="40614-137">W części Act testu, żądań do `TestServer` wystąpienia dla ścieżki "/" i odpowiedź jest do odczytu do ciągu.</span><span class="sxs-lookup"><span data-stu-id="40614-137">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="40614-138">Ten ciąg jest porównywana z oczekiwany ciąg "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="40614-138">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="40614-139">Jeśli są zgodne, test przechodzi poprawnie; w przeciwnym razie awarii.</span><span class="sxs-lookup"><span data-stu-id="40614-139">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="40614-140">Teraz można dodać kilka testy dodatkowe integracji, aby upewnić się, że pierwsze sprawdzanie funkcji działa za pośrednictwem aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="40614-140">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="40614-141">Należy pamiętać, że nie naprawdę próbujesz testu poprawności sprawdzania liczba pierwsza z tych testów, ale zamiast aplikacji sieci web wykonuje oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="40614-141">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="40614-142">Masz już pokrycie testu jednostki, który daje pewność działania w `PrimeService`, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="40614-142">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Eksplorator testów](integration-testing/_static/test-explorer.png)

<span data-ttu-id="40614-144">Dowiedz się więcej o testów jednostkowych w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) artykułu.</span><span class="sxs-lookup"><span data-stu-id="40614-144">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="40614-145">Testowanie Mvc i Razor integracji</span><span class="sxs-lookup"><span data-stu-id="40614-145">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="40614-146">Projekty testowe, które zawierają widokami Razor wymagają `<PreserveCompilationContext>` można ustawić wartości true w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="40614-146">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="40614-147">Projekty tego elementu zostanie wygenerowany błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="40614-147">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="40614-148">Refaktoryzacja do używania oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="40614-148">Refactoring to use middleware</span></span>

<span data-ttu-id="40614-149">Refaktoryzacja jest proces zmiany kodu aplikacji, aby zwiększyć jego projekt bez zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="40614-149">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="40614-150">W idealnym przypadku należy zrobić po mechanizm przekazywania testów, ponieważ te upewnij się, że zachowanie systemu nie zmienia się przed i po zmianie.</span><span class="sxs-lookup"><span data-stu-id="40614-150">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="40614-151">Trwa wyszukiwanie w taki sposób, w którym zaimplementowano pierwsze sprawdzanie logikę w aplikacji sieci web `Configure` metody, zobacz:</span><span class="sxs-lookup"><span data-stu-id="40614-151">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="40614-152">Ten kod działa, ale jest daleko od sposób wykonania tego rodzaju funkcji w aplikacji platformy ASP.NET Core nawet tak proste, jak jest to.</span><span class="sxs-lookup"><span data-stu-id="40614-152">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="40614-153">Wyobraź sobie co `Configure` metoda będzie wyglądać w razie potrzeby można dodać do niego tyle kodu, za każdym razem Dodaj inny adres URL punktu końcowego!</span><span class="sxs-lookup"><span data-stu-id="40614-153">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="40614-154">Co należy rozważyć możliwość polega na dodaniu [MVC](xref:mvc/overview) do aplikacji i Tworzenie kontrolera w celu obsługi podstawowe sprawdzanie.</span><span class="sxs-lookup"><span data-stu-id="40614-154">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="40614-155">Jednak przy założeniu, że nie zostanie muszą inne funkcje MVC, które są nieco overkill.</span><span class="sxs-lookup"><span data-stu-id="40614-155">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="40614-156">Możesz można jednak korzystać z platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware), która pomoże nam Hermetyzowanie pierwsze sprawdzanie logikę w jej własnej klasy i osiągnięcia lepszego [separacji](http://deviq.com/separation-of-concerns/) w `Configure` Metoda.</span><span class="sxs-lookup"><span data-stu-id="40614-156">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="40614-157">Chcesz zezwolić na ścieżce używa oprogramowania pośredniczącego należy określić jako parametru, więc oczekuje klasy oprogramowanie pośredniczące `RequestDelegate` i `PrimeCheckerOptions` wystąpienie w jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="40614-157">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="40614-158">Jeśli ścieżka żądania nie pasuje to oprogramowanie pośredniczące jest skonfigurowany, można oczekiwać, wystarczy wywołać następne oprogramowanie pośredniczące w łańcuchu i nic nie rób dalszych.</span><span class="sxs-lookup"><span data-stu-id="40614-158">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="40614-159">Pozostała część kod implementacji, który znajdował się w `Configure` znajduje się teraz w `Invoke` metody.</span><span class="sxs-lookup"><span data-stu-id="40614-159">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="40614-160">Ponieważ zależy od oprogramowania pośredniczącego `PrimeService` usługi, jest również żąda wystąpienie tej usługi z konstruktora.</span><span class="sxs-lookup"><span data-stu-id="40614-160">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="40614-161">Platformę zapewni tej usługi za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection), zakładając, że został on skonfigurowany, na przykład w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="40614-161">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="40614-162">Ponieważ to oprogramowanie pośredniczące działa jako punkt końcowy w łańcuchu delegata żądania, gdy jest zgodna z jego ścieżki, nie jest Brak wywołania `_next.Invoke` podczas tego oprogramowania pośredniczącego obsługuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="40614-162">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="40614-163">Z tego oprogramowania pośredniczącego w miejscu i niektóre przydatne metody rozszerzenia utworzone, aby ułatwić jego konfigurowania, refactored `Configure` metody wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="40614-163">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="40614-164">Po tym refaktoryzacji masz pewność, że aplikacja sieci web wciąż działa jak poprzednio, ponieważ testy integracji wszystkich przekazywanie.</span><span class="sxs-lookup"><span data-stu-id="40614-164">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="40614-165">Jest dobrym pomysłem jest Zatwierdź zmiany do kontroli źródła, po zakończeniu refaktoryzacji i Twoje testy zostały zaliczone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="40614-165">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="40614-166">Jeśli jest ćwiczenia testu Driven Development [należy rozważyć dodanie zatwierdzania do cykl czerwony-zielony-Zrefaktoryzuj](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="40614-166">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="40614-167">Resources</span><span class="sxs-lookup"><span data-stu-id="40614-167">Resources</span></span>

* [<span data-ttu-id="40614-168">Testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="40614-168">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="40614-169">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="40614-169">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="40614-170">Kontrolery testów</span><span class="sxs-lookup"><span data-stu-id="40614-170">Testing controllers</span></span>](xref:mvc/controllers/testing)
