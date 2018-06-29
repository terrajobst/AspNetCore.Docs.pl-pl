---
title: Metody filtru dla elementu Razor strony platformy ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak utworzyć filtr metody dla stron Razor w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 70f762f32a9e4fda01418a47e3eb7d7224639a0a
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092846"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Metody filtru dla elementu Razor strony platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Filtry stron razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Zezwalaj stron Razor do uruchomienia kodu przed i po uruchomieniu programu obsługi stron Razor. Są podobne do filtrów stron razor [platformy ASP.NET Core MVC filtry akcji](xref:mvc/controllers/filters#action-filters), z wyjątkiem nie można zastosować do metody obsługi poszczególnych stron. 

Filtry stron razor:

* Uruchomić kod po wybraniu metody obsługi, ale przed wystąpieniem wiązania modelu.
* Uruchomić kod przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.
* Uruchomienia kodu po wykonaniu metody obsługi.
* Można zaimplementować na stronie lub globalnie.
* Nie można zastosować do metody obsługi określonej strony.

Można uruchomić kod przed metoda obsługi jest wykonywana przy użyciu konstruktora strony lub oprogramowanie pośredniczące, ale tylko filtry Razor strony mają dostęp do [element HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Filtry mają [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) pochodnych parametr, który zapewnia dostęp do `HttpContext`. Na przykład [zaimplementować atrybutu filtru](#ifa) próbki dodaje nagłówek odpowiedzi, coś, co nie można przeprowadzić z konstruktorów lub oprogramowania pośredniczącego.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Filtry stron razor zawierają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:

* Metod synchronicznych:

    * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : wywoływana po wybrana została metoda obsługi, ale przed modelu występuje powiązania.
    * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : metoda wywoływana przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.
    * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : metoda wywoływana po wykonaniu metody obsługi, zanim wynik akcji.

* Metod asynchronicznych:

    * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywołana asynchronicznie po metoda obsługi została wybrana, ale przed modelu występuje powiązania.
    * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywołana asynchronicznie przed wywołaniem metody obsługi po zakończeniu wiązania modelu.

> [!NOTE]
> Implementowanie **albo** synchronicznego lub wersji asynchronicznego interfejsu filtru, nie oba. Platformę najpierw sprawdza, czy filtr implementuje interfejs asynchronicznych, a jeśli tak, który wywołuje. Jeśli nie wywołuje metody interfejsu synchronicznego. Jeśli zostaną zaimplementowane dotyczą obu interfejsów, są nazywane tylko metody asynchronicznej. Tę samą zasadę stosuje się do zastąpienia na stronach, wdrożenie synchronicznego lub wersja async zastąpienie, nie oba.

## <a name="implement-razor-page-filters-globally"></a>Implementowanie globalnie filtrów stron Razor

Poniższy kod implementuje `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

W powyższym kodzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) nie jest wymagana. Jest używany w próbce o podanie informacji śledzenia dla aplikacji.

Poniższy kod umożliwia `SampleAsyncPageFilter` w `Startup` klasy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Poniższy kod przedstawia pełną `Startup` klasy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Poniższy kod wywołania `AddFolderApplicationModelConvention` dotyczyć `SampleAsyncPageFilter` tylko stron */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Poniższy kod implementuje synchroniczne `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Poniższy kod umożliwia `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementowanie filtrów stron Razor przez zastąpienie metody filtru

Poniższy kod zastępuje synchroniczne filtry Razor strony:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a>Implementowanie atrybutu filtru

Wbudowany filtr na podstawie atrybutów [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtru może być podklasą klasy. Następujący filtr dodaje do odpowiedzi nagłówek:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Poniższy kod ma zastosowanie `AddHeader` atrybutu:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Zobacz [zastępowanie domyślna kolejność](xref:mvc/controllers/filters#overriding-the-default-order) instrukcje dotyczące Zastępowanie kolejności.

Zobacz [anulowania i krótki circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) instrukcje zwarcia potoku filtru z filtru. 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a>Atrybut Filtr autoryzacji

[Autoryzacji](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atrybut można stosować do `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
