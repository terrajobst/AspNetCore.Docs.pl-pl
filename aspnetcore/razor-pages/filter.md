---
title: Metody filtrowania dla Razor Pages w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak tworzyć metody filtrowania dla Razor Pages w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: cd772da8ed565bc779d8c6bcc7c9949a0c1c7c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660756"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Metody filtrowania dla Razor Pages w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Na stronie Razor filtry [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) zezwalają Razor Pages na uruchomienie kodu przed uruchomieniem programu obsługi stron Razor i po nim. Filtry stron Razor są podobne do [ASP.NET Core filtrów akcji MVC](xref:mvc/controllers/filters#action-filters), z wyjątkiem tego, że nie można ich stosować do metod obsługi poszczególnych stron.

Filtry stron Razor:

* Uruchom kod po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
* Uruchom kod przed wykonaniem metody obsługi, po utworzeniu powiązania modelu.
* Uruchom kod po wykonaniu metody obsługi.
* Można zaimplementować na stronie lub globalnie.
* Nie można zastosować do określonych metod obsługi stron.
* Może mieć zależności konstruktora wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di). Aby uzyskać więcej informacji, zobacz [Servicefilterattribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) i [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).

Podczas gdy konstruktory stron i oprogramowanie pośredniczące umożliwiają wykonywanie kodu niestandardowego przed wykonaniem metody obsługi, tylko filtry strony Razor umożliwiają dostęp do <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> i strony. Oprogramowanie pośredniczące ma dostęp do `HttpContext`, ale nie do "kontekstu strony". Filtry mają <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> parametr pochodny, który zapewnia dostęp do `HttpContext`. Na przykład, implementacja przykładu [atrybutu filtru](#ifa) dodaje nagłówek do odpowiedzi, co nie jest możliwe za pomocą konstruktorów lub oprogramowania pośredniczącego.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([jak pobrać](xref:index#how-to-download-a-sample))

Filtry stron Razor zapewniają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:

* Metody synchroniczne:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : wywołuje się po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : wywoływana przed wykonaniem metody obsługi, po zakończeniu powiązania modelu.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : wywoływane po wykonaniu metody obsługi przed wynikiem akcji.

* Metody asynchroniczne:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywołano asynchronicznie po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywołano asynchronicznie przed wywołaniem metody obsługi, po zakończeniu powiązania modelu.

Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu. Struktura sprawdza najpierw, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to. Jeśli nie, wywołuje metody interfejsu synchronicznego. W przypadku implementacji obu interfejsów są wywoływane tylko metody asynchroniczne. Ta sama reguła dotyczy zastąpień na stronach, implementuje synchroniczną lub asynchroniczną wersję przesłonięcia, a nie obu.

## <a name="implement-razor-page-filters-globally"></a>Zaimplementuj globalne filtry stron Razor

Poniższy kod implementuje `IAsyncPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

W poprzednim kodzie `ProcessUserAgent.Write` jest kodem dostarczonym przez użytkownika, który współpracuje z ciągiem agenta użytkownika.

Poniższy kod umożliwia `SampleAsyncPageFilter` w klasie `Startup`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

Poniższy kod wywołuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby zastosować `SampleAsyncPageFilter` tylko do stron w */Movies*:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

Poniższy kod implementuje synchroniczną `IPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Poniższy kod umożliwia `SamplePageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementowanie filtrów stron Razor przez zastępowanie metod filtrowania

Poniższy kod przesłania asynchroniczne filtry stron Razor:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Implementowanie atrybutu filtru

Wbudowany filtr <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filtru oparty na atrybutach może być podklasą. Poniższy filtr dodaje nagłówek do odpowiedzi:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Poniższy kod stosuje `AddHeader` atrybutu:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

Użyj narzędzia, takiego jak narzędzia deweloperskie przeglądarki, aby przeanalizować nagłówki. W obszarze **nagłówki odpowiedzi**zostanie wyświetlona `author: Rick`.

Zobacz [przesłanianie domyślnej kolejności,](xref:mvc/controllers/filters#overriding-the-default-order) Aby uzyskać instrukcje dotyczące zastępowania kolejności.

Zobacz [Anulowanie i krótkie obwody](xref:mvc/controllers/filters#cancellation-and-short-circuiting) , aby uzyskać instrukcje dotyczące krótkiego obwodu potoku filtru z filtru.

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Autoryzuj atrybut filtru

Atrybut [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) można zastosować do `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Na stronie Razor filtry [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) zezwalają Razor Pages na uruchomienie kodu przed uruchomieniem programu obsługi stron Razor i po nim. Filtry stron Razor są podobne do [ASP.NET Core filtrów akcji MVC](xref:mvc/controllers/filters#action-filters), z wyjątkiem tego, że nie można ich stosować do metod obsługi poszczególnych stron.

Filtry stron Razor:

* Uruchom kod po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
* Uruchom kod przed wykonaniem metody obsługi, po utworzeniu powiązania modelu.
* Uruchom kod po wykonaniu metody obsługi.
* Można zaimplementować na stronie lub globalnie.
* Nie można zastosować do określonych metod obsługi stron.

Kod można uruchomić przed wykonaniem metody obsługi przy użyciu konstruktora stron lub oprogramowania pośredniczącego, ale tylko filtry strony Razor mają dostęp do elementu [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Filtry mają parametr pochodny [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) , który zapewnia dostęp do `HttpContext`. Na przykład, implementacja przykładu [atrybutu filtru](#ifa) dodaje nagłówek do odpowiedzi, co nie jest możliwe za pomocą konstruktorów lub oprogramowania pośredniczącego.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([jak pobrać](xref:index#how-to-download-a-sample))

Filtry stron Razor zapewniają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:

* Metody synchroniczne:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : wywołuje się po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : wywoływana przed wykonaniem metody obsługi, po zakończeniu powiązania modelu.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : wywoływane po wykonaniu metody obsługi przed wynikiem akcji.

* Metody asynchroniczne:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywołano asynchronicznie po wybraniu metody obsługi, ale przed wystąpieniem powiązania modelu.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywołano asynchronicznie przed wywołaniem metody obsługi, po zakończeniu powiązania modelu.

> [!NOTE]
> Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a nie** obu. Struktura sprawdza najpierw, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to. Jeśli nie, wywołuje metody interfejsu synchronicznego. W przypadku implementacji obu interfejsów są wywoływane tylko metody asynchroniczne. Ta sama reguła dotyczy zastąpień na stronach, implementuje synchroniczną lub asynchroniczną wersję przesłonięcia, a nie obu.

## <a name="implement-razor-page-filters-globally"></a>Zaimplementuj globalne filtry stron Razor

Poniższy kod implementuje `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

W poprzednim kodzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) nie jest wymagana. Jest on używany w przykładzie w celu zapewnienia informacji o śledzeniu dla aplikacji.

Poniższy kod umożliwia `SampleAsyncPageFilter` w klasie `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Poniższy kod przedstawia kompletną klasę `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Poniższy kod wywołuje `AddFolderApplicationModelConvention`, aby zastosować `SampleAsyncPageFilter` tylko do stron w */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Poniższy kod implementuje synchroniczną `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Poniższy kod umożliwia `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementowanie filtrów stron Razor przez zastępowanie metod filtrowania

Poniższy kod przesłania synchroniczne filtry stron Razor:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Implementowanie atrybutu filtru

Wbudowany filtr [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtru oparty na atrybutach może być podklasą. Poniższy filtr dodaje nagłówek do odpowiedzi:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Poniższy kod stosuje `AddHeader` atrybutu:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Zobacz [przesłanianie domyślnej kolejności,](xref:mvc/controllers/filters#overriding-the-default-order) Aby uzyskać instrukcje dotyczące zastępowania kolejności.

Zobacz [Anulowanie i krótkie obwody](xref:mvc/controllers/filters#cancellation-and-short-circuiting) , aby uzyskać instrukcje dotyczące krótkiego obwodu potoku filtru z filtru. 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Autoryzuj atrybut filtru

Atrybut [Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) można zastosować do `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
