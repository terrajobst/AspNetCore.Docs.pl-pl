---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Objaśnienie procesu kompilacji | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten temat zawiera wskazówki dotyczące skali korporacyjnej kompilacją i procesem wdrażania. Podejście opisane w tym temacie używa Engin kompilacji Microsoft niestandardowe...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 581b7e996bf5aa4c76a6bf3d55a758677c0bf897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804319"
---
<a name="understanding-the-build-process"></a>Objaśnienie procesu kompilacji
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten temat zawiera wskazówki dotyczące skali korporacyjnej kompilacją i procesem wdrażania. Podejście opisane w tym temacie używa niestandardowych plików projektu aparatu Microsoft Build Engine (MSBuild), aby zapewnić precyzyjną kontrolę nad wszystkimi aspektami procesu. W plikach projektów niestandardowych elementów docelowych MSBuild są używane do uruchamiania narzędzia wdrażania, takich jak Internet Information Services (IIS) Narzędzie Web Deployment (MSDeploy.exe) i narzędzie do wdrażania bazy danych VSDBCMD.exe.
> 
> > [!NOTE]
> > Poprzednim temacie [objaśnienie pliku projektu](understanding-the-project-file.md)opisano najważniejsze składniki pliku projektu programu MSBuild i wprowadzono pojęcie Podziel pliki projektu do obsługi wdrożenia w wielu środowiskach docelowych. Jeśli nie znasz już z tych koncepcji, należy zapoznać się [objaśnienie pliku projektu](understanding-the-project-file.md) przed rozpoczęciem pracy za pośrednictwem tego tematu.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="build-and-deployment-overview"></a>Kompilowanie i wdrażanie — omówienie

W [rozwiązania Contact Manager](the-contact-manager-solution.md), trzy pliki kontrolowania procesu kompilacji i wdrażania:

- A *pliku projektu uniwersalnej* (*Publish.proj*). Zawiera instrukcje tworzenia i wdrażania, które nie zmienił się od środowiska docelowego.
- *Plik projektu o specyficznych dla środowiska* (*Env Dev.proj*). Kompilowanie i wdrażanie ustawień, które są specyficzne dla środowiska docelowego określonego zawiera. Na przykład można użyć *Env Dev.proj* plik, aby podać ustawienia środowiska dla deweloperów lub testowym i Utwórz alternatywną plik o nazwie *Env Stage.proj* Aby określić ustawienia dla przemieszczania środowisko.
- A *pliku polecenia* (*Dev.cmd Publikuj*). Zawiera MSBuild.exe polecenia określającego, pliki projektu, który ma zostać wykonany. Można utworzyć pliku poleceń dla każdego środowiska docelowego, gdzie każdy plik zawiera poleceniu MSBuild.exe, który określa plik inny projekt specyficznymi dla środowiska. Dzięki temu deweloper wdrożenia w różnych środowiskach, uruchamiając plik odpowiednie polecenie.

W przykładowym rozwiązaniu można znaleźć tych trzech plików w folderze rozwiązania publikowania.

![](understanding-the-build-process/_static/image1.png)

Zanim przyjrzymy się te pliki, które bardziej szczegółowo, Spójrzmy na działania całego procesu kompilacji w przypadku użycia tej metody. Na wysokim poziomie proces kompilowania i wdrażania wygląda następująco:

![](understanding-the-build-process/_static/image2.png)

Pierwszą rzeczą, która jest dwa pliki projektu&#x2014;zawierającego universal instrukcje dotyczące kompilowania i wdrażania jednego i jeden zawierający ustawienia specyficzne dla środowiska&#x2014;są scalane w pliku pojedynczego projektu. Program MSBuild następnie pracuje z instrukcjami w pliku projektu. Tworzy każdy z projektów w rozwiązaniu przy użyciu pliku projektu dla każdego projektu. Następnie wywołuje się z innymi narzędziami, takimi jak narzędzie Web Deploy (MSDeploy.exe) i narzędzie VSDBCMD jako miejsce wdrożenia zawartości sieci web i baz danych do środowiska docelowego.

Od początku do końca proces kompilowania i wdrażania wykonuje następujące zadania:

1. Usuwa zawartość katalogu wyjściowego, w ramach przygotowania do nowej kompilacji.
2. Tworzy on każdego projektu w rozwiązaniu:

    1. Dla projektów sieci web&#x2014;w takim przypadku aplikacji sieci web ASP.NET MVC i platformy WCF usługi sieci web&#x2014;procesu kompilacji tworzy pakiet wdrażania sieci web, dla każdego projektu.
    2. Dla projektów bazy danych proces kompilacji tworzy manifest wdrożenia (pliku .deploymanifest) dla każdego projektu.
3. Używa ona narzędzie VSDBCMD.exe, aby wdrożyć każdy projekt bazy danych w rozwiązaniu przy użyciu różnych właściwości z plików projektu&#x2014;docelowy ciąg połączenia i nazwę bazy danych&#x2014;wraz z pliku .deploymanifest.
4. Narzędzia programu MSDeploy.exe używa do każdego projektu sieci web w rozwiązaniu do kontrolowania procesu wdrażania przy użyciu różnych właściwości z plików projektu wdrożenia.

Przykładowe rozwiązanie można użyć do śledzenia tego procesu bardziej szczegółowo.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania pliki projektu specyficznego dla środowiska dla środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Wywoływanie kompilacji i procesu wdrażania

Aby wdrożyć rozwiązanie Contact Manager w środowisku testowym dla deweloperów, uruchamia Deweloper *Dev.cmd Publikuj* pliku polecenia. Wywołuje to MSBuild.exe, określając *Publish.proj* jako plik projektu do wykonania i *Env Dev.proj* jako wartości parametru.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** przełącznika (skrót od **/fileLogger**) rejestruje dane wyjściowe kompilacji do pliku o nazwie *msbuild.log* w bieżącym katalogu. Aby uzyskać więcej informacji, zobacz [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).


W tym momencie MSBuild zacznie działać, ładuje *Publish.proj* plików i rozpoczyna przetwarzanie instrukcje znajdujące się w nim. Pierwsza instrukcja informuje program MSBuild, aby zaimportować projektu pliku, który **TargetEnvPropsFile** określa parametr.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** parametr określa *Env Dev.proj* plików, dzięki czemu program MSBuild Scala zawartość *Env Dev.proj* mezzanine do  *Publish.Proj* pliku.

Dalej elementy, które MSBuild napotka w pliku scalonego projektu są właściwości grupy. Właściwości są przetwarzane w kolejności, w jakiej występują w pliku. Program MSBuild tworzy parę klucz wartość dla każdej właściwości, zapewniając, że spełnione są wszystkie określone warunki. Właściwości zdefiniowane w dalszej części w pliku spowoduje zastąpienie właściwości o takiej samej nazwie, wcześniej zdefiniowane w pliku. Na przykład, rozważmy **OutputRoot** właściwości.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Gdy MSBuild przetwarza pierwszy **OutputRoot** elementu, zapewniając o podobnej nazwie parametr nie zostanie dostarczona, ustawia wartość **OutputRoot** właściwość **... \Publish\Out**. Gdy wystąpi drugi **OutputRoot** elementu, jeśli warunek jest **true**, spowoduje zastąpienie wartości **OutputRoot** właściwość z wartością **OutDir** parametru.

Następny element, który wykryje MSBuild jest grupą pojedynczy element zawierający element o nazwie **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


Program MSBuild przetwarza tę instrukcję, tworząc listę elementów o nazwie **ProjectsToBuild**. W tym przypadku listy elementów zawiera pojedynczą wartość&#x2014;ścieżkę i nazwę pliku rozwiązania.

W tym momencie wszystkie pozostałe elementy są elementów docelowych. Obiekty docelowe są przetwarzane inaczej od właściwości i elementy&#x2014;zasadniczo elementy docelowe nie są przetwarzane chyba że są one jawnie określone przez użytkownika lub wywołany przez innej konstrukcji w pliku projektu. Pamiętaj, że otwarcie **projektu** znacznik obejmuje **defaulttargets —** atrybutu.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


To powoduje, że program MSBuild, aby wywołać **FullPublish** obiektu docelowego, jeśli obiekty docelowe nie są określone, gdy zostanie wywołana MSBuild.exe. **FullPublish** docelowy nie zawiera żadnych zadań; zamiast tego po prostu Określa listę zależności.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Ta zależność informuje program MSBuild tego w celu wykonania **FullPublish** obiektu docelowego, należy ją wywołać tej listy elementów docelowych w podanej kolejności:

1. Należy wywołać **czysty** docelowej.
2. Należy wywołać **BuildProjects** docelowej.
3. Należy wywołać **GatherPackagesForPublishing** docelowej.
4. Należy wywołać **PublishDbPackages** docelowej.
5. Należy wywołać **PublishWebPackages** docelowej.

### <a name="the-clean-target"></a>Czyste miejsce docelowe

**Czysty** docelowego po prostu usuwa katalog wyjściowy i całą jego zawartość jako przygotowania do nowej kompilacji.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Należy zauważyć, że zawiera element docelowy **ItemGroup** elementu. Podczas definiowania właściwości i elementów w obrębie **docelowej** elementu, tworzysz *dynamiczne* właściwości i elementów. Innymi słowy właściwości lub elementy nie są przetwarzane do momentu docelowy jest wykonywany. Katalog wyjściowy nie istnieje lub zawiera żadnych plików do momentu rozpoczęcia procesu kompilacji, więc nie można tworzyć  **\_FilesToDelete** listy jako elementy statyczne; musiały czekać na ukończenie wykonywania jest w toku. Jako takie należy utworzyć listę jako dynamiczny element docelowy.

> [!NOTE]
> W tym przypadku ponieważ **czysty** element docelowy jest pierwszy do wykonania, rzeczywistych trzeba użyć grupy elementów dynamicznych. Jednak dobrym rozwiązaniem jest używać właściwości dynamicznych i elementy w tego rodzaju scenariusza, jak możesz chcieć wykonać obiekty docelowe w innej kolejności w pewnym momencie.  
> Należy również dążyć do uniknięcia zadeklarowanie elementów, które nigdy nie będą używane. W przypadku elementów, które będą używane tylko przez określony element docelowy należy rozważyć umieszczenie ich w docelowym, aby usunąć wszelkie niepotrzebne koszty w procesie kompilacji.


Dynamiczne elementy specjalnie, **czysty** element docelowy jest dość prosta i korzysta z wbudowanej **komunikat**, **Usuń**, i **removedir —** zadań do:

1. Wyślij wiadomość do rejestratora.
2. Tworzenie listy plików do usunięcia.
3. Usuń pliki.
4. Usuń katalog wyjściowy.

### <a name="the-buildprojects-target"></a>Element docelowy BuildProjects

**BuildProjects** docelowej zasadniczo tworzy wszystkie projekty w przykładowym rozwiązaniu.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Ten element docelowy został opisany w poprzednim temacie szczegółowo [objaśnienie pliku projektu](understanding-the-project-file.md), aby zilustrować, jak zadania i elementy docelowe odwoływać się do właściwości i elementów. W tym momencie interesuje Cię głównie **MSBuild** zadania. To zadanie służy do kompilacji wielu projektów. Zadanie nie tworzy nowe wystąpienie klasy MSBuild.exe; do tworzenia każdego projektu używa bieżącej uruchomionego wystąpienia. Kluczowych punktów orientacyjnych w tym przykładzie przedstawiono właściwości wdrożenia:

- **DeployOnBuild** właściwości powoduje, że program MSBuild uruchamiał wszelkie instrukcje wdrażania w ustawieniach projektu, po zakończeniu kompilacji każdego projektu.
- **DeployTarget** właściwość identyfikuje element docelowy do wywołania po utworzeniu projektu. W tym przypadku **pakietu** docelowej tworzy dane wyjściowe projektu do pakietu sieci web do wdrożenia.

> [!NOTE]
> **Pakietu** miejsce docelowe wywołuje Web Publishing potoku (WPP), który zapewnia integrację MSBuild i narzędzia Web Deploy. Jeśli chcesz Przyjrzyj się wbudowane elementy docelowe udostępnianych przez potok WPP, przejrzyj *Microsoft.Web.Publishing.targets* pliku w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>Element docelowy GatherPackagesForPublishing

Jeśli Przestudiowanie **GatherPackagesForPublishing** obiektu docelowego, można zauważyć, że faktycznie nie zawiera żadnych zadań. Zamiast tego zawiera grupy pojedynczy element, który definiuje trzy elementy dynamiczne.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Te elementy odnoszą się do pakietów wdrożeniowych, które zostały utworzone podczas **BuildProjects** docelowy został wykonany. Możesz nie można zdefiniować te elementy statycznie w pliku projektu, ponieważ pliki, do których można znaleźć elementy nie istnieją aż **BuildProjects** docelowy jest wykonywany. Zamiast tego, elementy muszą być zdefiniowane dynamicznie w czasie docelowym, który nie jest wywoływany do momentu po **BuildProjects** docelowy jest wykonywany.

Elementy nie są używane w ramach tego obiektu docelowego&#x2014;ten element docelowy, po prostu tworzy elementy i metadane skojarzone z każdej wartości elementu. Gdy te elementy są przetwarzane, **PublishPackages** element będzie zawierać dwie wartości, ścieżka do *ContactManager.Mvc.deploy.cmd* pliku i ścieżkę do  *ContactManager.Service.deploy.cmd* pliku. Narzędzie Web Deploy tworzy te pliki jako część pakietu sieci web dla każdego projektu, i są to pliki, które musi zostać wywołany na serwerze docelowym, aby można było wdrożyć pakietów. Jeśli otworzysz jeden z tych plików, po prostu zobaczysz polecenie MSDeploy.exe z różnymi wartościami parametrów specyficzne dla kompilacji.

**DbPublishPackages** element będzie zawierać pojedynczej wartości, a ścieżka do *ContactManager.Database.deploymanifest* pliku.

> [!NOTE]
> Plik .deploymanifest jest generowany, gdy tworzysz projekt bazy danych, przy czym ten sam schemat w pliku projektu MSBuild. Zawiera wszystkie informacje wymagane do wdrożenia bazy danych, w tym lokalizacji schematu bazy danych (.dbschema) i szczegóły wszystkie skrypty przed wdrożeniem i po wdrożeniu. Aby uzyskać więcej informacji, zobacz [Przegląd bazy danych kompilowania i wdrażania](https://msdn.microsoft.com/library/aa833165.aspx).


Dowiesz się więcej na temat sposobu wdrażania pakietów i manifesty wdrożenia bazy danych są tworzone i używane w [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md) i [wdrażanie projektów baz danych](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Element docelowy PublishDbPackages

Krótko mówiąc, **PublishDbPackages** miejsce docelowe wywołuje narzędzie VSDBCMD, aby wdrożyć **ContactManager** bazy danych do środowiska docelowego. Konfigurowanie wdrażania bazy danych obejmuje wiele decyzji i niuanse i dowiesz się więcej o tym w [wdrażanie projektów baz danych](deploying-database-projects.md) i [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). W tym temacie skupimy się na ten element docelowy faktycznie działania.

Po pierwsze, zwróć uwagę, że otwierający tag **dane wyjściowe** atrybutu.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Jest to przykład *przetwarzaniu wsadowym obiektów docelowych*. W plikach projektowych MSBuild przetwarzanie wsadowe jest techniką Iterowanie po kolekcji. Wartość **dane wyjściowe** atrybutu **"% (DbPublishPackages.Identity)"**, odwołuje się do **tożsamości** właściwości metadanych **DbPublishPackages**  listy elementów. Tej notacji, **Outputs=%***(ItemList.ItemMetadataName)*, jest tłumaczony jako:

- Podziel elementy w **DbPublishPackages** w partie elementów, które zawierają takie same **tożsamości** wartość metadanych.
- Wykonanie docelowego raz na partię.

> [!NOTE]
> **Tożsamość** jest jednym z [wartości wbudowanych metadanych](https://msdn.microsoft.com/library/ms164313.aspx) przypisany do każdego elementu przy tworzeniu. Odwołuje się do wartości **Include** atrybutu w **elementu** elementu&#x2014;innymi słowy, ścieżkę i nazwę elementu.


W tym przypadku ponieważ nigdy nie powinna być więcej niż jeden element o tej samej ścieżki i nazwy pliku, zasadniczo pracujemy z rozmiary partii jednego. Element docelowy jest wykonywane raz dla każdego pakietu bazy danych.

Możesz zobaczyć podobne Notacja w  **\_Cmd** właściwość, która tworzy polecenie VSDBCMD za pomocą odpowiednich przełączników.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


W tym przypadku **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, i **%(DbPublishPackages.FullPath)** wszystkie odnoszą się do wartości metadanych **DbPublishPackages** elementu kolekcji. **\_Cmd** właściwość jest używana przez **Exec** zadania, które wywołuje polecenie.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


W wyniku tej notacji **Exec** zadania spowoduje utworzenie partie na podstawie unikatowych kombinacji **DatabaseConnectionString**, **TargetDatabase**i **FullPath** wartości metadanych i zadania będą wykonywane raz dla każdej partii. Jest to przykład *przetwarzaniu wsadowym zadań*. Jednakże, ponieważ poziom miejsca docelowego, przetwarzanie wsadowe już został podzielony naszych kolekcji elementów w partie pojedynczą pozycją **Exec** zadanie będzie uruchamiane jeden raz i tylko jeden raz dla każdej iteracji, obiektu docelowego. Innymi słowy to zadanie wywołuje narzędzie VSDBCMD jeden raz dla każdego pakietu bazy danych w rozwiązaniu.

> [!NOTE]
> Aby uzyskać więcej informacji na temat obiektu docelowego i przetwarzanie wsadowe zadań, zobacz MSBuild [przetwarzania wsadowego](https://msdn.microsoft.com/library/ms171473.aspx), [metadane elementu w przetwarzaniu wsadowym obiektów docelowych](https://msdn.microsoft.com/library/ms228229.aspx), i [metadane elementu w przetwarzaniu wsadowym zadań](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Element docelowy PublishWebPackages

Na tym etapie została wywołana **BuildProjects** obiektu docelowego, który generuje pakiet wdrażania sieci web, dla każdego projektu w przykładowym rozwiązaniu. Towarzyszący każdy pakiet jest *pliku deploy.cmd* pliku, który zawiera polecenia MSDeploy.exe niezbędne do wdrożenia pakietu środowiska docelowego, a *SetParameters.xml* plik, który określa wymagane szczegóły środowiska docelowego. Została również wywołana **GatherPackagesForPublishing** obiektu docelowego, co powoduje generowanie elementu Kolekcja zawierająca *pliku deploy.cmd* pliki interesuje Cię. Zasadniczo **PublishWebPackages** docelowy wykonuje następujące funkcje:

- Obsługuje ona *SetParameters.xml* pliku dla każdego pakietu zawierały poprawne informacje dla środowiska docelowego przy użyciu **xmlpoke —** zadania.
- Wywołuje *pliku deploy.cmd* pliku dla każdego pakietu za pomocą odpowiednich przełączników.

Podobnie jak w przypadku **PublishDbPackages** obiektu docelowego, **PublishWebPackages** docelowy używa przetwarzaniu wsadowym obiektów docelowych w celu zapewnienia, że obiekt docelowy jest wykonywane raz dla każdego pakietu sieci web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


W elemencie docelowym **Exec** zadanie jest wykorzystywane do uruchamiania *pliku deploy.cmd* pliku dla każdego pakietu sieci web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Aby uzyskać więcej informacji na temat konfigurowania wdrożenia pakietów sieci web, zobacz [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Wniosek

W tym temacie podano wskazówki dotyczące sposobu Podziel pliki projektu są używane do kontrolowania procesu kompilacji i wdrażania, od początku do końca, aby uzyskać przykładowe rozwiązanie Contact Manager. Takie podejście umożliwia uruchomieniem złożone, skali przedsiębiorstwa wdrożeniami w kroku pojedynczej i powtarzalnej, uruchamiając plik polecenia specyficzne dla środowiska.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i potok WPP, zobacz [wewnątrz aparatu Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzednie](understanding-the-project-file.md)
> [dalej](building-and-packaging-web-application-projects.md)
