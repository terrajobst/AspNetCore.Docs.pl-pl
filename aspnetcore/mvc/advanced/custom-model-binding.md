---
title: "Powiązanie niestandardowe modelu w platformy ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów w ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 941aa9e3ff4e4a75714e11b79d913418d0514d1e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="custom-model-binding-in-aspnet-core"></a>Powiązanie niestandardowe modelu w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Wiązanie modelu umożliwia akcji kontrolera pracować bezpośrednio z modelu typów (przekazany jako argumenty metody), a nie niż żądania HTTP. Mapowanie między przychodzącego żądania danych i aplikacji modele jest obsługiwany przez integratorów modeli. Deweloperzy mogą rozszerzać funkcjonalność powiązanie modelu wbudowanych zaimplementowanie integratorów modeli niestandardowe (chociaż zazwyczaj nie trzeba zapisać własnego dostawcę).

[Przykładowy widok lub pobrania z witryny GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Domyślne ograniczenia integratora modelu

Domyślne integratorów modeli obsługuje większość popularnych typów danych .NET Core i powinna spełniać potrzeby większość deweloperów. Oczekiwane powiązać tekstowych danych wejściowych z żądania bezpośrednio do typów modeli. Konieczne może być Przekształć dane wejściowe przed jej powiązania. Na przykład, jeśli masz klucz, który może służyć do wyszukiwania danych modelu. Można pobrać danych opartą na kluczu, można użyć niestandardowego integratora modelu.

## <a name="model-binding-review"></a>Przejrzyj powiązanie modelu

Wiązanie modelu używa definicji określonych typów, który działa na. A *typu prostego* jest konwertowana z jednego ciągu w danych wejściowych. A *typu złożonego* jest konwertowana z wielu wartości wejściowe. Platformę określa różnicę oparte na istnienie `TypeConverter`. Zalecane jest tworzenie konwertera typów, jeśli masz prostą `string`  ->  `SomeType` mapowania, które nie wymagają zasobów zewnętrznych.

Przed utworzeniem własnego niestandardowego integratora modelu, to warto recenzowania jak istniejący model i integratorów są implementowane. Należy wziąć pod uwagę [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) które mogą służyć do konwertowania ciągów algorytmem base64 na tablice typu byte. Tablice bajtów często są przechowywane jako pliki lub pola obiektu BLOB bazy danych.

### <a name="working-with-the-bytearraymodelbinder"></a>Praca z ByteArrayModelBinder

Ciągi algorytmem Base64 może służyć do reprezentowania danych binarnych. Na przykład poniższy obraz mogą być kodowane jako ciąg.

![DotNet bot](custom-model-binding/images/bot.png "dotnet bot")

Mała część ciągu zakodowanego pokazano na poniższej ilustracji:

![zakodowany bot DotNet](custom-model-binding/images/encoded-bot.png "zakodowane bot dotnet")

Postępuj zgodnie z instrukcjami [README w próbce](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) można przekonwertować ciągu zakodowanego algorytmem base64 do pliku.

ASP.NET Core MVC może przejąć ciągi znaków algorytmem base64 i użyć `ByteArrayModelBinder` przekonwertować go do tablicy typu byte. [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) z zaimplementowanym [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapy `byte[]` argumenty `ByteArrayModelBinder`:

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

Podczas tworzenia własnego niestandardowego integratora modelu, można zaimplementować własne `IModelBinderProvider` wpisz lub użyj [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Poniższy przykład przedstawia użycie `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem base64 `byte[]` i zapisać wynik w pliku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Możesz zamieścić ciąg kodowany w formacie base64 dla tej metody interfejsu api przy użyciu narzędzia, takiego jak [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Tak długo, jak obiekt wiążący można powiązać dane żądania odpowiednio nazwane właściwości lub argumentów, powiedzie się wiązania modelu. Poniższy przykład przedstawia użycie `ByteArrayModelBinder` z modelu widoku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Przykładowe integratora modelu niestandardowych

W tej sekcji możemy implementacji niestandardowego integratora modelu który:

- Konwertuje przychodzące żądanie danych jednoznacznie argumenty klucza.
- Używa programu Entity Framework Core, aby pobrać skojarzonej jednostki.
- Przekazuje skojarzonej jednostki jako argument do metody akcji.

Następujące przykładowe używa `ModelBinder` atrybutu `Author` modelu:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

W powyższym kodzie `ModelBinder` atrybut określa typ `IModelBinder` która powinna być używana do powiązania `Author` parametry akcji. 

`AuthorEntityBinder` Jest używana do powiązania `Author` parametru przez pobieranie jednostkę ze źródła danych przy użyciu programu Entity Framework Core i `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Poniższy kod przedstawia sposób użycia `AuthorEntityBinder` w metodzie akcji:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

`ModelBinder` Atrybut może służyć do zastosowania `AuthorEntityBinder` parametrów, które nie używają domyślnych Konwencji:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

W tym przykładzie, ponieważ nazwa argumentu nie jest domyślnie `authorId`, został określony przy użyciu parametru `ModelBinder` atrybutu. Należy pamiętać, uproszczone metody akcji i kontrolerów w porównaniu do wyszukiwania jednostki w metodzie akcji. Logika pobrać autor przy użyciu programu Entity Framework Core została przeniesiona do integratora modelu. Może to być znacznego uproszczenia, jeśli istnieje kilka metod powiązać modelu autora, które mogą pomóc w wykonaj [suchej zasady](http://deviq.com/don-t-repeat-yourself/).

Możesz zastosować `ModelBinder` atrybutu poszczególnych właściwości (takie jak na viewmodel) lub parametrami metody akcji, aby określić pewnych integratora modelu lub modelu nazw dla właśnie tego typu lub akcji.

### <a name="implementing-a-modelbinderprovider"></a>Implementowanie ModelBinderProvider

Zamiast stosowania atrybutu, można zaimplementować `IModelBinderProvider`. Jest to implementowania integratorów wbudowana Struktura. Po określeniu typu integratora sieci działa na, określ typ argumentu generuje, **nie** akceptuje integratora Twoje dane wejściowe. Następujących dostawców integratora współpracuje z `AuthorEntityBinder`. Gdy jest ona dodawana do kolekcji dostawców MVC, nie trzeba używać `ModelBinder` atrybutu `Author` lub `Author` wpisane parametrów.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Uwaga: Zwraca poprzedni kod `BinderTypeModelBinder`. `BinderTypeModelBinder` działa jako fabryka integratorów modeli i zapewnia iniekcji zależności (Podpisane). `AuthorEntityBinder` Wymaga Podpisane EF Core dostępu do. Użyj `BinderTypeModelBinder` Jeśli Twoje integratora modelu wymaga usług z Podpisane.

Aby użyć dostawcę integratora modelu niestandardowe, dodaj ją w `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Podczas obliczania integratorów modeli, Kolekcja dostawców jest badana w kolejności. Pierwszy dostawcy, który zwraca obiekt wiążący jest używany.

Na poniższej ilustracji przedstawiono domyślne integratorów modeli z debugera.

![Domyślna integratorów modeli](custom-model-binding/images/default-model-binders.png "domyślne integratorów modeli")

Dodawanie dostawcy do końca kolekcji może spowodować integratora modelu wbudowanych, wywoływana przed użytkownika niestandardowego integratora jest stosowany. W tym przykładzie dodaniu dostawcy niestandardowego na początku kolekcji, aby upewnić się, jest on używany do `Author` argumentów akcji.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Zalecenia i najlepsze rozwiązania

Integratorów modeli niestandardowych:
- Nie należy próbować ustawiania kodów stanu lub zwracania wyników (na przykład 404 — Nie znaleziono). W przypadku niepowodzenia wiązania modelu [filtr akcji](xref:mvc/controllers/filters) lub logiki w ramach tej metody akcji powinna obsługiwać awarii.
- Są najbardziej przydatny w przypadku wyeliminowanie powtarzających się kodu oraz problemów powiązanych z metody akcji.
- Zwykle nie można użyć do przekonwertowania ciągu na typ niestandardowy, [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) jest zwykle lepszym rozwiązaniem.
