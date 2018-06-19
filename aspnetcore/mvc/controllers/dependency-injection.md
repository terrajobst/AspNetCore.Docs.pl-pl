---
title: Iniekcji zależności do kontrolerów w ASP.NET Core
author: ardalis
description: Odkryj, jak kontrolery ASP.NET Core MVC zażądać ich zależności jawnie za pośrednictwem ich konstruktorów z iniekcji zależności w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: c3e26d294d51dc7044158b05c1ac39015c494610
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071805"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Iniekcji zależności do kontrolerów w ASP.NET Core

<a name="dependency-injection-controllers"></a>

Przez [Steve Smith](https://ardalis.com/)

Kontrolerów MVC ASP.NET Core należy zażądać ich zależności jawnie za pośrednictwem ich konstruktorów. W niektórych przypadkach akcji kontrolera poszczególnych może wymagać usługi, a jego może nie mieć sensu żądanie na poziomie kontrolera. W takim przypadku również można wstrzyknąć usługi jako parametr metody akcji.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Iniekcji zależności

Iniekcji zależności to technika, który następuje [zasady odwracanie zależności](http://deviq.com/dependency-inversion-principle/), dzięki czemu aplikacje mogą składa się z modułów luźno powiązanych. Platformy ASP.NET Core ma wbudowaną obsługę [iniekcji zależności](../../fundamentals/dependency-injection.md), która ułatwia aplikacji do testowania i obsługi.

## <a name="constructor-injection"></a>Iniekcji konstruktora

Rozszerzenie platformy ASP.NET Core wbudowaną obsługę iniekcji zależności na podstawie konstruktora do kontrolerów MVC. Po prostu dodając typ usługi do kontrolera jako parametru konstruktora, platformy ASP.NET Core podejmie próbę rozpoznania tego typu za pomocą swojej wbudowanej w kontenerze usług. Usługi są zwykle, ale nie zawsze zdefiniowane, za pomocą interfejsów. Na przykład jeśli aplikacja ma logiki biznesowej, które jest zależny od bieżącego czasu, można wstrzyknąć to usługa, która pobiera czas (zamiast kodować je), co pozwoliłoby testów do przekazania w implementacjach korzystających z określonym czasie.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementowanie interfejsu podobny do tego, tak aby były używane w czasie wykonywania zegar systemowy jest prosta:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Dzięki temu w miejscu możemy korzystać z usługi w kontrolera. W takim przypadku dodano logikę do `HomeController` `Index` metodę w celu wyświetlenia pozdrowienia użytkownikowi na podstawie czasu dnia.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Czy możemy uruchomić aplikację teraz, firma Microsoft najprawdopodobniej wystąpi błąd:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Ten błąd występuje, gdy firma Microsoft nie zostały skonfigurowane w usługach `ConfigureServices` metody w naszym `Startup` klasy. Aby określić, która zażąda `IDateTime` mają zostać rozwiązane, za pomocą wystąpienia `SystemDateTime`, Dodaj wiersz wyróżnione na liście poniżej, aby Twoje `ConfigureServices` — metoda:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Tej konkretnej usługi można zaimplementować za pomocą dowolnej z kilku opcji istnienia różnych (`Transient`, `Scoped`, lub `Singleton`). Zobacz [iniekcji zależności](../../fundamentals/dependency-injection.md) zrozumieć, jak każdej z tych opcji zakresu wpływają na zachowanie usługi.

Po skonfigurowaniu usługi uruchamiania aplikacji i przechodząc na stronę główną powinien być wyświetlany komunikat na podstawie czasu zgodnie z oczekiwaniami:

![Serwer pozdrowienia](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Zobacz [logikę kontrolera testu](testing.md) informacje na temat jawne żądanie zależności [ http://deviq.com/explicit-dependencies-principle/ ](http://deviq.com/explicit-dependencies-principle/) kontrolery ułatwia kod do testowania.

Iniekcji zależności wbudowanych platformy ASP.NET Core obsługuje posiadanie tylko jednego konstruktora dla klasy żądanie usługi. Jeśli masz więcej niż jeden konstruktor może wystąpić wyjątek z informacją:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Jak Stany komunikat o błędzie, należy rozwiązać ten problem, w posiadanie jednego konstruktora. Możesz również [Zamień obsługi iniekcji zależności Domyślna implementacja innej](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), duża liczba obsługują wiele konstruktorów.

## <a name="action-injection-with-fromservices"></a>Akcja iniekcja FromServices

Czasami niepotrzebne usługi dla więcej niż jednej akcji w kontrolerze. W takim przypadku rozsądne może okazać się do dodania usługi jako parametr do metody akcji. Jest to zrobić przez oznaczenie parametr z atrybutem `[FromServices]` w sposób pokazany poniżej:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Uzyskiwanie dostępu do ustawień z kontrolera

Uzyskiwanie dostępu do aplikacji lub Konfiguracja ustawień z poziomu kontrolera jest wspólnym wzorcem. Tego dostępu należy używać wzorzec opcje opisane w [konfiguracji](xref:fundamentals/configuration/index). Zazwyczaj nie powinny żądania ustawienia bezpośrednio w kontrolerze przy użyciu iniekcji zależności. Lepszym rozwiązaniem jest żądanie `IOptions<T>` wystąpienia, gdzie `T` jest klasa konfiguracji potrzebne.

Aby pracować z wzorcem opcji, należy utworzyć klasę, która reprezentuje opcje, takie jak ta:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Następnie należy skonfigurować aplikację do korzystania z modelu opcje i dodać do kolekcji usług w klasie konfiguracji `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Na liście powyżej, możemy konfigurowania aplikacji na odczytywanie ustawień z pliku w formacie JSON. Można również skonfigurować ustawienia całkowicie w kodzie, jak w powyższym kodzie komentarze. Zobacz [konfiguracji](xref:fundamentals/configuration/index) dla dalszego opcji konfiguracji.

Po określeniu obiekt jednoznacznie konfiguracji (w tym przypadku `SampleWebSettings`) i dodaniu go do kolekcji usług, możesz poprosić go z dowolnego kontrolera lub metody akcji przez zażądanie wystąpienia `IOptions<T>` (w tym przypadku `IOptions<SampleWebSettings>`) . Poniższy kod przedstawia, jak jeden z monitem ustawień z kontrolera:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Następujące opcje wzorzec umożliwia ustawienia i konfigurację być odłączona od siebie i zapewnia następujące jest kontrolerem [separacji](http://deviq.com/separation-of-concerns/), ponieważ nie musi wiedzieć, jak i gdzie można znaleźć ustawienia informacje. On również ułatwia kontrolera testu jednostkowego [logikę kontrolera testu](testing.md), ponieważ istnieje nie [statycznych przylepna](http://deviq.com/static-cling/) lub bezpośrednie tworzenie wystąpień klas ustawienia należące do klasy kontrolera.
