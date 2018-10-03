---
title: Niestandardowe wiązanie modelu w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak wiązanie modelu umożliwia akcji kontrolera do pracy bezpośrednio z typami modelu w programie ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: b8745241b0699d270bb8f3a56ab614b0ca49e64b
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045539"
---
# <a name="custom-model-binding-in-aspnet-core"></a>Niestandardowe wiązanie modelu w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów (przekazanej jako argumenty tej metody), zamiast niż żądania HTTP. Mapowanie między przychodzących modeli danych i aplikacji żądanie jest obsługiwane przez integratorów modelu. Deweloperzy mogą rozszerzać funkcję wiązania modelu wbudowanych, implementując integratorów modelu niestandardowego (choć zazwyczaj nie trzeba zapisać własnego dostawcę).

[Wyświetlanie lub pobieranie przykładowy z serwisu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Domyślne ograniczenia integratora modelu

Domyślne integratorów modelu obsługi najbardziej popularnych typów danych platformy .NET Core i powinny spełniać wymagania większości deweloperów. Oczekuje, że powiązanie tekstowych danych wejściowych z żądania bezpośrednio do typów modelu. Może być konieczne do przekształcania danych wejściowych przed powiązanie. Na przykład, jeśli masz klucz, który może służyć do wyszukiwania danych modelu. Można pobrać danych na podstawie klucza, można użyć niestandardowego integratora modelu.

## <a name="model-binding-review"></a>Przegląd wiązanie modelu

Wiązanie modelu używa określonej definicji dla typów, które działa na. A *typu prostego* jest konwertowany z jednego ciągu w danych wejściowych. A *typu złożonego* jest konwertowana z wielu wartości wejściowych. Struktura określa różnicę oparte na istnienie `TypeConverter`. Zalecane jest tworzenie konwertera typów, jeśli masz prostą `string`  ->  `SomeType` mapowania, która nie wymaga zasobów zewnętrznych.

Przed utworzeniem własnego niestandardowego integratora modelu, to warto recenzowania jak istniejący model integratorów są implementowane. Należy wziąć pod uwagę [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) której można przekonwertować algorytmem Base64 ciągów na tablice typu byte. Tablice bajtów często są przechowywane jako pliki lub pól obiektu BLOB bazy danych.

### <a name="working-with-the-bytearraymodelbinder"></a>Praca z ByteArrayModelBinder

Ciągi zakodowane w formacie Base64 może służyć do reprezentowania danych binarnych. Na przykład poniższa ilustracja mogą być zakodowane jako ciąg.

![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")

Niewielki fragment ciągu zakodowanego pokazano na poniższej ilustracji:

![bot DotNet zakodowane](custom-model-binding/images/encoded-bot.png "bot dotnet zakodowany")

Postępuj zgodnie z instrukcjami w [README w próbce](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) można przekonwertować ciągu zakodowanego algorytmem base64.

ASP.NET Core MVC wykonać ciąg kodowany w formacie base64 i użyj `ByteArrayModelBinder` Aby przekonwertować go na tablicę bajtów. [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) który implementuje [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapuje `byte[]` argumenty `ByteArrayModelBinder`:

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

Podczas tworzenia własnego niestandardowego integratora modelu, można zaimplementować własną `IModelBinderProvider` wpisz lub użyj [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem base64 `byte[]` i zapisać wynik w pliku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Możesz ZADAĆ ciągu zakodowanego algorytmem base64, aby ta metoda interfejsu api przy użyciu narzędzia, takiego jak [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Tak długo, jak obiekt wiążący można powiązać dane żądania poziomu odpowiednio nazwanych właściwości lub argumentów, wiązanie modelu zakończy się pomyślnie. Poniższy przykład pokazuje, jak używać `ByteArrayModelBinder` za pomocą modelu widoku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Przykładowy obiekt wiążący modelu niestandardowego

W tej sekcji firma Microsoft zaimplementowania niestandardowego integratora modelu który:

- Konwertuje dane żądania przychodzące na silnie typizowaną kluczowych argumentów.
- Używa platformy Entity Framework Core, aby pobrać skojarzone jednostki.
- Przekazuje skojarzona jednostka jako argument do metody akcji.

Następujące przykładowe używa `ModelBinder` atrybutu na `Author` modelu:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

W poprzednim kodzie `ModelBinder` atrybut określa typ `IModelBinder` która powinna być używana do powiązania `Author` parametrów akcji. 

`AuthorEntityBinder` Do powiązania zostanie użyte `Author` parametru przez pobieranie jednostki ze źródła danych przy użyciu platformy Entity Framework Core i `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Poniższy kod przedstawia sposób użycia `AuthorEntityBinder` w metodzie akcji:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Atrybut może służyć do zastosowania `AuthorEntityBinder` do parametrów, które nie używają domyślnych Konwencji:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

W tym przykładzie, ponieważ nazwa argumentu nie jest domyślnie `authorId`, jest określony przy użyciu parametru `ModelBinder` atrybutu. Należy pamiętać, uproszczone metody akcji i kontrolerów w porównaniu do wyszukiwania jednostki w metodzie akcji. Logic Apps można pobrać autor przy użyciu platformy Entity Framework Core została przeniesiona do integratora modelu. Może to być znaczne uproszczenie, jeśli masz kilka metod, które należy powiązać `Author` modelu i może ułatwić Ci wykonać [susz zasady](http://deviq.com/don-t-repeat-yourself/).

Można zastosować `ModelBinder` atrybutu do właściwości określonego modelu (takich jak na viewmodel) lub parametrami metody akcji, aby określić niektórych integratora modelu lub modelu po prostu tego typu lub akcji.

### <a name="implementing-a-modelbinderprovider"></a>Implementowanie ModelBinderProvider

Zamiast stosowania atrybutu, można zaimplementować `IModelBinderProvider`. Jest to, jak integratorów wbudowana struktura są implementowane. Po określeniu typu usługi integratora operuje na, określ typ argumentu daje, **nie** akceptuje swoje integratora danych wejściowych. Dla następującego dostawcy integratorów modeli w programach `AuthorEntityBinder`. Gdy jest ona dodawana do kolekcji dostawców MVC, nie trzeba używać `ModelBinder` atrybutu na `Author` lub `Author` typizowane parametry.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Uwaga: Zwraca poprzedni kod `BinderTypeModelBinder`. `BinderTypeModelBinder` działa jako fabryki integratorów modelu i zapewnia wstrzykiwanie zależności (DI). `AuthorEntityBinder` Wymaga DI na dostęp do programu EF Core. Użyj `BinderTypeModelBinder` Jeśli Twoje integratora modelu wymaga usług od DI.

Aby użyć dostawcę integratora modelu niestandardowego, dodaj ją w `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Podczas obliczania integratorów modeli, Kolekcja dostawców jest weryfikowane w kolejności. Pierwszy dostawca, która zwraca obiekt wiążący jest używany.

Na poniższej ilustracji przedstawiono domyślne integratorów modelu z debugera.

![domyślne integratorów](custom-model-binding/images/default-model-binders.png "domyślne integratorów modelu")

Dodawanie dostawcy do końca kolekcji może spowodować integratora modelu wbudowanych, wywoływana przed Twojego niestandardowego integratora ma szansę. W tym przykładzie dostawca niestandardowy zostanie dodany na początku kolekcji, aby upewnić się, jest on używany do `Author` argumentów akcji.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Zalecenia i najlepsze rozwiązania

Integratorów modelu niestandardowego:
- Nie należy próbować zestawu kodów stanu lub zwracania wyników (na przykład 404 Nie znaleziono). W przypadku niepowodzenia wiązania modelu [filtr akcji](xref:mvc/controllers/filters) lub logiki w ramach sama metoda akcji powinna obsługiwać awarii.
- Są najbardziej przydatne do eliminacji powtarzających się kod i odciąż przekrojowe zagadnienia z metody akcji.
- Zazwyczaj nie powinny służyć do przekonwertowania ciągu na typ niestandardowy, [ `TypeConverter` ](/dotnet/api/system.componentmodel.typeconverter) zazwyczaj jest lepszym rozwiązaniem.
