---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Objaśnienie pliku projektu | Dokumentacja firmy Microsoft
author: jrjlee
description: Pliki projektu aparatu Microsoft Build Engine (MSBuild) znajdują się w ramach procesu kompilacji i wdrażania. W tym temacie zaczyna się omówienie pojęć dotyczących programu MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377207"
---
<a name="understanding-the-project-file"></a>Objaśnienie pliku projektu
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pliki projektu aparatu Microsoft Build Engine (MSBuild) znajdują się w ramach procesu kompilacji i wdrażania. W tym temacie zaczyna się omówienie pojęć dotyczących programu MSBuild i pliku projektu. Opisuje najważniejsze składniki, które będzie trafisz na podczas pracy z plikami projektu i działa za pośrednictwem przykład sposobu użycia plików projektu w celu wdrażania aplikacji w rzeczywistych warunkach.
> 
> Zawartość:
> 
> - Jak program MSBuild używa plików projektu MSBuild do kompilowania projektów.
> - Jak MSBuild integruje się z wdrożenia technologii, takich jak Internet Information Services (IIS) Narzędzie wdrażania sieci Web (Web Deploy).
> - Jak zrozumieć kluczowe składniki pliku projektu.
> - Jak można użyć plików projektu, tworzyć i wdrażać złożone aplikacje.


## <a name="msbuild-and-the-project-file"></a>MSBuild i pliku projektu

Podczas tworzenia i tworzyć rozwiązania w programie Visual Studio, Visual Studio używa MSBuild do tworzenia każdego projektu w rozwiązaniu. Każdy projekt programu Visual Studio zawiera plik projektu MSBuild z rozszerzeniem pliku, który odzwierciedla typu projektu&#x2014;na przykład projekt C# (.csproj), języku wizualnego projektu (.vbproj) lub projekt bazy danych (.dbproj). Aby skompilować projekt, program MSBuild musi przetworzyć plik projektu skojarzony z projektem. Plik projektu jest dokumentu XML, który zawiera wszystkie informacje i instrukcje, że program MSBuild wymaga w celu kompilowania projektu, takich jak zawartość, aby uwzględnić wymagania dotyczące platformy, informacji o wersji, serwer sieci web lub ustawienia serwera bazy danych, a zadania, które muszą być wykonywane.

Pliki projektu MSBuild opierają się na [schematu MSBuild XML](https://msdn.microsoft.com/library/5dy88c2e.aspx), i dzięki temu proces kompilacji jest całkowicie otwarty i przejrzysty. Ponadto nie trzeba zainstalować program Visual Studio aby można było używać aparatu MSBuild&#x2014;MSBuild.exe pliku wykonywalnego, który jest częścią programu .NET Framework i można go uruchomić z wiersza polecenia. Jako deweloper możesz tworzyć we własnych plikach projektu programu MSBuild nakładają wyrafinowane i szczegółową kontrolę nad jak swoje projekty są zbudowane i wdrażane przy użyciu schematu MSBuild XML. Te pliki projektu niestandardowe działają w taki sam sposób, jak pliki projektów odnoszących się program Visual Studio generuje automatycznie.

> [!NOTE]
> Umożliwia także pliki projektów programu MSBuild z usługą Team Build w Team Foundation Server (TFS). Na przykład można użyć plików projektu w scenariuszach ciągłej integracji (CI) zautomatyzować wdrożenia w środowisku testowym, gdy nowy kod jest zaewidencjonowany. Aby uzyskać więcej informacji, zobacz [Konfigurowanie serwera Team Foundation Server dla automatycznego wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Plik projektu, konwencje nazewnictwa

Podczas tworzenia plików projektu, można użyć dowolnego rozszerzenia pliku, które chcesz. Jednak aby ułatwić rozwiązania dla innych użytkowników dowiedzieć się, należy użyć tych typowych Konwencji:

- Użyj rozszerzenia plików Proj, podczas tworzenia pliku projektu, który kompiluje projekty.
- Użyj rozszerzenia .targets, podczas tworzenia pliku projektu do wielokrotnego użytku, aby zaimportować do innych plików projektów. Pliki z rozszerzeniem .targets zwykle nie zbudować prawie wszystko samodzielnie, po prostu zawierają instrukcje, które można importować do plików .proj.

### <a name="integration-with-deployment-technologies"></a>Integracja z usługą technologie wdrażania

Jeśli znasz już projektów aplikacji sieci web w programie Visual Studio 2010, takich jak aplikacje sieci web ASP.NET i aplikacji sieci web platformy ASP.NET MVC, będziesz wiedzieć, że te projekty zawierają wbudowaną obsługę pakowanie i wdrażanie aplikacji sieci web w środowisku docelowym. **Właściwości** strony, aby uwzględnić te projekty **pakowaniu/publikowaniu Web** i **Pakuj/Publikuj SQL** kart, które umożliwiają skonfigurowanie sposobu składniki usługi Aplikacja są spakowane i wdrażane. Pokazuje to **pakowaniu/publikowaniu Web** karty:

![](understanding-the-project-file/_static/image1.png)

Podstawową używaną technologią za te możliwości jest znany jako Web potok publikowania (WPP). Potok WPP zasadniczo oferuje MSBuild i [narzędzia Web Deploy](https://go.microsoft.com/?linkid=9805122) ze sobą w celu zapewnienia kompletnego procesu kompilacji, tworzenie pakietów i wdrażanie aplikacji sieci web.

Dobra wiadomość jest taka, możesz skorzystać z punktów integracji udostępnianych przez potok WPP podczas tworzenia plików niestandardowego projektu dla projektów sieci web. Instrukcje dotyczące wdrażania można uwzględnić w pliku projektu, co pozwala na kompilowanie projektów, tworzyć pakiety wdrażania sieci web i zainstalować te pakiety na serwerach zdalnych za pośrednictwem pliku pojedynczego projektu i jedno wywołanie do programu MSBuild. Można również wywołać inne pliki wykonywalne, jako część procesu kompilacji. Na przykład można uruchomić narzędzie wiersza polecenia VSDBCMD.exe wdrożyć bazę danych z pliku schematu. W miarę upływu w tym temacie zobaczysz, jak można było korzystać z tych możliwości, aby spełnić wymagania organizacji scenariuszy wdrażania.

> [!NOTE]
> Aby uzyskać więcej informacji na temat sposobu działania procesu wdrażania aplikacji sieci web, zobacz [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Struktura pliku projektu

Zanim przyjrzymy się proces kompilacji, które bardziej szczegółowo, warto trwa kilka minut, aby zapoznać się z podstawowa struktura pliku projektu programu MSBuild. W tej sekcji omówiono typowe elementy, które będzie występować podczas przeglądu, edytowania lub tworzenia pliku projektu. W szczególności dowiesz się:

- Jak używać *właściwości* Zarządzanie zmienne dla procesu kompilacji.
- Jak używać *elementów* do identyfikowania danych wejściowych do procesu kompilacji, takie jak pliki kodu.
- Jak używać *cele* i *zadania* zapewnienie instrukcje wykonania MSBuild, za pomocą *właściwości* i *elementów* zdefiniowanej w innym miejscu w plik projektu.

To pokazuje relację między kluczami elementów w pliku projektu programu MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Element projektu

[Projektu](https://msdn.microsoft.com/library/bcxfsh87.aspx) element jest elementem głównym każdego pliku projektu. Oprócz identyfikowania schemat XML pliku projektu **projektu** element może zawierać atrybutów, aby określić punkty wejścia dla procesu kompilacji. Na przykład w [Contact Manager przykładowe rozwiązanie](the-contact-manager-solution.md), *Publish.proj* plik Określa, czy kompilacja powinna być uruchamiana, wywołując element docelowy o nazwie **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Właściwości i warunki

Plik projektu musi zwykle zapewniają wiele różnych rodzajów informacji, aby pomyślnie tworzyć i wdrażać swoje projekty. Te elementy informacje mogą obejmować nazwy serwera, parametry połączenia, poświadczenia, konfiguracje kompilacji, źródłowe i docelowe ścieżki plików i wszelkie inne informacje, które chcesz dołączyć do obsługi dostosowywania. W pliku projektu, właściwości muszą być zdefiniowane w ramach [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elementu. Właściwości programu MSBuild składają się z pary klucz wartość. W ramach **PropertyGroup** elementu, nazwa elementu definiuje klucz właściwości i zawartość elementu definiuje wartości właściwości. Na przykład można zdefiniować właściwości o nazwie **ServerName** i **ConnectionString** do przechowywania statycznej serwera nazwę i parametry połączenia.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Aby pobrać wartości właściwości, należy użyć formatu <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Na przykład, aby pobrać wartość <strong>ServerName</strong> właściwość, należy wpisać:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Zostaną wyświetlone zapoznać się z przykładami i kiedy należy używać wartości właściwości w dalszej części tego tematu.


Osadzanie informacji o jako właściwości statycznych w pliku projektu nie zawsze jest idealnym rozwiązaniem podejście do zarządzania procesem kompilacji. W wielu scenariuszach będziesz chciał uzyskania informacji z innych źródeł lub zwiększenie możliwości dostępnych dla użytkownika o podanie informacji z poziomu wiersza polecenia. Program MSBuild umożliwia określenie dowolnej wartości właściwości jako parametr wiersza polecenia. Na przykład użytkownik może podać wartość dla **ServerName** po użytkownik uruchamia MSBuild.exe w wierszu polecenia.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat argumentów i parametrów można używać z MSBuild.exe, zobacz [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).


Aby uzyskać wartości zmiennych środowiskowych i właściwości projektu wbudowanych, można użyć tej samej składni właściwości. Wiele powszechnie używanych właściwości są zdefiniowane dla Ciebie, a następnie używać w plikach projektu, łącznie z nazwą odpowiedniego parametru. Na przykład, aby pobrać Bieżąca platforma projektu&#x2014;na przykład **x86** lub **AnyCpu**&#x2014;mogą obejmować **$(Platform)** odwołaniem do właściwości w plik projektu. Aby uzyskać więcej informacji, zobacz [makra dla poleceń i właściwości kompilacji](https://msdn.microsoft.com/library/c02as0cs.aspx), [wspólne właściwości projektów MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), i [właściwości zastrzeżonych](https://msdn.microsoft.com/library/ms164309.aspx).

Właściwości są często używane w połączeniu z *warunki*. Obsługuje większość elementów MSBuild **warunek** atrybut, który pozwala określić kryteria, na których program MSBuild należy ocenić element. Na przykład należy wziąć pod uwagę tę definicję właściwości:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Gdy program MSBuild przetwarza tę definicję właściwości, najpierw sprawdza, czy **$(OutputRoot)** wartość właściwości jest dostępna. Jeśli wartość właściwości jest puste&#x2014;innymi słowy, użytkownik nie dostarczył wartość tej właściwości&#x2014;warunek to **true** i wartość właściwości jest równa **... \Publish\Out**. Jeśli użytkownik udostępnił wartość tej właściwości, warunek jest **false** i nie jest używana wartość właściwości statycznej.

Aby uzyskać więcej informacji na temat różnych sposobów, w którym można określić warunki, zobacz [warunki MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementy i grupy elementów

Jedną z ważnych ról w pliku projektu jest zdefiniowanie danych wejściowych do procesu kompilacji. Zazwyczaj te dane wejściowe są pliki&#x2014;kodu pliki, pliki konfiguracji, pliki poleceń i innych plików, które należy przetworzyć lub Kopiuj jako część procesu kompilacji. W schemacie projektu MSBuild te dane wejściowe są reprezentowane przez [elementu](https://msdn.microsoft.com/library/ms164283.aspx) elementów. W pliku projektu, elementy muszą być zdefiniowane w ramach [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elementu. Podobnie jak w przypadku **właściwość** elementów można nazwać **elementu** elementu dowolny sposób. Jednakże, należy określić **Include** atrybut do identyfikowania plików lub symbolu wieloznacznego, który reprezentuje element.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Określając wielu **elementu** elementów o takiej samej nazwie, efektywnie tworzysz nazwana Lista zasobów. Dobrym sposobem, aby zobaczyć to w akcji jest zapoznaj się z jednego z plików projektu, tworzonych w programie Visual Studio. Na przykład *ContactManager.Mvc.csproj* plik w przykładowym rozwiązaniu zawiera wiele elementów grup, każda z kilku identycznie nazwane **elementu** elementów.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


W ten sposób w pliku projektu jest poinstruować program MSBuild do tworzenia list plików, które muszą zostać przetworzone w taki sam sposób&#x2014; **odwołania** lista zawiera zestawy, które muszą być spełnione dla udanej kompilacji  **Skompilować** lista zawiera pliki kodu, które muszą być skompilowane, oraz **zawartości** lista zawiera zasoby, które muszą zostać skopiowane niezmieniony. Omówimy sposób odwołuje się do procesu kompilacji i używa tych elementów w dalszej części tego tematu.

Elementy może również obejmować [itemmetadata —](https://msdn.microsoft.com/library/ms164284.aspx) elementów podrzędnych. Te są zdefiniowane przez użytkownika pary klucz wartość i zasadniczo reprezentuje właściwości, które są specyficzne dla tego elementu. Na przykład wiele **skompilować** obejmują elementy w pliku projektu **DependentUpon** elementów podrzędnych.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Oprócz metadanych elementu utworzone przez użytkownika wszystkie elementy są przypisane różne metadane wspólne przy tworzeniu. Aby uzyskać więcej informacji, zobacz [metadane dobrze znanego elementu](https://msdn.microsoft.com/library/ms164313.aspx).


Możesz utworzyć **ItemGroup** elementów w obrębie głównego poziomu **projektu** element lub w ramach określonych **docelowej** elementów. **ItemGroup** obsługują także elementy **warunek** atrybuty, które umożliwia dostosowywanie danych wejściowych do procesu kompilacji, zgodnie z warunkami, takich jak konfiguracja projektu lub platformy.

### <a name="targets-and-tasks"></a>Elementy docelowe i zadania

W schemacie MSBuild [zadań](https://msdn.microsoft.com/library/77f2hx1s.aspx) element reprezentuje poszczególnych kompilacji instrukcji (lub zadań). Program MSBuild obejmuje wiele wstępnie zdefiniowanych zadań. Na przykład:

- **Kopiowania** zadania kopiowania plików do nowej lokalizacji.
- **Csc** zadanie wywołuje kompilatora Visual C#.
- **Vbc** zadanie wywołuje kompilatora Visual Basic.
- **Exec** zadanie jest uruchamiane określonego programu.
- **Komunikat** zadań zapisuje komunikat do rejestratora.

> [!NOTE]
> Aby uzyskać szczegółowe informacje, które są dostępne gotowych zadań, zobacz [odwołanie do zadania MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Aby uzyskać więcej informacji na temat zadań, w tym jak utworzyć własne zadania niestandardowe zobacz [zadania programu MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Zadania zawsze musi być zawarta w [docelowej](https://msdn.microsoft.com/library/t50z2hka.aspx) elementów. A **docelowej** element to zbiór jednego lub więcej zadań, które są wykonywane sekwencyjnie, a plik projektu może zawierać wiele elementów docelowych. Jeśli chcesz uruchamiać zadania lub zestawu zadań, można wywołać docelowy, który je zawiera. Załóżmy na przykład, która rejestruje komunikat pliku prostego projektu.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Obiekt docelowy z wiersza polecenia, można wywołać za pomocą **/t** przełącznik, aby określić cel.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternatywnie, można dodać **defaulttargets —** atrybutu **projektu** element, aby określić cele, które chcesz wywołać.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


W takim przypadku nie trzeba określić obiekt docelowy z wiersza polecenia. Można po prostu określić w pliku projektu i wywoła MSBuild **FullPublish** docelowego dla Ciebie.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Obiekty docelowe i zadania mogą obejmować **warunek** atrybutów. W efekcie można pominąć cały elementów docelowych lub poszczególne zadania, jeśli są spełnione określone warunki.

Ogólnie rzecz biorąc podczas tworzenia użytecznych zadań i obiektów docelowych, konieczne będzie odwoływać się do właściwości i elementy, które zostały zdefiniowane w innym miejscu w pliku projektu:

- Aby użyć wartości właściwości, wpisz <strong>$(</strong><em>PropertyName</em><strong>)</strong>, gdzie <em>PropertyName</em> nazywa się <strong>właściwość</strong> element lub nazwę parametru.
- Aby użyć elementu, wpisz <strong>@(</strong><em>ItemName</em><strong>)</strong>, gdzie <em>ItemName</em> nazywa się <strong>elementu</strong> elementu.

> [!NOTE]
> Należy pamiętać, że jeśli tworzysz wiele elementów o takiej samej nazwie, którą tworzysz listy. Z kolei jeśli tworzysz wiele właściwości o takiej samej nazwie, ostatnią wartość właściwości, należy podać zastąpią wszelkie poprzednie właściwości o takiej samej nazwie&#x2014;właściwość może zawierać tylko jedną wartość.


Na przykład w *Publish.proj* plików w przykładowym rozwiązaniu, Przyjrzyj się **BuildProjects** docelowej.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


W tym przykładzie można zaobserwować tych kluczowych zagadnieniach:

- Jeśli **BuildingInTeamBuild** parametr jest określony i ma wartość **true**, żadne zadania należące do tego elementu zostanie wykonana.
- Element docelowy zawiera jedno wystąpienie [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) zadania. To zadanie pozwala tworzyć innych projektów programu MSBuild.
- **ProjectsToBuild** element jest przekazywany do zadania. Ten element może reprezentować listę plików projektu lub rozwiązania, wszystko to zdefiniowane przez **ProjectsToBuild** elementu elementów w obrębie grupy elementów. W tym przypadku **ProjectsToBuild** element odwołuje się do pliku jednego rozwiązania.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Wartości właściwości przekazany do **MSBuild** zadanie obejmuje parametrów o nazwie **OutputRoot** i **konfiguracji**. Jeśli nie są ustawione na wartości parametrów, jeśli są one udostępniane lub wartości właściwości statycznej.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Możesz również zobaczyć, że **MSBuild** zadanie wywołuje obiekt docelowy o nazwie **kompilacji**. Jest to jeden z kilku wbudowanych obiektów docelowych, które są powszechnie używane w plikach projektu programu Visual Studio i są dostępne w plikach projektu niestandardowych, jak **kompilacji**, **czysty**, **odbudować**, i **publikowania**. Dowiesz się więcej o korzystaniu z obiektów docelowych i zadań, do kontroli procesu kompilacji, a także o **MSBuild** zadań w szczególności w dalszej części tego tematu.

> [!NOTE]
> Aby uzyskać więcej informacji na temat elementów docelowych, zobacz [elementów docelowych MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Podział plików projektu w celu obsługi wielu środowisk

Załóżmy, że chcesz można było wdrożyć rozwiązanie w wielu środowiskach, takich jak serwery testowe, tymczasowe platformy i środowisk produkcyjnych. Konfiguracja może znacząco różnią się między tymi środowiskami&#x2014;nie tylko pod względem serwerów nazw, parametry połączenia i tak dalej, ale też potencjalnie pod względem poświadczeń, ustawienia zabezpieczeń i wiele innych czynników. Jeśli potrzebujesz to zrobić regularnie, nie jest tak naprawdę wskazane edytować wiele właściwości w pliku projektu, za każdym razem, gdy przełączasz się środowiska docelowego. Nie jest to idealne rozwiązanie będą musieli nieskończone listę wartości właściwości do procesu kompilacji.

Na szczęście jest alternatywą. Program MSBuild umożliwia podzielić konfiguracji kompilacji na wielu plikach projektów. Aby zobaczyć, jak to działa, w przykładowym rozwiązaniu, zwróć uwagę, że istnieją dwa pliki niestandardowego projektu:

- *Publish.Proj*, który zawiera właściwości, elementy, i obiekty docelowe, które są wspólne dla wszystkich środowisk.
- *ENV Dev.proj*, który zawiera właściwości, które są specyficzne dla środowiska deweloperskiego.

Teraz należy zauważyć, że *Publish.proj* plik zawiera [importu](https://msdn.microsoft.com/library/92x05xfs.aspx) elementu tuż poniżej otwarcia **projektu** tagu.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Zaimportować** element jest używany w celu zaimportowania zawartości innego pliku projektu MSBuild do bieżącego pliku projektu MSBuild. W tym przypadku **TargetEnvPropsFile** parametr zawiera nazwę pliku projektu, który chcesz zaimportować. Po uruchomieniu programu MSBuild, możesz podać wartość dla tego parametru.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


To skutecznie Scala zawartość tych dwóch plików pliku pojedynczego projektu. W ten sposób można utworzyć jeden projekt zawierający konfigurację kompilacji uniwersalnych i wiele plików dodatkowych projekt zawierający właściwości specyficzne dla środowiska. W rezultacie po prostu uruchomienie polecenia za pomocą wartości parametru różnych pozwala wdrażać rozwiązania do innego środowiska.

![](understanding-the-project-file/_static/image3.png)

Podział plików projektu w ten sposób jest dobrym rozwiązaniem, należy wykonać. Umożliwia deweloperom wdrażanie w wielu środowiskach za pomocą jednego polecenia unikając dublowania właściwości kompilacji uniwersalnych w wielu plikach projektów.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania pliki projektu specyficznego dla środowiska dla środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Wniosek

W tym temacie podano ogólne wprowadzenie do plików projektu MSBuild i wyjaśniono sposób tworzenia własnych niestandardowych plików projektu do kontrolowania procesu kompilacji. Również wprowadzonymi koncepcji dzielenia plików projektu w instrukcje universal kompilacji i właściwości specyficzne dla środowiska kompilacji, aby ułatwić tworzenie i wdrażanie projektów do wielu miejsc docelowych.

Następny temat [objaśnienie procesu kompilacji](understanding-the-build-process.md), zapewnia lepszy wgląd w sposób korzystania plików projektu w celu sterowania kompilowania i wdrażania przez wdrożenie rozwiązania przy użyciu realistycznej stopień złożoności.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i potok WPP, zobacz [wewnątrz aparatu Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzednie](setting-up-the-contact-manager-solution.md)
> [dalej](understanding-the-build-process.md)
