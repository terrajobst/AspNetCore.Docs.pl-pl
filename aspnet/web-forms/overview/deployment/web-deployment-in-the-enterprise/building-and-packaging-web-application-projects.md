---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Skompilowanie i utworzenie pakietu projekty aplikacji sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: "Aby wdrożyć projekt aplikacji sieci web w środowisku serwera zdalnego, należy najpierw jest aby skompilować projekt i generowanie pakiety wdrażania sieci web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: c05f725c9e6b493a6af8f5b5d20dbc9ff73a1ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="building-and-packaging-web-application-projects"></a>Skompilowanie i utworzenie pakietu projekty aplikacji sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Aby wdrożyć projekt aplikacji sieci web w środowisku serwera zdalnego, należy najpierw jest skompilować projekt i generowanie pakietu wdrożeniowego sieci web. W tym temacie opisano, jak działa proces kompilacji projektów aplikacji sieci web. W szczególności wyjaśniono:
> 
> - Jak Web potok publikowania (WPP) rozszerza proces kompilacji, w tym funkcji wdrażania.
> - Jak usługi Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy) włącza aplikację sieci web do pakietu wdrożeniowego.
> - Sposobu działania procesu kompilacji i tworzenia pakietów i które pliki są tworzone.


W programie Visual Studio 2010 procesem kompilacji i wdrażania projektów aplikacji sieci web jest obsługiwana przez WPP. WPP zawiera zestaw elementów docelowych Microsoft kompilacji Engine (MSBuild), które zapewniają rozszerzenie funkcjonalności programu MSBuild i włącz ją zintegrować z narzędzia Web Deploy. W programie Visual Studio zostaną wyświetlone ta rozszerzona funkcjonalność na stronach właściwości projektu aplikacji sieci web. **Pakowaniu/publikowaniu Web** strony wraz z **Pakuj/Publikuj SQL** strona umożliwia skonfigurowanie, jak jest dostarczana projektu aplikacji sieci web wdrożenia po zakończeniu procesu kompilacji.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Jak działa WPP?

Jeśli użytkownik Spójrz na pliku projektu dla C# — projekt aplikacji opartych na sieci web, widać, że importuje dwa pliki .targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Pierwszy **importu** instrukcja jest wspólny dla wszystkich projektów Visual C#. Ten plik *Microsoft.CSharp.targets*, zawiera elementy docelowe i zadania, które są określone na Visual C#. Na przykład do kompilatora C# (**Csc**) zadanie jest wywoływane w tym miejscu. *Microsoft.CSharp.targets* pliku z kolei importów *Microsoft.Common.targets* pliku. Określa elementy docelowe, które są wspólne dla wszystkich projektów, takie jak **kompilacji**, **odbudować**, **Uruchom**, **skompilować**, i **Oczyść** . Drugi **importu** instrukcja jest specyficzne dla projektów aplikacji sieci web. *Microsoft.WebApplication.targets* pliku z kolei importów *Microsoft.Web.Publishing.targets* pliku. *Microsoft.Web.Publishing.targets* plików zasadniczo *jest* WPP. Określa elementy docelowe, tak samo, jak **pakietu** i **MSDeployPublish**, który wywoływanie narzędzia Web Deploy wykonywanie różnych zadań wdrażania.

Aby zrozumieć, jak te dodatkowe elementy docelowe używają, skontaktuj się z Menedżera przykładowe rozwiązanie Otwórz *Publish.proj* plików i Przyjrzyjmy się **BuildProjects** docelowej.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Ten element docelowy używa **MSBuild** zadań do tworzenia różnych projektów. Powiadomienie **DeployOnBuild** i **DeployTarget** właściwości:

- **DeployOnBuild = true** właściwości zasadniczo oznacza "Chcę, aby wykonać dodatkowe docelowej, gdy kompilacja zakończy się pomyślnie."
- **DeployTarget** właściwości identyfikuje nazwę elementu docelowego mają do wykonania, gdy **DeployOnBuild** właściwości jest równa **true**. W takim przypadku jest określenie, które mają MSBuild, aby wykonać **pakietu** docelowej po skompilowaniu projektu.

**Pakietu** docelowy jest zdefiniowany w *Microsoft.Web.Publishing.targets* pliku. Zasadniczo ten element docelowy pobiera dane wyjściowe kompilacji projektu aplikacji sieci web i konwertuje go na pakiet wdrożeniowy sieci web, który można opublikować na serwerze sieci web usług IIS.

> [!NOTE]
> Aby wyświetlić plik projektu (na przykład *ContactManager.Mvc.csproj*) w programie Visual Studio 2010, należy najpierw wyładować projektu z rozwiązania. W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij przycisk **Zwolnij projekt**. Ponownie kliknij prawym przyciskiem myszy węzeł projektu, a następnie kliknij przycisk **Edytuj***[plik projektu]*). Plik projektu zostanie otwarty w postaci XML raw. Pamiętaj, aby ponownie załadować projekt, gdy wszystko będzie gotowe.  
> Aby uzyskać więcej informacji na docelowych elementów MSBuild, zadania i **importu** instrukcje, zobacz [opis pliku projektu](understanding-the-project-file.md). Aby uzyskać więcej informacji na temat wprowadzenie do plików projektu i WPP, zobacz [wewnątrz kompilacji aparatu Microsoft: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i Bartholomew łączy, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Co to jest pakiet wdrożeniowy sieci Web?

Podczas tworzenia i wdrażania projektu aplikacji sieci web za pomocą programu Visual Studio 2010 lub za pomocą MSBuild bezpośrednio, wynik końcowy jest zwykle *pakietu wdrożeniowego sieci web*. Pakiet wdrożeniowy sieci web jest plik zip. Zawiera wszystko, co który program IIS i Web Deploy wymagają, aby ponownie utworzyć aplikację sieci web w tym:

- Dane wyjściowe skompilowanej aplikacji sieci web, w tym zawartości, pliki zasobów, pliki konfiguracji, JavaScript i kaskadowych styl zasobów arkuszy (CSS) i tak dalej.
- Zestawy dla projektu aplikacji sieci web i dla każdego odwołania do projektów w rozwiązaniu.
- Skrypty SQL, aby wygenerować żadnych baz danych, które jest wdrażany z aplikacji sieci web.

Wygenerowany pakiet wdrożeniowy sieci web można opublikować na serwerze sieci web usług IIS na różne sposoby. Na przykład można wdrożyć w zdalnie przez usługę sieci Web wdrażanie agenta zdalnego lub obsługi wdrażania sieci Web na serwerze docelowym lub można użyć Menedżera usług IIS, można ręcznie zaimportować pakiet na docelowym serwerze sieci web. Aby uzyskać więcej informacji na tych metod wdrażania, zobacz [Wybieranie podejście prawo do wdrożenia w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Jak działa proces kompilacji?

Oznacza to, co się stanie po kompilacji i pakietu projektu aplikacji sieci web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Podczas kompilowania projektu aplikacji sieci web, proces kompilacji generuje plik o nazwie *[Nazwa projektu]. SourceManifest.xml*. Wraz z pliku projektu i danych wyjściowych kompilacji to *. SourceManifest.xml* pliku informuje narzędzie Web Deploy potrzebne do uwzględnienia w pakiecie wdrożeniowym sieci web. Przy użyciu tych danych wejściowych, narzędzie Web Deploy generuje pakietu wdrożeniowego sieci web o nazwie *.zip [Nazwa projektu]*.

Obok pakietu wdrożeniowego sieci web proces kompilacji generuje dwa pliki, które mogą pomóc w za pomocą pakietu:

- *. Pliku deploy.cmd* plik zawiera zestaw sparametryzowanych poleceń narzędzia Web Deploy (MSDeploy.exe) publikujących pakietu wdrażania w sieci web do zdalnego serwera sieci web usług IIS. Uruchomiona *. pliku deploy.cmd* pliku z odpowiednimi parametrami zwykle zapewnia szybciej i łatwiej alternatywnych do konstruowania ręcznie MSDeploy.exe polecenia samodzielnie.
- *SetParameters.xml* plików zawiera zestaw wartości parametrów dla polecenia MSDeploy.exe. Te wartości zawierają właściwości, takie jak nazwa aplikacji sieci web usług IIS, do której chcesz wdrożyć pakiet wartości żadnych punktów końcowych usługi i parametry połączenia są zdefiniowane w *web.config* pliku i dowolnej właściwości wdrożenia wartości określone na stronach właściwości projektu.

*SetParameters.xml* plik ma kluczowe znaczenie dla zarządzania procesu wdrażania. Ten plik jest generowany dynamicznie zgodnie z zawartością projektu aplikacji sieci web. Na przykład dodać parametry połączenia do Twojej *web.config* pliku, proces kompilacji automatycznie wykryje parametry połączenia, w związku z tym parametryzacja wdrożenia i utworzyć wpis w  *SetParameters.xml* pliku pozwala zmodyfikować parametrów połączenia w ramach procesu wdrażania. Następnym temacie [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md), roli tego pliku bardziej szczegółowo opisano, jak i opisano różne sposoby, w którym można go zmodyfikować podczas kompilacji i wdrożenia.

> [!NOTE]
> W programie Visual Studio 2010 WPP nie obsługuje prekompilowanie stron w aplikacji sieci web przed opakowania. Na następną wersję programu Visual Studio i WPP obejmuje możliwość wstępnej kompilacji aplikacji sieci web jako opcja tworzenia pakietów.


## <a name="conclusion"></a>Wniosek

W tym temacie podano omówienie kompilacji i proces tworzenia pakietu dla projektów aplikacji sieci web w programie Visual Studio 2010. Opisano, jak WPP umożliwia wywoływanie narzędzia Web Deploy polecenia programu MSBuild, i wyjaśniono, jak działania procesu kompilacji i tworzenia pakietów.

Po utworzeniu pakietu wdrożeniowego sieci web, następnym krokiem jest jej wdrożenia. Aby uzyskać więcej informacji o tym, zobacz [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md) i [wdrażanie pakietów sieci Web](deploying-web-packages.md).

## <a name="further-reading"></a>Dalsze informacje

Tematy dalej w tym samouczku [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md) i [wdrażanie pakietów sieci Web](deploying-web-packages.md), zawierają wskazówki dotyczące sposobu używania po utworzeniu pakietu sieci web. Końcowe samouczek w tej serii [zaawansowanego wdrożenia w przedsiębiorstwie Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), znajdują się wskazówki dotyczące sposobu dostosowywania i rozwiązywanie problemów z proces tworzenia pakietu.

Aby uzyskać więcej informacji na temat wprowadzenie do plików projektu i WPP, zobacz [wewnątrz kompilacji aparatu Microsoft: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i Bartholomew łączy, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Poprzednie](understanding-the-build-process.md)
[dalej](configuring-parameters-for-web-package-deployment.md)
