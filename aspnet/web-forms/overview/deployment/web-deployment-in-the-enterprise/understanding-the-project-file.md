---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Opis pliku projektu | Dokumentacja firmy Microsoft
author: jrjlee
description: "Istotą procesem kompilacji i wdrażania znajdują się pliki projektu Microsoft kompilacji Engine (MSBuild). W tym temacie rozpoczyna się od omówienie MSBuild..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-project-file"></a>Opis pliku projektu
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Istotą procesem kompilacji i wdrażania znajdują się pliki projektu Microsoft kompilacji Engine (MSBuild). W tym temacie rozpoczyna się od omówienie MSBuild i pliku projektu. Opisuje najważniejsze składniki, które będzie napotkać podczas pracy z plikami projektu i działa za pośrednictwem przykład stosowania pliki projektu do wdrażania aplikacji rzeczywistych.
> 
> Zawartość:
> 
> - Jak program MSBuild używa pliki projektu MSBuild do tworzenia projektów.
> - Jak program MSBuild integruje się z technologii wdrażania, takie jak usługi Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy).
> - Jak zrozumieć najważniejsze składniki pliku projektu.
> - Jak można użyć pliki projektu do tworzenia i wdrażania aplikacji złożonych.


## <a name="msbuild-and-the-project-file"></a>MSBuild i pliku projektu

Podczas tworzenia i tworzenie rozwiązań programu Visual Studio, Visual Studio będzie korzystać MSBuild do tworzenia każdego projektu w rozwiązaniu. Każdy projekt programu Visual Studio zawiera plik projektu programu MSBuild z rozszerzeniem odzwierciedlający typ projektu & #x 2014; na przykład projektu C# (.csproj), projekt Visual języku (.vbproj) lub projekt bazy danych (.dbproj). Aby można było skompilować projekt, MSBuild musi przetworzyć pliku projektu skojarzony z projektem. Plik projektu jest dokument XML, który zawiera wszystkie informacje i wskazówki, że MSBuild wymaga, aby skompilować projekt, takie jak zawartość, aby uwzględnić wymagania dotyczące platformy, informacje na temat wersji, serwer sieci web lub ustawień serwera bazy danych i zadania, które należy wykonać.

Pliki projektu MSBuild są oparte na [schematu XML programu MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), i w związku z tym procesu kompilacji jest całkowicie otwarty i przejrzysty. Ponadto nie trzeba zainstalować program Visual Studio Aby korzystać z aparatu MSBuild & #x 2014; plik wykonywalny MSBuild.exe jest częścią programu .NET Framework i można go uruchomić z wiersza polecenia. Deweloperzy mogą jednostki własne pliki projektu MSBuild nałożyć zaawansowane i szczegółową kontrolę nad jak projekty są wbudowane i wdrażane za pomocą schematu XML programu MSBuild. Te pliki projektu niestandardowych działa w taki sam sposób, jak pliki projektu programu Visual Studio automatycznie generowanych przez.

> [!NOTE]
> Umożliwia także pliki projektu programu MSBuild w usłudze Team Build w Team Foundation Server (TFS). Na przykład można użyć plików projektu w scenariuszach ciągłej integracji (CI) do automatyzowania wdrażania w środowisku testowym, po zaewidencjonowaniu nowy kod. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Team Foundation Server dla automatycznego wdrażania Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Konwencje nazewnictwa plików projektu

Podczas tworzenia plików projektu, można użyć dowolnego rozszerzenie pliku, który chcesz. Jednak aby rozwiązań ułatwia użytkownikom zrozumienie, należy użyć tych typowych konwersji:

- Użyj rozszerzenia .proj podczas tworzenia pliku projektu, która tworzy projektów.
- Użyj rozszerzenia .targets, tworząc plik projektu wielokrotnego użytku do zaimportowania do innych plików projektu. Pliki z rozszerzeniem .targets zwykle nie kompilacji niczego samodzielnie, po prostu zawierają instrukcje, które można importować do plików .proj.

### <a name="integration-with-deployment-technologies"></a>Integracja z technologii wdrażania

Jeśli się projekty aplikacji sieci web w programie Visual Studio 2010, takich jak aplikacje sieci web ASP.NET i aplikacji sieci web platformy ASP.NET MVC, wiadomo, że te projekty obejmują wbudowaną obsługę pakowania i wdrażania aplikacji sieci web w środowisku docelowym. **Właściwości** strony Projekty te obejmują **pakowaniu/publikowaniu Web** i **Pakuj/Publikuj SQL** karty, których można użyć do skonfigurowania jak składniki sieci aplikacji są umieszczone i wdrożone. Pokazuje **pakowaniu/publikowaniu Web** karty:

![](understanding-the-project-file/_static/image1.png)

Podstawową technologią za tych funkcji jest znany jako sieci Web potok publikowania (WPP). WPP zasadniczo przełącza MSBuild i [narzędzia Web Deploy](https://go.microsoft.com/?linkid=9805122) ze sobą w celu zapewnienia ukończenia procesu kompilacji pakietu i wdrażania aplikacji sieci web.

Dobre wieści jest, że można wykorzystać punktów integracji, które zapewnia WPP podczas tworzenia plików projektów niestandardowych dla projektów sieci web. Instrukcje dotyczące wdrażania można uwzględnić w pliku projektu, który służy do tworzenia projektów, tworzy pakietu wdrożeniowego sieci web i zainstalować te pakiety na serwerach zdalnych za pośrednictwem pliku pojedynczego projektu i wywołanie MSBuild. Możesz także wywołać inne pliki wykonywalne jako część procesu kompilacji. Na przykład można uruchomić narzędzie wiersza polecenia VSDBCMD.exe, aby wdrożyć bazę danych z pliku schematu. W trakcie tego tematu zobaczysz, jak można korzystać z tych funkcji w celu spełnienia wymagań przedsiębiorstwa scenariuszy wdrażania.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących sposobu działania procesu wdrażania aplikacji sieci web, zobacz [omówienie wdrażania projektu aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomia pliku projektu

Obejrzyj procesu tworzenia bardziej szczegółowo, jest przed warte kilka chwil, aby zapoznać się z podstawowej struktury pliku projektu programu MSBuild. Ta sekcja zawiera omówienie elementów częściej napotykanych podczas Przejrzyj, edytowania lub tworzenia pliku projektu. W szczególności dowiesz się:

- Jak używać *właściwości* do zarządzania zmienne dla procesu kompilacji.
- Jak używać *elementów* Aby zidentyfikować dane wejściowe, aby proces kompilacji, takich jak pliki kodu.
- Jak używać *cele* i *zadania* zapewniające instrukcje wykonywania dla programu MSBuild, za pomocą *właściwości* i *elementów* zdefiniowany w innym miejscu w plik projektu.

Oznacza to relacji między elementami klucza w pliku projektu programu MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Element projektu

[Projektu](https://msdn.microsoft.com/library/bcxfsh87.aspx) element jest elementem głównym każdego pliku projektu. Oprócz identyfikowania schematu XML w pliku projektu **projektu** elementu mogą zawierać atrybutów, aby określić punkty wejścia procesu kompilacji. Na przykład w [kontaktów Menedżerze przykładowe rozwiązanie](the-contact-manager-solution.md), *Publish.proj* pliku Określa, czy kompilacja ma zostać uruchomiony przez wywołanie metody docelowy o nazwie **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Właściwości i warunki

Plik projektu potrzebuje zazwyczaj zapewniają wiele różnych rodzajów informacji, aby pomyślnie tworzyć i wdrażać własne projekty. Te elementy informacji mogą obejmować nazwy serwera, parametry połączenia, poświadczenia, konfiguracje kompilacji, ścieżki plików źródłowych i docelowych i inne informacje, które chcesz dołączyć do obsługi dostosowania. W pliku projektu właściwości muszą być zdefiniowane w ramach [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elementu. Właściwości programu MSBuild składają się z pary klucz wartość. W ramach **PropertyGroup** element, nazwa elementu definiuje klucz właściwości i zawartość elementu definiuje wartość właściwości. Na przykład można zdefiniować właściwości o nazwie **ServerName** i **ConnectionString** na przechowywanie parametrów połączenia i nazwę serwera statycznych.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Aby pobrać wartości właściwości, użyj formatu **$(***PropertyName***) ***.* Na przykład, aby pobrać wartość **ServerName** właściwości, należy wpisać:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Zostanie wyświetlony się przykłady tego, jak i kiedy należy używać wartości właściwości w dalszej części tego tematu.


Osadzanie informacji o jako statycznej właściwości w pliku projektu nie zawsze jest idealne podejście do zarządzania procesu kompilacji. W wiele scenariuszy można uzyskać informacji z innych źródeł lub zwiększenie możliwości dostępnych dla użytkownika o podanie informacji z wiersza polecenia. MSBuild umożliwia określenie wartości właściwości jako parametr wiersza polecenia. Na przykład użytkownik może podać wartość dla **ServerName** gdy użytkownik uruchamia MSBuild.exe w wierszu polecenia.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na argumenty i przełączników, których można używać z MSBuild.exe, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Za pomocą tej samej składni właściwości można uzyskać wartości zmiennych środowiskowych i właściwości projektu wbudowanych. Wiele typowych właściwości są definiowane dla Ciebie i używać w plikach projektu przez dołączenie nazwy parametru odpowiednie. Na przykład, aby pobrać Bieżąca platforma projektu & #x 2014; na przykład **x86** lub **AnyCpu**& #x 2014; może zawierać **$(Platform)** odwołania do właściwości w plik projektu. Aby uzyskać więcej informacji, zobacz [makra dla poleceń kompilacji oraz właściwości](https://msdn.microsoft.com/library/c02as0cs.aspx), [wspólne właściwości projektów MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), i [właściwości zastrzeżone](https://msdn.microsoft.com/library/ms164309.aspx).

Właściwości są często używane w połączeniu z *warunki*. Obsługuje większość elementów MSBuild **warunku** atrybut, który pozwala określić kryteria, na których MSBuild należy ocenić elementu. Rozważmy na przykład tej definicji właściwości:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Gdy program MSBuild przetwarza tej definicji właściwości, najpierw sprawdza, czy **$(OutputRoot)** wartość właściwości jest dostępna. Jeśli wartość właściwości jest puste & #x 2014; innymi słowy, użytkownik nie podanej wartości dla tej właściwości & #x 2014; wyrażenie **true** i ma ustawioną wartość właściwości **... \Publish\Out**. Jeśli użytkownik ma podanej wartości dla tej właściwości, wyrażenie **false** i nie jest używana wartość właściwości statycznej.

Aby uzyskać więcej informacji na różne sposoby, w którym można określić warunki, zobacz [warunki MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementy i grupy elementów

Jedną z ról ważne pliku projektu jest określenie wejścia do procesu kompilacji. Zwykle te dane wejściowe są pliki & #x 2014 r., pliki kodu, pliki konfiguracji, pliki poleceń i inne pliki potrzebne do przetwarzania lub skopiuj jako część procesu kompilacji. W schemacie projektu MSBuild te dane wejściowe są reprezentowane przez [elementu](https://msdn.microsoft.com/library/ms164283.aspx) elementów. W pliku projektu elementów musi być zdefiniowany w ramach [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elementu. Podobnie jak **właściwości** elementy, można określić nazwę **elementu** elementu jednak chcesz. Jednak należy określić **Include** atrybutu, aby zidentyfikować pliku lub symbolu wieloznacznego, który reprezentuje element.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Określając wielu **elementu** elementów o takiej samej nazwie, efektywnie tworzysz nazwana Lista zasobów. Dobrym sposobem Zobacz to działanie jest zapoznaj się z wewnątrz jednej plików projektu, które tworzy program Visual Studio. Na przykład *ContactManager.Mvc.csproj* plik w przykładowe rozwiązanie zawiera wiele elementów grup, każda z kilku o identycznej nazwie **elementu** elementów.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


W ten sposób pliku projektu jest poinstruowanie MSBuild, aby utworzyć listę plików, które muszą być przetworzone w tej samej sposób & #x 2014; **odwołania** lista zawiera zestawy, które muszą być w celu pomyślnego utworzenia kompilacji **Skompilować** lista zawiera pliki kodu, które muszą być skompilowane i **zawartości** lista zawiera zasoby, które muszą zostać skopiowane niezmieniony. Wyjaśniono, jak proces kompilacji przywołuje i używa tych elementów w dalszej części tego tematu.

Element mogą również obejmować [itemmetadata —](https://msdn.microsoft.com/library/ms164284.aspx) elementy podrzędne. Są zdefiniowane przez użytkownika pary klucz wartość i zasadniczo reprezentuje właściwości, które są specyficzne dla tego elementu. Na przykład wiele **skompilować** obejmują elementy elementu w pliku projektu **DependentUpon** elementy podrzędne.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Oprócz metadanych elementu utworzonych przez użytkownika wszystkie elementy są przypisane różne metadane wspólnej przy tworzeniu. Aby uzyskać więcej informacji, zobacz [metadane dobrze znanego elementu](https://msdn.microsoft.com/library/ms164313.aspx).


Można utworzyć **ItemGroup** elementów na poziomie głównym **projektu** element lub w określonych **docelowej** elementów. **ItemGroup** elementów również obsługiwać **warunku** atrybuty, które można dostosować dane wejściowe, aby proces kompilacji warunki, takie jak konfiguracja projektu lub platformy.

### <a name="targets-and-tasks"></a>Obiektów docelowych i zadań

W schemacie MSBuild [zadań](https://msdn.microsoft.com/library/77f2hx1s.aspx) element reprezentuje kompilacji pojedynczych instrukcji (lub zadania). MSBuild zawiera wiele wstępnie zdefiniowanych zadań. Na przykład:

- **Kopiowania** zadań kopiuje pliki do nowej lokalizacji.
- **Csc** zadań wywołuje kompilatora Visual C#.
- **Vbc** zadań wywołuje kompilator Visual Basic.
- **Exec** zadanie jest uruchamiane określonego programu.
- **Komunikat** zadań zapisuje komunikat rejestrator.

> [!NOTE]
> Szczegółowe informacje dotyczące zadań, które są dostępne fabrycznej, zobacz [odwołanie do zadania MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Aby uzyskać więcej informacji na temat zadań, takich jak tworzenie własnych niestandardowych zadań, zobacz [zadania programu MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Zadania zawsze musi być zawarty w [docelowej](https://msdn.microsoft.com/library/t50z2hka.aspx) elementów. A **docelowej** element to zbiór jednego lub więcej zadań, które są wykonywane sekwencyjnie i plik projektu może zawierać wielu elementów docelowych. Jeśli chcesz uruchomić zadanie lub zestaw zadań wywołuje element docelowy, który je zawiera. Załóżmy na przykład plik prostego projektu, który rejestruje komunikat.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Element docelowy w wierszu polecenia można wywołać za pomocą **/t** przełącznik, aby określić cel.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Alternatywnie można dodać **defaulttargets —** atrybutu **projektu** element, aby określić cele, które chcesz wywołać.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


W takim przypadku nie trzeba określić obiekt docelowy z wiersza polecenia. Po prostu można określić plik projektu i wywoła MSBuild **FullPublish** docelowy dla Ciebie.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Zarówno obiektów docelowych i zadań mogą obejmować **warunku** atrybutów. Tak można pominąć całego elementów docelowych lub poszczególne zadania, jeśli są spełnione następujące warunki.

Ogólnie rzecz biorąc podczas tworzenia użytecznych zadań i elementów docelowych, konieczne będzie odwoływać się do właściwości i elementy, które zostały zdefiniowane w innym miejscu w pliku projektu:

- Aby użyć wartości właściwości, wpisz **$(***PropertyName***)**, gdzie *PropertyName* jest nazwą **właściwości** element lub nazwę parametr.
- Aby użyć elementu, wpisz **@(***ItemName***)**, gdzie *ItemName* jest nazwą **elementu** elementu.

> [!NOTE]
> Należy pamiętać, że jeśli tworzysz wiele elementów o takiej samej nazwie, tworzysz listy. Z kolei w przypadku utworzenia wielu właściwości o tej samej nazwie, ostatnią wartość właściwości podane spowoduje zastąpienie poprzedniej właściwości z tej samej nazwy & #x 2014; właściwość może zawierać tylko jedną wartość.


Na przykład w *Publish.proj* plików w przykładowe rozwiązanie, Przyjrzyjmy się **BuildProjects** docelowej.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


W tym przykładzie można zaobserwować pod uwagę:

- Jeśli **BuildingInTeamBuild** parametr został określony i ma wartość **true**, będzie można wykonać żadnych zadań w obrębie tego celu.
- Element docelowy zawiera pojedyncze wystąpienie [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) zadań. To zadanie umożliwia tworzenie innych projektów MSBuild.
- **ProjectsToBuild** elementu jest przekazany do zadania. Ten element może reprezentować listę plików projektu lub rozwiązania, wszystkie zdefiniowane przez **ProjectsToBuild** elementu elementów w obrębie grupy elementów. W takim przypadku **ProjectsToBuild** odwołuje się plik rozwiązania pojedynczy element.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Przekazany do wartości właściwości **MSBuild** zadanie obejmuje parametrów o nazwie **OutputRoot** i **konfiguracji**. Jeśli nie są one są ustawione na wartości parametru, jeśli są one udostępniane lub wartości właściwości statycznej.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Możesz też sprawdzić, które **MSBuild** zadań wywołuje element docelowy o nazwie **kompilacji**. To jest kilka wbudowanych elementów docelowych, które są powszechnie używane w plikach projektu Visual Studio i są dostępne w plikach projektu niestandardowych tak samo, jak **kompilacji**, **wyczyść**, **odbudować**, i **publikowania**. Dowiesz się więcej o korzystaniu z obiektów docelowych i zadań do kontroli procesów kompilacji i o **MSBuild** zadań w szczególności w dalszej części tego tematu.

> [!NOTE]
> Aby uzyskać więcej informacji na obiekty docelowe, zobacz [docelowych elementów MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Podział plików projektu do obsługi wielu środowisk

Załóżmy, że chcesz można było wdrożyć rozwiązanie w wielu środowiskach, takich jak serwery testu, przemieszczania platform i środowisk produkcyjnych. Konfiguracji mogą się znacznie różnić między tych środowisk & #x 2014; nie tylko pod względem nazwy serwerów, parametry połączenia i tak dalej, ale także potencjalnie pod względem poświadczeń, ustawienia zabezpieczeń i wiele innych czynników. Jeśli potrzebujesz w tym celu regularnie, nie jest naprawdę wskazane edytować wiele właściwości w pliku projektu, zawsze Przełącz środowiska docelowego. Nie jest to idealne rozwiązanie wymagające nieskończone lista wartości właściwości do procesu kompilacji.

Na szczęście stanowi alternatywę. MSBuild pozwala podzielić konfiguracji kompilacji na wielu plikach projektów. Aby zobaczyć, jak to działa, w rozwiązaniu próbki Zwróć uwagę, że dostępne są dwa pliki projektu niestandardowych:

- *Publish.Proj*, który zawiera właściwości, elementy, i elementów docelowych, które są wspólne dla wszystkich środowisk.
- *ENV Dev.proj*, który zawiera właściwości, które są specyficzne dla środowiska deweloperskiego.

Teraz należy zauważyć, że *Publish.proj* plik zawiera [importu](https://msdn.microsoft.com/library/92x05xfs.aspx) element znajdujący się bezpośrednio pod otwarcia **projektu** tagu.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**Zaimportować** element służy do importowania zawartość innego pliku projektu MSBuild do bieżącego pliku projektu MSBuild. W takim przypadku **TargetEnvPropsFile** parametru zapewnia nazwę pliku projektu do zaimportowania. Można podać wartość tego parametru podczas uruchamiania programu MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


To skutecznie łączy zawartość tych dwóch plików do tworzenia projektów. W ten sposób można utworzyć jeden projekt plik zawierający konfigurację kompilacji uniwersalnych i wielu plików dodatkowych projekt zawierający właściwości specyficzne dla danego środowiska. W związku z tym po prostu uruchomiono polecenie z wartością parametru różnych umożliwia wdrażanie rozwiązania do innego środowiska.

![](understanding-the-project-file/_static/image3.png)

Podział plików projektu w ten sposób jest dobrym rozwiązaniem, które należy wykonać. Umożliwia deweloperom wdrażanie w wielu środowiskach za pomocą jednego polecenia, unikając duplikatów właściwości uniwersalnych kompilacji w wielu plikach projektów.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące sposobu dostosowywania pliki projektu określonego środowiska dla środowiska serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Wniosek

W tym temacie podano ogólne wprowadzenie do pliki projektu MSBuild oraz wyjaśniono, jak utworzyć własne niestandardowe plików projektu Aby kontrolować proces kompilacji. On również wprowadzono koncepcję dzielenia pliki projektu do kompilacji uniwersalnych instrukcje i właściwości specyficzne dla środowiska kompilacji, ułatwiają tworzenie i wdrażanie projektów do wielu miejsc docelowych.

Następnym temacie [opis procesu kompilacji](understanding-the-build-process.md), zapewnia uzyskać lepszy wgląd w sposób korzystania pliki projektu do sterowania kompilowanie i wdrażanie przez Instruktaż wdrożenie rozwiązania z realistyczne poziom złożoności.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat wprowadzenie do plików projektu i WPP, zobacz [wewnątrz kompilacji aparatu Microsoft: przy użyciu programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi i Bartholomew łączy, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Poprzednie](setting-up-the-contact-manager-solution.md)
[dalej](understanding-the-build-process.md)
