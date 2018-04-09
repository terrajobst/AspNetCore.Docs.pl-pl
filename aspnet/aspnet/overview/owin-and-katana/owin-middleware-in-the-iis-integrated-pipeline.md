---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Oprogramowanie pośredniczące OWIN w usługach IIS zintegrowane potoku | Dokumentacja firmy Microsoft
author: Praburaj
description: W tym artykule przedstawiono sposób uruchamiania składników oprogramowania pośredniczącego OWIN (OMCs) w zintegrowanym potoku usług IIS i działa jak ustawić zdarzenia w potoku OMC na. Należy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Oprogramowanie pośredniczące OWIN w zintegrowanym potoku usług IIS
====================
przez [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> W tym artykule przedstawiono sposób uruchamiania składników oprogramowania pośredniczącego OWIN (OMCs) w zintegrowanym potoku usług IIS i działa jak ustawić zdarzenia w potoku OMC na. Należy przejrzeć [Omówienie projektu Katana](an-overview-of-project-katana.md) i [OWIN uruchamiania klasy wykrywania](owin-startup-class-detection.md) przed przeczytaniem tego samouczka. W tym samouczku zapisał Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Krzysztof roaming, Praburaj Thiagarajan i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Mimo że [OWIN](an-overview-of-project-katana.md) składników oprogramowania pośredniczącego (OMCs) jest przeznaczony głównie do działania w potoku niezwiązane z żadnym serwerem, można uruchomić w zintegrowanym potoku usług IIS oraz OMC (**jest w trybie klasycznym *nie* obsługiwane**). OMC może również działać w zintegrowanym potoku usług IIS przez zainstalowanie następującego pakietu z konsoli Menedżera pakietów (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Oznacza to, że wszystkie struktury aplikacji, nawet te, które nie są jeszcze można uruchamiać poza usług IIS i System.Web, mogą korzystać z istniejących składników oprogramowania pośredniczącego OWIN. 

> [!NOTE]
> Wszystkie `Microsoft.Owin.Security.*` pakietów wysyłki z nowym systemem tożsamości w programie Visual Studio 2013 (na przykład: plików cookie, Account firmy Microsoft, Google, Facebook, Twitter, [tokenów Bearer](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, serwer autoryzacji, JWT, Azure Active Katalog usług federacyjnych Active directory) są tworzone jako OMCs i mogą być używane w scenariuszach siebie i hostowanych przez usługi IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Jak oprogramowanie pośredniczące OWIN wykonuje w zintegrowanym potoku usług IIS

W przypadku aplikacji konsoli OWIN potoku aplikacji utworzony za pomocą [konfiguracji uruchamiania](owin-startup-class-detection.md) ustawiono według kolejności składniki są dodawane przy użyciu `IAppBuilder.Use` metody. Oznacza to, że potok OWIN w [Katana](an-overview-of-project-katana.md) środowiska uruchomieniowego przetworzy OMCs w kolejności zostały zarejestrowane przy użyciu `IAppBuilder.Use`. W zintegrowanym potoku usług IIS Potok żądań składa się z [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) subskrybuje wstępnie zdefiniowany zestaw zdarzeń potoku, takich jak [powstaniem zdarzenia BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)itp.

Jeśli firma Microsoft porównania OMC niż [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) na świecie ASP.NET OMC musi być zarejestrowana w zdarzeniu poprawne uprzednio zdefiniowanej potoku. Na przykład HttpModule `MyModule` będzie pobrać wywoływane, gdy dotrze żądanie [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) etap w potoku:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Aby OMC brać udziału w tej kolejności wykonywania tego samego, oparty na zdarzeniach [Katana](an-overview-of-project-katana.md) kod środowiska uruchomieniowego skanowania za pomocą [konfiguracji uruchamiania](owin-startup-class-detection.md) i poszczególnych składników oprogramowania pośredniczącego, które subskrybuje Zdarzenie zintegrowanego potoku. Na przykład następujący kod OMC i rejestracji temu można zobaczyć domyślne rejestracji zdarzeń składników oprogramowania pośredniczącego. (Aby uzyskać szczegółowe instrukcje dotyczące tworzenia klasy początkowej OWIN, zobacz [OWIN uruchamiania klasy wykrywania](owin-startup-class-detection.md).)

1. Utwórz pusty projekt sieci web aplikacji i nadaj mu nazwę **owin2**.
2. Z konsoli Menedżera pakietów (PMC), uruchom następujące polecenie: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Dodaj `OWIN Startup Class` i nadaj mu nazwę `Startup`. Zamień wygenerowany kod poniżej (zmiany zostały wyróżnione):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Naciśnij klawisz F5, aby uruchomić aplikację.

Ustawienie konfiguracji uruchamiania potoku składników oprogramowania pośredniczącego trzy, pierwsze dwa wyświetlanie informacji diagnostycznych oraz ostatnią odpowiadanie na zdarzenia (i również wyświetlanie informacji diagnostycznych). `PrintCurrentIntegratedPipelineStage` Metoda Wyświetla zdarzeń zintegrowanego potoku to oprogramowanie pośredniczące jest wywoływane po i wiadomości. W oknie danych wyjściowych wyświetla następujące informacje:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Środowisko uruchomieniowe Katana zamapować wszystkich składników oprogramowania pośredniczącego OWIN do [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) domyślnie, które odpowiada zdarzenie potoku usług IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Etap znaczników

Można oznaczyć OMCs można wykonać na poszczególnych etapów potoku przy użyciu `IAppBuilder UseStageMarker()` — metoda rozszerzenia. Aby uruchomić zestaw składników oprogramowania pośredniczącego w szczególności etapie, Wstaw znacznik etapu bezpośrednio po ostatniej części jest zestawem podczas rejestracji. Istnieją reguły, na którym etapie w potoku można wykonywać oprogramowanie pośredniczące i składniki kolejności musi działać (reguły opisano szczegółowo w dalszej części tego samouczka). Dodaj `UseStageMarker` metodę `Configuration` kodu, jak pokazano poniżej:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Wywołania konfiguruje wszystkich składników oprogramowania pośredniczącego wcześniej zarejestrowane (w tym przypadku naszej dwa składniki diagnostycznych) do uruchamiania na etapie uwierzytelniania potoku. Ostatni składnik oprogramowania pośredniczącego (która wyświetla diagnostyki i odpowiada na żądania) zostanie uruchomiony na `ResolveCache` etap ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) zdarzeń).

Naciśnij klawisz F5, aby uruchomić aplikację. W oknie danych wyjściowych zawiera następujące czynności:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reguły znacznika etapu

Składniki oprogramowania pośredniczącego Owin (OMC) można skonfigurować do uruchamiania w następujących zdarzeń etap potoku OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Domyślnie OMCs jednocześnie ostatniego zdarzenia (`PreHandlerExecute`). Dlatego naszym pierwszym przykładowy kod wyświetlany "PreExecuteRequestHandler".
2. Można użyć `app.UseStageMarker` metodę, aby zarejestrować OMC, aby uruchomić wcześniej, na każdym etapie potoku OWIN na liście `PipelineStage` wyliczenia.
3. Potok OWIN i potoku usług IIS jest określona, w związku z tym wywołania `app.UseStageMarker` musi być w kolejności. Nie można ustawić programu obsługi zdarzeń zdarzenie poprzedza ostatnie zdarzenie zarejestrowany z `app.UseStageMarker`. Na przykład *po* wywołanie:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   wywołuje się `app.UseStageMarker` przekazywanie `Authenticate` lub `PostAuthenticate` nie będzie można go uznać i nie zostanie wygenerowany wyjątek. Uruchom na etapie najnowszej, która domyślnie jest OMCs `PreHandlerExecute`. Znaczniki etapu są używane do były wcześniej Uruchom. Jeśli określisz znaczników etap poza kolejnością, możemy zaokrąglona do wcześniejszych znacznika. Innymi słowy znacznik etapu Dodawanie mówi "Uruchom nie później niż etap X". Uruchom w OMC przy najbliższej znacznika etapu dodane po ich w potoku OWIN.
4. Najwcześniejszym etapie wywołań `app.UseStageMarker` usługi wins. Na przykład jeśli przełącznik kolejność `app.UseStageMarker` wywołania z naszych poprzednim przykładzie:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   W oknie dane wyjściowe będą wyświetlane: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs wszystkie działania w `AuthenticateRequest` etap, ponieważ ostatnia OMC zarejestrowany z `Authenticate` zdarzenia i `Authenticate` zdarzeń poprzedza inne zdarzenia.
