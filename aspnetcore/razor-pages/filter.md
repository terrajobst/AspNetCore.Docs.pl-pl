---
title: Metody filtrowania dla stron Razor w programie ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak utworzyć metody filtrowania dla stron Razor w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 5b233d95c9fbab09c64072377b85b40b127df7b7
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205941"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Metody filtrowania dla stron Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Strona razor filtry [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Zezwalaj stron Razor do uruchomienia kodu przed i po uruchomieniu programu obsługi stron Razor. Filtry stron razor są podobne do [ASP.NET Core MVC filtry akcji](xref:mvc/controllers/filters#action-filters), z wyjątkiem nie można zastosować do metody obsługi poszczególnych stron. 

Strona razor filtrów:

* Uruchom kod, po wybraniu metody obsługi, ale przed wystąpieniem wiązania modelu.
* Uruchom kod, przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.
* Uruchomienie kodu po wykonaniu metody programu obsługi.
* Można zaimplementować na stronie lub globalnie.
* Nie można zastosować do metody obsługi określonej strony.

Kod może działać, zanim metoda obsługi jest wykonywana przy użyciu konstruktora strony lub oprogramowanie pośredniczące, ale tylko filtry stron Razor mają dostęp do [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Filtry mają [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) pochodne parametr, który zapewnia dostęp do `HttpContext`. Na przykład [implementacji atrybutu filtru](#ifa) Przykładowa aplikacja dodaje nagłówek odpowiedzi, coś, co nie można wykonać za pomocą konstruktorów i oprogramowania pośredniczącego.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Filtry stron razor zapewniają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:

* Synchroniczne metody:

    * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : metoda wywoływana po metody obsługi został wybrany, ale przed modelu występuje powiązania.
    * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : metoda wywoływana przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.
    * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : metoda wywoływana po wykonaniu metody obsługi, zanim wynik akcji.

* Metody asynchroniczne:

    * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywoływane asynchronicznie po metody obsługi został wybrany, ale przed modelu występuje powiązania.
    * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywoływane asynchronicznie przed wywołaniem metody obsługi po zakończeniu wiązania modelu.

> [!NOTE]
> Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, nie obydwa. Struktura najpierw sprawdza, czy filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę. W przeciwnym razie wywołuje metody synchronicznej interfejsu. Jeśli oba interfejsy są wdrożone, są nazywane tylko metody asynchronicznej. Ta sama zasada dotyczy przesłonięć na stronach, zaimplementuj synchronicznej lub asynchronicznej wersję override, nie obydwa.

## <a name="implement-razor-page-filters-globally"></a>Implementowanie globalnie filtrów stron Razor

Poniższy kod implementuje `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

W poprzednim kodzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) nie jest wymagana. W przykładzie służy do zapewnienia informacji o śledzeniu dla aplikacji.

Poniższy kod umożliwia `SampleAsyncPageFilter` w `Startup` klasy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Poniższy kod przedstawia pełny `Startup` klasy:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Poniższy kod wywoła `AddFolderApplicationModelConvention` do zastosowania `SampleAsyncPageFilter` tylko stron */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Poniższy kod implementuje synchronicznej `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Poniższy kod umożliwia `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Strona Razor Implementowanie filtry poprzez zastąpienie metody filtru

Poniższy kod zastępuje synchroniczne filtrów stron Razor:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a>Implementowanie atrybutu filtru

Wbudowany filtr opartych na atrybutach [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtru może być podklasą klasy. Następujący filtr dodaje do odpowiedzi nagłówek:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Poniższy kod stosuje `AddHeader` atrybutu:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Zobacz [zastąpienie domyślnej kolejności](xref:mvc/controllers/filters#overriding-the-default-order) instrukcje w przypadku przesłaniania kolejności.

Zobacz [anulowania i krótki circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) instrukcje dotyczące zwarcie potoku filtru z filtru. 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a>Autoryzuj atrybutu filtru

[Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atrybut można stosować do `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
