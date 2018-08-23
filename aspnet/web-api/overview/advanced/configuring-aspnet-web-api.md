---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurowanie wzorca ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 81119b35fb375bb2299edc4f08289a89aecfcd88
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754016"
---
<a name="configuring-aspnet-web-api-2"></a>Konfigurowanie wzorca ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W tym temacie opisano sposób konfigurowania interfejsu API sieci Web platformy ASP.NET.

- [Ustawienia konfiguracji](#settings)
- [Konfigurowanie interfejsu API sieci Web przy użyciu hostingu platformy ASP.NET](#webhost)
- [Konfigurowanie interfejsu API sieci Web przy użyciu własnym hostingu OWIN](#selfhost)
- [Globalnie w sieci Web interfejsu API usługi](#services)
- [Konfiguracja dla kontrolera.](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Ustawienia konfiguracji

Ustawienia konfiguracji interfejsu API sieci Web są definiowane w [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) klasy.

| Element członkowski | Opis |
| --- | --- |
| **Klasy DependencyResolver** | Umożliwia wstrzykiwanie zależności dla kontrolerów. Zobacz [za pomocą mechanizmu rozpoznawania zależności internetowy interfejs API](dependency-injection.md). |
| **Filtry** | Filtry akcji. |
| **Programy formatujące** | [Programy formatujące typy nośnika](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Określa, czy serwer powinien zawierać szczegóły błędu, takie jak komunikaty o wyjątkach i ślady stosu, w komunikatach odpowiedzi HTTP. Zobacz [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicjator** | Funkcja, która wykonuje końcowe inicjowanie **HttpConfiguration**. |
| **MessageHandlers** | [Programy obsługi komunikatów HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Kolekcja reguł dla wiązania parametrów na akcji kontrolera. |
| **Właściwości** | Zbiór właściwości ogólnych. |
| **Trasy** | Kolekcja tras. Zobacz [routingu we wzorcu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Usługi** | Kolekcja usług. Zobacz [usług](#services). |


## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurowanie interfejsu API sieci Web przy użyciu hostingu platformy ASP.NET

W aplikacji ASP.NET należy skonfigurować interfejs API sieci Web, wywołując [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) w **aplikacji\_Start** metody. **Konfiguruj** metoda przyjmuje delegata z pojedynczym parametrem typu **HttpConfiguration**. Wykonaj wszystkie swoje kod wewnątrz obiektu delegowanego.

Oto przykład korzystający z delegata anonimowego:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

W programie Visual Studio 2017 szablonu projektu "Aplikacja sieci Web programu ASP.NET" automatycznie konfiguruje kodu konfiguracji, jeśli wybierzesz "Interfejs API sieci Web" w **nowy projekt ASP.NET** okna dialogowego.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Szablon projektu tworzy plik o nazwie WebApiConfig.cs wewnątrz aplikacji\_folder początkowy. Ten plik kodu definiuje delegata, w którym należy umieścić kod Konfiguracja interfejsu API sieci Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Szablon projektu dodaje również kod, który wywołuje delegata z **aplikacji\_Start**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurowanie interfejsu API sieci Web przy użyciu własnym hostingu OWIN

Jeśli masz własny hostingu z oprogramowaniem OWIN, Utwórz nową **HttpConfiguration** wystąpienia. Wykonywać żadnej konfiguracji dla tego wystąpienia, a następnie przekaż wystąpienie do **Owin.UseWebApi** — metoda rozszerzenia.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Samouczek [Użyj OWIN do Self-Host wzorca ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) pokazuje wszystkie czynności.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globalnie w sieci Web interfejsu API usługi

**HttpConfiguration.Services** kolekcja zawiera zbiór usług globalnych, które używa interfejsu API sieci Web w celu wykonywania różnych zadań, takich jak kontroler wybór i negocjującej zawartość.

> [!NOTE]
> **Usług** Kolekcja nie jest mechanizm ogólnego przeznaczenia iniekcji odnajdywania lub zależności usługi. Przechowuje tylko typy usług, które są znane w ramach interfejsu API sieci Web.


**Usług** kolekcji jest inicjowany z domyślny zestaw usług i możesz podać własne niestandardowe implementacje. Wiele wystąpień jest obsługiwane w pewnych usługach, podczas gdy inne osoby mogą mieć tylko jedno wystąpienie. (Jednak możesz również podać usług na poziomie kontrolera; zobacz [konfiguracji poszczególnych kontrolerów](#percontrollerconfig).

Jednym wystąpieniu usługi


| Usługa | Opis |
| --- | --- |
| **IActionValueBinder** | Pobiera wiązanie parametru. |
| **IApiExplorer** | Pobiera opisy interfejsów API ujawnianych przez aplikację. Zobacz [Tworzenie strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Pobiera listę zestawów dla aplikacji. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Weryfikuje model, który jest odczytywany z treści żądania przez element formatujący typu nośnika. |
| **IContentNegotiator** | Przeprowadza negocjowanie zawartości. |
| **IDocumentationProvider** | Zawiera dokumentację dotyczącą interfejsów API. Wartość domyślna to **null**. Zobacz [Tworzenie strony pomocy dla interfejsu API sieci Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Wskazuje, czy host powinien buforować treść jednostki wiadomości HTTP. |
| **IHttpActionInvoker** | Wywołuje akcję kontrolera. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Wybiera akcji kontrolera. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktywuje kontrolera. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Wybiera kontroler. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Zawiera listę typów kontroler internetowego interfejsu API w aplikacji. Zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicjuje struktury śledzenia. Zobacz [śledzenie we wzorcu ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Udostępnia moduł zapisujący śledzenia. Wartość domyślna to moduł zapisujący śledzenia "pusta". Zobacz [śledzenie we wzorcu ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Udostępnia pamięć podręczną modułów weryfikacji modelu. |

Usługi z wieloma wystąpieniami


|                 Usługa                 |                                                                                                              Opis                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Zwraca listę filtrów dla akcji kontrolera.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Zwraca integratora modelu dla danego typu.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Udostępnia metadane dla modelu.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Udostępnia moduł weryfikacji dla modelu.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Tworzy dostawcę wartości. Aby uzyskać więcej informacji, zobacz wpis w blogu wstrzymania Mike [jak utworzyć dostawcę wartości niestandardowe w WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Aby dodać implementację niestandardową do wielu wystąpień usługi, należy wywołać **Dodaj** lub **Wstaw** na **usług** kolekcji:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Aby zastąpić jednego wystąpienia usługi implementację niestandardową, należy wywołać **Zastąp** na **usług** kolekcji:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfiguracja dla kontrolera.

Można zastąpić następujące ustawienia na zasadzie na kontrolerze:

- Programy formatujące typy nośnika
- Parametr zasad powiązania
- Usługi

Aby to zrobić, należy zdefiniować niestandardowy atrybut, który implementuje **IControllerConfiguration** interfejsu. Następnie należy zastosować atrybut do kontrolera.

Poniższy przykład zastępuje elementy formatujące domyślny typ nośnika elementu formatującego niestandardowych.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

**IControllerConfiguration.Initialize** metoda przyjmuje dwa parametry:

- **HttpControllerSettings** obiektu
- **HttpControllerDescriptor** obiektu

**HttpControllerDescriptor** zawiera opis kontrolera, w którym można sprawdzić w celach informacyjnych (na przykład aby rozróżnić dwa kontrolery).

Użyj **HttpControllerSettings** obiektu, aby skonfigurować kontroler. Ten obiekt zawiera podzbiór parametry konfiguracji, które można zastąpić na podstawie poszczególnych kontrolerów. Wszystkie ustawienia, które nie ulegają zmianom, domyślnie globalnego **HttpConfiguration** obiektu.
