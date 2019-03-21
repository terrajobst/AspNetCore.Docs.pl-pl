---
title: Migrowanie moduły i programy obsługi HTTP do oprogramowania pośredniczącego platformy ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209849"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrowanie moduły i programy obsługi HTTP do oprogramowania pośredniczącego platformy ASP.NET Core

Przez [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

W tym artykule pokazano, jak przeprowadzić migrację istniejących ASP.NET [moduły HTTP do programów obsługi z system.webserver](/iis/configuration/system.webserver/) do platformy ASP.NET Core [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Moduły i programy obsługi poprawiony

Przed przejściem do oprogramowania pośredniczącego platformy ASP.NET Core, najpierw przypomnijmy jak działają moduły HTTP do programów obsługi:

![Program obsługi modułów](http-modules/_static/moduleshandlers.png)

**Programy obsługi są:**

* Klasy, które implementują [IHttpHandler](/dotnet/api/system.web.ihttphandler)

* Używane do obsługi żądań przy użyciu określonej nazwy pliku lub rozszerzenie, takich jak *.report*

* [Skonfigurowane](/iis/configuration/system.webserver/handlers/) w *pliku Web.config*

**Dostępne są następujące moduły:**

* Klasy, które implementują [IHttpModule](/dotnet/api/system.web.ihttpmodule)

* Wywoływane dla każdego żądania

* Możliwość zwarcie (zatrzymać dalsze przetwarzanie żądania)

* Możliwość dodania do odpowiedzi HTTP, lub Utwórz własne

* [Skonfigurowane](/iis/configuration/system.webserver/modules/) w *pliku Web.config*

**Kolejność, w którym modułów przetworzyć żądań przychodzących jest określana przez:**

1. [Cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx), czyli zdarzenia serii, wywoływane przez platformę ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)itp. Każdy moduł można utworzyć procedury obsługi dla jednego lub wielu zdarzeń.

2. Zdarzenie, dla którego, kolejność, w którym są one konfigurowane w *Web.config*.

Oprócz modułów, można dodać procedury obsługi zdarzeń cyklu życia do Twojej *Global.asax.cs* pliku. Te procedury obsługi uruchomić po obsługi w modułach skonfigurowany.

## <a name="from-handlers-and-modules-to-middleware"></a>Programy obsługi i moduły do oprogramowania pośredniczącego

**Oprogramowanie pośredniczące są prostsze niż moduły HTTP do programów obsługi:**

* Moduły, programy obsługi, *Global.asax.cs*, *Web.config* (z wyjątkiem konfiguracji usług IIS) i znikną cyklu życia aplikacji

* Role, moduły i programy obsługi zostały przejęte przez oprogramowanie pośredniczące

* Oprogramowanie pośredniczące są skonfigurowane przy użyciu kodu, a nie w *pliku Web.config*

* [Rozgałęzianie w potoku](xref:fundamentals/middleware/index#use-run-and-map) pozwala wysyłać żądania do określonej, opartego na oprogramowaniu pośredniczącym nie tylko adres URL, ale także dla nagłówków żądań, ciągi zapytań, itp.

**Oprogramowanie pośredniczące są bardzo podobne do modułów:**

* Wywoływane w zasadzie dla każdego żądania

* Możliwość zwarcie żądanie, według [nie przekazaniem żądania do następnego oprogramowania pośredniczącego](#http-modules-shortcircuiting-middleware)

* Możliwość tworzenia własnych odpowiedzi HTTP

**Oprogramowanie pośredniczące i moduły są przetwarzane w innej kolejności:**

* Kolejność oprogramowania pośredniczącego zależy od kolejności, w którym wstawiane są one potokiem żądań, gdy zamówienie modułów opiera się przede wszystkim na [cyklu życia aplikacji](https://msdn.microsoft.com/library/ms227673.aspx) zdarzenia

* Kolejność oprogramowania pośredniczącego w odpowiedzi jest odwrotnie niż to, w przypadku żądań, a kolejność modułów jest taka sama dla żądań i odpowiedzi

* Zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![Oprogramowanie pośredniczące](http-modules/_static/middleware.png)

Należy pamiętać o tym, jak na ilustracji powyżej oprogramowania pośredniczącego uwierzytelniania short-circuited żądania.

## <a name="migrating-module-code-to-middleware"></a>Migrowanie kodu modułu do oprogramowania pośredniczącego

Istniejący moduł HTTP będzie wyglądać podobnie do poniższego:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Jak pokazano na [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) stronie pośredniczące platformy ASP.NET Core to klasa, która udostępnia `Invoke` wykonywanie metody `HttpContext` i zwracanie `Task`. Nowego oprogramowania pośredniczącego będzie wyglądać następująco:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Powyższy szablon oprogramowania pośredniczącego została pobrana z sekcji [pisanie oprogramowania pośredniczącego](xref:fundamentals/middleware/write).

*MyMiddlewareExtensions* klasy pomocnika ułatwia konfigurowanie oprogramowania pośredniczącego w swojej `Startup` klasy. `UseMyMiddleware` Metoda dodaje klasa oprogramowania pośredniczącego do potoku żądania. Usługi wymagane przez oprogramowanie pośredniczące Pobierz wprowadzony w Konstruktorze oprogramowania pośredniczącego.

<a name="http-modules-shortcircuiting-middleware"></a>

Modułu może przerwać żądanie, na przykład jeśli użytkownik nie ma uprawnień:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Oprogramowanie pośredniczące wykonuje tę nie wywołując `Invoke` na następne oprogramowanie pośredniczące w potoku. Zachowaj należy pamiętać, że to nie pełni zakończyć żądania, ponieważ poprzednie middlewares nadal zostanie wywołany, gdy odpowiedź sprawia, że jego sposób za pośrednictwem potoku.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Podczas migracji funkcji Twojego modułu do nowego oprogramowania pośredniczącego, może się okazać, że Twój kod nie kompilacji, ponieważ `HttpContext` klasy zmienił się znacznie w programie ASP.NET Core. [Później](#migrating-to-the-new-httpcontext), pokazano, jak przeprowadzić migrację do nowego obiektu ASP.NET Core HttpContext.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrowanie wstawienia modułu Potok żądań

Moduły HTTP zazwyczaj są dodawane do żądania potoku przy użyciu *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Skonwertuje ją przez [dodanie nowego oprogramowania pośredniczącego](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) do potoku żądania w swojej `Startup` klasy:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Dokładnie miejscu w potoku, w którym wstawisz nowego oprogramowania pośredniczącego zależy od zdarzenia, które system zrobił to automatycznie jako moduł (`BeginRequest`, `EndRequest`, itp.) i ich kolejność na liście modułów w *Web.config*.

Jak wcześniej wspomniano, istnieje nie cyklu życia aplikacji w programie ASP.NET Core i kolejności przetwarzania odpowiedzi przez oprogramowanie pośredniczące różni się od kolejności, używany przez moduły. To może spowodować, że szeregowania decyzji trudniejsze.

Jeśli kolejność staje się problem, można podzielić modułu wielu składników oprogramowania pośredniczącego, które może zostać określona niezależnie.

## <a name="migrating-handler-code-to-middleware"></a>Migrowanie kodu programu obsługi do oprogramowania pośredniczącego

Program obsługi HTTP, wygląda mniej więcej tak:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

W projekcie platformy ASP.NET Core będzie to przełożyć na oprogramowanie pośredniczące podobny do następującego:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

To oprogramowanie pośredniczące jest bardzo podobny do oprogramowania pośredniczącego odpowiadający modułów. Tylko rzeczywistego różnicą jest to, że w tym miejscu nie ma żadnych wywołanie `_next.Invoke(context)`. Ma to sens, ponieważ program obsługi ma pod koniec potokiem żądań, zostanie nie następne oprogramowanie pośredniczące do wywołania.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrowanie wstawienia Potok żądań obsługi

Konfigurowanie programu obsługi HTTP odbywa się w *Web.config* i wygląda następująco:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Można to skonwertować przez dodanie nowej procedury obsługi oprogramowania pośredniczącego do potoku żądania w swojej `Startup` klasy, podobnie jak przekonwertować z modułów oprogramowania pośredniczącego. Problem za pomocą tego podejścia polega na tym, że prześle wszystkich żądań do nowego programu obsługi oprogramowania pośredniczącego. Jednak mają tylko z danym rozszerzeniem dotarcie żądania do oprogramowania pośredniczącego. Który pozwoli uzyskać te same funkcje, które miał za pomocą programu obsługi HTTP.

Rozwiązanie polega na gałąź potoku dla żądań z danego rozszerzenia za pomocą `MapWhen` — metoda rozszerzenia. Można to zrobić w taki sam `Configure` metody, w którym dodajesz innym oprogramowaniu pośredniczącym:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` przyjmuje następujące parametry:

1. Lambda, która pobiera `HttpContext` i zwraca `true` Jeśli żądanie powinno przestaną działać gałęzi. Oznacza to, że można rozgałęziać żądań nie tylko na podstawie ich rozszerzeń, ale również na nagłówki żądania, parametry ciągu zapytania itp.

2. Lambda, która pobiera `IApplicationBuilder` i dodaje całe oprogramowanie pośredniczące dla gałęzi. Oznacza to, że można dodać dodatkowe oprogramowanie pośredniczące do gałęzi przed obsługi oprogramowania pośredniczącego.

Oprogramowanie pośredniczące dodawane do potoku przed gałęzi, zostanie wywołana na wszystkie żądania; gałąź będzie mieć żadnego wpływu na nich.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Opcje oprogramowania pośredniczącego przy użyciu wzorca opcje ładowania

Niektóre moduły i programy obsługi ma opcji konfiguracji, które są przechowywane w *Web.config*. Jednak w programie ASP.NET Core nowy model konfiguracji jest używany zamiast *Web.config*.

Nowy [system konfiguracji](xref:fundamentals/configuration/index) zapewnia tych opcji, aby rozwiązać ten problem:

* Opcje w oprogramowaniu pośredniczącym, wstrzyknąć bezpośrednio, jak pokazano w [następnej sekcji](#loading-middleware-options-through-direct-injection).

* Użyj [wzorzec opcje](xref:fundamentals/configuration/options):

1. Tworzenie klasy utrzymującej opcje oprogramowania pośredniczącego, na przykład:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Store wartości opcji

   System konfiguracji służy do przechowywania wartości opcji dowolnym ma. Jednak większość witryn użyj *appsettings.json*, więc przeniesiemy tego podejścia:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* Oto nazwę sekcji. Nie musi być taka sama jak nazwa klasy opcji.

3. Kojarzenie wartości opcji z klasy opcji

    Wzorzec opcje używa framework iniekcji zależności platformy ASP.NET Core w celu skojarzenia typu opcji (takich jak `MyMiddlewareOptions`) przy użyciu `MyMiddlewareOptions` obiekt, który zapewnia dostęp do rzeczywistego opcji.

    Aktualizacja usługi `Startup` klasy:

   1. Jeśli używasz *appsettings.json*, dodaj go do konstruktora konfiguracji w `Startup` Konstruktor:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Skonfiguruj usługę opcje:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Skojarz opcje z klasy opcji:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Wstrzyknąć opcje do konstruktora oprogramowania pośredniczącego. Jest to podobne do wprowadzanie opcji w kontroler.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware) metodę rozszerzenia, który dodaje oprogramowaniu pośredniczącym, aby `IApplicationBuilder` dba o iniekcji zależności.

   To nie jest ograniczona do `IOptions` obiektów. Inny obiekt, który wymaga oprogramowania pośredniczącego może wprowadzony w ten sposób.

## <a name="loading-middleware-options-through-direct-injection"></a>Opcje oprogramowania pośredniczącego przy użyciu iniekcji bezpośrednie ładowanie

Wzorzec opcje ma tę zaletę, że tworzy luźne, sprzężenia między wartościami opcje i ich odbiorców. Gdy klasa opcji został skojarzony z wartości rzeczywistej opcje, inne klasy mogą uzyskać dostęp do opcji przez strukturę iniekcji zależności. Nie ma potrzeby w celu przejścia wokół wartości opcji.

To dzieli jednak jeśli chcesz użyć tego samego oprogramowania pośredniczącego dwa razy, przy użyciu różnych opcji. Na przykład autoryzacji oprogramowanie pośredniczące używane w różnych oddziałach, umożliwiając różne role. Dwa obiekty różnych opcji nie można skojarzyć z klasy jednej opcji.

Rozwiązanie jest uzyskanie obiektów opcje przy użyciu wartości z rzeczywistych opcje usługi `Startup` klasy i przekazać te bezpośrednio do poszczególnych wystąpień tego oprogramowania pośredniczącego.

1. Dodaj drugi klucz, który *appsettings.json*

   Aby dodać drugi zestaw opcji, aby *appsettings.json* pliku, użyj nowego klucza umożliwiającą jej jednoznaczną identyfikację:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Pobieranie wartości opcji i przekazywać je do oprogramowania pośredniczącego. `Use...` Metody rozszerzenia, (które dodaje oprogramowania pośredniczącego do potoku) jest miejscem logiczne do przekazywania wartości opcji: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Włącza oprogramowanie pośredniczące przyjmować parametr opcji. Podaj przeciążenia `Use...` — metoda rozszerzenia (który przyjmuje parametr opcji i przekazuje go do `UseMiddleware`). Gdy `UseMiddleware` jest wywoływana z parametrami, przekazuje parametry do konstruktora oprogramowania pośredniczącego podczas tworzenia wystąpienia obiektu oprogramowania pośredniczącego.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Należy zauważyć, jak to otacza obiekt opcje w `OptionsWrapper` obiektu. To implementuje `IOptions`, zgodnie z oczekiwaniami, konstruktora oprogramowania pośredniczącego.

## <a name="migrating-to-the-new-httpcontext"></a>Migracja do nowego obiektu HttpContext.

Wcześniej, `Invoke` metoda w oprogramowania pośredniczącego przyjmuje parametr typu `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` znacznie zmienił się w programie ASP.NET Core. W tej sekcji pokazano, jak tłumaczenie najczęściej używanych właściwości [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) do nowego `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Unikatowy identyfikator (odpowiednika System.Web.HttpContext)**

Zawiera unikatowy identyfikator dla każdego żądania. Bardzo przydatne do uwzględnienia w dziennikach.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** i **HttpContext.Request.RawUrl** przełożyć na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Odczyt wartości formularza, tylko wtedy, gdy zawartość podtyp *x--www-form-urlencoded* lub *dane formularza*.

**HttpContext.Request.InputStream** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Użyj tego kodu tylko w przypadku obsługi typu oprogramowanie pośredniczące, na końcu potoku.
>
>Może odczytywać nieprzetworzonej treści, jak pokazano powyżej jednokrotnie dla danego żądania. Oprogramowanie pośredniczące próby odczytu treści po pierwszym odczytu będzie odczytywał element body puste.
>
>To nie ma zastosowania do odczytu formularza, jak pokazano wcześniej, ponieważ zostanie to zrobione buforu.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** i **HttpContext.Response.StatusDescription** przełożyć na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** i **HttpContext.Response.ContentType** przełożyć na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** na jego własnej również przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** przekłada się na:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Wysyłaniu pliku omówiono [tutaj](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Wysyłanie nagłówków odpowiedzi jest skomplikowane faktem, że jeśli ustawisz je po nic został zapisany treść odpowiedzi, mogą nie będą wysyłane.

Rozwiązanie jest można ustawić metodę wywołania zwrotnego, która zostanie wywołana po prawej stronie przed zapisaniem uruchamia odpowiedzi. Najlepiej odbywa się na początku `Invoke` metody w oprogramowania pośredniczącego. Jest to metoda wywołania zwrotnego, określająca swoje nagłówki odpowiedzi.

Poniższy kod ustawia metodę wywołania zwrotnego wywoływana `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Metody wywołania zwrotnego będzie wyglądać następująco:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Pliki cookie są przesyłane do przeglądarki w *Set-Cookie* nagłówka odpowiedzi. W rezultacie wysyłanie plików cookie wymaga tego samego wywołania zwrotnego, które jest używane do wysyłania nagłówków odpowiedzi:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Metody wywołania zwrotnego będzie wyglądać podobnie do poniższego:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Funkcje obsługi protokołu HTTP i moduły HTTP danych — omówienie](/iis/configuration/system.webserver/)
* [Konfiguracja](xref:fundamentals/configuration/index)
* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
