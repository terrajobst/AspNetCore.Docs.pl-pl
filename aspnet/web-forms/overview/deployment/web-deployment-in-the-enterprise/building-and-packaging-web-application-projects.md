---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Kompilowanie i tworzenie pakietów projektów aplikacji sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: Wdrażanie projektu aplikacji sieci web w środowisku serwera zdalnego, należy najpierw jest skompilować projekt i generować pakiety wdrażania sieci web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: ff8312d16dff2a9eec9ae909bca5e72d52f17094
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382611"
---
<a name="building-and-packaging-web-application-projects"></a>Kompilowanie i tworzenie pakietów projektów aplikacji sieci Web
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wdrażanie projektu aplikacji sieci web w środowisku serwera zdalnego, należy najpierw jest skompilować projekt i wygeneruj pakiet wdrażania sieci web. W tym temacie opisano, jak działa proces kompilacji dla projektów aplikacji sieci web. W szczególności opisano:
> 
> - Jak Web potok publikowania (WPP) rozszerza procesu kompilacji, aby uwzględnić funkcji wdrażania.
> - Jak narzędzie Internet Information Services (IIS) Web Deployment (Web Deploy) włącza aplikację sieci web do pakietu wdrożeniowego.
> - Sposobu działania procesu kompilacji i tworzenie pakietów i jakie pliki są tworzone.


W programie Visual Studio 2010 proces kompilowania i wdrażania projektów aplikacji sieci web jest obsługiwana przez potok WPP. Potok WPP zawiera zestaw elementów docelowych aparatu Microsoft Build Engine (MSBuild), które rozszerzają funkcjonalność programu MSBuild i włącz ją zintegrować z narzędzia Web Deploy. W programie Visual Studio zostaną wyświetlone to rozszerzoną funkcjonalność na stronach właściwości projektu aplikacji sieci web. **Pakowaniu/publikowaniu Web** strony wraz z **Pakuj/Publikuj SQL** stronie umożliwia skonfigurowanie sposobu spakowane projektu aplikacji sieci web wdrożenia po zakończeniu procesu kompilacji.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Jak działa potok WPP?

Jeśli zapoznaj się z pliku projektu dla języka C# — projekt aplikacji oparte na sieci web, możesz zobaczyć importuje dwa pliki .targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Pierwszy **importu** instrukcja jest wspólne dla wszystkich projektów języka Visual C#. Ten plik *Microsoft.CSharp.targets*, zawiera elementy docelowe i zadania, które są określone w Visual C#. Na przykład, kompilator C# (**Csc**) zadanie jest wywoływane w tym miejscu. *Microsoft.CSharp.targets* pliku z osobna Importy *Microsoft.Common.targets* pliku. Określa elementy docelowe, które są wspólne dla wszystkich projektów, takie jak **kompilacji**, **odbudować**, **Uruchom**, **skompilować**, i **wyczyść** . Drugi **importu** instrukcja jest specyficzne dla projektów aplikacji sieci web. *Microsoft.WebApplication.targets* pliku z osobna Importy *Microsoft.Web.Publishing.targets* pliku. *Microsoft.Web.Publishing.targets* pliku zasadniczo *jest* potok WPP. Definiuje obiekty docelowe, takie jak **pakietu** i **MSDeployPublish**, który wywoływanie narzędzia Web Deploy wykonywanie różnych zadań wdrażania.

Aby dowiedzieć się, jak są używane te dodatkowe obiekty docelowe, w przykładowym rozwiązaniu Contact Manager Otwórz *Publish.proj* plik i przyjrzyj się **BuildProjects** docelowej.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Ten element docelowy używa **MSBuild** zadań do tworzenia różnych projektów. Zwróć uwagę **DeployOnBuild** i **DeployTarget** właściwości:

- **DeployOnBuild = true** właściwość oznacza "Chcę, aby wykonać dodatkowe docelowej po pomyślnym zakończeniu kompilacji."
- **DeployTarget** właściwość identyfikuje nazwę docelowego ma zostać wykonany, kiedy **DeployOnBuild** właściwości jest równa **true**. W tym przypadku określasz, czy chcesz, aby program MSBuild, aby wykonać **pakietu** docelowej po utworzeniu projektu.

**Pakietu** docelowej jest zdefiniowany w *Microsoft.Web.Publishing.targets* pliku. Zasadniczo ten element docelowy pobiera dane wyjściowe kompilacji projektu aplikacji sieci web i konwertuje go na pakiet wdrożeniowy sieci web, który można opublikować na serwerze sieci web usług IIS.

> [!NOTE]
> Aby wyświetlić plik projektu (na przykład <em>ContactManager.Mvc.csproj</em>) w programie Visual Studio 2010, należy najpierw Cofnij ładowanie projektu z rozwiązania. W <strong>Eksploratora rozwiązań</strong> , kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij przycisk <strong>Zwolnij projekt</strong>. Ponownie kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij przycisk <strong>Edytuj</strong><em>[plik projektu]</em>). Plik projektu zostanie otwarty w nieprzetworzonej postaci XML. Pamiętaj, aby ponownie załadować projekt, gdy wszystko będzie gotowe.  
> Aby uzyskać więcej informacji na temat elementów docelowych MSBuild, zadań, a <strong>importu</strong> instrukcje, zobacz [objaśnienie pliku projektu](understanding-the-project-file.md). Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i potok WPP, zobacz [wewnątrz aparatu Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Co to jest pakiet wdrażania sieci Web?

Podczas tworzenia i wdrażania projektu aplikacji sieci web za pomocą programu Visual Studio 2010 lub bezpośrednio za pomocą programu MSBuild, efekt jest zazwyczaj *pakiet wdrażania sieci web*. Pakiet wdrażania sieci web jest plikiem zip. Zawiera wszystko, który program IIS i narzędzia Web Deploy niezbędna, aby ponownie utworzyć aplikację sieci web w tym:

- Skompilowane dane wyjściowe aplikacji sieci web, w tym zawartość, pliki zasobów, pliki konfiguracji, JavaScript i kaskadowych zasoby (CSS) arkusze stylów i tak dalej.
- Zestawy dla projektu aplikacji sieci web i dla każdego odwołania do projektów w rozwiązaniu.
- Skrypty SQL, aby wygenerować baz danych, które wdrażasz za pomocą aplikacji sieci web.

Wygenerowany pakiet wdrażania sieci web można opublikować ją na serwerze sieci web usług IIS na różne sposoby. Na przykład wdrożyć go zdalnie, przeznaczone dla usługi zdalnego agenta narzędzia Web Deploy lub obsługi wdrażania sieci Web na serwerze docelowym lub można użyć Menedżera usług IIS, aby ręcznie zaimportować pakiet na docelowym serwerze sieci web. Aby uzyskać więcej informacji na temat tych metod wdrażania, zobacz [Wybieranie podejścia prawo do wdrażania w Internecie](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Jak działa proces kompilacji?

Pokazuje to, co się stanie, gdy kompilujesz i pakietu projektu aplikacji sieci web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Podczas tworzenia projektu aplikacji sieci web, proces kompilacji generuje plik o nazwie *[Nazwa projektu]. SourceManifest.xml*. Wraz z pliku projektu i dane wyjściowe kompilacji to *. SourceManifest.xml* plik informuje narzędzie Web Deploy to, czego potrzebuje do uwzględnienia w pakiecie wdrożeniowym sieci web. Korzystając z tych danych wejściowych, narzędzie Web Deploy generuje pakiet wdrażania sieci web o nazwie *zip [Nazwa projektu]*.

Wraz z pakietu wdrożeniowego sieci web proces kompilacji generuje dwa pliki, które mogą pomóc Ci na użycie pakietu:

- *. Pliku deploy.cmd* plik zawiera zestaw sparametryzowanych poleceń narzędzia Web Deploy (MSDeploy.exe), które publikują pakietu wdrażania sieci web do zdalnego serwera sieci web usług IIS. Uruchamianie *. pliku deploy.cmd* pliku z odpowiednimi parametrami zwykle zapewnia szybciej i łatwiej alternatywne do ręcznego tworzenia MSDeploy.exe polecenia samodzielnie.
- *SetParameters.xml* plików zawiera zestaw wartości parametrów do polecenia MSDeploy.exe. Wartości te zawierają właściwości, takie jak nazwa aplikacji sieci web usług IIS, do którego ma zostać wdrożony pakiet wartości wszystkie punkty końcowe usługi i parametrów połączenia jest zdefiniowana w *web.config* pliku i dowolnej właściwości wdrożenia wartości zdefiniowane na stronach właściwości projektu.

*SetParameters.xml* plik ma kluczowe znaczenie dla zarządzania procesu wdrażania. Ten plik jest generowany dynamicznie zgodnie z zawartością projektu aplikacji sieci web. Na przykład dodaj ciąg połączenia usługi *web.config* pliku procesu kompilacji umożliwia automatyczne wykrywanie parametrów połączenia, odpowiednio parametryzacja wdrożenia i utworzyć wpis w  *SetParameters.xml* plików, które umożliwiają modyfikowanie parametrów połączenia w ramach procesu wdrażania. Następny temat [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md), roli tego pliku w bardziej szczegółowo opisano, jak i w tym artykule opisano różne sposoby, w którym można go zmodyfikować podczas kompilowania i wdrażania.

> [!NOTE]
> W programie Visual Studio 2010 potok WPP nie obsługuje prekompilowanie stron w aplikacji sieci web przed pakowania. Następna wersja programu Visual Studio i potok WPP obejmuje możliwość wstępnej kompilacji aplikacji sieci web jako opcja tworzenia pakietów.


## <a name="conclusion"></a>Wniosek

W tym temacie podano omówienie tworzenia i przetwarzania tworzenia pakietów dla projektów aplikacji sieci web w programie Visual Studio 2010. Opisano, jak potok WPP umożliwia wywoływanie narzędzia Web Deploy polecenia programu MSBuild i wyjaśniono, jak działania procesu kompilacji i pakowania w usłudze.

Po utworzeniu pakietu wdrożeniowego sieci web, następnym krokiem jest jej wdrożenie. Aby uzyskać więcej informacji na temat tego, zobacz [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md) i [wdrażanie pakietów internetowych](deploying-web-packages.md).

## <a name="further-reading"></a>Dalsze informacje

Tematy dalej w tym samouczku [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md) i [wdrażanie pakietów internetowych](deploying-web-packages.md), zapewnić wskazówki dotyczące sposobu używania pakietu sieci web został utworzony. Ostatni samouczek z tej serii [zaawansowane wdrażanie w przedsiębiorstwie Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), znajdują się wskazówki dotyczące sposobu dostosowywania i rozwiązywanie problemów z procesem tworzenia pakietów.

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i potok WPP, zobacz [wewnątrz aparatu Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzednie](understanding-the-build-process.md)
> [dalej](configuring-parameters-for-web-package-deployment.md)
