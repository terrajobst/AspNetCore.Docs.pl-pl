---
title: Niestandardowe powiązanie modelu w ASP.NET Core
author: ardalis
description: Dowiedz się, jak powiązanie modelu umożliwia akcjom kontrolera bezpośrednie działanie z typami modeli w ASP.NET Core.
ms.author: riande
ms.date: 01/06/2020
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 511cf39bfedfc55d2f75842daf4445d2aaf4872d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659867"
---
# <a name="custom-model-binding-in-aspnet-core"></a>Niestandardowe powiązanie modelu w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

[Steve Kowalski](https://ardalis.com/) i [Kirka Larkin](https://twitter.com/serpent5)

Powiązanie modelu umożliwia działanie kontrolera do pracy bezpośrednio z typami modeli (przekazane jako argumenty metod), a nie żądaniami HTTP. Mapowanie między danymi żądań przychodzących a modelami aplikacji jest obsługiwane przez program Binders. Deweloperzy mogą rozszerzać wbudowaną funkcję powiązania modelu, implementując niestandardowe powiązania modelu (zazwyczaj nie trzeba pisać własnego dostawcy).

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="default-model-binder-limitations"></a>Domyślne ograniczenia spinacza modelu

Domyślne powiązania modeli obsługują większość wspólnych typów danych .NET Core i powinny spełniać większość potrzeb deweloperów. Oczekują one powiązania danych tekstowych na podstawie żądania bezpośrednio z typami modeli. Może być konieczne przekształcenie danych wejściowych przed ich powiązaniem. Na przykład, jeśli masz klucz, którego można użyć do wyszukiwania danych modelu. Do pobierania danych opartych na kluczu można użyć spinacza modelu niestandardowego.

## <a name="model-binding-review"></a>Przegląd powiązań modelu

Powiązanie modelu używa określonych definicji dla typów, w których działa. *Typ prosty* jest konwertowany z pojedynczego ciągu w danych wejściowych. *Typ złożony* jest konwertowany z wielu wartości wejściowych. Struktura określa różnice w zależności od istnienia `TypeConverter`. Zalecamy utworzenie konwertera typów, jeśli istnieje proste `string` -> mapowania `SomeType`, które nie wymaga zasobów zewnętrznych.

Przed utworzeniem własnego spinacza modelu niestandardowego warto przejrzeć sposób implementacji istniejących spinaczy modelu. Rozważmy <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder>, których można użyć do przekonwertowania ciągów zakodowanych algorytmem Base64 na tablice bajtowe. Tablice bajtowe są często przechowywane jako pliki lub pola obiektów BLOB bazy danych.

### <a name="working-with-the-bytearraymodelbinder"></a>Praca z ByteArrayModelBinder

Ciągi kodowane algorytmem Base64 mogą służyć do reprezentowania danych binarnych. Na przykład obraz może być zakodowany jako ciąg. Przykład zawiera obraz jako ciąg zakodowany algorytmem Base64 w [Base64String. txt](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/3.x/CustomModelBindingSample/Base64String.txt).

ASP.NET Core MVC może pobrać ciąg zakodowany w formacie base64 i użyć `ByteArrayModelBinder`, aby przekonwertować go na tablicę bajtów. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> mapuje argumenty `byte[]` do `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        var loggerFactory = context.Services.GetRequiredService<ILoggerFactory>();
        return new ByteArrayModelBinder(loggerFactory);
    }

    return null;
}
```

Podczas tworzenia własnego spinacza modelu niestandardowego można zaimplementować własny typ `IModelBinderProvider` lub użyć <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.

Poniższy przykład pokazuje, jak użyć `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem Base64 na `byte[]` i zapisać wynik do pliku:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_Post)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Można OPUBLIKOWAĆ ciąg zakodowany algorytmem Base64 w tej metodzie interfejsu API za pomocą narzędzia, takiego jak [Poster](https://www.getpostman.com/):

![Postman](custom-model-binding/images/postman.png "Postman")

Tak długo, jak spinacz może powiązać dane żądania do odpowiednio nazwanych właściwości lub argumentów, powiązanie modelu zakończy się pomyślnie. Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` z modelem widoku:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/ImageController.cs?name=snippet_SaveProfile&highlight=2)]

## <a name="custom-model-binder-sample"></a>Przykładowy spinacz modelu niestandardowego

W tej sekcji zaimplementujemy niestandardowy spinacz modelu, który:

- Konwertuje przychodzące dane żądania na argumenty klucza o jednoznacznie określonym typie.
- Używa Entity Framework Core, aby pobrać skojarzoną jednostkę.
- Przekazuje skojarzoną jednostkę jako argument do metody akcji.

Poniższy przykład używa atrybutu `ModelBinder` w modelu `Author`:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

W poprzednim kodzie atrybut `ModelBinder` określa typ `IModelBinder`, który ma być używany do powiązania parametrów akcji `Author`.

Następująca Klasa `AuthorEntityBinder` wiąże parametr `Author` przez pobranie jednostki ze źródła danych przy użyciu Entity Framework Core i `authorId`:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=snippet_Class)]

> [!NOTE]
> Poprzednia Klasa `AuthorEntityBinder` jest przeznaczona do zilustrowania niestandardowego spinacza modelu. Klasa nie jest przeznaczona do zilustrowania najlepszych rozwiązań dotyczących scenariusza wyszukiwania. W polu odnośnik Powiąż `authorId` i zbadaj bazę danych w metodzie Action. To podejście oddziela błędy powiązań modelu z przypadków `NotFound`.

Poniższy kod ilustruje sposób używania `AuthorEntityBinder` w metodzie Action:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_Get&highlight=2)]

Atrybut `ModelBinder` może służyć do zastosowania `AuthorEntityBinder` do parametrów, które nie używają Konwencji domyślnych:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=snippet_GetById&highlight=2)]

W tym przykładzie, ponieważ nazwa argumentu nie jest domyślną `authorId`, jest określona w parametrze przy użyciu atrybutu `ModelBinder`. Zarówno kontroler, jak i Metoda akcji są uproszczone w porównaniu z wyszukiwaniem jednostki w metodzie akcji. Logika pobierania autora przy użyciu Entity Framework Core jest przenoszona do spinacza modelu. Może to być znaczące uproszczenie, gdy istnieje kilka metod, które wiążą się z modelem `Author`.

Można zastosować atrybut `ModelBinder` do poszczególnych właściwości modelu (na przykład na ViewModel) lub do parametrów metody akcji, aby określić określony spinacz modelu lub nazwę modelu dla tylko tego typu lub akcji.

### <a name="implementing-a-modelbinderprovider"></a>Implementowanie elementu ModelBinderProvider

Zamiast stosować atrybut, można zaimplementować `IModelBinderProvider`. Jest to sposób implementacji wbudowanych powiązań struktury. Po określeniu typu, na którym działa Twój spinacz, należy określić typ argumentu, który produkuje, a **nie** dane wejściowe zaakceptowane przez spinacz. Następujący dostawca programu Binder współpracuje z `AuthorEntityBinder`. Po dodaniu do kolekcji dostawców MVC nie trzeba używać atrybutu `ModelBinder` w parametrach typu `Author` lub `Author`.

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Uwaga: Poprzedni kod zwraca `BinderTypeModelBinder`. `BinderTypeModelBinder` pełni rolę fabryki dla segregatorów modelu i zapewnia iniekcję zależności (DI). `AuthorEntityBinder` wymaga od DI do uzyskania dostępu EF Core. Użyj `BinderTypeModelBinder`, jeśli model spinacza wymaga usług z DI.

Aby użyć niestandardowego dostawcy segregatorów modelu, Dodaj go w `ConfigureServices`:

[!code-csharp[](custom-model-binding/samples/3.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

Podczas oceniania powiązań modelu kolekcja dostawców jest sprawdzana w podanej kolejności. Używany jest pierwszy dostawca zwracający spinacz. Dodanie swojego dostawcy do końca kolekcji może spowodować wywołanie wbudowanego spinacza modelu, zanim niestandardowy spinacz będzie miał szansę. W tym przykładzie niestandardowy dostawca zostanie dodany do początku kolekcji, aby upewnić się, że jest on używany do `Author` argumentów akcji.

### <a name="polymorphic-model-binding"></a>Powiązanie modelu polimorficznego

Powiązanie z różnymi modelami typów pochodnych jest znane jako powiązanie modelu polimorficzne. Powiązanie modelu niestandardowego polimorficzne jest wymagane, gdy wartość żądania musi być powiązana z konkretnym typem modelu pochodnego. Powiązanie modelu polimorficznego:

* Nie jest typowy dla interfejsu API REST, który jest przeznaczony do współpracy ze wszystkimi językami.
* Utrudniają powody dotyczące modeli powiązanych.

Jeśli jednak aplikacja wymaga powiązania modelu polimorficznego, implementacja może wyglądać podobnie do następującego kodu:

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Zalecenia i najlepsze rozwiązania

Niestandardowe powiązania modelu:

- Nie należy próbować ustawiać kodów stanu ani zwracać wyników (na przykład nie znaleziono 404). Jeśli powiązanie modelu nie powiedzie się, [Filtr akcji](xref:mvc/controllers/filters) lub logika w samej metodzie akcji powinien obsłużyć błąd.
- Są najbardziej przydatne do eliminowania powtarzających się kodu i krzyżowego obaw z metod akcji.
- Zazwyczaj nie należy używać do konwertowania ciągu na typ niestandardowy, a <xref:System.ComponentModel.TypeConverter> jest zazwyczaj lepszym rozwiązaniem.

::: moniker-end
::: moniker range="< aspnetcore-3.0"

Przez [Steve Smith](https://ardalis.com/)

Powiązanie modelu umożliwia działanie kontrolera do pracy bezpośrednio z typami modeli (przekazane jako argumenty metod), a nie żądaniami HTTP. Mapowanie między danymi żądań przychodzących a modelami aplikacji jest obsługiwane przez program Binders. Deweloperzy mogą rozszerzać wbudowaną funkcję powiązania modelu, implementując niestandardowe powiązania modelu (zazwyczaj nie trzeba pisać własnego dostawcy).

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="default-model-binder-limitations"></a>Domyślne ograniczenia spinacza modelu

Domyślne powiązania modeli obsługują większość wspólnych typów danych .NET Core i powinny spełniać większość potrzeb deweloperów. Oczekują one powiązania danych tekstowych na podstawie żądania bezpośrednio z typami modeli. Może być konieczne przekształcenie danych wejściowych przed ich powiązaniem. Na przykład, jeśli masz klucz, którego można użyć do wyszukiwania danych modelu. Do pobierania danych opartych na kluczu można użyć spinacza modelu niestandardowego.

## <a name="model-binding-review"></a>Przegląd powiązań modelu

Powiązanie modelu używa określonych definicji dla typów, w których działa. *Typ prosty* jest konwertowany z pojedynczego ciągu w danych wejściowych. *Typ złożony* jest konwertowany z wielu wartości wejściowych. Struktura określa różnice w zależności od istnienia `TypeConverter`. Zalecamy utworzenie konwertera typów, jeśli istnieje proste `string` -> mapowania `SomeType`, które nie wymaga zasobów zewnętrznych.

Przed utworzeniem własnego spinacza modelu niestandardowego warto przejrzeć sposób implementacji istniejących spinaczy modelu. Rozważmy <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinder>, których można użyć do przekonwertowania ciągów zakodowanych algorytmem Base64 na tablice bajtowe. Tablice bajtowe są często przechowywane jako pliki lub pola obiektów BLOB bazy danych.

### <a name="working-with-the-bytearraymodelbinder"></a>Praca z ByteArrayModelBinder

Ciągi kodowane algorytmem Base64 mogą służyć do reprezentowania danych binarnych. Na przykład obraz może być zakodowany jako ciąg. Przykład zawiera obraz jako ciąg zakodowany algorytmem Base64 w [Base64String. txt](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/samples/2.x/CustomModelBindingSample/Base64String.txt).

ASP.NET Core MVC może pobrać ciąg zakodowany w formacie base64 i użyć `ByteArrayModelBinder`, aby przekonwertować go na tablicę bajtów. <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Binders.ByteArrayModelBinderProvider> mapuje argumenty `byte[]` do `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

Podczas tworzenia własnego spinacza modelu niestandardowego można zaimplementować własny typ `IModelBinderProvider` lub użyć <xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute>.

Poniższy przykład pokazuje, jak użyć `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem Base64 na `byte[]` i zapisać wynik do pliku:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post1)]

Można OPUBLIKOWAĆ ciąg zakodowany algorytmem Base64 w tej metodzie interfejsu API za pomocą narzędzia, takiego jak [Poster](https://www.getpostman.com/):

![Postman](custom-model-binding/images/postman.png "Postman")

Tak długo, jak spinacz może powiązać dane żądania do odpowiednio nazwanych właściwości lub argumentów, powiązanie modelu zakończy się pomyślnie. Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` z modelem widoku:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Przykładowy spinacz modelu niestandardowego

W tej sekcji zaimplementujemy niestandardowy spinacz modelu, który:

- Konwertuje przychodzące dane żądania na argumenty klucza o jednoznacznie określonym typie.
- Używa Entity Framework Core, aby pobrać skojarzoną jednostkę.
- Przekazuje skojarzoną jednostkę jako argument do metody akcji.

Poniższy przykład używa atrybutu `ModelBinder` w modelu `Author`:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Data/Author.cs?highlight=6)]

W poprzednim kodzie atrybut `ModelBinder` określa typ `IModelBinder`, który ma być używany do powiązania parametrów akcji `Author`.

Następująca Klasa `AuthorEntityBinder` wiąże parametr `Author` przez pobranie jednostki ze źródła danych przy użyciu Entity Framework Core i `authorId`:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> Poprzednia Klasa `AuthorEntityBinder` jest przeznaczona do zilustrowania niestandardowego spinacza modelu. Klasa nie jest przeznaczona do zilustrowania najlepszych rozwiązań dotyczących scenariusza wyszukiwania. W polu odnośnik Powiąż `authorId` i zbadaj bazę danych w metodzie Action. To podejście oddziela błędy powiązań modelu z przypadków `NotFound`.

Poniższy kod ilustruje sposób używania `AuthorEntityBinder` w metodzie Action:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

Atrybut `ModelBinder` może służyć do zastosowania `AuthorEntityBinder` do parametrów, które nie używają Konwencji domyślnych:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

W tym przykładzie, ponieważ nazwa argumentu nie jest domyślną `authorId`, jest określona w parametrze przy użyciu atrybutu `ModelBinder`. Zarówno kontroler, jak i Metoda akcji są uproszczone w porównaniu z wyszukiwaniem jednostki w metodzie akcji. Logika pobierania autora przy użyciu Entity Framework Core jest przenoszona do spinacza modelu. Może to być znaczące uproszczenie, gdy istnieje kilka metod, które wiążą się z modelem `Author`.

Można zastosować atrybut `ModelBinder` do poszczególnych właściwości modelu (na przykład na ViewModel) lub do parametrów metody akcji, aby określić określony spinacz modelu lub nazwę modelu dla tylko tego typu lub akcji.

### <a name="implementing-a-modelbinderprovider"></a>Implementowanie elementu ModelBinderProvider

Zamiast stosować atrybut, można zaimplementować `IModelBinderProvider`. Jest to sposób implementacji wbudowanych powiązań struktury. Po określeniu typu, na którym działa Twój spinacz, należy określić typ argumentu, który produkuje, a **nie** dane wejściowe zaakceptowane przez spinacz. Następujący dostawca programu Binder współpracuje z `AuthorEntityBinder`. Po dodaniu do kolekcji dostawców MVC nie trzeba używać atrybutu `ModelBinder` w parametrach typu `Author` lub `Author`.

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Uwaga: Poprzedni kod zwraca `BinderTypeModelBinder`. `BinderTypeModelBinder` pełni rolę fabryki dla segregatorów modelu i zapewnia iniekcję zależności (DI). `AuthorEntityBinder` wymaga od DI do uzyskania dostępu EF Core. Użyj `BinderTypeModelBinder`, jeśli model spinacza wymaga usług z DI.

Aby użyć niestandardowego dostawcy segregatorów modelu, Dodaj go w `ConfigureServices`:

[!code-csharp[](custom-model-binding/samples/2.x/CustomModelBindingSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-10)]

Podczas oceniania powiązań modelu kolekcja dostawców jest sprawdzana w podanej kolejności. Używany jest pierwszy dostawca zwracający spinacz. Dodanie swojego dostawcy do końca kolekcji może spowodować wywołanie wbudowanego spinacza modelu, zanim niestandardowy spinacz będzie miał szansę. W tym przykładzie niestandardowy dostawca zostanie dodany do początku kolekcji, aby upewnić się, że jest on używany do `Author` argumentów akcji.

### <a name="polymorphic-model-binding"></a>Powiązanie modelu polimorficznego

Powiązanie z różnymi modelami typów pochodnych jest znane jako powiązanie modelu polimorficzne. Powiązanie modelu niestandardowego polimorficzne jest wymagane, gdy wartość żądania musi być powiązana z konkretnym typem modelu pochodnego. Powiązanie modelu polimorficznego:

* Nie jest typowy dla interfejsu API REST, który jest przeznaczony do współpracy ze wszystkimi językami.
* Utrudniają powody dotyczące modeli powiązanych.

Jeśli jednak aplikacja wymaga powiązania modelu polimorficznego, implementacja może wyglądać podobnie do następującego kodu:

[!code-csharp[](custom-model-binding/samples/3.x/PolymorphicModelBindingSample/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Zalecenia i najlepsze rozwiązania

Niestandardowe powiązania modelu:

- Nie należy próbować ustawiać kodów stanu ani zwracać wyników (na przykład nie znaleziono 404). Jeśli powiązanie modelu nie powiedzie się, [Filtr akcji](xref:mvc/controllers/filters) lub logika w samej metodzie akcji powinien obsłużyć błąd.
- Są najbardziej przydatne do eliminowania powtarzających się kodu i krzyżowego obaw z metod akcji.
- Zazwyczaj nie należy używać do konwertowania ciągu na typ niestandardowy, a <xref:System.ComponentModel.TypeConverter> jest zazwyczaj lepszym rozwiązaniem.

::: moniker-end
