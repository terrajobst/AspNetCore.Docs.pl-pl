---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Oprogramowanie pośredniczące OWIN w usługach IIS zintegrowany potok | Dokumentacja firmy Microsoft
author: Praburaj
description: W tym artykule przedstawiono sposób uruchamiania składników oprogramowania pośredniczącego OWIN (OMCs) w zintegrowanym potoku usług IIS i działa jak skonfigurować zdarzenie potoku OMC. Wykonaj następujące czynności...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 1c2f7a9b948331eae2f5b44f25219adb76a0c745
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379063"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Oprogramowanie pośredniczące OWIN w zintegrowanym potoku usług IIS
====================
przez [projektu Praburaj](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> W tym artykule przedstawiono sposób uruchamiania składników oprogramowania pośredniczącego OWIN (OMCs) w zintegrowanym potoku usług IIS i działa jak skonfigurować zdarzenie potoku OMC. Należy zapoznać się z [Omówienie projektu Katana](an-overview-of-project-katana.md) i [wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md) przed odczytaniem w tym samouczku. Ten samouczek został napisany przez Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj projektu i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Mimo że [OWIN](an-overview-of-project-katana.md) składników oprogramowania pośredniczącego (OMCs) jest przeznaczony głównie do działania w potoku niezależny od serwera, można uruchomić OMC w zintegrowanym potoku usług IIS oraz (**trybu klasycznego jest *nie* obsługiwane**). OMC może również działać w zintegrowanym potoku usług IIS przez zainstalowanie następującego pakietu z konsoli Menedżera pakietów (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Oznacza to, że wszystkie struktury aplikacji, nawet te, które nie są jeszcze mogli uruchamiać poza usług IIS i System.Web, mogą korzystać z istniejących składników oprogramowania pośredniczącego OWIN. 

> [!NOTE]
> Wszystkie `Microsoft.Owin.Security.*` pakietów wysyłki za pomocą nowego systemu tożsamości w programie Visual Studio 2013 (na przykład: pliki cookie, Account firmy Microsoft, Google, Facebook, Twitter, [tokenu elementu nośnego](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, serwer autoryzacji, tokenów JWT, usługi Azure Active Katalog usług federacyjnych Active directory) są tworzone jako OMCs i mogą być używane w scenariuszach Self-Hosted i hostowanych przez usługi IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Jak wykonuje oprogramowanie pośredniczące OWIN w zintegrowanym potoku usług IIS

W przypadku aplikacji konsoli OWIN potoku aplikacji utworzonych przy użyciu [konfiguracji uruchamiania](owin-startup-class-detection.md) jest ustawiona według kolejności, składniki są dodawane przy użyciu `IAppBuilder.Use` metody. Oznacza to, że potok OWIN w [Katana](an-overview-of-project-katana.md) środowiska uruchomieniowego przetworzy OMCs w kolejności, zostały zarejestrowane przy użyciu `IAppBuilder.Use`. W zintegrowanym potoku usług IIS składa się Potok żądań [elementy HttpModule](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) subskrybuje wstępnie zdefiniowany zestaw zdarzeń potoku, takich jak [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)itp.

Gdy porównamy OMC niż [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) na świecie ASP.NET OMC muszą być zarejestrowane zdarzenia poprawne potoku wstępnie zdefiniowane. Na przykład HttpModule `MyModule` Pobierz wywoływane, gdy żądanie przejdzie [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) etapu w potoku:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Aby OMC do wzięcia udziału w tej kolejności wykonania tego samego, oparte na zdarzeniach [Katana](an-overview-of-project-katana.md) kod środowiska uruchomieniowego skanowania za pomocą [konfiguracji uruchamiania](owin-startup-class-detection.md) i subskrybuje wszystkich składników oprogramowania pośredniczącego Zdarzenie zintegrowanego potoku. Na przykład poniższy kod OMC i rejestracji temu można zobaczyć domyślne rejestracji zdarzeń składników oprogramowania pośredniczącego. (Aby uzyskać szczegółowe instrukcje dotyczące tworzenia klasy początkowej OWIN, zobacz [wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md).)

1. Utwórz pusty projekt sieci web aplikacji i nadaj mu **owin2**.
2. W konsoli Menedżera pakietów (PMC) uruchom następujące polecenie: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Dodaj `OWIN Startup Class` i nadaj mu nazwę `Startup`. Zastąp wygenerowany kod następującym (zmiany zostały wyróżnione):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Naciśnij klawisz F5, aby uruchomić aplikację.

Konfiguracja uruchamiania Konfiguruje potok za pomocą składników oprogramowania pośredniczącego trzy pierwsze dwa wyświetlanie informacji diagnostycznych i ostatnią odpowiadanie na zdarzenia (i również wyświetlanie informacji diagnostycznych). `PrintCurrentIntegratedPipelineStage` Metoda Wyświetla zdarzenia zintegrowanego potoku to oprogramowanie pośredniczące jest wywoływane na oraz wiadomość. W oknie danych wyjściowych wyświetla następujące informacje:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Środowisko uruchomieniowe Katana mapowane wszystkich składników oprogramowania pośredniczącego OWIN do [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) domyślnie, która odnosi się do zdarzenia potoku usług IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Znaczniki etapu

Możesz oznaczyć OMCs można wykonać na poszczególnych etapów potoku przy użyciu `IAppBuilder UseStageMarker()` — metoda rozszerzenia. Aby uruchomić zestaw składników oprogramowania pośredniczącego w określonym etapie, Wstaw znacznik etapu tuż po ostatnim składnikiem to zestaw podczas rejestracji. Istnieją reguły, na którym etapie w potoku można uruchamiać oprogramowanie pośredniczące i składniki kolejności należy uruchomić (zasady zostały wyjaśnione w dalszej części tego samouczka). Dodaj `UseStageMarker` metody `Configuration` kodu, jak pokazano poniżej:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Wywołanie konfiguruje wszystkie składniki oprogramowania pośredniczącego wcześniej zarejestrowanego (w tym przypadku nasze dwa składniki diagnostycznych) do uruchomienia w fazie uwierzytelniania potoku. Ostatni składnik oprogramowania pośredniczącego (wyświetla diagnostyki i odpowiada na żądania) będą uruchamiane na `ResolveCache` etapu ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) zdarzeń).

Naciśnij klawisz F5, aby uruchomić aplikację. W oknie danych wyjściowych zawiera następujące czynności:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reguły znacznika etapu

Składniki oprogramowania pośredniczącego Owin (OMC) można skonfigurować tak, by było uruchamiane następujące zdarzenia etapu potoku OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Domyślnie OMCs uruchamiane na ostatnie zdarzenie (`PreHandlerExecute`). Dlatego naszym pierwszym przykładowy kod wyświetlany "PreExecuteRequestHandler".
2. Możesz użyć `app.UseStageMarker` metodę, aby zarejestrować OMC przeprowadzić wcześniej, na każdym etapie potoku OWIN na liście `PipelineStage` wyliczenia.
3. Potok OWIN i potoku usługi IIS są porządkowane, w związku z tym wywołania `app.UseStageMarker` musi znajdować się w kolejności. Nie można ustawić programu obsługi zdarzeń do zdarzenia, które poprzedza ostatniego zdarzenia zarejestrowane w usłudze do `app.UseStageMarker`. Na przykład *po* wywołanie:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   wywołania `app.UseStageMarker` przekazywanie `Authenticate` lub `PostAuthenticate` nie będzie można uznać i zostanie zgłoszony żaden wyjątek. OMCs Uruchom na etapie najnowsze, która domyślnie jest `PreHandlerExecute`. Znaczniki etapu są używane do je przeprowadzić wcześniej. Jeśli określisz znaczniki etapu poza kolejnością, firma Microsoft zaokrąglanie do wcześniejszych znacznika. Innymi słowy dodanie znacznika etapu mówi "Run nie później niż etap X". Przebieg OMC firmy w najbliższej znacznika etapu dodane po ich w potoku OWIN.
4. Najwcześniejszym etapie wywołania `app.UseStageMarker` usługi wins. Na przykład, Jeśli przełączasz się kolejność `app.UseStageMarker` wywołania z poprzedniego przykładu:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   W oknie dane wyjściowe będą wyświetlane: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs wszystkie działania w `AuthenticateRequest` etap, ponieważ ostatnie OMC zarejestrowane w usłudze `Authenticate` zdarzenia i `Authenticate` zdarzeń poprzedza inne zdarzenia.
