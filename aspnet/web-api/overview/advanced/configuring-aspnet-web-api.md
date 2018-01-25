---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Konfigurowanie składnika ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurowanie składnika ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W tym temacie opisano sposób konfigurowania interfejsu API sieci Web platformy ASP.NET.

- [Ustawienia konfiguracji](#settings)
- [Konfigurowanie witryny sieci Web interfejsu API z hostingu ASP.NET](#webhost)
- [Konfigurowanie witryny sieci Web interfejsu API z własnym hostingu OWIN](#selfhost)
- [Usługi interfejsu API sieci Web globalne](#services)
- [Konfiguracja dla kontrolera.](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Ustawienia konfiguracji

Ustawienia konfiguracji interfejsu API sieci Web są zdefiniowane w [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) klasy.

| Element członkowski | Opis |
| --- | --- |
| **Klasy DependencyResolver** | Umożliwia iniekcji zależności dla kontrolerów. Zobacz [za pomocą mechanizmu rozpoznawania zależności interfejsu API sieci Web](dependency-injection.md). |
| **Filtry** | Filtry akcji. |
| **Elementy formatujące** | [Programy formatujące typy nośnika](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Określa, czy serwer ma uwzględniać szczegóły błędu, takie jak komunikaty wyjątku i ślady stosu, w komunikatów odpowiedzi HTTP. Zobacz [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicjator** | Funkcja, która wykonuje końcowego inicjowania elementu **HttpConfiguration**. |
| **MessageHandlers** | [Programy obsługi komunikatów HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Kolekcja reguł dla wiązania parametrów na akcji kontrolera. |
| **Właściwości** | Zbiór właściwości ogólnych. |
| **Trasy** | Kolekcja tras. Zobacz [routingu w składniku ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Usługi** | Kolekcja usług. Zobacz [usług](#services). |


## <a name="prerequisites"></a>Wymagania wstępne

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurowanie witryny sieci Web interfejsu API z hostingu ASP.NET

W aplikacji ASP.NET, należy skonfigurować interfejs API sieci Web przez wywołanie metody [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) w **aplikacji\_Start** metody. **Konfiguruj** metoda przyjmuje delegata z pojedynczym parametrem typu **HttpConfiguration**. Wykonaj wszystkie Twoje konfiguracyjny wewnątrz obiektu delegowanego.

Oto przykład przy użyciu anonimowego delegata:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

W programie Visual Studio 2017 r, szablon projektu "Aplikacja sieci Web ASP.NET" automatycznie konfiguruje kod konfiguracji po wybraniu "Interfejsu API sieci Web" w **nowy projekt ASP.NET** okna dialogowego.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Szablon projektu tworzy plik o nazwie WebApiConfig.cs wewnątrz aplikacji\_folder początkowy. Ten plik kodu definiuje delegata, w którym należy umieścić kodu konfiguracji interfejsu API sieci Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Szablon projektu również dodaje kod, który wywołuje delegata z **aplikacji\_Start**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurowanie witryny sieci Web interfejsu API z własnym hostingu OWIN

Jeśli jesteś własnym hostingu z oprogramowaniem OWIN, Utwórz nową **HttpConfiguration** wystąpienia. Wykonywać żadnej konfiguracji dla tego wystąpienia, a następnie przekazać wystąpienia **Owin.UseWebApi** — metoda rozszerzenia.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Samouczek [OWIN Użyj Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) zawiera wszystkie czynności.

<a id="services"></a>
## <a name="global-web-api-services"></a>Usługi interfejsu API sieci Web globalne

**HttpConfiguration.Services** kolekcja zawiera zbiór usług globalnych, które używa interfejsu API sieci Web do wykonywania różnych zadań, takich jak kontroler wybór i negocjującej zawartość.

> [!NOTE]
> **Usług** kolekcji nie jest mechanizm ogólnego przeznaczenia iniekcji odnajdowania lub zależność usługi. Przechowywane są tylko typy usług, które są znane w ramach interfejsu API sieci Web.


**Usług** kolekcja jest zainicjowana za pomocą domyślnego zestawu usług i możesz podać własne implementacji niestandardowych. Niektóre usługi obsługuje wiele wystąpień, podczas gdy inne osoby mogą mieć tylko jedno wystąpienie. (Jednak można też podać usługi na poziomie kontrolera; zobacz [konfiguracji poszczególnych kontrolerów](#percontrollerconfig).

Usługi w jednym wystąpieniu


| Usługa | Opis |
| --- | --- |
| **IActionValueBinder** | Pobiera wiązanie parametru. |
| **IApiExplorer** | Pobiera opisy interfejsów API udostępniany przez aplikację. Zobacz [tworzenia strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Pobiera listę zestawów dla aplikacji. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Weryfikuje model, który będzie odczytywać treść żądania przez element formatujący typu nośnika. |
| **IContentNegotiator** | Przeprowadza negocjowanie zawartości. |
| **IDocumentationProvider** | Zawiera dokumentacja interfejsów API. Wartość domyślna to **null**. Zobacz [tworzenia strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Wskazuje, czy host powinien buforować treść jednostki wiadomości HTTP. |
| **IHttpActionInvoker** | Wywołuje akcji kontrolera. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Wybiera akcji kontrolera. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktywuje kontrolera. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Wybiera kontrolera. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Zawiera listę typów kontrolera interfejsu API sieci Web w aplikacji. Zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicjuje struktury śledzenia. Zobacz [śledzenie w składniku ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Udostępnia moduł zapisujący śledzenia. Wartość domyślna to moduł zapisujący śledzenia "pusta". Zobacz [śledzenie w składniku ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Udostępnia pamięć podręczną z modułów weryfikacji modelu. |

Wiele wystąpień usług


| Usługa | Opis |
| --- | --- |
| **IFilterProvider** | Zwraca listę filtrów dla akcji kontrolera. |
| **ModelBinderProvider** | Zwraca integratora modelu dla danego typu. |
| **ModelMetadataProvider** | Udostępnia metadane dla modelu. |
| **ModelValidatorProvider** | Udostępnia moduł weryfikacji dla modelu. |
| **ValueProviderFactory** | Tworzy dostawcę wartości. Aby uzyskać więcej informacji, zobacz wstrzymania Jan blogu [jak utworzyć dostawcę wartości niestandardowe w WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |.

Aby dodać usługę wielowystąpieniowy implementacja niestandardowa, należy wywołać **Dodaj** lub **Wstaw** na **usług** kolekcji:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Aby zastąpić jednego wystąpienia usługi implementacja niestandardowa, należy wywołać **Zastąp** na **usług** kolekcji:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfiguracja dla kontrolera.

Można zastąpić następujące ustawienia na podstawie na kontrolerze:

- Programy formatujące typy nośnika
- Parametr powiązanie reguły
- Usługi

Aby to zrobić, należy zdefiniować atrybutu niestandardowego, który implementuje **IControllerConfiguration** interfejsu. Następnie zastosuj atrybut do kontrolera.

Poniższy przykład zastępuje elementy formatujące domyślny typ nośnika elementu formatującego niestandardowych.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** metoda przyjmuje dwa parametry:

- **HttpControllerSettings** obiektu
- **HttpControllerDescriptor** obiektu

**HttpControllerDescriptor** zawiera opis kontrolera, który można sprawdzić w celach informacyjnych (powiedzieć, aby odróżnić dwa kontrolery).

Użyj **HttpControllerSettings** obiekt, aby skonfigurować kontroler. Ten obiekt zawiera podzbiór parametry konfiguracji, które można zastąpić na podstawie poszczególnych kontrolerów. Wszystkie ustawienia, które nie zmieniają domyślnie globalnej **HttpConfiguration** obiektu.
