---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Opis procesu kompilacji | Dokumentacja firmy Microsoft
author: jrjlee
description: "W tym temacie przedstawiono wskazówki procesu kompilacji i wdrożenia skali przedsiębiorstwa. Podejście opisane w tym temacie używa niestandardowych Microsoft Engin kompilacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 551e31a7a2d0a4e6259f74977c2f8e21cb694e42
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-build-process"></a>Opis procesu kompilacji
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie przedstawiono wskazówki procesu kompilacji i wdrożenia skali przedsiębiorstwa. Podejście opisane w tym temacie używa niestandardowych plików projektu Microsoft kompilacji Engine (MSBuild), aby zapewnić precyzyjną kontrolę nad wszystkich aspektów procesu. W plikach projektu docelowych elementów MSBuild niestandardowe są używane do uruchamiania narzędzia wdrażania, takich jak usługi Internet Information Services (IIS) Narzędzie Web Deployment (MSDeploy.exe) i narzędzia wdrażania bazy danych VSDBCMD.exe.
> 
> > [!NOTE]
> > Poprzedniego tematu [opis pliku projektu](understanding-the-project-file.md), opisano najważniejsze składniki pliku projektu MSBuild i wprowadzono pojęcie podziału pliki projektu do obsługi wdrożenia w wielu środowiskach docelowych. Jeśli nie masz już znasz tych pojęć, należy przejrzeć [opis pliku projektu](understanding-the-project-file.md) przed rozpoczęciem pracy za pośrednictwem tego tematu.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="build-and-deployment-overview"></a>Omówienie wdrożenia i kompilacji

W [rozwiązania kontaktów Menedżerze](the-contact-manager-solution.md), trzy pliki sterowanie procesem kompilacji i wdrażania:

- A *pliku projektu uniwersalnej* (*Publish.proj*). Zawiera instrukcje kompilowanie i wdrażanie, które nie zmieniają się między środowiskami docelowego.
- *Pliku projektu określonego środowiska* (*Env Dev.proj*). Zawiera kompilowanie i wdrażanie ustawień, które są specyficzne dla środowiska danego przeznaczenia. Na przykład można użyć *Env Dev.proj* plik, aby zapewnić ustawienia środowiska dewelopera lub testowym i Utwórz alternatywnych plik o nazwie *Env Stage.proj* zapewnienie ustawień przemieszczania środowisko.
- A *pliku polecenia* (*Dev.cmd publikowania*). Zawiera MSBuild.exe polecenie, które określa pliki projektu, które chcesz wykonać. Można utworzyć pliku poleceń dla każdego środowiska docelowego, gdzie każdy plik zawiera polecenie MSBuild.exe, które określa plik inny projekt określonego środowiska. Dzięki temu deweloper wdrożyć w różnych środowiskach, uruchamiając plik odpowiednie polecenie.

W rozwiązaniu próbki można znaleźć tych trzech plików w folderze rozwiązania publikowania.

![](understanding-the-build-process/_static/image1.png)

Przed wyświetleniu tych plików bardziej szczegółowo Spójrzmy na działania całego procesu kompilacji posłuż się tą metodą. Na wysokim poziomie procesem kompilacji i wdrażania wygląda następująco:

![](understanding-the-build-process/_static/image2.png)

Pierwszą czynnością, która jest dwa projektu plików & #x 2014; zawierający uniwersalnych kompilacji i instrukcje dotyczące wdrażania i jeden zawierający ustawienia charakterystyczne dla środowiska & #x 2014; są scalane w pliku pojedynczego projektu. MSBuild następnie działa przy użyciu instrukcji w pliku projektu. Tworzy listę projektów w rozwiązaniu, dla każdego projektu przy użyciu pliku projektu. Następnie wywołuje do innych narzędzi, takich jak narzędzie Web Deploy (MSDeploy.exe) i narzędzie VSDBCMD, aby wdrożyć zawartość sieci web i baz danych w środowisku docelowym.

Od początku do końca proces kompilacji i wdrożenia wykonuje te zadania:

1. Usuwa zawartość do katalogu wyjściowego, w ramach przygotowań do świeże kompilacji.
2. Zbudował każdego projektu w rozwiązaniu:

    1. Projekty sieci web & #x 2014; w takim przypadku aplikacji sieci web platformy ASP.NET MVC i WCF sieci web usługi & #x 2014; proces kompilacji tworzy pakietu wdrożeniowego sieci web dla każdego projektu.
    2. Dla projektów bazy danych proces kompilacji tworzy manifest rozmieszczenia (plik .deploymanifest) dla każdego projektu.
3. Narzędzie VSDBCMD.exe używa do każdego projektu bazy danych w rozwiązaniu, korzystając z różnych z plików projektu i #x 2014; ciąg połączenia docelowego oraz nazwę bazy danych i #x 2014; wraz z pliku .deploymanifest wdrożenia.
4. Narzędzia programu MSDeploy.exe używa do wdrożenia każdego projektu sieci web w rozwiązaniu, korzystając z różnych z plików projektu Aby kontrolować proces wdrażania.

Przykładowe rozwiązanie służy do śledzenia ten proces bardziej szczegółowo.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące sposobu dostosowywania pliki projektu określonego środowiska dla środowiska serwera, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Wywoływanie kompilacji i proces wdrażania

Aby wdrożyć rozwiązanie Contact Manager do środowiska testowego developer, uruchamia dewelopera *Dev.cmd publikowania* pliku polecenia. Wywołuje to MSBuild.exe, określając *Publish.proj* jako plik projektu do wykonania i *Env Dev.proj* jako wartości parametru.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl** przełącznika (skrót od **/fileLogger**) rejestruje dane wyjściowe kompilacji w pliku o nazwie *msbuild.log* w bieżącym katalogu. Aby uzyskać więcej informacji, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/en-us/library/ms164311.aspx).


W tym momencie MSBuild zacznie działać, ładuje *Publish.proj* plików i rozpoczyna przetwarzanie z instrukcjami wyświetlanymi w nim. Pierwsza instrukcja informuje program MSBuild Importowanie projektu pliku **TargetEnvPropsFile** określa parametr.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile** określa parametr *Env Dev.proj* pliku, więc MSBuild Scala zawartość *Env Dev.proj* pliku do  *Publish.Proj* pliku.

Dalej elementy, które MSBuild napotka w pliku projektu scalony to grupy właściwości. Właściwości są przetwarzane w kolejności, w jakiej występują w pliku. MSBuild tworzy parę klucz wartość dla każdej właściwości, zapewniając, że wszystkie określone warunki są spełnione. Wszystkie właściwości o tej samej nazwie wcześniej zdefiniowane w pliku zostaną zastąpione właściwości później zdefiniowane w pliku. Rozważmy na przykład **OutputRoot** właściwości.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Gdy MSBuild przetwarza pierwszy **OutputRoot** elementu, podając parametr o podobnej nazwie nie została dostarczona, ustawia wartość **OutputRoot** właściwości **... \Publish\Out**. Po napotkaniu drugi **OutputRoot** elementu, jeśli wynikiem warunku jest **true**, spowoduje zastąpienie wartości **OutputRoot** właściwość z wartością **OutDir** parametru.

Następny element, który napotka MSBuild jest jeden element grupy, zawierający element o nazwie **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild przetwarza tej instrukcji przez utworzenie listy elementów o nazwie **ProjectsToBuild**. W takim przypadku listy elementów zawiera pojedynczą wartość & #x 2014; ścieżkę i nazwę pliku rozwiązania.

W tym momencie pozostałe elementy są elementy docelowe. Obiekty docelowe są przetwarzane inaczej z właściwości i elementy & #x 2014; zasadniczo elementów docelowych nie są przetwarzane chyba, że są one jawnie określone przez użytkownika lub wywołany przez inny konstrukcji w pliku projektu. Odwołania, który otwarcia **projektu** zawiera tag **defaulttargets —** atrybutu.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


To powoduje, że program MSBuild na wywołania **FullPublish** określony obiekt docelowy, jeśli nie są elementy docelowe po wywołaniu MSBuild.exe. **FullPublish** docelowy nie zawiera żadnych zadań; zamiast tego po prostu Określa listę zależności.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Ta zależność informuje program MSBuild tego w celu wykonania **FullPublish** docelowej, należy go wywoływać tej listy elementów docelowych w podanej kolejności:

1. Należy wywołać **wyczyść** docelowej.
2. Należy wywołać **BuildProjects** docelowej.
3. Należy wywołać **GatherPackagesForPublishing** docelowej.
4. Należy wywołać **PublishDbPackages** docelowej.
5. Należy wywołać **PublishWebPackages** docelowej.

### <a name="the-clean-target"></a>Element docelowy czyste

**Wyczyść** docelowej usuwa zasadniczo do katalogu wyjściowego i całą jego zawartość jako przygotowanie świeże kompilacji.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Należy zauważyć, że zawiera element docelowy **ItemGroup** elementu. Podczas definiowania właściwości lub elementów w obrębie **docelowej** elementu, tworzysz *dynamiczne* właściwości i elementów. Innymi słowy właściwości lub elementy nie są przetwarzane, aż obiekt docelowy jest wykonywana. Katalog wyjściowy nie istnieje lub ma żadnych plików do momentu rozpoczęcia procesu kompilacji, więc nie można utworzyć  **\_FilesToDelete** listy jako element statyczny; należy poczekać do czasu wykonywania jest przetwarzane. Tak należy utworzyć listę jako elementów dynamicznych w docelowym.

> [!NOTE]
> W takim przypadku ponieważ **wyczyść** docelowy jest pierwszą osobą, która można wykonać, nie istnieje potrzeba rzeczywistych używanie grupy elementów dynamicznych. Jednak dobrym rozwiązaniem jest użycie właściwości dynamicznych i elementów w tym typie scenariusza, jak należy wykonać elementy docelowe w innej kolejności w pewnym momencie.  
> Należy również celem uniknąć deklarowanie elementów, które nigdy nie będą używane. Jeśli masz elementy, które będą używane tylko przez określony element docelowy, należy rozważyć umieszczenie ich wewnątrz obiektu docelowego, aby usunąć wszystkie zbędne obciążenie procesu kompilacji.


Dynamiczne elementy Odłóż, **wyczyść** docelowy jest bardzo prosta i korzysta z wbudowanej **komunikat**, **usunąć**, i **removedir —**zadań do:

1. Wyślij wiadomość do rejestratora.
2. Tworzenie listy plików do usunięcia.
3. Usuń pliki.
4. Usuń katalog wyjściowy.

### <a name="the-buildprojects-target"></a>Element docelowy BuildProjects

**BuildProjects** docelowej zasadniczo tworzy wszystkich projektów w rozwiązaniu próbki.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Ten element docelowy został opisany szczegółowo w temacie poprzedniej [opis pliku projektu](understanding-the-project-file.md), aby zilustrować, jak zadań i obiekty docelowe odwoływać się do właściwości i elementów. W tym momencie interesuje Cię głównie **MSBuild** zadań. To zadanie służy do tworzenia wielu projektów. Zadanie nie tworzy nowe wystąpienie klasy MSBuild.exe; używa bieżącej uruchomione wystąpienie do każdego projektu kompilacji. Właściwości wdrożenia są najważniejsze zainteresowania w tym przykładzie:

- **DeployOnBuild** instruuje MSBuild, aby uruchomić wszystkie instrukcje dotyczące wdrażania w ustawieniach projektu po zakończeniu kompilacji każdego projektu.
- **DeployTarget** właściwość identyfikuje docelowego, który chcesz wywołać po utworzeniu projektu. W takim przypadku **pakietu** docelowej tworzy dane wyjściowe projektu do pakietu sieci web do wdrożenia.

> [!NOTE]
> **Pakietu** docelowej wywołuje sieci Web publikowania potoku (WPP), która zapewnia integrację MSBuild i Web Deploy. Aby przyjrzeć wbudowane obiekty docelowe, które udostępnia WPP, przejrzyj *Microsoft.Web.Publishing.targets* plików w folderze % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>Element docelowy GatherPackagesForPublishing

Jeśli Przestudiowanie **GatherPackagesForPublishing** docelowej, można zauważyć, że faktycznie nie zawiera żadnych zadań. Zamiast tego zawiera grupy pojedynczy element, który definiuje trzy elementów dynamicznych.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Te elementy można znaleźć pakiety wdrożeniowe, które zostały utworzone podczas obliczania **BuildProjects** docelowej zostało wykonane. Nie zdefiniujesz te elementy statycznie w pliku projektu, ponieważ nie istnieją pliki, do których odwołuje się elementy, aż do **BuildProjects** docelowy jest wykonywana. Zamiast tego elementów muszą być zdefiniowane dynamicznie w elemencie docelowym, który nie jest wywoływany do momentu po **BuildProjects** docelowy jest wykonywana.

Elementy nie są używane w ramach tego docelowego & #x 2014; ten element docelowy po prostu tworzy elementy i metadane skojarzone z każdej wartości elementu. Po przetworzeniu tych elementów są **PublishPackages** element będzie zawierać dwie wartości, ścieżka do *ContactManager.Mvc.deploy.cmd* pliku i ścieżkę do  *ContactManager.Service.deploy.cmd* pliku. Narzędzie Web Deploy tworzy te pliki jako część pakietu sieci web dla każdego projektu, i są to pliki, które należy wywołać na serwerze docelowym w celu wdrożenia pakietów. Jeśli otworzysz jednego z tych plików zasadniczo zobaczysz polecenie MSDeploy.exe z różnych wartości parametru specyficzne dla kompilacji.

**DbPublishPackages** element będzie zawierać pojedynczą wartość, ścieżka do *ContactManager.Database.deploymanifest* pliku.

> [!NOTE]
> Plik .deploymanifest jest generowany podczas kompilowania projektu bazy danych i używa tego samego schematu jako plik projektu programu MSBuild. Zawiera wszystkie informacje wymagane do wdrożenia bazy danych, takie jak lokalizacja schematu bazy danych (.dbschema) i szczegóły skrypty przed wdrożeniem i po wdrożeniu. Aby uzyskać więcej informacji, zobacz [Przegląd bazy danych kompilacji i wdrażania](https://msdn.microsoft.com/en-us/library/aa833165.aspx).


Dowiesz się więcej o sposobie wdrażania pakietów i manifestów wdrożenia bazy danych są tworzone i używane w [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md) i [wdrażania projektów bazy danych](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Element docelowy PublishDbPackages

Krótko mówiąc, **PublishDbPackages** docelowej wywołuje narzędzie VSDBCMD, aby wdrożyć **ContactManager** bazy danych w środowisku docelowym. Konfigurowanie wdrażania bazy danych obejmuje wiele decyzji i wszystkie szczegóły, a dowiesz się więcej na temat [wdrażania projektów bazy danych](deploying-database-projects.md) i [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). W tym temacie firma Microsoft będzie skupić się na ten element docelowy faktycznie działania.

Po pierwsze, zwróć uwagę, że otwierający tag zawiera **dane wyjściowe** atrybutu.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


To jest przykład *przetwarzaniu wsadowym obiektów docelowych*. W plikach projektu MSBuild przetwarzanie wsadowe to technika potrzeby iteracji w kolekcji. Wartość **dane wyjściowe** atrybutu **"% (DbPublishPackages.Identity)"**, odwołuje się do **tożsamości** właściwości metadanych **DbPublishPackages**  listy elementów. Ta notacja **dane wyjściowe = %***(ItemList.ItemMetadataName)*, jest translacja jako:

- Podziel elementy w **DbPublishPackages** w partie elementów, które zawierają takie same **tożsamości** wartości metadanych.
- Wykonanie docelowego raz w każdej partii.

> [!NOTE]
> **Tożsamość** jest jednym z [wartości wbudowanych metadanych](https://msdn.microsoft.com/en-us/library/ms164313.aspx) przypisany do każdego elementu po utworzeniu. Odnosi się do wartości **Include** atrybutu w **elementu** elementu & #x 2014; innymi słowy, ścieżkę i nazwę elementu.


W takim przypadku ponieważ nigdy nie może mieć więcej niż jeden element o tej samej ścieżki i nazwy pliku, zasadniczo pracujemy o rozmiarze partii jednego. Element docelowy jest wykonywana raz dla każdego pakietu bazy danych.

Zostanie wyświetlony podobne Notacja w  **\_Cmd** właściwość, która tworzy polecenie VSDBCMD z odpowiednich przełączników.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


W takim przypadku **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, i **%(DbPublishPackages.FullPath)** wszystkie odnoszą się do wartości metadanych **DbPublishPackages** Kolekcja elementów.  **\_Cmd** jest używana przez **Exec** zadania, które wywołuje polecenie.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


W wyniku tego notacji **Exec** zadania spowoduje utworzenie partie oparte na unikatowych kombinacji **DatabaseConnectionString**, **TargetDatabase**i **FullPath** wartości metadanych i zadanie będzie wykonywane raz dla każdej partii. To jest przykład *przetwarzanie wsadowe zadań*. Jednakże, ponieważ przetwarzanie wsadowe poziom docelowy ma już podzielony naszych kolekcji elementów w partiach pojedynczego elementu **Exec** zadanie będzie uruchamiane raz i tylko jeden raz dla każdej iteracji obiektu docelowego. Innymi słowy to zadanie wywołuje narzędzie VSDBCMD raz dla każdego pakietu bazy danych w rozwiązaniu.

> [!NOTE]
> Aby uzyskać więcej informacji na docelowy i przetwarzanie wsadowe zadań, zobacz MSBuild [wsadowe](https://msdn.microsoft.com/en-us/library/ms171473.aspx), [metadane elementu w przetwarzaniu wsadowym obiektów docelowych](https://msdn.microsoft.com/en-US/library/ms228229.aspx), i [metadane elementu w przetwarzaniu wsadowym zadań](https://msdn.microsoft.com/en-us/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Element docelowy PublishWebPackages

W tym punkcie została wywołana **BuildProjects** docelowa generuje pakietu wdrożeniowego sieci web dla każdego projektu w rozwiązaniu próbki. Towarzyszące każdy pakiet jest *pliku deploy.cmd* pliku, który zawiera polecenia MSDeploy.exe niezbędne do wdrożenia pakietu środowiska docelowego, a *SetParameters.xml* plik, który określa niezbędne szczegóły środowiska docelowego. Zostały również wywołać **GatherPackagesForPublishing** docelowej, która generuje element kolekcja zawierająca *pliku deploy.cmd* pliki interesuje Cię. Zasadniczo **PublishWebPackages** docelowy wykonuje następujące funkcje:

- Obsługuje on *SetParameters.xml* pliku każdego pakietu na zawiera prawidłowe informacje dotyczące dla środowiska docelowego przy użyciu **xmlpoke —** zadań.
- Wywołuje *pliku deploy.cmd* pliku dla każdego pakietu przy użyciu odpowiednich przełączników.

Podobnie jak **PublishDbPackages** docelowej, **PublishWebPackages** docelowy używa przetwarzaniu wsadowym obiektów docelowych w celu zapewnienia, że element docelowy jest wykonywane raz dla każdego pakietu sieci web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


W docelowym **Exec** zadanie służy do uruchamiania *pliku deploy.cmd* pliku dla każdego pakietu sieci web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Aby uzyskać więcej informacji na temat konfigurowania wdrażania pakietów sieci web, zobacz [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Wniosek

W tym temacie podano wskazówki użycia podziału pliki projektu do sterowania procesem kompilacji i wdrażania od początku do końca dla kontaktów Menedżerze przykładowe rozwiązanie. W ten sposób umożliwia uruchomienie złożone, skali przedsiębiorstwa wdrożenia w pojedynczej i powtarzalnej kroku, uruchamiając plik poleceń określonego środowiska.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat wprowadzenie do plików projektu i WPP, zobacz [wewnątrz kompilacji aparatu Microsoft: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i Bartholomew łączy, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Poprzednie](understanding-the-project-file.md)
[dalej](building-and-packaging-web-application-projects.md)
