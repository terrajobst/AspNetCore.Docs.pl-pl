---
title: Praca z modelem aplikacji w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak odczytywanie i przetwarzanie modelu aplikacji, aby zmodyfikować zachowanie elementów MVC platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/application-model
ms.openlocfilehash: 6b0591a877c0d82e0ee6ab002eb6a6650753677b
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208599"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a>Praca z modelem aplikacji w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Definiuje ASP.NET Core MVC *model aplikacji* reprezentująca składniki aplikacji MVC. Może odczytywać i modyfikować ten model, aby zmodyfikować zachowanie elementów MVC. Domyślnie program MVC zgodna z konwencjami niektórych klas, które są uważane za kontrolerów, metod na tych klas są akcje i zachowaniem parametrów i routing. Można dostosować to zachowanie do potrzeb Twojej aplikacji przez utworzenie własnych Konwencji odpowiadającym i stosując je globalnie lub jako atrybuty.

## <a name="models-and-providers"></a>Modele i dostawców

Model aplikacji ASP.NET Core MVC obejmują zarówno abstrakcyjne interfejsów, jak i klasy konkretnej implementacji, które opisują aplikacji MVC. Ten model jest wynikiem odnajdywanie kontrolerów, akcji, parametrów akcji, trasy i filtrów w zależności od domyślnych Konwencji aplikacji MVC. Poprzez współdziałanie z modelem aplikacji, można zmodyfikować aplikację z różnych Konwencji z domyślnym zachowaniem platformy MVC. Parametry nazwy, trasy i filtry są wszystkie używane jako dane konfiguracji dla akcji i kontrolerów.

Model aplikacji platformy ASP.NET Core MVC ma następującą strukturę:

* ApplicationModel
  * Kontrolery (ControllerModel)
    * Akcje (ActionModel)
      * Parametry (ParameterModel)

Każdy poziom modelu ma dostęp do wspólnego `Properties` zbierania i niższych poziomach dostępu i zastąpić wartości właściwości ustawione przez wyższy poziom w hierarchii. Właściwości zostaną utrwalone w `ActionDescriptor.Properties` utworzenia akcji. Następnie podczas obsługi żądania żadnych właściwości Konwencji dodawany lub modyfikowany jest możliwy za pośrednictwem `ActionContext.ActionDescriptor.Properties`. Za pomocą właściwości to doskonały sposób, aby skonfigurować swoje filtry, integratorów modeli, itp. na podstawie poszczególnych akcji.

> [!NOTE]
> `ActionDescriptor.Properties` Kolekcji nie jest bezpieczny wątkowo (w przypadku zapisów), po zakończeniu uruchamiania aplikacji. Konwencje są najlepszym sposobem bezpiecznego dodania danych do tej kolekcji.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Platforma ASP.NET Core MVC ładuje model aplikacji przy użyciu wzorca dostawcy, zdefiniowane przez [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfejsu. W tej sekcji omówiono niektóre szczegóły wewnętrznej implementacji dotyczące tej funkcji dostawcy. To jest temat zaawansowana — większość aplikacji, które korzystają z modelu aplikacji to zrobić poprzez współdziałanie z Konwencji.

Implementacje `IApplicationModelProvider` interfejsu "wrap", z każdym wywołaniem implementacji `OnProvidersExecuting` w kolejności rosnącej na podstawie jego `Order` właściwości. `OnProvidersExecuted` Zostanie następnie wywołana metoda w odwrotnej kolejności. Struktura zdefiniowano wielu dostawców:

Pierwszy (`Order=-1000`):

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Następnie (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Kolejność, w których dwóch dostawców o tej samej wartości `Order` są nazywane jest nieokreślone i dlatego nie powinny być powoływane.

> [!NOTE]
> `IApplicationModelProvider` to zaawansowane pojęcia dla autorów framework rozszerzyć. Ogólnie rzecz biorąc aplikacje powinny używać konwencji i platform należy używać dostawcy. Klucza różnica polega na tym, że dostawcy są zawsze uruchamiane przed Konwencji.

`DefaultApplicationModelProvider` Ustanawia wiele zachowania domyślne używane przez platformę ASP.NET Core MVC. Jej obowiązki obejmują:

* Dodawanie filtrów globalnych do kontekstu
* Dodanie kontrolerów do kontekstu
* Dodawanie metod publicznych kontrolera jako akcje
* Dodawanie parametrów metody akcji dla kontekstu
* Stosowanie się trasy i innych atrybutów

Niektóre z wbudowanymi zachowaniami są implementowane przez `DefaultApplicationModelProvider`. Ten dostawca jest odpowiedzialny za konstruowanie [ `ControllerModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), który z kolei odwołuje się do [ `ActionModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), i [ `ParameterModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) wystąpień. `DefaultApplicationModelProvider` Klasa to szczegół implementacji wewnętrzną strukturę, który może i zmieni się w przyszłości. 

`AuthorizationApplicationModelProvider` Odpowiada za stosowanie zachowania związanego z `AuthorizeFilter` i `AllowAnonymousFilter` atrybutów. [Dowiedz się więcej o tych atrybutów](xref:security/authorization/simple).

`CorsApplicationModelProvider` Implementuje zachowania związanego z `IEnableCorsAttribute` i `IDisableCorsAttribute`i `DisableCorsAuthorizationFilter`. [Dowiedz się więcej na temat mechanizmu CORS](xref:security/cors).

## <a name="conventions"></a>Konwencje

Model aplikacji definiuje abstrakcje Konwencji, zapewniających prostszy sposób, aby dostosować zachowanie modeli niż zastępując cały model lub dostawcy. Te obiekty abstrakcyjne są zalecanym sposobem modyfikowania zachowania aplikacji. Konwencje umożliwiają pisanie kodu, która dynamicznie będzie miała zastosowanie dostosowań. Gdy [filtry](xref:mvc/controllers/filters) oferują możliwość modyfikowania zachowania struktury, dostosowania pozwalają na kontrolę, jak razem przewodowej całej aplikacji.

Dostępne są następujące konwencje:

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Konwencje są stosowane przez dodanie ich do MVC opcji lub poprzez implementację `Attribute`s, stosując je do kontrolerów, akcji lub parametry akcji (podobnie jak [ `Filters` ](xref:mvc/controllers/filters)). W przeciwieństwie do filtrów konwencje są wykonywane tylko podczas uruchamiania aplikacji, nie jako część każdego żądania.

### <a name="sample-modifying-the-applicationmodel"></a>Przykład: Modyfikowanie ApplicationModel

Następująca Konwencja umożliwia dodawanie właściwości do modelu aplikacji. 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Konwencje modelu aplikacji są stosowane jako opcje po dodaniu MVC `ConfigureServices` w `Startup`.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Właściwości są dostępne z `ActionDescriptor` kolekcji właściwości w ramach akcji kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Przykład: Modyfikowanie opisu ControllerModel

Co w poprzednim przykładzie można także modyfikować aby uwzględnić właściwości niestandardowe modelu kontrolera. To spowoduje zastąpienie istniejących właściwości o takiej samej nazwie, określone w modelu aplikacji. Następujący atrybut Konwencji dodano opis na poziomie kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Ta konwencja jest stosowana jako atrybut na kontrolerze.

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Właściwość "description" jest dostępny w taki sam sposób jak w poprzednich przykładach.

### <a name="sample-modifying-the-actionmodel-description"></a>Przykład: Modyfikowanie opisu ActionModel

Konwencja oddzielny atrybut można zastosować do poszczególnych czynności, zastępowanie zachowania już zastosowana na poziomie aplikacji lub kontrolera.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Zastosowanie do akcji w kontrolerze w poprzednim przykładzie pokazano, jak zastępuje ona Konwencji poziomie kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Przykład: Modyfikowanie ParameterModel

Następująca Konwencja mogą być stosowane do parametrów akcji do modyfikowania ich `BindingInfo`. Następująca Konwencja wymaga, aby parametr parametru trasy; inne potencjalne powiązanie źródeł (na przykład wartości ciągu zapytania), są ignorowane.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Ten atrybut można stosować do dowolnej parametr akcji:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Przykład: Modyfikowanie nazwy ActionModel

Modyfikuje następującej konwencji `ActionModel` można zaktualizować *nazwa* akcji, do którego jest stosowany. Nowa nazwa jest podać jako parametr do atrybutu. Ta nowa nazwa jest używany przez routingu, więc będzie miało wpływ na trasy, używany w celu osiągnięcia tą metodą akcji.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Ten atrybut jest stosowany do metody akcji w `HomeController`:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Mimo że w nazwie metody istotna jest `SomeName`, atrybut zastępuje Konwencji MVC przy użyciu nazwy metody i zastępuje Nazwa akcji z `MyCoolAction`. W związku z tym, trasy używany w celu osiągnięcia ta akcja jest `/Home/MyCoolAction`.

> [!NOTE]
> W tym przykładzie jest zasadniczo taki sam, jak przy użyciu wbudowanych [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) atrybutu.

### <a name="sample-custom-routing-convention"></a>Przykład: Niestandardowe Konwencji routingu

Możesz użyć `IApplicationModelConvention` dostosować działa jak routingu. Na przykład następująca Konwencja będą zawierały kontrolerami przestrzenie nazw do ich tras, zastępując `.` w przestrzeni nazw za pomocą `/` dla trasy:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Konwencji jest dodawany jako opcję uruchamiania.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Możesz dodać konwencje do Twojej [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) , uzyskując dostęp do `MvcOptions` przy użyciu `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Ten przykład dotyczy tę Konwencję tras, które nie korzystają z trasami atrybutów, których kontroler ma "Namespace" w nazwie. Następujący kontroler pokazuje tę Konwencję:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Sposób użycia modelu aplikacji w WebApiCompatShim

Platforma ASP.NET Core MVC korzysta z innego zestawu Konwencji z wzorca ASP.NET Web API 2. Za pomocą Konwencji niestandardowych, można zmodyfikować zachowanie aplikację ASP.NET Core MVC, aby były zgodne z tą aplikacją interfejsu API sieci Web. Microsoft dostarczany [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specjalnie do tego celu.

> [!NOTE]
> Dowiedz się więcej o [migracji z interfejsu API sieci Web platformy ASP.NET](xref:migration/webapi).

Aby użyć podkładek zgodności interfejsu API sieci Web, należy dodać pakiet do projektu, a następnie dodaj Konwencji do MVC, wywołując `AddWebApiConventions` w `Startup`:

```csharp
services.AddMvc().AddWebApiConventions();
```

Konwencje podane według podkładki są stosowane tylko do części aplikacji, których niektóre atrybuty zastosowanych do nich. Następujące cztery atrybuty są używane do formantu, co kontrolery powinny mieć ich konwencje zmodyfikowane przez konwencje podkładek:

* [UseWebApiActionConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Konwencje akcji

`UseWebApiActionConventionsAttribute` Służy do mapowania metoda HTTP do akcji na podstawie ich nazwy (na przykład `Get` mapującej do `HttpGet`). Dotyczy tylko akcje, które nie korzystają z trasami atrybutów.

### <a name="overloading"></a>Przeciążenie

`UseWebApiOverloadingAttribute` Służy do stosowania `WebApiOverloadingApplicationModelConvention` Konwencji. Dodaje tę konwencję `OverloadActionConstraint` do procesu wyboru akcji, co ogranicza akcji kandydata do tych, dla których żądanie spełnia wszystkie parametry — opcjonalne.

### <a name="parameter-conventions"></a>Konwencje parametru

`UseWebApiParameterConventionsAttribute` Służy do stosowania `WebApiParameterConventionsApplicationModelConvention` Konwencji akcji. Ta konwencja określa, czy proste typy używane jako parametry akcji są powiązane z identyfikatora URI domyślnie, gdy typy złożone są powiązane z treści żądania.

### <a name="routes"></a>Trasy

`UseWebApiRoutesAttribute` Formanty czy `WebApiApplicationModelConvention` jest stosowana Konwencja kontrolera. Po włączeniu ta Konwencja umożliwia obsługę [obszarów](xref:mvc/controllers/areas) do trasy.

Oprócz zestaw Konwencji, zawiera pakiet zgodności `System.Web.Http.ApiController` podstawowej klasy, która zastępuje ten, który został udostępniony przez interfejs API sieci Web. Dzięki temu kontrolerach napisanych dla interfejsu API sieci Web i dziedziczenie z jego `ApiController` działać zgodnie z zostały zaprojektowane, podczas uruchamiania na platformie ASP.NET Core MVC. Ta klasa kontrolera podstawowego zostanie nadany ze wszystkimi `UseWebApi*` atrybuty wymienione powyżej. `ApiController` Udostępnia właściwości, metody i typy wyników, które są zgodne z tych dostępnych w interfejsie API sieci Web.

## <a name="using-apiexplorer-to-document-your-app"></a>Dokumentów aplikacji za pomocą ApiExplorer

Udostępnia model aplikacji [ `ApiExplorer` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) właściwości na każdym poziomie, który może służyć do przechodzenia struktury aplikacji. Może to być używane do [generowania strony pomocy dla interfejsów API sieci Web przy użyciu narzędzia, takie jak Swagger](xref:tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Ujawnia właściwość `IsVisible` właściwości, które można ustawić, aby określić części modelu aplikacji, które powinny zostać ujawnione. Można to ustawienie można skonfigurować przy użyciu Konwencji:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Korzystając z tego podejścia (dodatkowe konwencje i w razie potrzeby), można włączyć lub wyłączyć widoczność interfejsu API na dowolnym poziomie w aplikacji. 
