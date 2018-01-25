---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: "Tworzenie projektów sieci Web ASP.NET w programie Visual Studio 2013 | Dokumentacja firmy Microsoft"
author: tdykstra
description: "W tym temacie opisano opcje tworzenia projektów sieci web ASP.NET w programie Visual Studio 2013 z aktualizacji 3 w tym miejscu są niektóre z nowych funkcji dla c Projektowanie sieci web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Tworzenie projektów sieci Web ASP.NET w programie Visual Studio 2013
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

> W tym temacie opisano opcje tworzenia projektów sieci web ASP.NET w programie Visual Studio 2013 z aktualizacją Update 3
> 
> Oto niektóre z nowych funkcji do tworzenia aplikacji sieci web w porównaniu do wcześniejszych wersji programu Visual Studio:
> 
> - Prosty interfejs użytkownika służący do tworzenia projektów tej oferty [obsługę wielu platform ASP.NET](#add) (Web Forms, MVC i interfejsu API sieci Web).
> - [ASP.NET Identity](#indauth), nowego systemu członkostwa programu ASP.NET, który działa tak samo we wszystkich platform ASP.NET i współpracuje z oprogramowania innego niż IIS hostingu w sieci web.
> - Korzystanie z [Bootstrap](#bootstrap) aby zapewnić dynamiczne możliwości projektowania i tworzenia motywów.
> - Nowe funkcje w formularzach sieci Web, który używany przeznaczonej tylko dla platformy MVC, takie jak [Tworzenie projektu testu automatycznego](#testproj) i [szablonu witryny intranetowej](#winauth).
> 
> Aby uzyskać informacje o sposobie tworzenia projektów sieci web dla usługi w chmurze Azure lub usługi Azure Mobile Services, zobacz [wprowadzenie do usług Azure Cloud Services i platformy ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) i [tworzenie aplikacji Liderzy za pomocą usługi Azure Mobile Services na platformie .NET Zaplecza](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Ten artykuł dotyczy [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) zainstalowane.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projekty aplikacji sieci Web i projektów witryny sieci web

ASP.NET umożliwia wybór między dwa rodzaje projektów sieci web: *sieci web projektów aplikacji* i *projektów witryny sieci web*. Firma Microsoft zaleca projekty aplikacji sieci web dla nowych aplikacji, a ten artykuł dotyczy tylko projektów aplikacji sieci web. Aby uzyskać więcej informacji, zobacz [projekty aplikacji sieci Web i projektów witryny sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) w witrynie MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Omówienie tworzenia projektu aplikacji sieci web

W poniższej procedurze pokazano, jak utworzyć projekt sieci web:

1. Kliknij przycisk **nowy projekt** w **Start** strony lub **pliku** menu.
2. W **nowy projekt** okna dialogowego, kliknij przycisk **Web** w okienku po lewej stronie i **aplikacji sieci Web ASP.NET** w środkowym okienku.

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image1.png)

    Możesz wybrać **chmury** w okienku po lewej stronie, aby utworzyć [usługi w chmurze Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [usługi mobilnej Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), lub [zadań WebJob Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). W tym temacie nie opisano te szablony.
3. W okienku po prawej stronie kliknij **Dodaj usługę Application Insights do projektu** pole wyboru, jeśli chcesz kondycji i monitorowania użycia aplikacji. Aby uzyskać więcej informacji, zobacz [monitorować wydajność w aplikacji sieci web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Określ projektu **nazwa**, **lokalizacji**i inne opcje, a następnie kliknij przycisk **OK**.

    **Nowy projekt ASP.NET** zostanie wyświetlone okno dialogowe.

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Kliknij szablon.

    ![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image3.png)
6. Jeśli chcesz dodać obsługę dodatkowych struktur nie są uwzględnione w szablonie, kliknij odpowiednie pole wyboru. (W podanym przykładzie można dodać MVC i/lub interfejs API sieci Web do projektu formularzy sieci Web.)

    ![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Jeśli chcesz dodać jednostkowy projekt testowy, kliknij przycisk **Dodaj testy jednostkowe**.

    ![Dodawanie testów jednostkowych](creating-web-projects-in-visual-studio/_static/image5.png)
8. Metodę uwierzytelniania inną niż szablon zawiera domyślne, kliknij przycisk **Zmień uwierzytelnianie**.

    ![Konfigurowanie uwierzytelniania przycisku](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Skonfiguruj dialog uwierzytelniania](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Tworzenie aplikacji sieci web lub maszyny wirtualnej na platformie Azure

Visual Studio zawiera funkcje, które ułatwiają pracę z usługami Azure do hostowania aplikacji sieci web. Można na przykład wykonaj wszystkie następujące bezpośrednio w środowisku IDE programu Visual Studio:

- Utwórz i Zarządzaj aplikacji sieci web lub maszyn wirtualnych, które udostępniają aplikacji za pośrednictwem Internetu.
- Wyświetl dzienniki utworzone przez aplikację podczas działania w chmurze.
- Uruchom w trybie debugowania zdalnego podczas uruchamiania aplikacji w chmurze.
- Viiew i zarządzać innymi usługami Azure, takich jak bazy danych SQL.

Możesz [tworzenia konta platformy Azure](https://www.windowsazure.com/pricing/free-trial/) bezpłatnie zawierającej podstawowe usługi, takich jak aplikacje sieci web, a jeśli jesteś subskrybentem MSDN możesz [aktywować korzyści dla](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) który umożliwiają miesięcznych środków na rzecz Microsoft Azure dodatkowe usługi. 

Domyślnie **nowy projekt ASP.NET** okno dialogowe umożliwia tworzenie aplikacji sieci web lub maszyny wirtualnej do nowego projektu sieci web. Jeśli nie chcesz utworzyć nową aplikację sieci web lub maszyny wirtualnej, wyczyść **Hostuj w chmurze** pole wyboru.

![Utwórz zasoby zdalne](creating-web-projects-in-visual-studio/_static/image8.png)

Podpis pola wyboru może być **Hostuj w chmurze** lub **Utwórz zasoby zdalne**, a w obu przypadkach efekt jest taki sam. Jeśli pozostaw zaznaczone pole wyboru, Visual Studio tworzy aplikację sieci web w usłudze Azure App Service domyślnie. Pole listy rozwijanej można użyć do zmiany **maszyny wirtualnej** Jeśli wolisz. Jeśli jeszcze nie zalogowano Cię na platformie Azure, zostanie wyświetlony monit o poświadczenia platformy Azure. Po zalogowaniu, okno dialogowe umożliwia konfigurowanie zasobów, które Visual Studio utworzy dla projektu. Na poniższej ilustracji przedstawiono okno dialogowe dla aplikacji sieci web; Opcje dostępne w przypadku tworzenia maszyny wirtualnej.

![Konfiguruj ustawienia aplikacji Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Aby uzyskać więcej informacji na temat używania tego procesu tworzenia zasobów platformy Azure, zobacz [wprowadzenie do platformy Azure i ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) i [tworzenia maszyny wirtualnej dla witryny sieci web z programem Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

W dalszej części tego artykułu zawiera więcej informacji na temat dostępnych szablonów i ich opcji. Artykuł wprowadza również Bootstrap, układu i motywów framework używane w szablonach.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web Project Templates

Szablony Visual Studio będzie korzystać do tworzenia projektów sieci web. Szablon projektu może tworzyć pliki i foldery w nowym projekcie, zainstalować pakietów NuGet i udostępnia kodu do podstawowe działającą aplikację. Szablony implementuje najnowszych standardów sieci web i służą jedynie do zademonstrowania, że najlepsze rozwiązania dotyczące jak używać technologii ASP.NET, jak również zapewniają szybki start dotyczące tworzenia własnych aplikacji.

Szablony projektów sieci web dla projektów przeznaczonych dla platformy .NET 4.5 lub nowszej wersji programu .NET framework programu Visual Studio 2013 zawiera następujące opcje:

- [Pusty szablon](#empty)
- [Szablon formularzy sieci Web](#wf)
- [Szablon MVC](#mvc)
- [Szablon interfejsu API sieci Web](#webapi)
- [Jeden szablon strony aplikacji](#spa)
- [Szablon usługi mobilnej Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 szablonów](#vs2012)

Można także zainstalować rozszerzenie programu Visual Studio, która zapewnia [szablonu usługi Facebook](#facebook).

Aby uzyskać informacje o sposobie tworzenia projektów przeznaczonych dla platformy .NET 4, zobacz [programu Visual Studio 2012 szablony](#vs2012) dalszej części tego tematu.

Aby uzyskać informacje o sposobie tworzenia aplikacji ASP.NET dla klientów mobilnych, zobacz [Mobile obsługi w programie ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Pusty szablon

Udostępnia pustego szablonu systemu od zera minimalna folderów i plików dla aplikacji sieci web ASP.NET, takich jak plik projektu (*.csproj* lub. *vbproj*) i *Web.config* pliku. Można dodać obsługę sieci Web Forms, MVC i/lub interfejs API sieci Web za pomocą pola wyboru w obszarze **. Dodaj foldery i podstawowe odwołania dla:** etykiety.

W przypadku pustego szablonu są dostępne żadne opcje uwierzytelniania. Funkcje uwierzytelniania jest zaimplementowana w przykładowych aplikacji, a pustego szablonu nie tworzy przykładowej aplikacji.

<a id="wf"></a>
### <a name="web-forms-template"></a>Szablon formularzy sieci Web

Formularze sieci Web, które framework zapewnia następujące funkcje, które pozwalają na szybkie tworzenie witryn sieci web, które w interfejsu użytkownika i danych uzyskiwać dostęp do funkcji:

- WYSIWYG Projektant w programie Visual Studio.
- Formanty serwera, które renderowania kodu HTML i dostosować przez ustawienie właściwości i style.
- Rozbudowane gamę kontroli dostępu do danych oraz wyświetlania danych.
- Model zdarzeń, który opisuje zdarzenia, które można zaprogramować jak zaprogramować aplikacji klienckiej, takich jak WPF.
- Automatyczne zachowania stanu (dane) między żądaniami HTTP.

Ogólnie rzecz biorąc tworzenie aplikacji formularzy sieci Web wymaga mniej programistycznej niż tworzenia tej samej aplikacji za pomocą struktury ASP.NET MVC. Formularze sieci Web nie jest jednak tylko do szybkiego opracowywania aplikacji. Istnieje wiele złożonych aplikacji handlowych i struktur, rozszerzający formularzy sieci Web.

Strony formularzy sieci Web i formantów na stronie automatycznie generować znacznie kod znaczników, który jest wysyłany do przeglądarki, dlatego nie masz rodzaj precyzyjną kontrolę nad HTML, który oferuje ASP.NET MVC. Deklaratywne modelu do konfigurowania strony i kontrolki minimalizuje ilość kodu, trzeba napisać, ale ukrywa niektórych zachowań HTML i HTTP. Na przykład go nie zawsze jest możliwe do określenia dokładnie znaczników, jaki może być generowane przez formant.

Framework formularzy sieci Web nie nadają się łatwo jako ASP.NET MVC do rozwiązań programistycznych opartych na wzorce takich jak [test-driven development,](http://en.wikipedia.org/wiki/Test-driven_development), [separacji](http://en.wikipedia.org/wiki/Separation_of_concerns), [odwracanie z formant](http://en.wikipedia.org/wiki/Inversion_of_control), i [iniekcji zależności](http://en.wikipedia.org/wiki/Dependency_injection). Jeśli chcesz zapisać kod brana pod uwagę w ten sposób możesz; Podobnie nie jest jak automatyczne, ponieważ jest on platformę ASP.NET MVC. [MVP formularzy sieci Web ASP.NET](http://webformsmvp.com/) projektu zawiera metody, która ułatwia rozdzielenie problemy i testowania przy zachowaniu szybkie wdrażanie, opracowywanie formularzy sieci Web został utworzony w celu dostarczenia. Microsoft SharePoint jest oparty na MVP formularzy sieci Web.

Szablon formularzy sieci Web tworzy przykładowej aplikacji formularzy sieci Web, która używa [Bootstrap](#bootstrap) aby zapewnić dynamiczne funkcje projektowania i tworzenia motywów. Na poniższej ilustracji przedstawiono strony głównej.

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image10.png)

Aby uzyskać więcej informacji na temat formularzy sieci Web, zobacz [formularzy sieci Web ASP.NET](https://asp.net/web-forms). Aby uzyskać informacje o szablonie formularzy sieci Web jest dla Ciebie, zobacz [tworzenia podstawowej aplikacji formularzy sieci Web przy użyciu programu Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Szablon MVC

ASP.NET MVC została zaprojektowana w celu ułatwienia rozwiązań programistycznych opartych na wzorców, takich jak [test-driven development,](http://en.wikipedia.org/wiki/Test-driven_development), [separacji](http://en.wikipedia.org/wiki/Separation_of_concerns), [Inwersja kontroli](http://en.wikipedia.org/wiki/Inversion_of_control), i [iniekcji zależności](http://en.wikipedia.org/wiki/Dependency_injection). Platformę zachęca oddzielanie warstwy logiki biznesowej aplikacji sieci web z jej warstwy prezentacji. Dzieląc aplikację na modeli (M), widoki (V) i kontrolerów (C), platformy ASP.NET MVC ułatwia zarządzanie złożonością w dużych aplikacji.

Z platformą ASP.NET MVC działać więcej bezpośrednio z kodu HTML i HTTP niż w formularzach sieci Web. Na przykład formularzy sieci Web automatycznie można zachować stanu między żądaniami HTTP, ale masz kod, który jawnie w nazwie wzorca MVC. Zaletą modelu MVC jest umożliwia pełnej kontroli nad dokładnie czynności aplikacji i jak działa w środowisku sieci web. Wadą jest to, że trzeba napisać kod więcej.

MVC została zaprojektowana rozszerzalny, umożliwiając deweloperom zasilania dostosować strukturę dla potrzeb aplikacji. Ponadto kod źródłowy platformy ASP.NET MVC jest dostępny w ramach licencji OSI.

Szablon MVC umożliwia tworzenie przykładowej aplikacji MVC 5, która używa [Bootstrap](#bootstrap) aby zapewnić dynamiczne funkcje projektowania i tworzenia motywów. Na poniższej ilustracji przedstawiono strony głównej.

![MVC przykładowej aplikacji](creating-web-projects-in-visual-studio/_static/image11.png)

Aby uzyskać więcej informacji na temat MVC, zobacz [ASP.NET MVC](https://asp.net/mvc). Aby uzyskać informacje o sposobie wybierz szablon MVC 4, zobacz [szablony programu Visual Studio 2012](#vs2012) dalszej części tego artykułu.

<a id="webapi"></a>
### <a name="web-api-template"></a>Szablon interfejsu API sieci Web

Szablon interfejsu API sieci Web tworzy próbki usługę sieci web opartą na interfejsu API sieci Web, w tym strony pomocy interfejsu API oparty na MVC.

Interfejs API sieci Web platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne. Interfejs API sieci Web ASP.NET jest idealną platformą do tworzenia usługi RESTful w programie .NET Framework.

Szablon interfejsu API sieci Web tworzy próbki usługi sieci web. Na poniższych ilustracjach przedstawiono przykładowe strony pomocy.

![Strona pomocy interfejsu API sieci Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Strona pomocy interfejsu API sieci Web UZYSKAĆ interfejsu API](creating-web-projects-in-visual-studio/_static/image13.png)

Aby uzyskać więcej informacji na temat interfejsu API sieci Web, zobacz [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Szablon aplikacji jednostronicowej

Szablon jednej strony aplikacji JEDNOSTRONICOWEJ tworzenia przykładowej aplikacji, która używa JavaScript i HTML 5 i [elementami KnockoutJS](http://knockoutjs.com/) na kliencie i interfejsu API sieci Web platformy ASP.NET na serwerze.

Opcja uwierzytelniania tylko dla szablonu SPA jest [indywidualnych kont użytkowników](#indauth).

Na poniższej ilustracji przedstawiono przykładową aplikację, która tworzy szablon SPA początkowy stan.

![SPA przykładowej aplikacji](creating-web-projects-in-visual-studio/_static/image14.png)

Aby uzyskać informacje o sposobie tworzenia aplikacji przy użyciu szablonu SPA, zobacz [interfejsu API sieci Web - zewnętrznych usług uwierzytelniania](../../../web-api/overview/security/external-authentication-services.md).

Aby uzyskać więcej informacji o aplikacji jednej strony ASP.NET i dodatkowe szablony SPA korzystających z platform JavaScript niż elementami KnockoutJS informacje zobacz następujące zasoby:

- [ASP.NET pojedynczej strony aplikacji](../../../single-page-application/index.md).
- [Opis funkcji zabezpieczeń w szablonie SPA dla VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplikacje jednej strony: Tworzenie aplikacji Modern i sprawnie działających w sieci Web za pomocą programu ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Szablon usługi Facebook

Można zainstalować [rozszerzenie programu Visual Studio, który zawiera szablon usługi Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ten szablon umożliwia tworzenie przykładowej aplikacji, który jest przeznaczony do działania w witrynie sieci web usługi Facebook. Jest on oparty na platformie ASP.NET MVC i używa interfejsu API sieci Web dla funkcji aktualizacji w czasie rzeczywistym.

Nie opcje uwierzytelniania są dostępne dla szablonu usługi Facebook, ponieważ Facebook aplikacje uruchamiane w witrynie Facebook i polegać na uwierzytelnianie w serwisie Facebook.

Aby uzyskać więcej informacji na temat aplikacji ASP.NET Facebook, zobacz [aktualizacji interfejsu API usługi Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 szablonów

Okno dialogowe Tworzenie projektu sieci web programu Visual Studio 2013 nie zapewnia dostępu do niektórych szablonów, które były dostępne w programie Visual Studio 2012. Jeśli chcesz użyć jednej z tych szablonów, możesz kliknąć węzeł programu Visual Studio 2012 w okienku po lewej stronie okna dialogowego Nowy projekt programu Visual Studio.

![Szablony Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

**Programu Visual Studio 2012** węzeł umożliwia wybranie następujących szablonów sieci web, które nie mają dostępu do w liście domyślnych szablonów dla programu Visual Studio 2013:

- Aplikacja sieci Web platformy ASP.NET MVC 4
- Aplikacja sieci Web ASP.NET Dynamic Data Entities
- Kontrolka serwerowa AJAX dla ASP.NET
- Kontrolki serwerowej AJAX ASP.NET
- Kontrolka serwera programu ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Wykryto w szablonach projektu sieci web programu Visual Studio 2013

Szablony projektu Visual Studio 2013 użyj [Bootstrap](http://getbootstrap.com/), ramy układ i motywów utworzonych przez usługi Twitter. Bootstrap używa CSS3, aby zapewnić elastyczny projekt, co oznacza, że układów dynamicznie można dostosować do rozmiarów okna innej przeglądarki. Na przykład w oknie przeglądarki szerokości strony głównej utworzonej przez szablon formularzy sieci Web wygląda jak na poniższej ilustracji:

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image16.png)

Wprowadź mniejszą niż okna, a kolumny ułożone poziomo zostaną przeniesione na rozmieszczenie w pionie:

![Rozmieszczenie pionowej kolumnie ładowania początkowego](creating-web-projects-in-visual-studio/_static/image17.png)

Nieco bardziej zawęzić okna, a następnie włącza poziome menu u góry do ikony, które można kliknąć, aby rozwinąć w orientacji pionowej menu:

![Ikona bootstrap menu](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap menu pionowe](creating-web-projects-in-visual-studio/_static/image19.png)

Funkcja tworzenia motywów ładowania początkowego na umożliwia również łatwe wpływ zmian w aplikacji wyglądu i działania. Można na przykład, wykonaj następujące kroki, aby zmienić motyw.

1. W przeglądarce przejdź do [http://Bootswatch.com](http://Bootswatch.com), umożliwia wybranie motywu, a następnie kliknij przycisk **Pobierz**. (Spowoduje to pobranie *bootstrap.min.css* domyślnie; Jeśli chcesz sprawdzić kod CSS, Pobierz *bootstrap.css* zamiast wersji zminimalizowany.)
2. Skopiuj zawartość pobranego pliku CSS.
3. W programie Visual Studio Utwórz nową **arkusz stylów** plik o nazwie *bootstrap theme.css* w *zawartości* folder i Wklej pobrany CSS kodu do niego.
4. Otwórz *aplikacji\_Start/Bundle.config* i zmienić *bootstrap.css* do *bootstrap theme.css*.

Uruchom ponownie projekt, a aplikacja ma wygląda. Na poniższej ilustracji przedstawiono efekt Amelia motywu:

![Motyw Amelia ładowania początkowego](creating-web-projects-in-visual-studio/_static/image20.png)

Wiele motywów ładowania początkowego są dostępne wersje jednocześnie bezpłatny i premium. Ładowania początkowego oferuje szeroką gamę składników interfejsu użytkownika, takich jak [listach rozwijanych](http://twitter.github.io/bootstrap/components.html#dropdowns), [przycisk grup](http://twitter.github.io/bootstrap/components.html#buttonGroups), i [ikony](http://twitter.github.io/bootstrap/base-css.html#images). Aby uzyskać więcej informacji na temat ładowania początkowego zobacz [Bootstrap lokacji](http://twitter.github.io/bootstrap/).

Jeśli używasz narzędzia Projektant formularzy sieci Web w programie Visual Studio, należy pamiętać, Projektant nie obsługuje CSS3, dlatego nie dokładnie pokazuje wszystkie efekty motywy ładowania początkowego lub zmiany elastyczny układ. Jednak stron formularzy sieci Web będą wyświetlane poprawnie, podczas wyświetlania w przeglądarce.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Dodawanie obsługi dla dodatkowych struktur

Po wybraniu szablonu jest automatycznie wybierany pole wyboru framework(s) używany przez szablon. Na przykład w przypadku wybrania **formularzy sieci Web** szablonu, **formularzy sieci Web** pole wyboru jest zaznaczone i nie można usunąć jej zaznaczenia.

![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image21.png)

![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image22.png)

Można wybrać pola wyboru platforma, która nie jest zawarte w szablonie, aby można było dodać obsługę tego framework, po utworzeniu projektu. Na przykład, aby umożliwić korzystanie z formularzy sieci Web *.aspx* stron po wybraniu szablonu MVC wybierz **formularzy sieci Web** pole wyboru. Lub aby włączyć MVC, gdy używasz szablonu formularzy sieci Web, kliknij przycisk **MVC** pole wyboru. Dodawanie struktury włącza obsługę czasu projektowania, jak również w czasie wykonywania. Na przykład po dodaniu obsługi MVC do projektu formularzy sieci Web można będzie utworzyć szkielet widoków i kontrolerów.

Jeśli łączenie MVC i formularzy sieci Web w projekcie i Włącz [przyjazne adresy URL](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) w formularzach sieci Web może być nieoczekiwane routingu problemów, gdy jeden adres URL ma wiele elementów docelowych to możliwe. Najpierw trasy, które są zdefiniowane będą miały pierwszeństwo. Na przykład, jeśli masz `Home` kontrolera i *Home.aspx* strony, `http://contoso.com/home` adresu URL zostanie umieszczona na *Home.aspx* połączeń `EnableFriendlyUrls` metoda przed wywołaniem `MapRoute`metody w *RouteConfig.cs*, lub ten sam adres URL nastąpi przejście do domyślnego widoku dla Twojego `Home` kontrolera, jeśli wywołujesz `MapRoute` przed `EnableFriendlyUrls`.

Dodawanie struktury nie dodaje żadnych funkcji aplikacji przykładowej. Na przykład, jeśli dodasz formularzy sieci Web obsługuje po wybraniu szablonu MVC nie *Default.aspx* zostanie utworzony plik strony głównej. Tylko folderów, plików i odwołania wymagane do obsługi w ramach zostaną dodane. Z tego powodu dodawanie struktur nie powoduje zmiany opcji uwierzytelniania, które są implementowane przez kod w przykładowych aplikacji utworzony za pomocą szablonów. Na przykład po wybraniu pustego szablonu i dodać formularzy sieci Web lub MVC obsługuje, **skonfigurować uwierzytelnianie** przycisk nadal będzie wyłączony.

W poniższych sekcjach krótko opisano wpływ każdego pola wyboru.

### <a name="add-web-forms-support"></a>Dodawanie obsługi formularzy sieci Web

Tworzy pusty *aplikacji\_danych* i *modele* folderów i *Global.asax* pliku. Są one już tworzone przez wszystkie szablony innych niż pustego szablonu, zaznaczając pole wyboru formularzy sieci Web nie ma wpływu na innych szablonów.

Szablon formularzy sieci Web włącza przyjazne adresy URL domyślnie, ale po dodaniu obsługi formularzy sieci Web do innych szablonów zaznaczając pole wyboru formularzy sieci Web, który przyjazne adresy URL nie są automatycznie włączane.

### <a name="add-mvc-support"></a>Dodawanie obsługi MVC

Instaluje pakiety MVC Razor i NuGet stron sieci Web, tworzy puste *aplikacji\_danych*, *kontrolerów*, *modele*, i *widoków*folderów, tworzy *aplikacji\_Start* folder o *RouteConfig.cs* plików i tworzy *Global.asax* pliku.

### <a name="add-web-api-support"></a>Dodaj obsługę interfejsu API sieci Web

Instaluje pakiety WebApi i Newtonsoft.Json NuGet, tworzy puste *aplikacji\_danych*, *kontrolerów*, i *modele* folderów, tworzy  *Aplikacja\_Start* folder o *WebApiConfig.cs* plików i tworzy *Global.asax* pliku.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metody uwierzytelniania

Visual Studio 2013 oferuje kilka opcji uwierzytelniania dla szablonów Web Forms, MVC i interfejsu API sieci Web:

- [Bez uwierzytelniania](#noauth)
- [Indywidualne konta użytkowników](#indauth) (ASP.NET Identity, wcześniej znana jako członkostwa ASP.NET)
- [Konta organizacyjne](#orgauth) (Windows Server Active Directory lub Azure Active Directory)
- [Uwierzytelnianie systemu Windows](#winauth) (sieć Intranet)

![Skonfiguruj dialog uwierzytelniania](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Bez uwierzytelniania

W przypadku wybrania **bez uwierzytelniania**przykładowej aplikacji będzie zawierał nie stron sieci web do logowania, nie interfejsu użytkownika wskazujący, który jest zalogowany, Brak klasy członkowskiej bazie danych i nie parametrów połączenia dla bazy danych członkostwa.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Indywidualne konta użytkowników

W przypadku wybrania **indywidualnych kont użytkowników**, przykładowej aplikacji zostanie skonfigurowany do korzystania z tożsamości platformy ASP.NET (wcześniej znane jako członkostwa ASP.NET) do uwierzytelniania użytkowników. ASP.NET Identity umożliwia rejestrowanie konta, tworząc nazwy użytkownika i hasła w witrynie lub logowanie się przy użyciu dostawców sieci społecznościowych, takich jak Facebook, Google, Microsoft Account lub Twitter. Domyślny magazyn danych dla profilów użytkowników w produkcie ASP.NET Identity jest baza danych SQL Server LocalDB, której można wdrożyć dla lokacji produkcyjnej do programu SQL Server lub bazy danych SQL Azure.

W programie Visual Studio 2013 te funkcje są takie same jak programu Visual Studio 2012, ale ponownie zapisać kod źródłowy systemu członkostwa programu ASP.NET. Zalety nowego podstawowego kodu są następujące:

- Nowy system członkostwa opiera się na [OWIN](http://owin.org/) zamiast modułu uwierzytelnianie formularzy aplikacji ASP.NET. Oznacza to, że można używać ten sam mechanizm uwierzytelniania, czy używasz formularzy sieci Web lub MVC w usługach IIS lub w przypadku samodzielnej obsługi interfejsu API sieci Web lub SignalR.
- Nowa baza danych członkostwa jest zarządzana przez Entity Framework Code First, a wszystkie tabele są reprezentowane przez klas jednostek, które można modyfikować. Oznacza to, że można łatwo dostosować schematu bazy danych i związanych z profilu interfejsu użytkownika sieci web do własnych potrzeb, a możesz z łatwością wdrożyć aktualizacje za pomocą migracje Code First.

Nowy system członkostwa jest automatycznie implementowane w nowych szablonów i może być zaimplementowany ręcznie w każdym projekcie, przeznaczonego dla platformy .NET 4.5 lub nowszej.

ASP.NET Identity jest dobrym rozwiązaniem w przypadku tworzenia witryny sieci web internetowych, który jest przeznaczony głównie dla klientów zewnętrznych. Jeśli Twoja organizacja korzysta z usługi Active Directory lub usługi Office 365 i chcesz utworzyć projekt, który umożliwia single-sign-on dla pracowników i partnerów biznesowych **konta organizacyjne** opcja może być lepszym rozwiązaniem.

Aby uzyskać więcej informacji na temat opcji indywidualnych kont użytkowników zobacz następujące zasoby:

- [www.asp.net/identity](../../../identity/index.md). Dokumentacja dotyczących tożsamości platformy ASP.NET w witrynie sieci web programu ASP.NET.
- [Tworzenie aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Przedstawiono również sposób dostosowywania dane profilu użytkownika.
- [Interfejs API sieci Web - zewnętrznych usług uwierzytelniania](../../../web-api/overview/security/external-authentication-services.md)
- [Dodawanie do aplikacji ASP.NET w programie Visual Studio 2013 logowań zewnętrznych](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Konta organizacyjne

W przypadku wybrania **konta organizacyjne**, przykładowej aplikacji zostanie skonfigurowany do używania systemu Windows Identity Foundation (WIF) w celu uwierzytelniania na podstawie kont użytkowników w usłudze Azure Active Directory (Azure AD, która obejmuje usługi Office 365) lub Windows Server Active Directory. Aby uzyskać więcej informacji, zobacz [opcje uwierzytelniania konto organizacyjne](#orgauthoptions) dalszej części tego tematu.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

W przypadku wybrania **uwierzytelniania systemu Windows**, przykładowej aplikacji zostanie skonfigurowany do uwierzytelniania modułu IIS uwierzytelniania systemu Windows. Aplikacja wyświetla identyfikator domeny i użytkownika usługi Active directory lub konta komputera lokalnego, który jest zalogowany do systemu Windows, ale nie będzie zawierać rejestracja użytkownika lub dziennika w interfejsie użytkownika. Ta opcja jest przeznaczona dla witryn intranetowych.

Alternatywnie można utworzyć witryny intranetowej przy użyciu korzystającej z uwierzytelniania usług AD, wybierając [opcję w obszarze konta organizacyjne lokalnymi](#orgauthonprem). Opcja lokalnego korzysta z systemu Windows Identity Foundation (WIF) zamiast modułu uwierzytelniania systemu Windows. Dodatkowe kroki są wymagane w celu skonfigurowania opcji lokalnego, ale WIF umożliwia korzystanie z funkcji, które nie są dostępne z modułem uwierzytelniania systemu Windows. Na przykład z WIF można skonfigurować dostęp do aplikacji w usłudze Active Directory i zapytań danych katalogu.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opcje uwierzytelniania konta organizacyjnego

**Skonfigurować uwierzytelnianie** okna dialogowego oferuje kilka możliwości usługi Azure Active Directory (Azure AD, która obejmuje usługi Office 365) lub uwierzytelnianie konta systemu Windows Server Active Directory (AD):

- [Chmura - jednej organizacji](#orgauthsingle) (Azure AD lub AD za pomocą integracji katalogu z usługą Azure AD)
- [Chmura - wielu organizacji](#orgauthmulti) (Azure AD lub AD za pomocą integracji katalogu z usługą Azure AD)
- [Lokalne](#orgauthonprem) (AD)

Jeśli chcesz wypróbować jedną z opcji usługi Azure AD, ale nie masz jeszcze konta [kliknij tutaj, aby utworzyć konto usługi Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Jeśli zostanie wybrana jedna z opcji usługi Azure AD, Twój projekt wymaga bazy danych i należy zalogować się do konta administratora globalnego dla dzierżawy usługi Azure AD. Wprowadź nazwę i hasło dla konta organizacyjnego (na przykład admin@contoso.onmicrosoft.com) który ma uprawnienia administracyjne dla dzierżawy usługi Azure AD.
> 
> **Nie wprowadź poświadczenia dla konta Microsoft (na przykład contoso@hotmail.com) w oknie dialogowym logowania.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Chmura - uwierzytelniania jednej organizacji

![Uwierzytelnianie jednej organizacji](creating-web-projects-in-visual-studio/_static/image24.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w jednej usłudze Azure AD [dzierżawy](https://technet.microsoft.com/library/jj573650.aspx). Na przykład witryna jest contoso.com i będzie on dostępny do pracowników firmy Contoso, którzy są w dzierżawie contoso.onmicrosoft.com. Nie można skonfigurować usługi Azure AD, aby umożliwić użytkownikom z innych dzierżawców do dostępu do aplikacji.

#### <a name="domain"></a>Domain

Wprowadź domenę usługi Azure AD, który chcesz skonfigurować aplikację, na przykład: `contoso.onmicrosoft.com`. Jeśli masz [domeny niestandardowej](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), takich jak `contoso.com` zamiast `contoso.onmicrosoft.com`, można wprowadzić ta tutaj.

#### <a name="access-level"></a>Poziom dostępu

Jeśli aplikacja musi zapytania lub zaktualizować informacje o katalogu przy użyciu interfejsu API programu Graph, wybierz **logowanie jednokrotne, Czytaj dane katalogu** lub **logowanie jednokrotne, Odczyt i zapis danych katalogu**. W przeciwnym razie wybierz **rejestracji jednokrotnej**. Aby uzyskać więcej informacji, zobacz [poziomy dostępu aplikacji](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) i [przy użyciu interfejsu API programu Graph do zapytań usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Identyfikator URI Identyfikatora aplikacji

Domyślnie szablon tworzy aplikację identyfikator URI dla Ciebie przez dodanie nazwy projektu do domeny usługi Azure AD. Na przykład, jeśli nazwa projektu jest `Example` , a domena to `contoso.onmicrosoft.com`, aplikacja stanie się identyfikator URI `https://contoso.onmicrosoft.com/Example`. Aby ręcznie określić identyfikator URI aplikacji, rozwiń węzeł **więcej opcji** sekcji, a następnie wprowadź identyfikator URI aplikacji w polu tekstowym. Identyfikator URI musi zaczynać się od aplikacji `https://`.

Domyślnie jeśli aplikacja, która została już przydzielona w usłudze Azure AD ma tej samej aplikacji identyfikator URI korzystających z programu Visual Studio dla projektu, co projektu zostaną podłączone do istniejącej aplikacji zamiast inicjowania obsługi administracyjnej nowej. Nowa aplikacja na potrzeby aprowizacji w takim przypadku należy wyczyścić **Zastąp wpis aplikacji, jeśli istnieje wpis z takim samym identyfikatorze już** pole wyboru.

Jeśli **Zastąp** pole wyboru jest wyczyszczone i Visual Studio umożliwia znalezienie istniejącej aplikacji o tej samej aplikacji identyfikator URI, dołączając do identyfikatora URI został zamierza użyć tworzy nowy identyfikator URI. Na przykład, załóżmy, że nazwa projektu jest `Example`, pola tekstowego pozostanie puste, zostanie ono wyczyszczone **Zastąp** pole wyboru, a dzierżawy usługi Azure AD ma już aplikacji o identyfikatorze URI `https://contoso.onmicrosoft.com/Example`. W takim przypadku nowej aplikacji będą udostępniane z aplikacji, takich jak identyfikator URI `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Inicjowanie obsługi administracyjnej aplikacji w usłudze Azure AD

W celu udostępnienia aplikacji w usłudze Azure AD lub połączyć projekt z istniejących aplikacji, Visual Studio wymaga poświadczeń administratora globalnego dla domeny. Po kliknięciu **OK** w **skonfigurować uwierzytelnianie** okno dialogowe, zostanie wyświetlony monit o nazwę użytkownika i hasło administratora globalnego dla określonej domeny. Później, po kliknięciu **tworzenia projektu** w **nowy projekt ASP.NET** okna dialogowego, Visual Studio udostępnia aplikacji w usłudze Azure AD. Należy pamiętać, że w ramach tego procesu Visual Studio osadza wartości tajny klienta w pliku Web.config, która wygaśnie rok po utworzeniu.

Aby uzyskać informacje o sposobie tworzenia aplikacji, które używają **Cloud - jednej organizacji** uwierzytelniania, zobacz następujące zasoby:

- [Azure Authentication](../2012/windows-azure-authentication.md)
- [Dodawanie logowania jednokrotnego do aplikacji sieci Web przy użyciu usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Tworzenie aplikacji ASP.NET z wykorzystaniem usługi Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Zabezpieczenia programu ASP.NET Web API z usługi Azure AD i składniki Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Samouczków jeszcze nie zostały zaktualizowane dla programu Visual Studio 2013; Niektóre z jakiego samouczków kierujące wykonać ręcznie jest zautomatyzowany w programie Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Chmura - wielu organizacji uwierzytelniania

![Wiele organizacji uwierzytelniania](creating-web-projects-in-visual-studio/_static/image25.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w wielu usługi Azure AD [dzierżawców](https://technet.microsoft.com/library/jj573650.aspx). Na przykład witryna jest contoso.com i jego będą dostępne do pracowników firmy Contoso, którzy są w dzierżawie contoso.onmicrosoft.com i pracowników firmy Fabrikam, którzy są w dzierżawie fabrikam.onmicrosoft.com.

Ustawienia, które należy wprowadzić i inicjowania obsługi administracyjnej krok aplikacji są podobne do [uwierzytelniania jednej organizacji](#orgauthsingle).

Informacje o sposobie tworzenia aplikacji, które używają **Cloud - wielu organizacji** uwierzytelniania, zobacz następujące zasoby:

- [Łatwa integracja aplikacji sieci Web w usłudze Azure Active Directory, ASP.NET &amp; programu Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) na blogu zespołu usługi Active Directory.
- [Tworzenie wielodostępnych aplikacji sieci Web z usługą Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) samouczka. Samouczek jeszcze zostało zaktualizowane dla programu Visual Studio 2013; Niektóre co samouczka przekierowuje ręcznego wykonywania jest zautomatyzowany w programie Visual Studio 2013.
- [Musisz zalogować się pracy z własnych wiele aplikacji programu ASP.NET organizacji przed zarejestrowaniem się](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog przez Vittorio Bertocci, który objaśnia, jak rozwiązać typowe osób problem napotkać podczas tworzenia projektu, który korzysta z uwierzytelniania wielu organizacji.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Organizacyjnej uwierzytelniania lokalnego

![Organizacyjnej uwierzytelniania lokalnego](creating-web-projects-in-visual-studio/_static/image26.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w systemie Windows Server Active Directory (AD), a nie chcesz używać usługi Azure AD. Ta opcja służy do tworzenia witryny intranetowej lub witryny internetowej. Dla witryny internetowej użyj Active Directory Federation Services (ADFS), aby zapewnić dostęp do usługi AD. Aby uzyskać więcej informacji, zobacz [za pomocą opcji uwierzytelniania lokalnego organizacji (ADFS) z programu ASP.NET w programie Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Do witryny intranetowej Alternatywnie można wybrać [uwierzytelniania systemu Windows](#winauth) zamiast tej opcji. Dla opcji uwierzytelniania systemu Windows nie trzeba podać adres URL dokumentu metadanych. Jednak uwierzytelnianie systemu Windows nie zapewnia możliwości kontroli dostępu do aplikacji w usłudze Active Directory lub wykonać zapytania o dane katalogu.

#### <a name="on-premises-authority"></a>Urząd lokalny

Wprowadź adres URL, który wskazuje dokument metadanych. Dokument metadanych zawiera współrzędne urzędu. Aplikacja będzie korzystać z tych współrzędnych dysków przepływu logowania jednokrotnego w sieci web.

#### <a name="application-id-uri"></a>Identyfikator URI Identyfikatora aplikacji

Podaj unikatowy identyfikator URI, który AD służy do identyfikowania tej aplikacji, lub pozostaw pole puste, aby umożliwić programowi Visual Studio utwórz je.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

Ten dokument udostępnił podstawowa pomoc do tworzenia nowego projektu sieci web ASP.NET w programie Visual Studio 2013. Aby uzyskać więcej informacji o korzystaniu z programu Visual Studio do tworzenia aplikacji sieci web, zobacz [https://www.asp.net/visual-studio/](../../index.md).
