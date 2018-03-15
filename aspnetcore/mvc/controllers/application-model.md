---
title: Praca z Model aplikacji w programie ASP.NET Core
author: ardalis
description: "Dowiedz się, jak do odczytywania i modyfikowania modelu aplikacji, aby zmodyfikować zachowanie elementy MVC ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/application-model
ms.openlocfilehash: 36109a4264eda10e10a7dc071c257f7b4dc8b304
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="working-with-the-application-model-in-aspnet-core"></a>Praca z Model aplikacji w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Definiuje platformy ASP.NET Core MVC *model aplikacji* reprezentująca składniki aplikacji MVC. Może odczytywać i manipulowania ten model, aby zmodyfikować zachowanie elementy MVC. Domyślnie program MVC niektórych z konwencjami ustalenie klas, które są uważane za kontrolery, które metody dla tych klas są działania i zachowanie parametrów i routing. Można dostosować to zachowanie do potrzeb aplikacji, tworząc własne konwencje i zastosowaniu ich globalnie lub jako atrybuty.

## <a name="models-and-providers"></a>Modele i dostawców

Model aplikacji ASP.NET Core MVC obejmują zarówno interfejsami abstrakcyjne i konkretną implementację klasy, które opisują aplikacji MVC. Ten model jest wynikiem odnajdywanie kontrolerów, akcji, parametrów akcji, tras i filtrów w zależności od domyślnych Konwencji aplikacji MVC. Praca z modelu aplikacji, można zmodyfikować aplikacji zgodne z konwencjami różnych z domyślnym zachowaniem MVC. Parametry, nazwy trasy i filtry używane jako dane konfiguracyjne dla akcji i kontrolerów.

Model aplikacji platformy ASP.NET Core MVC ma następującą strukturę:

* ApplicationModel
    * Kontrolery (ControllerModel)
        * Akcje (ActionModel)
            * Parametry (ParameterModel)

Każdy poziom modelu ma dostęp do wspólnego `Properties` kolekcji i niższe poziomy dostępu i zastąpić wartości właściwości ustawione przez wyższego poziomu w hierarchii. Właściwości są zapisywane `ActionDescriptor.Properties` utworzenia akcje. Następnie, jeśli żądanie jest obsługiwane, wszystkie właściwości Konwencji dodane lub zmodyfikowane jest możliwy za pośrednictwem `ActionContext.ActionDescriptor.Properties`. Przy użyciu właściwości jest doskonałym sposobem skonfigurowania z filtrów, integratorów modeli itd. na podstawie-action.

> [!NOTE]
> `ActionDescriptor.Properties` Kolekcji nie jest wielowątkowość (w przypadku zapisów), po zakończeniu uruchamiania aplikacji. Konwencje to najlepszy sposób, aby bezpiecznie dodać dane do tej kolekcji.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

Platformy ASP.NET Core MVC ładuje modelu aplikacji przy użyciu wzorca dostawcy, zdefiniowane przez [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) interfejsu. W tej sekcji opisano niektóre szczegóły wewnętrznej implementacji tej funkcji dostawcy. To jest temat zaawansowany — większość aplikacji, które wykorzystują model aplikacji należy to zrobić, Praca z Konwencji.

Implementacje `IApplicationModelProvider` interfejsu "wrap", z każdego wywołania implementacji `OnProvidersExecuting` rosnąco na podstawie jego `Order` właściwości. `OnProvidersExecuted` Wywoływana jest metoda następnie w odwrotnej kolejności. Wielu dostawców są zdefiniowane w ramach:

Pierwszy (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Następnie (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Kolejność, w których dwóch dostawców z taką samą wartość `Order` są nazywane jest niezdefiniowana i dlatego nie powinny być stosowane.

> [!NOTE]
> `IApplicationModelProvider` to zaawansowane pojęcia dla autorów framework rozszerzenie. Ogólnie rzecz biorąc aplikacje powinny używać konwencji i platform, należy użyć dostawcy. Klucza różnica polega na tym, że dostawcy są zawsze uruchamiane przed Konwencji.

`DefaultApplicationModelProvider` Ustanawia wiele zachowania domyślne używane przez program ASP.NET Core MVC. Jego obowiązki obejmują:

* Dodawanie filtrów globalnych do kontekstu
* Dodanie kontrolerów w kontekście
* Dodawanie metod publicznych kontrolera jako akcje
* Dodawanie parametrów metody akcji w kontekście
* Stosowanie trasy i inne atrybuty

Niektóre wbudowane zachowań implementowanych przez `DefaultApplicationModelProvider`. Ten dostawca jest odpowiedzialny za konstruowanie [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), który z kolei odwołuje się do [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), i [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) wystąpień. `DefaultApplicationModelProvider` Klasa jest szczegółów implementacji wewnętrzną strukturę i zmieni się w przyszłości. 

`AuthorizationApplicationModelProvider` Jest odpowiedzialny za stosowanie zachowania związanego z `AuthorizeFilter` i `AllowAnonymousFilter` atrybutów. [Dowiedz się więcej o tych atrybutów](xref:security/authorization/simple).

`CorsApplicationModelProvider` Implementuje zachowania związanego z `IEnableCorsAttribute` i `IDisableCorsAttribute`i `DisableCorsAuthorizationFilter`. [Dowiedz się więcej o CORS](xref:security/cors).

## <a name="conventions"></a>Konwencje

Model aplikacji definiuje abstrakcje Konwencji, które zapewniają prostszy sposób, aby dostosować zachowanie modeli niż zastępowanie całego modelu lub dostawcy. Te obiekty abstrakcyjne są zalecanym sposobem modyfikowania zachowania aplikacji. Konwencje umożliwiają do pisania kodu, który będzie dynamicznie astosuj dostosowania. Gdy [filtry](xref:mvc/controllers/filters) umożliwiają modyfikowanie zachowania w ramach, dostosowania umożliwiają sprawowanie kontroli nad jak razem przewodowej całej aplikacji.

Dostępne są następujące konwencje:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Konwencje są stosowane przez dodanie ich do opcji MVC lub implementując `Attribute`s i zastosowaniu ich do kontrolerów, akcji lub parametry akcji (podobnie jak [ `Filters` ](xref:mvc/controllers/filters)). W przeciwieństwie do filtrów konwencje są wykonywane tylko podczas uruchamiania aplikacji, nie jako część każdego żądania.

### <a name="sample-modifying-the-applicationmodel"></a>Przykład: Modyfikacja ApplicationModel

Następującej konwencji umożliwia dodawanie właściwości do modelu aplikacji. 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Konwencje modelu aplikacji są stosowane jako opcje woluminowi MVC `ConfigureServices` w `Startup`.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Właściwości są dostępne z `ActionDescriptor` kolekcji właściwości w akcji kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Przykład: Modyfikacja opis ControllerModel

Tak jak w poprzednim przykładzie modelu kontrolera może być modyfikowany aby uwzględnić właściwości niestandardowe. To spowoduje zastąpienie istniejącej właściwości o takiej samej nazwie, jak określono w modelu aplikacji. Następujący atrybut Konwencji dodano opis na poziomie kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Konwencja jest stosowany jako atrybut na kontrolerze.

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Właściwość "opis" jest dostępny w taki sam sposób jak w poprzednich przykładach.

### <a name="sample-modifying-the-actionmodel-description"></a>Przykład: Modyfikacja opis ActionModel

Konwencja oddzielne atrybut można zastosować do poszczególnych działań, zastępowanie zachowania już stosowane na poziomie aplikacji lub kontrolera.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Stosowania tego działania w ramach kontrolera w poprzednim przykładzie pokazano, jak zastępuje on Konwencji poziomie kontrolera:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Przykład: Modyfikacja ParameterModel

Można zastosować następującą konwencją do parametrów akcji, aby zmodyfikować ich `BindingInfo`. Następującej konwencji wymaga parametru parametru trasy; inne potencjalne źródła powiązanie (na przykład wartości ciągu zapytania) są ignorowane.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Ten atrybut można stosować do żadnego parametru akcji:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Przykład: Modyfikowanie nazwy ActionModel

Modyfikuje następującej konwencji `ActionModel` zaktualizować *nazwa* akcji, do którego jest stosowana. Nowa nazwa jest podać jako parametr do atrybutu. Ta nowa nazwa jest używany przez routingu, więc będzie miało wpływ na trasy do parametru tej metody akcji.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Ten atrybut jest stosowany do metody akcji w `HomeController`:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Mimo że nazwa metody jest `SomeName`, atrybut zastępuje przy użyciu nazwy metody z Konwencją MVC i zastępuje nazwę akcji z `MyCoolAction`. W związku z tym trasy używany w celu osiągnięcia tej akcji jest `/Home/MyCoolAction`.

> [!NOTE]
> W tym przykładzie jest zasadniczo taki sam, jak za pomocą wbudowanych [Nazwa akcji](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) atrybutu.

### <a name="sample-custom-routing-convention"></a>Przykład: Niestandardowy Routing Konwencji

Można użyć `IApplicationModelConvention` dostosować działa jak routingu. Na przykład następującej konwencji dołączyć przestrzeni nazw kontrolerów do ich trasy, zastępując `.` w przestrzeni nazw z `/` w trasie:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Konwencji jest dodawana jako opcję uruchamiania.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Można dodać Konwencji do Twojej [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uzyskując dostęp do `MvcOptions` przy użyciu `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Ten przykład dotyczy tę Konwencję tras, które nie używają atrybutu routingu, gdy kontroler ma "Namespace" w nazwie. Następujący kontroler ilustruje tę Konwencję:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Użycie modelu aplikacji w WebApiCompatShim

Platformy ASP.NET Core MVC korzysta z innego zestawu Konwencji z ASP.NET Web API 2. Konwencje niestandardowych można zmodyfikować zachowanie aplikacji ASP.NET Core MVC być zgodny z aplikacji interfejsu API sieci Web. Microsoft jest dostarczany [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) specjalnie do tego celu.

> [!NOTE]
> Dowiedz się więcej o [migracji z interfejsu API sieci Web platformy ASP.NET](xref:migration/webapi).

Aby użyć podkładek zgodności interfejsu API sieci Web, należy dodać pakiet do projektu, a następnie dodaj konwencje do MVC, wywołując `AddWebApiConventions` w `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Konwencje pochodzącymi z podkładką są stosowane tylko do elementów aplikacji, które miały określonych atrybutów. Czterech atrybutów umożliwiają określanie kontrolerów powinny mieć ich konwencje zmodyfikowane przez konwencje podkładki:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Konwencje akcji

`UseWebApiActionConventionsAttribute` Jest używany do mapowania akcji na podstawie ich nazwy metody HTTP (na przykład `Get` czy mapowania `HttpGet`). Dotyczy tylko akcje, które nie używają atrybutu routingu.

### <a name="overloading"></a>Przeciążenie

`UseWebApiOverloadingAttribute` Służy do stosowania `WebApiOverloadingApplicationModelConvention` Konwencji. Dodaje tę konwencję `OverloadActionConstraint` do procesu wyboru akcji, co ogranicza akcje kandydata do tych, dla których żądanie spełnia wszystkie parametry opcjonalne.

### <a name="parameter-conventions"></a>Konwencje parametru

`UseWebApiParameterConventionsAttribute` Służy do stosowania `WebApiParameterConventionsApplicationModelConvention` Konwencji akcji. Konwencja określa, że proste typy używane jako parametry działania są powiązane z identyfikatora URI domyślnie podczas typy złożone są powiązane z treści żądania.

### <a name="routes"></a>Trasy

`UseWebApiRoutesAttribute` Formanty czy `WebApiApplicationModelConvention` jest stosowana Konwencja kontrolera. Po włączeniu tę Konwencję umożliwia obsługę [obszarów](xref:mvc/controllers/areas) do trasy.

Oprócz zestawie Konwencji, pakiet zgodności zawiera `System.Web.Http.ApiController` podstawowa klasa, która zastępuje udostępniony przez interfejs API sieci Web. Dzięki temu kontrolerów napisane dla interfejsu API sieci Web i dziedziczenie z jego `ApiController` będzie działać zgodnie z zostały zaprojektowane tak, podczas uruchamiania na platformie ASP.NET Core MVC. Ta klasa podstawowa kontroler zostanie nadany wszystkie `UseWebApi*` atrybuty wymienione powyżej. `ApiController` Udostępnia właściwości, metody i typy wyników, które są zgodne z tymi znaleziono w interfejsie API sieci Web.

## <a name="using-apiexplorer-to-document-your-app"></a>Przy użyciu ApiExplorer dokumentów aplikacji

Udostępnia model aplikacji [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) właściwości na każdym poziomie, który może służyć do przechodzenia struktury aplikacji. Może to być używane do [generowania strony pomocy dla interfejsów API sieci Web za pomocą takich narzędzi jak Swagger](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Ujawnia właściwości `IsVisible` właściwości, który można ustawić, aby określić części modelu aplikacji, które powinny zostać ujawnione. Można skonfigurować to ustawienie za pomocą Konwencji:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Przy użyciu tej metody (i konwencje dodatkowe, jeśli jest to wymagane), można włączyć lub wyłączyć widoczność interfejsu API na dowolnym poziomie w Twojej aplikacji. 
