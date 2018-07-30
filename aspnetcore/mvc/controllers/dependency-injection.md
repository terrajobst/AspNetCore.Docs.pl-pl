---
title: Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak kontrolerów platformy ASP.NET Core MVC zażądać ich zależności, które jawnie za pomocą ich konstruktory mogę przy użyciu iniekcji zależności w programie ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9dec9807e8fc2883144b2da518f36a7eb8ddc871
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342136"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core

<a name="dependency-injection-controllers"></a>

Przez [Steve Smith](https://ardalis.com/)

Kontrolerów MVC platformy ASP.NET Core powinien zażądać ich zależności, które jawnie za pomocą ich konstruktory. W niektórych przypadkach akcji kontrolera indywidualne mogą wymagać usługi, i może nie mieć sensu żądań na poziomie kontrolera. W takim przypadku możesz również iniekcję usługi jako parametr metody akcji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Wstrzykiwanie zależności jest techniką, która następuje po [zasady odwrócenie zależności](http://deviq.com/dependency-inversion-principle/), dzięki czemu aplikacje mogą się składać z luźno powiązanych modułów. Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md), która ułatwia aplikacji do testowania i obsługi.

## <a name="constructor-injection"></a>Iniekcji konstruktora

Platforma ASP.NET Core wbudowaną obsługę wstrzykiwania zależności w oparciu o konstruktory rozszerza kontrolerów MVC. Po prostu dodając typ usługi do kontrolera jako parametr konstruktora, ASP.NET Core będzie próbował rozpoznać typu przy użyciu jego wbudowanych w kontenerze usługi. Usługi są zwykle, ale nie zawsze zdefiniowane, przy użyciu interfejsów. Na przykład jeśli aplikacja ma logikę biznesową, która jest zależna od bieżącego czasu, należy wstrzyknąć to usługa, która pobiera czas (zamiast kodować je), co pozwoliłoby testów do przekazania w implementacji, korzystających z określonym czasie.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementowanie interfejsu podobny do tego, tak aby używał zegar systemowy w czasie wykonywania jest proste:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Dzięki temu w miejscu możemy użyć usługi w naszym kontrolera. W tym przypadku dodaliśmy logikę do `HomeController` `Index` metodę w celu wyświetlenia pozdrowienia dla użytkownika na podstawie czasu dnia.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Jeśli możemy uruchomić aplikację teraz, firma Microsoft będzie prawdopodobnie wystąpi błąd:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Ten błąd występuje, gdy firma Microsoft nie zostały skonfigurowane w usługach `ConfigureServices` method in Class metoda naszych `Startup` klasy. Aby określić, że żądania dla `IDateTime` mają zostać rozwiązane za pomocą wystąpienia `SystemDateTime`, Dodaj wyróżniony wiersz na liście poniżej, aby Twoje `ConfigureServices` metody:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Tej konkretnej usługi można zaimplementować przy użyciu dowolnego z kilku opcji inny okres istnienia (`Transient`, `Scoped`, lub `Singleton`). Zobacz [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) Aby zrozumieć, jak każdą z tych opcji zakresu będą wpływać na działanie usługi.

Po skonfigurowaniu usługi działania aplikacji i przejdź do strony głównej powinien wyświetlić komunikat na podstawie czasu zgodnie z oczekiwaniami:

![Powitanie serwera](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Zobacz [logikę kontrolera testu](testing.md) informacje na temat jawne żądanie zależności [ http://deviq.com/explicit-dependencies-principle/ ](http://deviq.com/explicit-dependencies-principle/) kontrolery ułatwia kodu do przetestowania.

Wstrzykiwanie zależności wbudowanych w platformy ASP.NET Core obsługuje posiadanie tylko jednego konstruktora dla klas żądania usługi. Jeśli masz więcej niż jeden konstruktor, może wystąpić wyjątek z informacją:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Jak komunikat o błędzie stwierdzający, możesz rozwiązać ten problem, przy użyciu jednego konstruktora. Możesz również [zastąpić domyślny kontener iniekcji zależności z implementacją firm](xref:fundamentals/dependency-injection#default-service-container-replacement), liczby, które obsługują wiele konstruktorów.

## <a name="action-injection-with-fromservices"></a>Akcja iniekcja FromServices

Czasami nie potrzebujesz usługi dla więcej niż jednej akcji w kontrolerze. W tym przypadku rozsądne może okazać się do dodania usługi jako parametr do metody akcji. Jest to realizowane przez oznaczenie parametr z atrybutem `[FromServices]` jak pokazano poniżej:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Uzyskiwanie dostępu do ustawień za pomocą kontrolera

Uzyskiwanie dostępu do aplikacji lub konfiguracji ustawień z poziomu kontrolera jest typowy wzorzec. Ten dostęp, należy używać wzorca opcje opisane w [konfiguracji](xref:fundamentals/configuration/index). Ogólnie nie powinno żądania ustawień bezpośrednio z poziomu kontrolera, przy użyciu iniekcji zależności. Lepszym rozwiązaniem jest żądanie `IOptions<T>` wystąpienia, gdzie `T` jest klasą konfiguracji potrzebne.

Aby pracować z wzorcem opcji, należy utworzyć klasę, która reprezentuje opcje, taką jak ta:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Następnie należy skonfigurować aplikację do używania tego modelu opcje i Dodaj klasy konfiguracji do kolekcji usługi w `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Na powyższej liście będziemy konfigurować aplikacji na odczytywanie ustawień z pliku w formacie JSON. Można również skonfigurować ustawienia całkowicie w kodzie, jak pokazano w powyższym kodzie komentarzem. Zobacz [konfiguracji](xref:fundamentals/configuration/index) dalszych opcji konfiguracji.

Po określeniu obiektu silnie typizowane konfiguracji (w tym przypadku `SampleWebSettings`) i dodaniu go do kolekcji usługi, możesz poprosić go dowolnego kontrolera lub metody akcji przez zażądanie wystąpienie `IOptions<T>` (w tym przypadku `IOptions<SampleWebSettings>`) . Poniższy kod pokazuje, jak jeden z monitem ustawienia za pomocą kontrolera:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Wzorzec opcje umożliwia ustawienia i konfigurację być całkowicie niezależni od siebie nawzajem i zapewnia obserwowanych kontrolera [separacji](http://deviq.com/separation-of-concerns/), ponieważ nie musi wiedzieć, jak i gdzie można znaleźć ustawienia informacje. On również ułatwia kontrolera testu jednostkowego [logikę kontrolera testu](testing.md), ponieważ istnieje nie [statyczne przylepna](http://deviq.com/static-cling/) lub bezpośrednie wystąpienia klasy ustawienia w obrębie klasy kontrolera.
