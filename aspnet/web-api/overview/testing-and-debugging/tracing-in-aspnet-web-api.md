---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "Śledzenie w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>Śledzenie w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Próbujesz debugowanie aplikacji sieci web, nie ma nie zastąpi dobry zestaw dzienników śledzenia. Ten samouczek pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET. Ta funkcja służy do śledzenia strukturę interfejsu API sieci Web do czego przed i po wywołuje w kontrolerze. Można również użyć do śledzenia swoim własnym kodem.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (współdziała również z programu Visual Studio 2015)
> - Składnik Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Włącz śledzenie w składniku Web API System.Diagnostics

Najpierw utworzymy nowy projekt aplikacji sieci Web ASP.NET. W programie Visual Studio z **pliku** menu, wybierz opcję **nowy**, następnie **projektu**. W obszarze **szablony**, **Web**, wybierz pozycję **aplikacji sieci Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wybierz szablon projektu interfejsu API sieci Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, następnie **konsoli Zarządzanie pakietów**.

W oknie Konsola Menedżera pakietów wpisz następujące polecenia.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Pierwsze polecenie instaluje najnowszy pakiet śledzenia interfejsu API sieci Web. Aktualizuje również pakietami podstawowymi interfejsu API sieci Web. Drugie polecenie aktualizuje pakiet WebApi.WebHost do najnowszej wersji.

> [!NOTE]
> Jeśli chcesz przeanalizować określonej wersji interfejsu API sieci Web, Użyj flagi wersji, podczas instalowania pakietu śledzenia.


Otwórz plik WebApiConfig.cs w aplikacji\_folder początkowy. Dodaj następujący kod do **zarejestrować** metody.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Ten kod dodaje [klasę SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) klasy potok składnika Web API. **Klasę SystemDiagnosticsTraceWriter** klasy zapisuje śladów do [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

Aby wyświetlić dane śledzenia, należy uruchomić aplikację w debugerze. W przeglądarce przejdź do `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Instrukcje śledzenia są zapisywane w oknie danych wyjściowych w programie Visual Studio. (Z **widoku** menu, wybierz opcję **dane wyjściowe**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Ponieważ **klasę SystemDiagnosticsTraceWriter** zapisuje dane śledzenia do **System.Diagnostics.Trace**, możesz zarejestrować obiekty nasłuchujące śledzenia dodatkowe; na przykład, aby zapisać śladów do pliku dziennika. Aby uzyskać więcej informacji na temat zapisywania śledzenia, zobacz [obiektów nasłuchujących śledzenia](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) temacie w witrynie MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurowanie klasę SystemDiagnosticsTraceWriter

Poniższy kod przedstawia sposób konfigurowania moduł zapisujący śledzenia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Istnieją dwa ustawienia, które mogą kontrolować:

- IsVerbose: W przypadku wartości FAŁSZ każdego śledzenia zawiera minimalne informacje. Jeśli PRAWDA, ślady zawierają więcej informacji.
- MinimumLevel: Ustawia minimalny poziom śledzenia. Poziomy śledzenia, w kolejności, to debugowania, informacje o Ostrzegaj, błąd i błąd krytyczny.

## <a name="adding-traces-to-your-web-api-application"></a>Dodawanie śladów do swojej aplikacji interfejsu API sieci Web

Dodawanie moduł zapisujący śledzenia umożliwia natychmiastowy dostęp do śledzenia utworzone przez potok składnika Web API. Moduł zapisujący śledzenia można także użyć do śledzenia swoim własnym kodem:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Aby uzyskać moduł zapisujący śledzenia, należy wywołać **HttpConfiguration.Services.GetTraceWriter**. Z kontrolera, ta metoda jest dostępna za pośrednictwem **ApiController.Configuration** właściwości.

Aby napisać śledzenia, należy wywołać **ITraceWriter.Trace** metody bezpośrednio, ale [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) klasa definiuje niektóre metody rozszerzenia, które są bardziej przyjazna dla użytkownika. Na przykład **informacji** metod przedstawionych powyżej tworzy śledzenia z poziomu śledzenia **informacji**.

## <a name="web-api-tracing-infrastructure"></a>Infrastruktura śledzenia interfejsu API sieci Web

W tej sekcji opisano, jak zapisać moduł zapisujący śledzenia niestandardowego interfejsu API sieci Web.

Pakietu Microsoft.AspNet.WebApi.Tracing jest oparty na bardziej ogólne Infrastruktura śledzenia w interfejsie API sieci Web. Zamiast Microsoft.AspNet.WebApi.Tracing, można także podłączyć niektórych innych biblioteki śledzenie/logowania, takie jak [NLog](http://nlog-project.org/) lub [log4net](http://logging.apache.org/log4net/).

Aby zbierać dane śledzenia, należy zaimplementować **ITraceWriter** interfejsu. Poniżej przedstawiono prosty przykład:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace** metoda tworzy śledzenia. Obiekt wywołujący określa poziom kategorii i śledzenia. Kategoria może być dowolnym ciągiem zdefiniowane przez użytkownika. Implementacji **śledzenia** należy wykonać następujące czynności:

1. Utwórz nową **TraceRecord**. Go zainicjować z żądania, kategorii i poziomie śledzenia, jak pokazano. Te wartości są dostarczane przez obiekt wywołujący.
2. Wywołanie *traceAction* delegowanie. Wewnątrz tego delegata wywołujący powinien wypełnić w pozostałej części **TraceRecord**.
3. Zapis **TraceRecord**, za pomocą żadnych technika rejestrowania, który chcesz. Tu przykładzie wywołuje po prostu **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Ustawienie moduł zapisujący śledzenia

Aby włączyć śledzenie, należy skonfigurować interfejs API sieci Web do użycia z **ITraceWriter** implementacji. W tym za pomocą **HttpConfiguration** obiektów, jak pokazano w poniższym kodzie:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Moduł zapisujący śledzenia tylko jedna może być aktywny. Domyślnie ustawia interfejsu API sieci Web &quot;pusta&quot; śledzenia, które nie działają. ( &quot;Pusta&quot; śledzenia istnieje, dzięki czemu kod śledzenia nie trzeba sprawdzić, czy moduł zapisujący śledzenia jest **null** przed zapisaniem śladu.)

## <a name="how-web-api-tracing-works"></a>Interfejs API, śledzenie działa jak sieci Web

Śledzenie wykorzystania interfejsu API sieci Web w interfejsu API sieci Web używa *fasad* wzorzec: gdy śledzenie jest włączone, interfejsu API sieci Web opakowuje różnych części Potok żądań z klasami, które wykonywać wywołania śledzenia.

Na przykład podczas wybierania kontrolera, używa potoku **IHttpControllerSelector** interfejsu. Z włączonym śledzeniem pipleline wstawia klasy, która implementuje **IHttpControllerSelector** , ale wywołania do rzeczywistego wykonania:

![Śledzenie sieci Web interfejsu API korzysta ze wzorca fasad.](tracing-in-aspnet-web-api/_static/image8.png)

Zalety tego projektu:

- Jeśli moduł zapisujący śledzenia nie należy dodawać, składniki śledzenia nie jest utworzona i nie wpływają wydajności.
- Jeśli takie jak zastąpić domyślne usługi **IHttpControllerSelector** z własnych niestandardowych implementacji śledzenia nie występuje, ponieważ śledzenie jest wykonywana przez obiekt otoki.

Można również zastąpić całej struktury śledzenia interfejsu API sieci Web własne niestandardowe framework, zastępując wartość domyślna **ITraceManager** usługi:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementowanie **ITraceManager.Initialize** zainicjować systemu śledzenia. Należy pamiętać, że spowoduje to zastąpienie *cały* framework śledzenia, łącznie ze wszystkimi kod śledzenia, który jest wbudowany w interfejsu API sieci Web.
