---
title: Niestandardowe powiązanie modelu w ASP.NET Core
author: ardalis
description: Dowiedz się, jak powiązanie modelu umożliwia akcjom kontrolera bezpośrednie działanie z typami modeli w ASP.NET Core.
ms.author: riande
ms.date: 11/13/2018
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: b2fbe6a9f11315d1fb8863fbf62e8929c7ff3fc2
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186877"
---
# <a name="custom-model-binding-in-aspnet-core"></a>Niestandardowe powiązanie modelu w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Powiązanie modelu umożliwia działanie kontrolera do pracy bezpośrednio z typami modeli (przekazane jako argumenty metod), a nie żądaniami HTTP. Mapowanie między danymi żądań przychodzących a modelami aplikacji jest obsługiwane przez program Binders. Deweloperzy mogą rozszerzać wbudowaną funkcję powiązania modelu, implementując niestandardowe powiązania modelu (zazwyczaj nie trzeba pisać własnego dostawcy).

[Wyświetlanie lub pobieranie przykładu z usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Domyślne ograniczenia spinacza modelu

Domyślne powiązania modeli obsługują większość wspólnych typów danych .NET Core i powinny spełniać większość potrzeb deweloperów. Oczekują one powiązania danych tekstowych na podstawie żądania bezpośrednio z typami modeli. Może być konieczne przekształcenie danych wejściowych przed ich powiązaniem. Na przykład, jeśli masz klucz, którego można użyć do wyszukiwania danych modelu. Do pobierania danych opartych na kluczu można użyć spinacza modelu niestandardowego.

## <a name="model-binding-review"></a>Przegląd powiązań modelu

Powiązanie modelu używa określonych definicji dla typów, w których działa. *Typ prosty* jest konwertowany z pojedynczego ciągu w danych wejściowych. *Typ złożony* jest konwertowany z wielu wartości wejściowych. Struktura określa różnice w zależności od istnienia `TypeConverter`. Zalecamy utworzenie konwertera typów, jeśli istnieje proste `string`  ->  `SomeType` mapowanie, które nie wymaga zasobów zewnętrznych.

Przed utworzeniem własnego spinacza modelu niestandardowego warto przejrzeć sposób implementacji istniejących spinaczy modelu. Rozważmy [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) , którego można użyć do przekonwertowania ciągów zakodowanych algorytmem Base64 na tablice bajtowe. Tablice bajtowe są często przechowywane jako pliki lub pola obiektów BLOB bazy danych.

### <a name="working-with-the-bytearraymodelbinder"></a>Praca z ByteArrayModelBinder

Ciągi kodowane algorytmem Base64 mogą służyć do reprezentowania danych binarnych. Na przykład poniższy obraz może być zakodowany jako ciąg.

![bot dotnet](custom-model-binding/images/bot.png "bot dotnet")

Na poniższej ilustracji pokazano małą część zakodowanego ciągu:

![kodowanie dotnet bot](custom-model-binding/images/encoded-bot.png "kodowanie dotnet bot")

Postępuj zgodnie z instrukcjami podanymi w pliku [README przykładu](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) , aby przekonwertować ciąg zakodowany w formacie base64 na plik.

ASP.NET Core MVC może przyjmować ciąg zakodowany algorytmem Base64 i używać `ByteArrayModelBinder` go, aby przekonwertować go na tablicę bajtów. [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) , który implementuje argumenty mapy `byte[]` [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) do `ByteArrayModelBinder`:

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

Podczas tworzenia własnego spinacza modelu niestandardowego można zaimplementować własny `IModelBinderProvider` typ lub użyć [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Poniższy przykład pokazuje, jak użyć `ByteArrayModelBinder` do przekonwertowania ciągu zakodowanego algorytmem Base64 `byte[]` na a i zapisać wynik do pliku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Można OPUBLIKOWAĆ ciąg zakodowany algorytmem Base64 w tej metodzie interfejsu API za pomocą narzędzia, takiego jak [Poster](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Tak długo, jak spinacz może powiązać dane żądania do odpowiednio nazwanych właściwości lub argumentów, powiązanie modelu zakończy się pomyślnie. Poniższy przykład przedstawia sposób użycia `ByteArrayModelBinder` z modelem widoku:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Przykładowy spinacz modelu niestandardowego

W tej sekcji zaimplementujemy niestandardowy spinacz modelu, który:

- Konwertuje przychodzące dane żądania na argumenty klucza o jednoznacznie określonym typie.
- Używa Entity Framework Core, aby pobrać skojarzoną jednostkę.
- Przekazuje skojarzoną jednostkę jako argument do metody akcji.

Poniższy przykład używa `ModelBinder` atrybutu `Author` w modelu:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

W poprzednim kodzie `ModelBinder` atrybut określa `IModelBinder` typ, który powinien być używany do wiązania `Author` parametrów akcji.

Następująca `AuthorEntityBinder` Klasa wiąże `Author` parametr przez pobranie jednostki ze `authorId`źródła danych przy użyciu Entity Framework Core i:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

> [!NOTE]
> Poprzednia `AuthorEntityBinder` Klasa jest przeznaczona do zilustrowania niestandardowego spinacza modelu. Klasa nie jest przeznaczona do zilustrowania najlepszych rozwiązań dotyczących scenariusza wyszukiwania. W celu wyszukania `authorId` należy powiązać bazę danych i zbadać ją w metodzie akcji. To podejście oddziela błędy powiązań modelu z `NotFound` przypadków.

Poniższy kod pokazuje, `AuthorEntityBinder` jak używać w metodzie akcji:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

Ten `ModelBinder` atrybut może służyć do `AuthorEntityBinder` stosowania parametrów do, które nie używają Konwencji domyślnych:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

W tym przykładzie, ponieważ nazwa argumentu nie jest wartością domyślną `authorId`, jest określona w parametrze `ModelBinder` przy użyciu atrybutu. Zarówno kontroler, jak i Metoda akcji są uproszczone w porównaniu z wyszukiwaniem jednostki w metodzie akcji. Logika pobierania autora przy użyciu Entity Framework Core jest przenoszona do spinacza modelu. Może to być znaczące uproszczenie, gdy istnieje kilka metod, które są powiązane z `Author` modelem.

Można zastosować `ModelBinder` atrybut do poszczególnych właściwości modelu (na przykład na ViewModel) lub do parametrów metody akcji, aby określić określony spinacz modelu lub nazwę modelu dla tylko tego typu lub akcji.

### <a name="implementing-a-modelbinderprovider"></a>Implementowanie elementu ModelBinderProvider

Zamiast stosować atrybut, można zaimplementować `IModelBinderProvider`. Jest to sposób implementacji wbudowanych powiązań struktury. Po określeniu typu, na którym działa Twój spinacz, należy określić typ argumentu, który produkuje, a **nie** dane wejściowe zaakceptowane przez spinacz. Następujący dostawca programu Binder współpracuje z `AuthorEntityBinder`. Po dodaniu do kolekcji dostawców MVC nie trzeba używać `ModelBinder` atrybutów z `Author` parametrami lub `Author`typem.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Uwaga: Poprzedni kod zwraca `BinderTypeModelBinder`. `BinderTypeModelBinder`działa jako fabryka dla segregatorów modelu i zapewnia iniekcję zależności (DI). Program `AuthorEntityBinder` wymaga dostępu EF Core. Użyj `BinderTypeModelBinder` , jeśli model spinacza wymaga usług z di.

Aby użyć niestandardowego dostawcy segregatorów modelu, Dodaj go w `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Podczas oceniania powiązań modelu kolekcja dostawców jest sprawdzana w podanej kolejności. Używany jest pierwszy dostawca zwracający spinacz.

Na poniższej ilustracji przedstawiono domyślne powiązania modelu z debugera.

![domyślne powiązania modelu](custom-model-binding/images/default-model-binders.png "domyślne powiązania modelu")

Dodanie swojego dostawcy do końca kolekcji może spowodować wywołanie wbudowanego spinacza modelu, zanim niestandardowy spinacz będzie miał szansę. W tym przykładzie niestandardowy dostawca zostanie dodany do początku kolekcji, aby upewnić się, że jest on używany dla `Author` argumentów akcji.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

### <a name="polymorphic-model-binding"></a>Powiązanie modelu polimorficznego

Powiązanie z różnymi modelami typów pochodnych jest znane jako powiązanie modelu polimorficzne. Powiązanie modelu niestandardowego polimorficzne jest wymagane, gdy wartość żądania musi być powiązana z konkretnym typem modelu pochodnego. Powiązanie modelu polimorficznego:

* Nie jest typowy dla interfejsu API REST, który jest przeznaczony do współpracy ze wszystkimi językami.
* Utrudniają powody dotyczące modeli powiązanych.

Jeśli jednak aplikacja wymaga powiązania modelu polimorficznego, implementacja może wyglądać podobnie do następującego kodu:

[!code-csharp[](custom-model-binding/3.0sample/PolymorphicModelBinding/ModelBinders/PolymorphicModelBinder.cs?name=snippet)]

## <a name="recommendations-and-best-practices"></a>Zalecenia i najlepsze rozwiązania

Niestandardowe powiązania modelu:

- Nie należy próbować ustawiać kodów stanu ani zwracać wyników (na przykład nie znaleziono 404). Jeśli powiązanie modelu nie powiedzie się, [Filtr akcji](xref:mvc/controllers/filters) lub logika w samej metodzie akcji powinien obsłużyć błąd.
- Są najbardziej przydatne do eliminowania powtarzających się kodu i krzyżowego obaw z metod akcji.
- Zwykle nie należy używać do konwertowania ciągu na typ niestandardowy, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) jest zazwyczaj lepszym rozwiązaniem.
