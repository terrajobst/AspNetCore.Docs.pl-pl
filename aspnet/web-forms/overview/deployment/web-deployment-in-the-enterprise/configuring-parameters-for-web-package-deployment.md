---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurowanie parametrów na potrzeby wdrażania pakietu sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób ustawiania wartości parametrów, takich jak nazwy aplikacji sieci web usług Internet Information Services (IIS), parametry połączenia i punktów końcowych usług...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880404"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurowanie parametrów na potrzeby wdrażania pakietu sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób ustawiania wartości parametrów, takich jak nazwy aplikacji sieci web usług Internet Information Services (IIS), parametry połączenia i punktów końcowych usług, podczas wdrażania pakietu sieci web do zdalnego serwera sieci web usług IIS.


Podczas budowania projektu aplikacji sieci web, kompilacji i proces tworzenia pakietu generuje trzy pliki klucza:

- A *.zip [Nazwa projektu]* pliku. To jest pakiet wdrożeniowy sieci web dla projektu aplikacji sieci web. Ten pakiet zawiera wszystkie zestawy, pliki, skryptów bazy danych i zasobami wymaganymi do odtworzenia aplikacji sieci web na zdalnym serwerze sieci web usług IIS.
- A *.deploy.cmd [Nazwa projektu]* pliku. Zawiera zestaw sparametryzowanych poleceń narzędzia Web Deploy (MSDeploy.exe) publikujących pakietu wdrażania w sieci web do zdalnego serwera sieci web usług IIS.
- A *[Nazwa projektu]. SetParameters.xml* pliku. Zapewnia to zestaw wartości parametrów dla polecenia MSDeploy.exe. Można zaktualizować wartości w tym pliku i przekaż go do narzędzia Web Deploy jako parametru wiersza polecenia podczas wdrażania pakietu sieci web.

> [!NOTE]
> Aby uzyskać więcej informacji o kompilacji i proces tworzenia pakietu, zobacz [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).


*SetParameters.xml* pliku dynamicznie jest generowana z pliku projektu aplikacji sieci web oraz wszelkie pliki konfiguracji w ramach projektu. Podczas tworzenia i pakietu projektu sieci Web potok publikowania (WPP) automatycznie wykrywa wiele zmiennych, które mogą zmienić między środowiskami wdrożenia, takich jak docelowej aplikacji sieci web usług IIS i wszelkie parametry połączenia bazy danych. Te wartości są automatycznie sparametryzowana w pakiecie wdrożeniowym sieci web i dodane do *SetParameters.xml* pliku. Na przykład dodać parametry połączenia do *web.config* plików projektu aplikacji sieci web, proces kompilacji wykryje tę zmianę i doda wpis do *SetParameters.xml* pliku w związku z tym.

W partii przypadków to automatyczne parametryzacja będą wystarczające. Jednak jeśli użytkownicy będą potrzebować różnicującej inne ustawienia między środowiskami wdrożenia, takich jak ustawienia aplikacji lub adresy URL punktu końcowego usługi, należy sprawdzić WPP parametryzacja tych wartości w pakiecie wdrożeniowym, a następnie dodaj odpowiednie pozycje do *SetParameters.xml* pliku. W kolejnych sekcjach wyjaśniono, jak to zrobić.

### <a name="automatic-parameterization"></a>Parametryzacja automatyczne

Podczas kompilacji, a pakiet aplikacji sieci web, WPP zostanie automatycznie parametryzacja następujące czynności:

- Miejsce docelowe usługi IIS sieci web aplikacji ścieżkę i nazwę.
- Parametry połączenia z dowolnym w Twojej *web.config* pliku.
- Parametry połączenia dla żadnych baz danych, należy dodać do **Pakuj/Publikuj SQL** kartę na stronach właściwości projektu.

Na przykład, jeśli masz zamiar kompilacji i pakietu [kontaktów Menedżerze](the-contact-manager-solution.md) to wygenerowanie przykładowe rozwiązanie bez dotykania procesu parametryzacja w jakikolwiek sposób WPP *ContactManager.Mvc.SetParameters.xml* pliku:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


W takim przypadku:

- **Nazwa aplikacji sieci Web usług IIS** parametrem jest ścieżka programu IIS, w której chcesz wdrożyć aplikację sieci web. Wartość domyślna jest pobierana z **pakowaniu/publikowaniu Web** stronę na stronach właściwości projektu.
- **Parametry połączenia w pliku Web.config ApplicationServices** parametr został wygenerowany na podstawie **connectionStrings/Dodaj** element *web.config* pliku. Reprezentuje ciąg połączenia, które powinny być używane do kontaktowania się z bazy danych członkostwa aplikacji. Wartości podane w tym miejscu zostanie zamieniony na wdrożone *web.config* pliku. Wartość domyślna jest pobierana z przed wdrożeniem *web.config* pliku.

WPP również parameterizes tych właściwości w pakiecie wdrożeniowym, który generuje. Po zainstalowaniu pakietu wdrożeniowego, można podać wartości tych właściwości. Jeśli musisz zainstalować pakiet ręcznie za pomocą Menedżera usług IIS, zgodnie z opisem w [ręczne instalowanie pakietów sieci Web](manually-installing-web-packages.md), Kreator instalacji monituje o podanie wartości parametrów. Po zainstalowaniu pakietu zdalnie przy użyciu *. pliku deploy.cmd* pliku, zgodnie z opisem w [wdrażanie pakietów sieci Web](deploying-web-packages.md), narzędzie Web Deploy wygląda na to *SetParameters.xml* pliku Podaj wartości parametrów. Można edytować wartości w *SetParameters.xml* ręcznie pliku lub plik można dostosować w ramach zautomatyzowanego procesu kompilacji i wdrożenia. Ten proces jest opisany bardziej szczegółowo w dalszej części tego tematu.

### <a name="custom-parameterization"></a>Parametryzacja niestandardowych

W bardziej złożonych scenariuszach wdrażania często należy parametryzacja dodatkowe właściwości, przed przystąpieniem do wdrażania projektu. Ogólnie rzecz biorąc powinien parametryzacja żadnych właściwości i ustawienia, które będą się różnić między środowiskami docelowego. Mogą do nich:

- Punkty końcowe w usługi *web.config* pliku.
- Ustawienia aplikacji w *web.config* pliku.
- Inne deklaratywne właściwości, które mają być monitować użytkowników o określić.

Najprostszym sposobem parametryzacja te właściwości jest dodanie *parameters.xml* plik do folderu głównego projektu aplikacji sieci web. Na przykład w rozwiązaniu kontaktów Menedżerze projektu ContactManager.Mvc zawiera *parameters.xml* pliku w folderze głównym.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Po otwarciu tego pliku, zobaczysz, że zawiera on pojedynczy **parametru** wpisu. Wpis używa zapytania XML Path Language (XPath) do lokalizowania i parametryzacja URL punktu końcowego usługi ContactService Windows Communication Foundation (WCF) w *web.config* pliku.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Oprócz parametryzacja URL punktu końcowego w pakiecie wdrożeniowym, WPP dodaje również odpowiedniego wpisu do *SetParameters.xml* pliku, który pobiera wygenerował obok pakietu wdrożeniowego.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Jeśli ręcznie zainstalować pakiet wdrożeniowy Menedżera usług IIS wyświetli monit o adres punktu końcowego usługi obok właściwości, które zostały automatycznie sparametryzowana. Po zainstalowaniu pakietu wdrożeniowego, uruchamiając *. pliku deploy.cmd* plików, można edytować *SetParameters.xml* plik, aby podać wartość adres punktu końcowego usługi wraz z wartości właściwości, które zostały automatycznie sparametryzowana.

Aby uzyskać szczegółowe informacje na temat tworzenia *parameters.xml* plików, zobacz [porady: Korzystanie z parametrów do konfigurowania ustawień podczas pakietu wdrożeniowego zainstalowano](https://msdn.microsoft.com/library/ff398068.aspx). Procedury o nazwie **używania parametrów wdrożenia dla ustawienia pliku Web.config** zawiera instrukcje krok po kroku.

## <a name="modifying-the-setparametersxml-file"></a>Modyfikowanie pliku SetParameters.xml

Jeśli planujesz wdrożyć pakiet aplikacji sieci web ręcznie&#x2014;uruchamiając *. pliku deploy.cmd* pliku lub uruchamiając MSDeploy.exe z wiersza polecenia&#x2014;nie ma nic do edycji ręcznie zatrzymaj  *SetParameters.xml* pliku przed ich wdrożeniem. Jednak podczas pracy na rozwiązanie skali przedsiębiorstwa, należy wdrożyć pakiet aplikacji sieci web w ramach większych i automatyczne procesem kompilacji i wdrażania. W tym scenariuszu należy aparat kompilacji firmy Microsoft (MSBuild), aby zmodyfikować *SetParameters.xml* pliku dla Ciebie. Można to zrobić za pomocą MSBuild **xmlpoke —** zadań.

[Kontaktów Menedżerze przykładowe rozwiązanie](the-contact-manager-solution.md) przedstawiono ten proces. Aby wyświetlić szczegóły, które mają zastosowanie w tym przykładzie zmodyfikowane przykłady kodu, które należy wykonać.

> [!NOTE]
> Szersze omówienie modelu pliku projektu w przykładowe rozwiązanie i wprowadzenie do plików projektów niestandardowych w ogólności, zobacz [opis pliku projektu](understanding-the-project-file.md) i [opis procesu kompilacji](understanding-the-build-process.md).


Najpierw wartości parametrów odsetek są zdefiniowane jako właściwości w pliku projektu określonego środowiska (na przykład *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Aby uzyskać wskazówki dotyczące sposobu dostosowywania pliki projektu określonego środowiska dla środowiska serwera, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Następnie *Publish.proj* pliku Importuje tych właściwości. Ponieważ każdy *SetParameters.xml* plik jest skojarzony z *. pliku deploy.cmd* plików i firma Microsoft ostatecznie mają plik projektu do każdego wywołania *. pliku deploy.cmd* pliku projektu Plik tworzy MSBuild *elementu* dla każdego *. pliku deploy.cmd* plików i definiuje właściwości odsetek jako *metadanych elementu*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


W takim przypadku:

- **ParametersXml** wartości metadanych wskazuje lokalizację *SetParameters.xml* pliku.
- **IisWebAppName** wartość jest ścieżką usług IIS, do której chcesz wdrożyć aplikację sieci web.
- **MembershipDBConnectionString** wartością jest ciąg połączenia dla bazy danych członkostwa i **MembershipDBConnectionName** wartość jest **nazwa** atrybutu odpowiadającego mu parametru w *SetParameters.xml* pliku.
- **ServiceEndpointValue** wartość jest adres punktu końcowego dla usługi WCF na serwerze docelowym i **ServiceEndpointParamName** wartość atrybutu nazwy odpowiadającego mu parametru w *SetParameters.xml* pliku.

Ponadto w *Publish.proj* pliku **PublishWebPackages** docelowa używa **xmlpoke —** zadań, aby zmodyfikować te wartości w *SetParameters.xml* pliku.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Można zauważyć, że każdy **xmlpoke —** zadanie określa cztery wartości atrybutów:

- **XmlInputPath** atrybut informuje zadania, gdzie można znaleźć pliku, który chcesz zmodyfikować.
- **Zapytania** atrybut jest kwerenda XPath, który identyfikuje węzeł XML, aby zmienić.
- **Wartość** atrybut jest nowa wartość ma zostać wstawiony do wybranego węzła XML.
- **Warunku** atrybut jest kryteria, na których zadanie powinny działać lub nie działać. W takich przypadkach warunek gwarantuje, że nie zostanie podjęta próba wstawienia wartości null ani być pusta w *SetParameters.xml* pliku.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano rolę *SetParameters.xml* plików oraz wyjaśniono, jak jest generowany, gdy kompilacji projektu aplikacji sieci web. Wyjaśniono, jak można parametryzacja dodatkowe ustawienia, dodając *parameters.xml* plik do projektu. Również opisano, jak można zmodyfikować *SetParameters.xml* pliku jako część procesu kompilacji większy, automatycznej, za pomocą **xmlpoke —** zadań w plikach projektu.

Następnym temacie [wdrażanie pakietów sieci Web](deploying-web-packages.md), w tym artykule opisano, jak można wdrożyć pakietu sieci web albo uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe polecenia bezpośrednio. W obu przypadkach można określić użytkownika *SetParameters.xml* pliku jako parametr wdrożenia.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacje na temat tworzenia pakietów sieci web, zobacz [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md). Aby uzyskać wskazówki na temat faktycznie wdrażania pakietu sieci web, zobacz [wdrażanie pakietów sieci Web](deploying-web-packages.md). Przewodnik krok po kroku dotyczące sposobu tworzenia *parameters.xml* plików, zobacz [porady: Korzystanie z parametrów do konfigurowania ustawień podczas pakietu wdrożeniowego zainstalowano](https://msdn.microsoft.com/library/ff398068.aspx).

Aby uzyskać więcej ogólnych informacji o parametryzacja w narzędzia Web Deploy, zobacz [parametryzacja wdrażania sieci Web, w akcji](https://go.microsoft.com/?linkid=9805119) (wpis w blogu).

> [!div class="step-by-step"]
> [Poprzednie](building-and-packaging-web-application-projects.md)
> [dalej](deploying-web-packages.md)
