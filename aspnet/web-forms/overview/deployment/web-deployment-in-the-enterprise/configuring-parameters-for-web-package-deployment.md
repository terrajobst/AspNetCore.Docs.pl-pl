---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób ustawiania wartości parametrów, takich jak nazwy aplikacji sieci web usług Internet Information Services (IIS), parametry połączenia i punkty końcowe usługi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386552"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób ustawiania wartości parametrów, takich jak nazwy aplikacji sieci web usług Internet Information Services (IIS), parametry połączenia i punkty końcowe usługi podczas wdrażania pakietu sieci web do zdalnego serwera sieci web usług IIS.


Podczas kompilowania projektu aplikacji sieci web, kompilacji i proces pakowania generuje trzy pliki klucza:

- A *zip [Nazwa projektu]* pliku. To jest pakiet wdrażania sieci web dla projektu aplikacji sieci web. Ten pakiet zawiera wszystkie zestawy, pliki, skrypty bazy danych i zasobów potrzebnych do ponowne utworzenie aplikacji sieci web, na zdalnym serwerze sieci web usług IIS.
- A *.deploy.cmd [Nazwa projektu]* pliku. Zawiera zestaw sparametryzowanych poleceń narzędzia Web Deploy (MSDeploy.exe), które publikują pakietu wdrażania sieci web do zdalnego serwera sieci web usług IIS.
- A *[Nazwa projektu]. SetParameters.xml* pliku. Zapewnia to zbiór wartości parametrów do polecenia MSDeploy.exe. Można zaktualizować wartości w tym pliku i przekaż go do narzędzia Web Deploy jako parametr wiersza polecenia podczas wdrażania pakietu sieci web.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia i przetwarzania pakietu, zobacz [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).


*SetParameters.xml* pliku jest generowana dynamicznie z pliku projektu aplikacji sieci web i wszystkie pliki konfiguracji w obrębie projektu. Podczas kompilowania i pakietów projektu sieci Web potok publikowania (WPP) automatycznie wykrywa wiele zmiennych, które mogą się zmieniać między środowiskach wdrożeniowych, takich jak docelowej aplikacji sieci web usług IIS i wszelkie parametry połączenia bazy danych. Te wartości są automatycznie sparametryzowany w pakiecie wdrożeniowym sieci web i dodawane do *SetParameters.xml* pliku. Na przykład dodaj ciąg połączenia *web.config* pliku w projekcie aplikacji sieci web, proces kompilacji wykryje tę zmianę i doda wpis do *SetParameters.xml* pliku w związku z tym.

W wielu przypadkach to automatyczne parametryzacji okażą się wystarczające. Jednak jeśli użytkownicy muszą się różnić w innych ustawień między środowiskami wdrożenia, takich jak ustawienia aplikacji lub adresy URL punktu końcowego usługi, należy stwierdzić WPP parametryzacja te wartości w pakiecie wdrożeniowym i dodać odpowiednie pozycje do *SetParameters.xml* pliku. W kolejnych sekcjach wyjaśniono, jak to zrobić.

### <a name="automatic-parameterization"></a>Parametryzacja automatyczne

Gdy kompilujesz i pakietów aplikacji sieci web, aby potok WPP automatycznie zdefiniować te rzeczy parametry:

- Miejsce docelowe IIS sieci web, aplikacji ścieżkę i nazwę.
- Każde połączenie ciągów w swojej *web.config* pliku.
- Parametry połączenia dla dowolnej bazy danych, możesz dodać do **Pakuj/Publikuj SQL** kartę na stronach właściwości projektu.

Na przykład, jeśli utworzysz i spakujesz [Contact Manager](the-contact-manager-solution.md) przykładowe rozwiązanie bez dotykania procesu parametryzacji w jakikolwiek sposób potok WPP wygeneruje to *ContactManager.Mvc.SetParameters.xml* pliku:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


W takim przypadku:

- **Nazwa aplikacji sieci Web usług IIS** parametrem jest ścieżka IIS, w której chcesz wdrożyć aplikację sieci web. Wartością domyślną jest pobierana z **pakowaniu/publikowaniu Web** strony na stronach właściwości projektu.
- **Parametry połączenia w pliku Web.config ApplicationServices** parametr został wygenerowany z **connectionStrings/Dodaj** element *web.config* pliku. Reprezentuje parametry połączenia, które aplikacja powinna używać do kontaktowania się z bazy danych członkostwa. Wartości podane w tym miejscu zostanie wprowadzony do wdrożonych *web.config* pliku. Wartością domyślną jest pobierana z przed wdrożeniem *web.config* pliku.

Potok WPP również parametryzuje dane tych właściwości w pakiecie wdrożeniowym, który generuje. Po zainstalowaniu pakietu wdrożeniowego, możesz podać wartości tych właściwości. Jeśli zainstalujesz pakiet ręcznie za pomocą Menedżera usług IIS, zgodnie z opisem w [ręczne instalowanie pakietów internetowych](manually-installing-web-packages.md), Kreator instalacji monituje o podanie wartości parametrów. Po zainstalowaniu pakietu zdalnie przy użyciu *. pliku deploy.cmd* pliku, zgodnie z opisem w [wdrażanie pakietów internetowych](deploying-web-packages.md), narzędzie Web Deploy będzie wyglądać tak *SetParameters.xml* plik Podaj wartości parametrów. Można edytować wartości w *SetParameters.xml* pliku ręcznie lub można dostosować plik w ramach zautomatyzowanego procesu kompilacji i wdrażania. Ten proces jest opisany bardziej szczegółowo w dalszej części tego tematu.

### <a name="custom-parameterization"></a>Parametryzacja niestandardowe

W bardziej złożonych scenariuszy wdrażania często warto zdefiniować parametry dodatkowe właściwości, przed przystąpieniem do wdrażania projektu. Ogólnie rzecz biorąc należy parametryzuj wszystkie właściwości i ustawienia, które będą się różnić od środowiska docelowego. Mogą do nich:

- Punkty końcowe w usługi *web.config* pliku.
- Ustawienia aplikacji w *web.config* pliku.
- Wszelkie deklaratywne właściwości, które mają być monitować użytkowników, aby określić.

Najprostszym sposobem parametryzacja tych właściwości jest dodanie *parameters.xml* pliku w folderze głównym projektu aplikacji sieci web. Na przykład w przypadku rozwiązania Contact Manager projektu ContactManager.Mvc zawiera *parameters.xml* pliku w folderze głównym.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Po otwarciu tego pliku, zobaczysz, że zawiera on pojedynczy **parametru** wpisu. Zapis używa zapytania języka ścieżki XML (XPath) do lokalizowania i parametryzacja URL punktu końcowego usługi ContactService Windows Communication Foundation (WCF) w *web.config* pliku.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Oprócz parametryzacja adres URL punktu końcowego w pakiecie wdrożeniowym, potok WPP dodaje również odpowiadający mu wpis do *SetParameters.xml* pliku, który pobiera wygenerowany wraz z pakietu wdrożeniowego.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Jeśli ręcznie zainstalować pakiet wdrożeniowy Menedżera usług IIS wyświetli monit o adres punktu końcowego usługi obok właściwości, które zostały automatycznie sparametryzowany. Po zainstalowaniu pakietu wdrożeniowego, uruchamiając *. pliku deploy.cmd* pliku, można edytować *SetParameters.xml* plik, aby podać wartość dla adresu punktu końcowego usługi wraz z wartości właściwości, które zostały automatycznie sparametryzowany.

Aby uzyskać szczegółowe informacje na temat tworzenia *parameters.xml* plików, zobacz [porady: użycie parametrów do konfigurowania ustawień podczas pakietu wdrożeniowego jest zainstalowany](https://msdn.microsoft.com/library/ff398068.aspx). Procedurę o nazwie **do parametrów wdrożenia na użytek ustawienia pliku Web.config** zawiera instrukcje krok po kroku.

## <a name="modifying-the-setparametersxml-file"></a>Modyfikowanie pliku SetParameters.xml

Jeśli planujesz wdrożyć pakiet aplikacji sieci web ręcznie&#x2014;albo uruchamiając *. pliku deploy.cmd* pliku lub uruchamiając MSDeploy.exe z wiersza polecenia&#x2014;nie ma nic do zatrzymuje ręczna Edycja  *SetParameters.xml* plików przed ich wdrożeniem. Jednak jeśli pracujesz na rozwiązanie skali korporacyjnej, konieczne może być wdrożyć pakiet aplikacji sieci web w ramach procesu większych i automatyczne kompilowanie i wdrażanie. W tym scenariuszu należy aparatu Microsoft Build Engine (MSBuild), aby zmodyfikować *SetParameters.xml* plik. Można to zrobić przy użyciu programu MSBuild **xmlpoke —** zadania.

[Contact Manager przykładowe rozwiązanie](the-contact-manager-solution.md) ilustruje ten proces. Aby wyświetlić jego szczegóły, które mają zastosowanie w tym przykładzie były edytowane przykłady kodu, które należy wykonać.

> [!NOTE]
> Omówienie szerszej model pliku projektu w przykładowym rozwiązaniu i zapoznać się z wprowadzeniem do projektu niestandardowych plików ogólnie rzecz biorąc, zobacz [objaśnienie pliku projektu](understanding-the-project-file.md) i [objaśnienie procesu kompilacji](understanding-the-build-process.md).


Najpierw wartości parametrów zainteresowania są definiowane jako właściwość w pliku projektu specyficznego dla środowiska (na przykład *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania pliki projektu specyficznego dla środowiska dla środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Następnie *Publish.proj* plik importuje te właściwości. Ponieważ każdy *SetParameters.xml* plik jest skojarzony z *. pliku deploy.cmd* pliku, a firma Microsoft ma ostatecznie plik projektu do każdego wywołania *. pliku deploy.cmd* pliku projektu Plik tworzy MSBuild *elementu* dla każdego *. pliku deploy.cmd* pliku i definiuje właściwości zainteresowania jako *metadanych elementu*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


W takim przypadku:

- **ParametersXml** wartości metadanych wskazuje lokalizację *SetParameters.xml* pliku.
- **IisWebAppName** wartość jest ścieżką usług IIS, do której chcesz wdrożyć aplikację sieci web.
- **MembershipDBConnectionString** wartość parametrów połączenia dla bazy danych członkostwa i **MembershipDBConnectionName** wartość **nazwa** atrybutu z odpowiadającym mu parametrem w *SetParameters.xml* pliku.
- **ServiceEndpointValue** wartość adresu punktu końcowego dla usługi WCF na serwerze docelowym i **ServiceEndpointParamName** wartość atrybutu name w odpowiadającym mu parametrem w *SetParameters.xml* pliku.

Na koniec w *Publish.proj* pliku **PublishWebPackages** docelowy używa **xmlpoke —** zadanie, aby zmodyfikować te wartości w *SetParameters.xml* pliku.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Należy zauważyć, że każdy **xmlpoke —** zadanie określa cztery wartości atrybutów:

- **XmlInputPath** atrybutu o tym zadania, gdzie można znaleźć pliku, który chcesz zmodyfikować.
- **Zapytania** atrybut jest kwerenda XPath, który identyfikuje węzeł XML, aby zmienić.
- **Wartość** atrybut jest nowa wartość ma zostać wstawiony do wybranego węzła XML.
- **Warunek** atrybut jest kryteria, na których zadania powinny działać albo nie działać. W takich przypadkach zapewnia nie próbuj wstawić wartość null lub jest pusty w warunku *SetParameters.xml* pliku.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano rolę *SetParameters.xml* pliku i wyjaśniono, jak jest generowany, gdy tworzysz projekt aplikacji sieci web. Wyjaśniono, jak możecie dodatkowe ustawienia, dodając *parameters.xml* plik do projektu. Ponadto opisano, jak można zmodyfikować *SetParameters.xml* pliku w ramach procesu większe, zautomatyzowanych kompilacji przy użyciu **xmlpoke —** zadań w plikach projektu.

Następny temat [wdrażanie pakietów internetowych](deploying-web-packages.md), w tym artykule opisano, jak wdrożyć pakiet sieci web albo uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe polecenia bezpośrednio. W obu przypadkach można określić swoje *SetParameters.xml* pliku jako parametr wdrożenia.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać informacje na temat tworzenia pakietów w sieci web, zobacz [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md). Aby uzyskać wskazówki na temat faktycznego wdrażania pakietu sieci web, zobacz [wdrażanie pakietów internetowych](deploying-web-packages.md). Przewodnik krok po kroku dotyczące sposobu tworzenia *parameters.xml* plików, zobacz [porady: użycie parametrów do konfigurowania ustawień podczas pakietu wdrożeniowego jest zainstalowany](https://msdn.microsoft.com/library/ff398068.aspx).

Aby uzyskać bardziej ogólne informacje na temat parametryzacji w narzędzia Web Deploy, zobacz [parametryzacji wdrażania sieci Web w działaniu](https://go.microsoft.com/?linkid=9805119) (wpis na blogu).

> [!div class="step-by-step"]
> [Poprzednie](building-and-packaging-web-application-projects.md)
> [dalej](deploying-web-packages.md)
