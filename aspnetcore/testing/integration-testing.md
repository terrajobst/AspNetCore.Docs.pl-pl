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
# <a name="integration-testing-in-aspnet-core"></a>Testowanie w ASP.NET Core integracji

Przez [Steve Smith](https://ardalis.com/)

Testowanie integracji gwarantuje, że składniki aplikacji działać prawidłowo, jeśli połączone ze sobą. Integracja obsługuje platformy ASP.NET Core testowania przy użyciu platform testów jednostkowych i wbudowane testu hosta sieci web, który może służyć do obsługi żądań bez obciążenie sieci.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Wprowadzenie do integracji testowania

Testy integracji Sprawdź, czy różne części aplikacji poprawnie współpracują ze sobą. W odróżnieniu od [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integracja testy obejmują często dotyczy infrastruktury aplikacji, takich jak bazy danych, system plików, zasobów sieciowych lub żądania sieci web i odpowiedzi. Testy jednostkowe używać elementów sztucznych lub zasymulować obiektów zamiast tych problemów, ale testy integracji ma na celu potwierdzenie, że system działa zgodnie z oczekiwaniami do tych systemów.

Testy integracji, ponieważ korzystają oni większych segmenty kodu i opierają się na elementy infrastruktury zazwyczaj rzędów wolniej niż testy jednostkowe można. W związku z tym jest dobrym rozwiązaniem jest ograniczenie ile testów integracji zapisu, zwłaszcza, jeśli takie samo zachowanie można przetestować z testu jednostkowego.

> [!NOTE]
> Jeśli niektóre zachowanie można przetestować przy użyciu testu jednostkowego lub testu integracji, preferowane testu jednostkowego, ponieważ będzie prawie zawsze można szybciej. Konieczne może być dziesiątki lub setki testy jednostek z wielu różnych danych wejściowych, ale tylko kilka najważniejsze scenariusze obejmujące testów integracji.

Testowanie logiki w ramach własnego metod jest zwykle domeny testów jednostkowych. Testowanie, jak aplikacja działa w ramach, na przykład z platformy ASP.NET Core lub z bazą danych jest gdzie testów integracji wchodzić w grę. Nie wymaga zbyt wiele testów integracji, aby upewnić się, że jesteś w stanie zapisanie wiersza do bazy danych i ponownie go odczytać. Nie należy przetestować każdej permutacji możliwe kodu dostępu do danych — należy przetestować za mało, aby mieć pewność, że aplikacja działa prawidłowo.

## <a name="integration-testing-aspnet-core"></a>Testowanie integracji platformy ASP.NET Core

Aby uzyskać ustawione do integracji wykonywania testów, należy utworzyć projektu testowego, Dodaj odwołanie do projektu sieci web platformy ASP.NET Core i zainstaluj moduł uruchamiający. Ten proces jest opisany w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentacji wraz z bardziej szczegółowe instrukcje na temat uruchamiania testów i zalecenia dotyczące nazewnictwa sieci testy i klasy testowe.

> [!NOTE]
> Oddzielanie testy jednostkowe i integracji testów przy użyciu różnych projektów. To pomaga zapewnić, że nie przypadkowego wprowadzenia zastrzeżenia co do infrastruktury do testy jednostkowe i pozwala łatwo wybrać zestaw testów do uruchomienia.

### <a name="the-test-host"></a>Host testów

Platformy ASP.NET Core obejmuje hosta testów, który można dodać do integracji projekty testowe, a używany do hostowania platformy ASP.NET Core aplikacji, test obsługujących żądania bez konieczności hosta sieci web prawdziwe. Dostarczona próbka zawiera projekt testowy integracji, który został skonfigurowany do używania [xUnit](https://xunit.github.io) i hosta testów. Używa `Microsoft.AspNetCore.TestHost` pakietu NuGet.

Raz `Microsoft.AspNetCore.TestHost` pakietu jest dołączony do projektu, można tworzyć i konfigurować `TestServer` w testach. Następującego testu przedstawiono sposób sprawdzania, czy żądania skierowane do katalogu głównego witryny zwraca "Witaj świecie!" i uruchamiać pomyślnie przed domyślnie sieci Web platformy ASP.NET Core pusty szablon utworzony przez program Visual Studio.

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Ten test jest przy użyciu wzorca Assert Act Rozmieść. Krok Rozmieść odbywa się w konstruktorze, która tworzy wystąpienie `TestServer`. Skonfigurowany `WebHostBuilder` będzie używane do tworzenia `TestHost`; w tym przykładzie `Configure` metody z systemu w ramach testu (SUT) `Startup` klasy jest przekazywana do `WebHostBuilder`. Ta metoda będzie służyć do konfigurowania potoku żądania z `TestServer` do tak samo jak serwer SUT może być skonfigurowany.

W części Act testu, żądań do `TestServer` wystąpienia dla ścieżki "/" i odpowiedź jest do odczytu do ciągu. Ten ciąg jest porównywana z oczekiwany ciąg "Hello World!". Jeśli są zgodne, test przechodzi poprawnie; w przeciwnym razie awarii.

Teraz można dodać kilka testy dodatkowe integracji, aby upewnić się, że pierwsze sprawdzanie funkcji działa za pośrednictwem aplikacji sieci web:

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Należy pamiętać, że nie naprawdę próbujesz testu poprawności sprawdzania liczba pierwsza z tych testów, ale zamiast aplikacji sieci web wykonuje oczekiwań. Masz już pokrycie testu jednostki, który daje pewność działania w `PrimeService`, jak pokazano poniżej:

![Eksplorator testów](integration-testing/_static/test-explorer.png)

Dowiedz się więcej o testów jednostkowych w [testy jednostkowe](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) artykułu.


### <a name="integration-testing-mvcrazor"></a>Testowanie Mvc i Razor integracji

Projekty testowe, które zawierają widokami Razor wymagają `<PreserveCompilationContext>` można ustawić wartości true w *.csproj* pliku:


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

Projekty tego elementu zostanie wygenerowany błąd podobny do następującego:
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Refaktoryzacja do używania oprogramowania pośredniczącego

Refaktoryzacja jest proces zmiany kodu aplikacji, aby zwiększyć jego projekt bez zmiany zachowania. W idealnym przypadku należy zrobić po mechanizm przekazywania testów, ponieważ te upewnij się, że zachowanie systemu nie zmienia się przed i po zmianie. Trwa wyszukiwanie w taki sposób, w którym zaimplementowano pierwsze sprawdzanie logikę w aplikacji sieci web `Configure` metody, zobacz:

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

Ten kod działa, ale jest daleko od sposób wykonania tego rodzaju funkcji w aplikacji platformy ASP.NET Core nawet tak proste, jak jest to. Wyobraź sobie co `Configure` metoda będzie wyglądać w razie potrzeby można dodać do niego tyle kodu, za każdym razem Dodaj inny adres URL punktu końcowego!

Co należy rozważyć możliwość polega na dodaniu [MVC](xref:mvc/overview) do aplikacji i Tworzenie kontrolera w celu obsługi podstawowe sprawdzanie. Jednak przy założeniu, że nie zostanie muszą inne funkcje MVC, które są nieco overkill.

Możesz można jednak korzystać z platformy ASP.NET Core [oprogramowanie pośredniczące](xref:fundamentals/middleware/index), która pomoże nam Hermetyzowanie pierwsze sprawdzanie logikę w jej własnej klasy i osiągnięcia lepszego [separacji](http://deviq.com/separation-of-concerns/) w `Configure` Metoda.

Chcesz zezwolić na ścieżce używa oprogramowania pośredniczącego należy określić jako parametru, więc oczekuje klasy oprogramowanie pośredniczące `RequestDelegate` i `PrimeCheckerOptions` wystąpienie w jego konstruktora. Jeśli ścieżka żądania nie pasuje to oprogramowanie pośredniczące jest skonfigurowany, można oczekiwać, wystarczy wywołać następne oprogramowanie pośredniczące w łańcuchu i nic nie rób dalszych. Pozostała część kod implementacji, który znajdował się w `Configure` znajduje się teraz w `Invoke` metody.

> [!NOTE]
> Ponieważ zależy od oprogramowania pośredniczącego `PrimeService` usługi, jest również żąda wystąpienie tej usługi z konstruktora. Platformę zapewni tej usługi za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection), zakładając, że został on skonfigurowany, na przykład w `ConfigureServices`.

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Ponieważ to oprogramowanie pośredniczące działa jako punkt końcowy w łańcuchu delegata żądania, gdy jest zgodna z jego ścieżki, nie jest Brak wywołania `_next.Invoke` podczas tego oprogramowania pośredniczącego obsługuje żądanie.

Z tego oprogramowania pośredniczącego w miejscu i niektóre przydatne metody rozszerzenia utworzone, aby ułatwić jego konfigurowania, refactored `Configure` metody wygląda następująco:

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Po tym refaktoryzacji masz pewność, że aplikacja sieci web wciąż działa jak poprzednio, ponieważ testy integracji wszystkich przekazywanie.

> [!NOTE]
> Jest dobrym pomysłem jest Zatwierdź zmiany do kontroli źródła, po zakończeniu refaktoryzacji i Twoje testy zostały zaliczone pomyślnie. Jeśli jest ćwiczenia testu Driven Development [należy rozważyć dodanie zatwierdzania do cykl czerwony-zielony-Zrefaktoryzuj](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Resources

* [Testowanie jednostek](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Kontrolery testów](xref:mvc/controllers/testing)
