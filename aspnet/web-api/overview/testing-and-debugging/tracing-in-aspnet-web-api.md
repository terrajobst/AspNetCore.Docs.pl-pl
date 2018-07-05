---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Śledzenie we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje, jak włączyć śledzenie w programie ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9c33e62a1d04bc7851be13560a2bd39e997448f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366867"
---
<a name="tracing-in-aspnet-web-api-2"></a>Śledzenie we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Jeśli chcesz debugować aplikację sieci web, to nie nie zastąpi dobry zestaw dzienniki śledzenia. W tym samouczku pokazano, jak włączyć śledzenie w programie ASP.NET Web API. Ta funkcja służy do śledzenia struktury Web API działanie przed i po wywołuje kontroler. Można również użyć do śledzenia własnego kodu.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) (współpracuje również z programu Visual Studio 2015)
> - Składnik Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Włącz System.Diagnostics śledzenie w składniku Web API

Najpierw utworzymy nowy projekt aplikacji sieci Web ASP.NET. W programie Visual Studio z **pliku** menu, wybierz opcję **New**, następnie **projektu**. W obszarze **szablony**, **Web**, wybierz opcję **aplikacji sieci Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wybierz szablon projektu interfejsu API sieci Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, następnie **konsoli Zarządzanie pakietów**.

W oknie Konsola Menedżera pakietów wpisz następujące polecenia.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Pierwsze polecenie powoduje zainstalowanie najnowszego pakietu śledzenia interfejsu API sieci Web. Aktualizuje core pakiety interfejsu API sieci Web. Drugie polecenie aktualizuje pakiet WebApi.WebHost do najnowszej wersji.

> [!NOTE]
> Pod kątem określonej wersji interfejsu API sieci Web, należy użyć flagi wersji, po zainstalowaniu pakietu śledzenia.


Otwórz plik WebApiConfig.cs w aplikacji\_folder początkowy. Dodaj następujący kod do **zarejestrować** metody.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Ten kod dodaje [klasę SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) klasy potok składnika Web API. **Klasę SystemDiagnosticsTraceWriter** klasy zapisuje informacje śledzenia do [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Aby wyświetlić ślady, uruchom aplikację w debugerze. W przeglądarce przejdź do `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Instrukcji śledzenia są zapisywane w oknie danych wyjściowych w programie Visual Studio. (Z **widoku** menu, wybierz opcję **dane wyjściowe**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Ponieważ **klasę SystemDiagnosticsTraceWriter** zapisuje informacje śledzenia do **System.Diagnostics.Trace**, możesz zarejestrować odbiorniki śledzenia dodatkowe; na przykład, aby zapisać ślady w pliku dziennika. Aby uzyskać więcej informacji na temat składników zapisywania śledzenia, zobacz [detektorów śledzenia](https://msdn.microsoft.com/library/4y5y10s7.aspx) tematu w witrynie MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurowanie klasę SystemDiagnosticsTraceWriter

Poniższy kod przedstawia sposób konfigurowania moduł zapisujący śledzenia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Istnieją dwa ustawienia, które mogą kontrolować:

- IsVerbose: W przypadku wartości FAŁSZ Każdy ślad zawiera minimalne informacje. W przypadku opcji true ślady zawierają więcej informacji.
- MinimumLevel: Ustawia minimalny poziom śledzenia. Poziomy śledzenia, w kolejności, są debugowania, Info, Ostrzegaj, błędów i błąd krytyczny.

## <a name="adding-traces-to-your-web-api-application"></a>Dodawanie danych śledzenia do aplikacji sieci Web interfejsu API

Dodawanie moduł zapisujący śledzenia umożliwia bezpośredni dostęp do śledzenia, utworzone przez potok składnika Web API. Moduł zapisujący śledzenia umożliwia również śledzenie własnego kodu:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Aby uzyskać moduł zapisujący śledzenia, należy wywołać **HttpConfiguration.Services.GetTraceWriter**. Z kontrolera, ta metoda jest dostępna za pośrednictwem **ApiController.Configuration** właściwości.

Do zapisu, śledzenia, można wywołać **ITraceWriter.Trace** metoda bezpośrednio, ale [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) klasa definiuje kilka metod rozszerzenia, które są bardziej przyjazny. Na przykład **informacje** metoda powyżej tworzy śledzenia z poziomu śledzenia **informacje**.

## <a name="web-api-tracing-infrastructure"></a>Infrastruktura śledzenia interfejsu API sieci Web

W tej sekcji opisano sposób pisania moduł zapisujący śledzenia niestandardowego interfejsu API sieci Web.

Pakiet Microsoft.AspNet.WebApi.Tracing jest oparty na bardziej ogólnych Infrastruktura śledzenia w interfejsie API sieci Web. Zamiast korzystać z Microsoft.AspNet.WebApi.Tracing, można także podłączyć niektóre inne biblioteki śledzenia/rejestrowanie, takich jak [NLog](http://nlog-project.org/) lub [log4net](http://logging.apache.org/log4net/).

Aby zbierać dane śledzenia, należy zaimplementować **ITraceWriter** interfejsu. Poniżej przedstawiono prosty przykład:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** metoda tworzy śledzenia. Obiekt wywołujący określa poziom kategorii i śledzenia. Kategoria może być dowolnym ciągiem zdefiniowanej przez użytkownika. Implementacja **śledzenia** należy wykonać następujące czynności:

1. Utwórz nową **TraceRecord**. Go zainicjować za pomocą żądania, kategorii i poziomie śledzenia, jak pokazano. Te wartości są dostarczane przez obiekt wywołujący.
2. Wywoływanie *traceAction* delegować. Wewnątrz tego delegata oczekuje obiektu wywołującego do wypełnienia w pozostałej części **TraceRecord**.
3. Zapis **TraceRecord**, przy użyciu dowolnej techniki rejestrowania, którą chcesz. Przykład pokazany w tym miejscu po prostu wywoła **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Ustawienie moduł zapisujący śledzenia

Aby włączyć śledzenie, należy skonfigurować interfejsu API sieci Web, aby użyć swojej **ITraceWriter** implementacji. W tym za pośrednictwem **HttpConfiguration** obiektu, jak pokazano w poniższym kodzie:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Moduł zapisujący śledzenia tylko jedna może być aktywne. Domyślnie, internetowy interfejs API ustawia &quot;pusta&quot; śledzenia, która nie wykonuje żadnych działań. ( &quot;Pusta&quot; śledzący istnieje, dzięki czemu kod śledzenia nie trzeba sprawdzić, czy moduł zapisujący śledzenia jest **null** przed napisaniem śledzenia.)

## <a name="how-web-api-tracing-works"></a>Jak internetowy interfejs API śledzenia działa

Śledzenie używa interfejsu API sieci Web w interfejsu API sieci Web używa *fasady* wzorzec: po włączeniu funkcji śledzenia interfejsu API sieci Web opakowuje różne części Potok żądań z klasami, które wykonują wywołania śledzenia.

Na przykład podczas wybierania kontrolera, potok używa **IHttpControllerSelector** interfejsu. Z włączonym śledzeniem pipleline wstawia klasę, która implementuje **IHttpControllerSelector** , ale wywołania za pośrednictwem rzeczywistej implementacji:

![Śledzenie interfejsu API sieci Web używa wzorca fasady.](tracing-in-aspnet-web-api/_static/image8.png)

Zalety tego projektu:

- Jeśli moduł zapisujący śledzenia nie zostaną dodane, składniki śledzenia nie są tworzone i mieć żadnego wpływu na wydajność.
- Jeśli zastąpisz domyślnych usług takich jak **IHttpControllerSelector** przy użyciu własnych niestandardowych implementacji śledzenia nie występuje, ponieważ śledzenie odbywa się przez obiekt otoki.

Możesz również zastąpić całej struktury framework śledzenia interfejsu API sieci Web za pomocą własnych niestandardowych framework, zastępując domyślne **ITraceManager** usługi:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementowanie **ITraceManager.Initialize** zainicjować systemu śledzenia. Należy pamiętać, że spowoduje to zastąpienie *całego* framework śledzenia, łącznie ze wszystkimi kod śledzenia, która jest wbudowana w interfejsu API sieci Web.
