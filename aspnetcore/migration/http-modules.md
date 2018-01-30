---
title: "Migrowanie programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: f104c9116cfaa4a82ac88e4a83b4b6f172eb2aa1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrowanie programów obsługi HTTP i modułów platformy ASP.NET Core oprogramowania pośredniczącego 

Przez [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

W tym artykule pokazano, jak przeprowadzić migrację istniejących ASP.NET [moduły HTTP i programów obsługi system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) do platformy ASP.NET Core [oprogramowanie pośredniczące](../fundamentals/middleware.md).

## <a name="modules-and-handlers-revisited"></a>Moduły i uruchomić ponownie programów obsługi

Przed przystąpieniem do platformy ASP.NET Core oprogramowanie pośredniczące, umożliwia najpierw recap jak działają moduły HTTP i obsługi:

![Obsługa modułów](http-modules/_static/moduleshandlers.png)

**Programy obsługi są:**

   * Klasy, które implementują [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Umożliwia obsługę żądania za pomocą podanej nazwy pliku lub rozszerzenie, takich jak *.report*

   * [Skonfigurowane](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) w *pliku Web.config*

**Moduły są:**

   * Klasy, które implementują [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Wywoływane dla każdego żądania

   * Możliwość zwarcia (zatrzymać dalsze przetwarzanie żądania)

   * Można dodać do odpowiedzi HTTP lub utworzyć własne

   * [Skonfigurowane](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) w *pliku Web.config*

**Kolejność, w którym modułów przetwarzania przychodzących żądań jest określany przez:**

   1. [Cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx), która jest zdarzenia serii wywoływane przez platformę ASP.NET: [powstaniem zdarzenia BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)itp. Każdy moduł można utworzyć programu obsługi dla co najmniej jednego zdarzenia.

   2. Dla tego samego zdarzenia kolejności, w której jest skonfigurowany w *Web.config*.

Oprócz modułów, można dodać obsługi zdarzeń cyklu życia z *Global.asax.cs* pliku. Po obsługi w modułach skonfigurowanych Uruchom te programy obsługi zdarzeń.

## <a name="from-handlers-and-modules-to-middleware"></a>Z programów obsługi i modułów oprogramowania pośredniczącego

**Oprogramowanie pośredniczące są prostsze niż moduły HTTP i obsługi:**

   * Modułów, programów obsługi, *Global.asax.cs*, *Web.config* (z wyjątkiem konfiguracji usług IIS) i znikną cyklu życia aplikacji

   * Role, moduły i programy obsługi zostały przejęte przez oprogramowanie pośredniczące

   * Oprogramowanie pośredniczące są skonfigurowane przy użyciu kodu, a nie w *pliku Web.config*

   * [Rozgałęzienie potoku](../fundamentals/middleware.md#middleware-run-map-use) umożliwia wysyłanie żądań do określonego oprogramowania pośredniczącego, oparte na nie tylko adres URL, ale także na nagłówków żądań, ciągi zapytań, itp.

**Oprogramowanie pośredniczące są bardzo podobne do modułów:**

   * Wywoływane w zasadzie dla każdego żądania

   * Możliwość zwarcia przez żądanie, [nie przekazanie żądania do następnego oprogramowania pośredniczącego](#http-modules-shortcircuiting-middleware)

   * Można tworzyć własne odpowiedzi HTTP

**Oprogramowanie pośredniczące i moduły są przetwarzane w innej kolejności:**

   * Kolejność oprogramowania pośredniczącego opiera się na kolejność, w którym wstawiane są one z potokiem żądań, gdy kolejność modułów opiera się głównie na [cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx) zdarzeń

   * Kolejność oprogramowania pośredniczącego odpowiedzi jest odwrotnie niż dla żądania, podczas kolejność modułów jest taki sam dla żądań i odpowiedzi

   * Zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Oprogramowanie pośredniczące](http-modules/_static/middleware.png)

Należy zwrócić uwagę, jak na ilustracji powyżej, oprogramowanie pośredniczące uwierzytelniania short-circuited żądania.

## <a name="migrating-module-code-to-middleware"></a>Migrowanie kodu modułu do oprogramowania pośredniczącego

Istniejący moduł HTTP będzie wyglądać podobnie do poniższego:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Jak pokazano w [oprogramowanie pośredniczące](../fundamentals/middleware.md) strony, oprogramowanie pośredniczące platformy ASP.NET Core jest klasa, która przedstawia `Invoke` metody z argumentami `HttpContext` i zwracanie `Task`. Nowego oprogramowania pośredniczącego będzie wyglądać następująco:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Powyższego szablonu oprogramowanie pośredniczące została pobrana z sekcji [zapisywania oprogramowanie pośredniczące](../fundamentals/middleware.md#middleware-writing-middleware).

*MyMiddlewareExtensions* Klasa pomocy ułatwia konfigurowanie oprogramowania pośredniczącego w Twojej `Startup` klasy. `UseMyMiddleware` Metody dodaje klasy oprogramowania pośredniczącego do potoku żądania. Usług wymaganych przez oprogramowanie pośredniczące uzyskać wprowadzonym w Konstruktorze przez oprogramowanie pośredniczące.

<a name="http-modules-shortcircuiting-middleware"></a>

Modułu może zakończyć żądania, na przykład jeśli użytkownik nie ma uprawnień:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Oprogramowanie pośredniczące obsługuje to nie wywołując `Invoke` na następne oprogramowanie pośredniczące w potoku. Należy pamiętać, że to nie pełni zakończyć żądania, ponieważ poprzednie middlewares nadal zostanie wywołany, gdy odpowiedź sprawia, że jego sposób za pośrednictwem potoku.

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Funkcje z modułu w przypadku migracji do nowego oprogramowania pośredniczącego, może się okazać, że kod nie kompilacji, ponieważ `HttpContext` klasy zmienił się znacznie w ASP.NET Core. [Później](#migrating-to-the-new-httpcontext), zobaczysz jak przeprowadzić migrację do nowego element HttpContext Core ASP.NET.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrowanie modułu wstawiania do potoku żądania

Moduły HTTP zazwyczaj są dodawane do potoku żądania przy użyciu *Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Konwertuj to przez [Dodawanie nowego oprogramowania pośredniczącego](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) do potoku żądania w Twojej `Startup` klasy:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Dokładne miejsce w potoku, gdzie wstawić nowego oprogramowania pośredniczącego zależy od zdarzenia, które go obsługiwane jako modułu (`BeginRequest`, `EndRequest`, itd.) i ich kolejność na liście modułów w *Web.config*.

Wcześniej wspomniano, nie bez cyklu życia aplikacji w ASP.NET Core a kolejności, w której odpowiedzi są przetwarzane przez oprogramowanie pośredniczące różni się od kolejności użytej przez moduły. To może należy zdecydować, czy porządkowania trudniejsze.

Jeśli kolejność staje się problem, można podzielić modułu na wiele składników oprogramowania pośredniczącego, które może zostać określona niezależnie.

## <a name="migrating-handler-code-to-middleware"></a>Migrowanie kod obsługi oprogramowania pośredniczącego

Program obsługi HTTP wygląda następująco:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

W projekcie platformy ASP.NET Core będzie to przełożyć na oprogramowanie pośredniczące podobny do poniższego:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

To oprogramowanie pośredniczące jest bardzo podobny do odpowiadającego modułów oprogramowania pośredniczącego. Tylko rzeczywistych różnica jest to, że w tym miejscu jest Brak wywołania `_next.Invoke(context)`. Ma to sens, ponieważ procedura obsługi jest na końcu potoku żądania zostanie nie następne oprogramowanie pośredniczące do wywołania.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrowanie wstawienia potoku żądania obsługi

Konfigurowanie programu obsługi HTTP odbywa się *Web.config* i wygląda następująco:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Można przekonwertować to przez dodanie nowych obsługi oprogramowania pośredniczącego do potoku żądania w Twojej `Startup` klasy, podobnie jak przekonwertować z modułów oprogramowania pośredniczącego. Tego podejścia przy rozwiązywaniu problemu jest to, że wszystkie żądania będzie wysyłać do nowego oprogramowania pośredniczącego obsługi. Jednak tylko żądania z danym rozszerzeniem nawiązać oprogramowania pośredniczącego. Która pozwoli uzyskać te same funkcje, jaką miał programu obsługi HTTP.

Rozwiązanie polega na Rozgałęzienie potoku żądań dla danego rozszerzenia, przy użyciu `MapWhen` — metoda rozszerzenia. Można to zrobić w tym samym `Configure` metody, gdzie możesz dodać inne oprogramowanie pośredniczące:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`przyjmuje następujące parametry:

1. Wyrażenie lambda, która przyjmuje `HttpContext` i zwraca `true` w przypadku żądania powinien wyłączenia gałęzi. Oznacza to, że można rozgałęzić żądania nie tylko na podstawie ich rozszerzenia, ale także nagłówków żądań parametrów ciągu zapytania, itp.

2. Wyrażenie lambda, która przyjmuje `IApplicationBuilder` i dodaje całe oprogramowanie pośredniczące dla gałęzi. Oznacza to, że można dodać dodatkowe oprogramowanie pośredniczące do gałęzi przed obsługi oprogramowania pośredniczącego.

Oprogramowanie pośredniczące dodane do potoku przed gałęzi, zostanie wywołany we wszystkich żądaniach; gałąź zostanie nie mają wpływu na nich.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Opcje oprogramowania pośredniczącego przy użyciu wzorca opcje ładowania

Niektóre moduły i programy obsługi mają opcje konfiguracji, które są przechowywane w *Web.config*. Jednak w przypadku platformy ASP.NET Core nowy model konfiguracji jest używany zamiast *Web.config*.

Nowy [system konfiguracji](xref:fundamentals/configuration/index) umożliwia te opcje, aby rozwiązać ten problem:

* Bezpośrednio wstrzyknąć opcje w oprogramowaniu pośredniczącym, jak pokazano w [następnej sekcji](#loading-middleware-options-through-direct-injection).

* Użyj [wzorzec opcje](xref:fundamentals/configuration/options):

1.  Tworzenie klasy utrzymującej opcje oprogramowania pośredniczącego, na przykład:

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Przechowywanie wartości opcji

    System konfiguracji umożliwia przechowywanie wartości opcji dowolnym ma. Jednak najbardziej Lokacje użyj *appsettings.json*, więc przeniesiemy tego podejścia:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* Oto nazwę sekcji. Nie musi być taka sama jak nazwa klasy opcje.

3. Kojarzenie wartości opcji z klasy opcji

    Wzorzec opcje używa framework iniekcji zależności platformy ASP.NET Core w celu skojarzenia typu opcje (takich jak `MyMiddlewareOptions`) z `MyMiddlewareOptions` obiektu, który ma rzeczywistego opcje.

    Aktualizacja Twojego `Startup` klasy:

    1.  Jeśli używasz *appsettings.json*, dodaj go do konstruktora konfiguracji w `Startup` konstruktora:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Skonfiguruj usługę opcje:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Skojarz opcji z klasy opcje:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Wstaw opcje do Twojej konstruktora oprogramowania pośredniczącego. Efekt jest podobny do iniekcję opcje do kontrolera.

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware) — metoda rozszerzenia, który dodaje oprogramowaniu pośredniczącym, aby `IApplicationBuilder` zajmuje się iniekcji zależności.

  To nie jest ograniczona do `IOptions` obiektów. Drugi obiekt, który wymaga oprogramowania pośredniczącego mogą zostać dodane w ten sposób.

## <a name="loading-middleware-options-through-direct-injection"></a>Opcje oprogramowania pośredniczącego za pomocą iniekcji bezpośredniego ładowania

Wzorzec opcje ma tę zaletę, utworzonych utracić sprzężenia między wartości opcji i ich odbiorców. Po klasy opcji został skojarzony z wartości rzeczywistych opcje, inne klasy mogą korzystać z opcji przez strukturę iniekcji zależności. Nie istnieje potrzeba do przekazania wokół wartości opcji.

To dzieli jednak jeśli chcesz użyć tego samego oprogramowania pośredniczącego dwa razy, przy użyciu różnych opcji. Na przykład autoryzacji oprogramowanie pośredniczące używane w różnych gałęziach, dzięki czemu różne role. Dwa obiekty różnych opcji nie można skojarzyć z klasy jednej opcji.

Rozwiązanie to uzyskać obiekty opcje przy użyciu wartości rzeczywistych opcje w Twojej `Startup` klasy i przekazywać je bezpośrednio na każde wystąpienie oprogramowania pośredniczącego.

1.  Dodaj drugi klucz do *appsettings.json*

    Aby dodać drugi zestaw opcji, aby *appsettings.json* plików, użyj nowego klucza w celu jego jednoznacznej identyfikacji:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Pobieranie wartości opcji i przekazywanie ich do oprogramowania pośredniczącego. `Use...` — Metoda rozszerzenia (która dodaje oprogramowania pośredniczącego do potoku) to logiczne miejsce do przekazywania wartości opcji: 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Włącz oprogramowaniu pośredniczącym, aby mieć parametr opcji. Udostępnij przeciążenie metody `Use...` — metoda rozszerzenia (która przyjmuje parametr opcje i przekazuje je do `UseMiddleware`). Gdy `UseMiddleware` jest wywoływana z parametrami przekazuje parametry do Twojej konstruktora oprogramowania pośredniczącego podczas tworzenia wystąpień obiektu oprogramowania pośredniczącego.

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Należy zwrócić uwagę, jak to opakowuje obiektu opcje w `OptionsWrapper` obiektu. To implementuje `IOptions`, zgodnie z oczekiwaniami konstruktora oprogramowania pośredniczącego.

## <a name="migrating-to-the-new-httpcontext"></a>Migracja do nowego element HttpContext

Wcześniej przedstawiono który `Invoke` metoda w oprogramowania pośredniczącego przyjmuje parametr typu `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`znacznie została zmieniona w ASP.NET Core. W tej sekcji przedstawiono sposób tłumaczenia najczęściej używane właściwości [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) do nowego `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Unikatowy identyfikator (odpowiednika System.Web.HttpContext)**

Zawiera unikatowy identyfikator dla każdego żądania. Bardzo przydatny w dziennikach.

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** i **HttpContext.Request.RawUrl** przełożyć na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** translates to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Odczytać wartości formularza, tylko wtedy, gdy typ zawartości sub *x--www-form-urlencoded* lub *dane formularza*.

**HttpContext.Request.InputStream** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Ten kod należy używać tylko w obsługi typu oprogramowanie pośredniczące, na końcu potoku.
>
>Można odczytać pierwotne treści, jak pokazano powyżej tylko raz dla żądania. Oprogramowanie pośredniczące próby odczytu treści po pierwszym odczytu będzie odczytywał element body puste.
>
>To nie ma zastosowania do odczytywania formularza, jak pokazano wcześniej, ponieważ która jest wykonywana z buforu.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** i **HttpContext.Response.StatusDescription** przełożyć na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** i **HttpContext.Response.ContentType** przełożyć na:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** na jego własnej tłumaczy także do:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** umożliwia to:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Wysyłaniu pliku omówiono [tutaj](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Wysyłanie nagłówków odpowiedzi jest złożona faktem, że ustawienie po niczego zostały zapisane w treści odpowiedzi, ich nie będą wysyłane.

Rozwiązanie jest ustalenie metody wywołania zwrotnego, która będzie wywoływana po prawej przed zapisaniem na uruchamia odpowiedzi. Najlepiej odbywa się na początku `Invoke` metoda oprogramowania pośredniczącego. Jest ta metoda wywołania zwrotnego ustawiający nagłówkach odpowiedzi.

Poniższy kod ustawia metodę wywołania zwrotnego o nazwie `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Metody wywołania zwrotnego będzie wyglądać następująco:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Pliki cookie są przesyłane do przeglądarki w *Set-Cookie* nagłówka odpowiedzi. W związku z tym wysyłanie plików cookie wymaga tego samego wywołania zwrotnego jako używane do wysyłania nagłówki odpowiedzi:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Metody wywołania zwrotnego będzie wyglądać podobnie do następującej:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Omówienie moduły HTTP i programów obsługi HTTP](/iis/configuration/system.webserver/)
* [Konfiguracja](xref:fundamentals/configuration/index)
* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware)
