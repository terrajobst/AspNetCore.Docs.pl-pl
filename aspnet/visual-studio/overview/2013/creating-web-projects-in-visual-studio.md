---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Tworzenie projektów sieci Web platformy ASP.NET, w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: tdykstra
description: W tym temacie wyjaśniono, że opcje tworzenia projektów sieci web platformy ASP.NET w programie Visual Studio 2013 z aktualizacją Update 3 w tym miejscu przedstawiono niektóre nowe funkcje dla języka c development sieci web...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 3d96d796d22c3511fedc45c024274300143b119b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754223"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Tworzenie projektów sieci Web platformy ASP.NET w programie Visual Studio 2013
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tym temacie opisano opcje tworzenia projektów sieci web platformy ASP.NET w programie Visual Studio 2013 z aktualizacją Update 3
> 
> Oto niektóre z nowych funkcji do tworzenia aplikacji internetowych w porównaniu do wcześniejszych wersji programu Visual Studio:
> 
> - Prosty interfejs użytkownika do tworzenia projektów tę ofertę [Obsługa wielu platform ASP.NET](#add) (Web Forms, MVC i interfejs API sieci Web).
> - [ASP.NET Identity](#indauth), nowego systemu członkostwa programu ASP.NET, która działa tak samo, we wszystkich platform ASP.NET i działa, za pomocą oprogramowania innych niż IIS hostingu w sieci web.
> - Korzystanie z [Bootstrap](#bootstrap) aby zapewnić dynamiczne możliwości projektowania i motywów.
> - Nowe funkcje dla formularzy sieci Web, używany do oferowana tylko w przypadku MVC, takie jak [tworzenie projektów testów automatycznych](#testproj) i [szablonu witryny intranetowej](#winauth).
> 
> Aby uzyskać informacje o sposobie tworzenia projektów sieci web dla usług Azure Cloud Services lub usług Azure Mobile Services, zobacz [rozpoczęcie korzystania z usług Azure Cloud Services i platformy ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) i [tworzenie aplikacji tablicy wyników za pomocą usługi Azure Mobile Services platformy .NET Zaplecze](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Ten artykuł dotyczy [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) zainstalowane.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projekty aplikacji sieci Web i projektów witryny sieci web

Program ASP.NET zapewnia wybór między dwoma rodzajami projektów sieci web: *projektów aplikacji sieci web* i *projektów witryny sieci web*. Firma Microsoft zaleca projektów aplikacji sieci web w przypadku nowych wdrożeń, a w tym artykule mają zastosowanie tylko do projektów aplikacji sieci web. Aby uzyskać więcej informacji, zobacz [Web Application Projects versus projektów witryny sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) w witrynie MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Omówienie tworzenia projektu aplikacji sieci web

Poniższe kroki pokazują jak utworzyć projekt sieci web:

1. Kliknij przycisk **nowy projekt** w **Start** strony lub **pliku** menu.
2. W **nowy projekt** okno dialogowe, kliknij przycisk **Web** w okienku po lewej stronie i **aplikacji sieci Web ASP.NET** w środkowym okienku.

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image1.png)

    Możesz wybrać **chmury** w okienku po lewej stronie, aby utworzyć [usługa w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [usług Azure Mobile](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), lub [zadanie Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Ten temat nie obejmuje tych szablonów.
3. W okienku po prawej stronie kliknij **Dodaj usługę Application Insights do projektu** pole wyboru, jeśli chcesz, aby Kondycja i monitorowanie użycia aplikacji. Aby uzyskać więcej informacji, zobacz [monitorowania wydajności w aplikacjach sieci web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Określ projekt **nazwa**, **lokalizacji**i inne opcje, a następnie kliknij przycisk **OK**.

    **Nowy projekt ASP.NET** zostanie wyświetlone okno dialogowe.

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Kliknij szablon.

    ![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image3.png)
6. Jeśli chcesz dodać obsługę dodatkowych platform, które nie są uwzględnione w szablonie, kliknij odpowiednie pole wyboru. (W tym przykładzie pokazano można dodać MVC i/lub interfejs API sieci Web na projekt formularzy sieci Web.)

    ![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Jeśli chcesz dodać projekt testu jednostkowego, kliknij przycisk **dodać testy jednostkowe**.

    ![Dodawanie testów jednostkowych](creating-web-projects-in-visual-studio/_static/image5.png)
8. Metodę uwierzytelniania inną niż szablon zawiera domyślne, kliknij przycisk **Zmień uwierzytelnianie**.

    ![Konfigurowanie uwierzytelniania przycisku](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Konfigurowanie okna dialogowego uwierzytelniania](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Tworzenie aplikacji sieci web lub maszyna wirtualna na platformie Azure

Visual Studio zawiera funkcje, które ułatwiają pracę z usługami platformy Azure do hostowania aplikacji sieci web. Możesz na przykład, wykonaj wszystkie poniższe bezpośrednio w środowisku IDE programu Visual Studio:

- Tworzenie i zarządzanie nimi, aplikacje sieci web lub maszyn wirtualnych, które aplikacji dostępne za pośrednictwem Internetu.
- Wyświetl dzienniki utworzone przez aplikację podczas jej działania w chmurze.
- Zdalnie korzystać z trybu debugowania podczas działania aplikacji w chmurze.
- Viiew i zarządzać innymi usługami platformy Azure, takich jak bazy danych SQL.

Możesz [utworzyć konto platformy Azure](https://www.windowsazure.com/pricing/free-trial/) za darmo zawierającej podstawowych usług, takich jak aplikacje sieci web, a jeśli jesteś subskrybentem MSDN możesz [aktywować korzyści](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) które podają miesięczne środki na korzystanie z usług Azure dodatkowe usługi. 

Domyślnie **nowy projekt ASP.NET** okno dialogowe pozwala na tworzenie aplikacji sieci web lub maszyny wirtualnej do nowego projektu sieci web. Jeśli nie chcesz utworzyć nową aplikację sieci web lub maszyny wirtualnej, należy wyczyścić **Hostuj w chmurze** pole wyboru.

![Utwórz zasoby zdalne](creating-web-projects-in-visual-studio/_static/image8.png)

Podpis pole wyboru musi być **Hostuj w chmurze** lub **Utwórz zasoby zdalne**, a w obu przypadkach efekt jest taki sam. Jeśli pole jest zaznaczone pole wyboru, Visual Studio tworzy aplikację internetową w usłudze Azure App Service domyślnie. Można użyć listy rozwijanej, aby zmienić tę instrukcję na **maszyny wirtualnej** Jeśli użytkownik sobie tego życzy. Jeśli jeszcze nie zalogowano do platformy Azure, zostanie wyświetlony monit o poświadczenia platformy Azure. Po zalogowaniu, okno dialogowe umożliwia konfigurowanie zasobów, które program Visual Studio zostanie utworzona w projekcie. Na poniższej ilustracji przedstawiono okno dialogowe dla aplikacji sieci web; Jeśli zdecydujesz się utworzyć maszynę wirtualną, zostaną wyświetlone różne opcje.

![Konfigurowanie ustawień aplikacji platformy Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Aby uzyskać więcej informacji na temat tego procesu należy używać do tworzenia zasobów platformy Azure, zobacz [Rozpoczynanie pracy z platformą Azure i programu ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) i [tworzenia maszyny wirtualnej dla witryny sieci web za pomocą programu Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

W dalszej części tego artykułu zawiera więcej informacji na temat dostępnych szablonów i ich opcji. Artykuł przedstawia również Bootstrap, układ i motywów framework wykorzystywany w szablonach.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Szablony projektów sieci Web programu Visual Studio 2013

Program Visual Studio używa szablony do tworzenia projektów sieci web. Szablon projektu można tworzyć pliki i foldery w nowym projekcie, instalowanie pakietów NuGet i zapewnienia przykładowy kod z prostymi działającą aplikację. Szablony wdrożenia najnowszych standardów sieci web i służą do zademonstrowania, że najważniejsze wskazówki dotyczące sposobu korzystają z technologii ASP.NET, a także umożliwiają szybkie uruchomienie na tworzenie własnych aplikacji.

Visual Studio 2013 udostępnia następujące opcje dla szablonów projektu sieci web dla projektów przeznaczonych dla platformy .NET 4.5 lub nowsze wersje programu .NET framework:

- [Pusty szablon](#empty)
- [Szablon formularzy sieci Web](#wf)
- [Szablon MVC](#mvc)
- [Szablon sieci Web interfejsu API](#webapi)
- [Pojedynczy szablon aplikacji](#spa)
- [Szablon usługi mobilnej platformy Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Szablony programu Visual Studio 2012](#vs2012)

Można także zainstalować rozszerzenia programu Visual Studio, która zapewnia [szablonu usługi Facebook](#facebook).

Aby uzyskać informacje o sposobie tworzenia projektów przeznaczonych dla platformy .NET 4, zobacz [szablony programu Visual Studio 2012](#vs2012) w dalszej części tego tematu.

Aby uzyskać informacje o sposobie tworzenia aplikacji platformy ASP.NET dla klientów mobilnych, zobacz [Mobile obsługi w programie ASP.NET:](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Pusty szablon

Udostępnia pustego szablonu, bez minimalnych foldery i pliki dla aplikacji sieci web platformy ASP.NET, takich jak plik projektu (*.csproj* lub. *vbproj*) i *Web.config* pliku. Obsługa formularzy sieci Web, MVC i/lub interfejs API sieci Web można dodać za pomocą pola wyboru w obszarze **. Dodaj foldery i podstawowe odwołania dla:** etykiety.

Dla pustego szablonu są dostępne żadne opcje uwierzytelniania. Uwierzytelnianie jest implementowana w przykładowych aplikacji, a pusty szablon nie tworzy przykładową aplikację.

<a id="wf"></a>
### <a name="web-forms-template"></a>Szablon formularzy sieci Web

Formularze sieci Web, które framework zapewnia następujące funkcje, które pozwalają na szybkie tworzenie witryn sieci web, które są bogatymi w interfejsie użytkownika i danych dostęp do funkcji:

- WYSIWYG Projektant w programie Visual Studio.
- Formanty serwera, które renderują HTML i które można dostosować przez ustawienie właściwości i stylów.
- Rozbudowane gamę formantów, aby uzyskać dostęp do danych i wyświetlania danych.
- Model zdarzeń, który przedstawia zdarzenia, które można programować, takie jak zaprogramować aplikacji klienckich, takich jak WPF.
- Automatyczne zachowania stanu (dane) między żądaniami HTTP.

Ogólnie rzecz biorąc tworzenie aplikacji formularzy sieci Web wymaga mniej pracy programistycznej niż tworzenia tej samej aplikacji za pomocą struktury ASP.NET MVC. Formularze sieci Web nie jest jednak tylko do szybkiego opracowywania aplikacji. Istnieje wiele złożonych aplikacji komercyjnych i struktur, zbudowany na podstawie formularzy sieci Web.

Strony formularzy sieci Web i formantów na stronie automatycznie generować znacznie kod znaczników, który jest wysyłany do przeglądarki, dlatego nie masz rodzaj szczegółową kontrolę nad tym kod HTML, który oferuje platformy ASP.NET MVC. Deklaratywny model do konfigurowania stron i kontrolek minimalizuje ilość kodu, trzeba napisać, lecz ukrywa niektóre zachowania kodu HTML i HTTP. Na przykład nie zawsze jest możliwe określenie dokładnie znaczników, jaki może zostać wygenerowana przez kontrolkę.

Środowiska formularzy sieci Web nie przystosowany łatwo jako ASP.NET MVC do wytwarzania oprogramowania na podstawie wzorców takich jak [programowania sterowanego testami](http://en.wikipedia.org/wiki/Test-driven_development), [separacji](http://en.wikipedia.org/wiki/Separation_of_concerns), [odwrotne położenie Kontrolka](http://en.wikipedia.org/wiki/Inversion_of_control), i [wstrzykiwanie zależności](http://en.wikipedia.org/wiki/Dependency_injection). Jeśli chcesz napisać kod uwzględniona w ten sposób, możesz to zrobić; po prostu nie jest jak automatyczne, znajduje się w platformę ASP.NET MVC. [ASP.NET Web Forms MVP](http://webformsmvp.com/) projektu zawiera metody, która ułatwia rozdzielenie problemów i testowania przy zachowaniu szybkiego tworzenia zawartości, która formularzy sieci Web została utworzona w celu udostępnienia. Microsoft SharePoint jest oparta na sieci Web Forms MVP.

Szablon formularzy sieci Web służy do tworzenia przykładowej aplikacji formularzy sieci Web, która używa [Bootstrap](#bootstrap) zapewnienie interaktywnych funkcji projektu i motywów. Poniższa ilustracja przedstawia strony głównej.

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image10.png)

Aby uzyskać więcej informacji na temat formularzy sieci Web, zobacz [wzorca ASP.NET Web Forms](https://asp.net/web-forms). Aby dowiedzieć się, jak szablon formularzy sieci Web jest dla Ciebie, zobacz [tworzenia podstawowej aplikacji formularzy sieci Web za pomocą programu Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Szablon MVC

ASP.NET MVC został zaprojektowany w celu ułatwienia wytwarzania oprogramowania na podstawie wzorców takich jak [programowania sterowanego testami](http://en.wikipedia.org/wiki/Test-driven_development), [separacji](http://en.wikipedia.org/wiki/Separation_of_concerns), [Inwersja kontroli](http://en.wikipedia.org/wiki/Inversion_of_control), i [wstrzykiwanie zależności](http://en.wikipedia.org/wiki/Dependency_injection). Struktura zachęca oddzielenie warstwy logiki biznesowej aplikacji sieci web od jej warstwy prezentacji. Dzieląc aplikację na modeli (M), widoki (V) i kontrolerów (C), platformy ASP.NET MVC może ułatwić zarządzanie złożonością w dużych aplikacji.

Za pomocą platformy ASP.NET MVC możesz pracować bardziej bezpośrednio z kodu HTML i protokołu HTTP niż w formularzach sieci Web. Na przykład formularzy sieci Web można automatycznie zachowania stanu między żądaniami HTTP, ale masz kod, który jawnie MVC. Zaletą modelu MVC to umożliwia pełnej kontroli nad dokładnie działania aplikacji i jak działa w środowisku sieci web. Wadą jest to, że trzeba napisać kod więcej.

MVC została opracowana jako rozszerzalny, zapewniając deweloperów power możliwość dostosowania framework odpowiadające ich potrzebom aplikacji. Ponadto kod źródłowy platformy ASP.NET MVC jest dostępny w ramach licencji OSI.

Szablon MVC służy do tworzenia przykładowej aplikacji MVC 5, która używa [Bootstrap](#bootstrap) zapewnienie interaktywnych funkcji projektu i motywów. Poniższa ilustracja przedstawia strony głównej.

![MVC przykładowej aplikacji](creating-web-projects-in-visual-studio/_static/image11.png)

Aby uzyskać więcej informacji na temat MVC, zobacz [platformy ASP.NET MVC](https://asp.net/mvc). Aby uzyskać informacje o sposobie wybierania szablonu MVC 4, zobacz [szablony programu Visual Studio 2012](#vs2012) w dalszej części tego artykułu.

<a id="webapi"></a>
### <a name="web-api-template"></a>Szablon interfejsu API sieci Web

Szablon interfejsu API sieci Web tworzy przykładowe usługi sieci web oparte na sieci Web interfejsu API, w tym stron pomocy interfejsu API oparty na MVC.

Web API platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. Web API platformy ASP.NET jest idealną platformą do tworzenia usługi RESTful na .NET Framework.

Szablon interfejsu API sieci Web tworzy przykładowe usługi sieci web. Na poniższych ilustracjach przedstawiono przykładowe strony pomocy.

![Strona pomocy interfejsu API sieci Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Strona pomocy interfejsu API sieci Web, Pobierz interfejsu API](creating-web-projects-in-visual-studio/_static/image13.png)

Aby uzyskać więcej informacji na temat interfejsu API sieci Web, zobacz [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Szablon aplikacji jednostronicowej

Szablon jednej strony aplikacji (SPA) służy do tworzenia przykładowej aplikacji, która używa języka JavaScript, HTML 5 i [KnockoutJS](http://knockoutjs.com/) na kliencie i Web API platformy ASP.NET na serwerze.

Opcja uwierzytelniania tylko dla szablonu SPA jest [indywidualne konta użytkowników](#indauth).

Początkowy stan przykładowej aplikacji, która tworzy szablon SPA można znaleźć w poniższej ilustracji.

![SPA przykładowej aplikacji](creating-web-projects-in-visual-studio/_static/image14.png)

Aby uzyskać informacje o sposobie tworzenia aplikacji przy użyciu szablonu z SPA, zobacz [interfejsu API sieci Web - zewnętrznych usług uwierzytelniania](../../../web-api/overview/security/external-authentication-services.md).

Aby uzyskać więcej informacji na temat aplikacje jednostronicowe ASP.NET oraz dodatkowe szablony SPA, korzystających z platformy JavaScript innych niż KnockoutJS zobacz następujące zasoby:

- [ASP.NET pojedynczej strony aplikacji](../../../single-page-application/index.md).
- [Omówienie funkcji zabezpieczeń w szablonie SPA w wersji RC w programie VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplikacje jednej strony: Tworzenie aplikacji sieci Web odpowiada za pomocą platformy ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Szablon usługi Facebook

Możesz zainstalować [rozszerzenia programu Visual Studio, który zawiera szablon usługi Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ten szablon umożliwia utworzenie aplikacji przykładowej, który jest przeznaczony do działania w witrynie sieci web usługi Facebook. Jest on oparty na platformie ASP.NET MVC i używa interfejsu API sieci Web dla funkcji aktualizacji w czasie rzeczywistym.

Żadnych opcji uwierzytelniania są dostępne dla szablonu usługi Facebook, ponieważ aplikacje usługi Facebook uruchamiane w ramach witryny Facebook i polegać na uwierzytelnianie w usłudze Facebook.

Aby uzyskać więcej informacji na temat aplikacji platformy ASP.NET w usłudze Facebook, zobacz [Aktualizowanie interfejsu API usługi Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Szablony programu Visual Studio 2012

Okno dialogowe tworzenia projektu sieci web programu Visual Studio 2013 nie zapewnia dostępu do niektórych szablonów, które były dostępne w programie Visual Studio 2012. Jeśli chcesz użyć jednego z tych szablonów, możesz kliknąć węzeł programu Visual Studio 2012 w lewym okienku okna dialogowego nowego projektu programu Visual Studio.

![Szablony programu Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

**Programu Visual Studio 2012** umożliwia węzła, możesz wybrać następujące szablony sieci web, które nie mają dostępu do domyślnej liście szablonów dla programu Visual Studio 2013:

- Aplikacja sieci Web platformy ASP.NET MVC 4
- Aplikacja sieci Web ASP.NET Dynamic Data Entities
- Kontrolki serwera ASP.NET AJAX
- Rozszerzenie kontrolki serwerowej AJAX dla ASP.NET
- Formant serwera ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Uruchomienie w szablonach projektu sieci web programu Visual Studio 2013

Szablony projektu Visual Studio 2013 korzystają [Bootstrap](http://getbootstrap.com/), układ i motywów framework, utworzone przez usługi Twitter. Usługa ładowania początkowego używa CSS3, aby zapewnić elastyczne, co oznacza, że układy dynamicznie dostosowują się do innej przeglądarki rozmiary okna. Na przykład w oknie przeglądarki szerokości strony głównej, utworzona przez szablon formularzy sieci Web wygląda podobnie do poniższej ilustracji:

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image16.png)

Upewnij się, Zmniejsz szerokość okna i ułożonymi w poziomie kolumny, Przenieś do rozmieszczenie w pionie:

![Rozmieszczenie bootstrap kolumnę pionową](creating-web-projects-in-visual-studio/_static/image17.png)

Nieco bardziej zawęzić okna i poziomy menu u góry jest przekształcany ikony, który można kliknąć, aby rozwinąć w orientacji pionowej menu:

![Ikony bootstrap menu](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap menu pionowe](creating-web-projects-in-visual-studio/_static/image19.png)

Można również użyć funkcji motywów Bootstrap firmy, można łatwo dokonać zmian w aplikacji wyglądu i działania. Na przykład można wykonać poniższe kroki, aby zmienić motyw.

1. W przeglądarce przejdź do [ http://Bootswatch.com ](http://Bootswatch.com), umożliwia wybranie motywu, a następnie kliknij przycisk **Pobierz**. (Spowoduje to pobranie *bootstrap.min.css* domyślnie; Jeśli chcesz sprawdzić kod CSS, Pobierz *bootstrap.css* zamiast wersji zminimalizowana.)
2. Skopiuj zawartość pobranego pliku CSS.
3. W programie Visual Studio Utwórz nowy **arkusza stylów** plik o nazwie *bootstrap-theme.css* w *zawartości* folder i Wklej pobrany CSS kodu do niego.
4. Otwórz *aplikacji\_Start/Bundle.config* i zmień *bootstrap.css* do *bootstrap-theme.css*.

Uruchom projekt ponownie, a aplikacja ma nowy wygląd. Poniższa ilustracja przedstawia efekt motyw Amelia:

![Motywie bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Wiele motywów Bootstrap są dostępne, wersji bezpłatnej i premium. Bootstrap oferuje szeroką gamę składników interfejsu użytkownika, takie jak [list rozwijanych](http://twitter.github.io/bootstrap/components.html#dropdowns), [przycisk grup](http://twitter.github.io/bootstrap/components.html#buttonGroups), i [ikony](http://twitter.github.io/bootstrap/base-css.html#images). Aby uzyskać więcej informacji na temat ładowania początkowego zobacz [Bootstrap witryny](http://twitter.github.io/bootstrap/).

Jeśli używany jest Projektant formularzy sieci Web w programie Visual Studio, należy pamiętać, że projektant nie obsługuje CSS3, dlatego dokładnie nie pokazuje wszystkie efekty Bootstrap motywy lub zmiany układu dynamicznego. Jednak stron formularzy sieci Web będą wyświetlane poprawnie, podczas wyświetlania w przeglądarce.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Dodano obsługę dodatkowych platform

Po wybraniu szablonu, automatycznie zaznaczono pole wyboru dla framework(s) używany przez szablon. Na przykład w przypadku wybrania **formularzy sieci Web** szablonu, **formularzy sieci Web** pole wyboru jest zaznaczone i nie można jej wyczyścić.

![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image21.png)

![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image22.png)

Można wybrać pole wyboru dla strukturę, która nie jest zawarta w szablonie, aby można było dodać obsługę tej struktury, gdy projekt zostanie utworzony. Na przykład, aby umożliwić korzystanie z formularzy sieci Web *.aspx* stron po wybraniu szablonu MVC wybierz **formularzy sieci Web** pole wyboru. Lub aby włączyć MVC, podczas korzystania z szablonów formularzy sieci Web, kliknij przycisk **MVC** pole wyboru. Dodanie platforma umożliwia pomocy technicznej czasu projektowania, jak również w czasie wykonywania. Na przykład jeśli dodasz Obsługa MVC do projektu formularzy sieci Web można tworzyć szkielety, widoków i kontrolerów.

Jeśli łączenie MVC i formularzy sieci Web w projekcie i Włącz [przyjazne adresy URL](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) w formularzach sieci Web, mogą istnieć nieoczekiwany routing problemów, gdzie jeden adres URL ma wiele elementów docelowych to możliwe. Najpierw tras, które są zdefiniowane będą miały pierwszeństwo. Na przykład, jeśli masz `Home` kontrolera i *Home.aspx* stronie `http://contoso.com/home` adres URL zostanie umieszczona na *Home.aspx* Jeśli wywołasz `EnableFriendlyUrls` metoda przed wywołaniem `MapRoute`method in Class metoda *RouteConfig.cs*, lub ten sam adres URL będzie przejdź do widoku domyślnego dla usługi `Home` kontrolera, jeśli wywołasz `MapRoute` przed `EnableFriendlyUrls`.

Dodawanie struktury nie dodaje żadnych funkcji aplikacji przykładowej. Na przykład jeśli dodasz formularzy sieci Web pomocy technicznej po wybraniu szablonu MVC nie *Default.aspx* zostanie utworzony plik strony głównej. Tylko folderów, plików i odwołań wymagane do obsługi w ramach są dodawane. Z tego powodu dodawanie struktur nie zmienia opcje uwierzytelniania, które są implementowane przez kod w przykładowe aplikacje utworzone za pomocą szablonów. Na przykład, jeśli wybierzesz pusty szablon i Dodaj formularzy sieci Web lub MVC obsługuje, **Konfigurowanie uwierzytelniania** przycisk nadal będzie wyłączony.

W poniższych sekcjach krótko opisano wpływ każdego pola wyboru.

### <a name="add-web-forms-support"></a>Dodanie obsługi formularzy sieci Web

Tworzy pusty *aplikacji\_danych* i *modeli* folderów i *Global.asax* pliku. Są one już tworzone przez wszystkie szablony innych niż pustego szablonu, więc zaznaczenie pola wyboru formularzy sieci Web nie ma wpływu na inne szablony.

Szablon formularzy sieci Web włącza przyjazne adresy URL, domyślnie, ale po dodaniu obsługi formularzy sieci Web do innych szablonów przez zaznaczenie pola wyboru formularzy sieci Web, który przyjazne adresy URL nie są automatycznie włączane.

### <a name="add-mvc-support"></a>Dodanie obsługi MVC

Instaluje pakiety MVC Razor i NuGet stron sieci Web, tworzy pustą *aplikacji\_danych*, *kontrolerów*, *modeli*, i *widoków*folderów, tworzy *aplikacji\_Start* folder z *RouteConfig.cs* pliku i tworzy *Global.asax* pliku.

### <a name="add-web-api-support"></a>Dodanie obsługi interfejsu API sieci Web

Instaluje pakiety Newtonsoft.Json NuGet i WebApi, tworzy pustą *aplikacji\_danych*, *kontrolerów*, i *modeli* folderów, tworzy  *Aplikacja\_Start* folder z *WebApiConfig.cs* pliku i tworzy *Global.asax* pliku.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metody uwierzytelniania

Visual Studio 2013 oferuje kilka opcji uwierzytelniania szablony formularzy sieci Web, MVC i interfejs API sieci Web:

- [Bez uwierzytelniania](#noauth)
- [Indywidualne konta użytkowników](#indauth) (produktu ASP.NET Identity, znana wcześniej jako członkostwa ASP.NET)
- [Konta organizacyjne](#orgauth) (Windows Server Active Directory lub Azure Active Directory)
- [Uwierzytelnianie Windows](#winauth) (sieć Intranet)

![Konfigurowanie okna dialogowego uwierzytelniania](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Bez uwierzytelniania

Jeśli wybierzesz **bez uwierzytelniania**Przykładowa aplikacja będzie zawierać nie stron sieci web do zalogowania się, nie UI wskazujący, który jest zalogowany, nie jednostki klasy członkowskiej bazie danych i nie parametrów połączenia dla bazy danych członkostwa.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Indywidualne konta użytkowników

Jeśli wybierzesz **indywidualne konta użytkowników**, przykładowa aplikacja zostanie skonfigurowany do użycia produktu ASP.NET Identity (wcześniej znane jako członkostwa ASP.NET), do uwierzytelniania użytkowników. ASP.NET Identity umożliwia użytkownikowi zarejestrować konto usługi, tworząc nazwy użytkownika i hasła w witrynie lub logowanie się przy użyciu dostawców sieci społecznościowych, takich jak Facebook, Google, Account Microsoft lub Twitter. Domyślny magazyn danych profilów użytkowników w produkcie ASP.NET Identity jest bazą danych programu SQL Server LocalDB, którą można wdrożyć dla witryny produkcyjnej do programu SQL Server lub usługi Azure SQL Database.

W programie Visual Studio 2013 te funkcje są takie same jak w programie Visual Studio 2012, ale ma został przepisany podstawowego kodu dla systemu członkostwa programu ASP.NET. Korzyści wynikające z nowych bazy kodu są następujące:

- Nowy system członkostwa opiera się na [OWIN](http://owin.org/) zamiast modułu uwierzytelnianie formularzy programu ASP.NET. Oznacza to, że ten sam mechanizm uwierzytelniania można używać zarówno korzystania z formularzy sieci Web lub MVC w usługach IIS lub samodzielnie przechowujesz interfejsu API sieci Web lub SignalR.
- Nowa baza danych członkostwa jest zarządzana przez Entity Framework Code First i wszystkie tabele są reprezentowane przez klas jednostek, które można modyfikować. Oznacza to, że można łatwo dostosować schematu bazy danych i powiązane z profilem interfejsu użytkownika sieci web do własnych potrzeb i łatwo możesz wdrożyć aktualizacje za pomocą migracje Code First.

Nowy system członkostwa jest zaimplementowane automatycznie w nowych szablonów i może być zaimplementowany ręcznie w każdym projekcie, który jest przeznaczony dla platformy .NET 4.5 lub nowszej.

ASP.NET Identity jest dobrym wyborem w przypadku tworzenia witryny sieci web internetowe, który jest przeznaczony głównie dla klientów zewnętrznych. Jeśli Twoja organizacja korzysta z usługi Active Directory lub Office 365 i chcesz utworzyć projekt, który umożliwia logowanie jednokrotne dla pracowników i partnerów biznesowych, **kont organizacyjnych** rozwiązaniem może być lepszym rozwiązaniem.

Aby uzyskać więcej informacji na temat opcji indywidualne konta użytkowników zobacz następujące zasoby:

- [www.asp.net/identity](../../../identity/index.md). Dokumentacji dotyczącej produktu ASP.NET Identity w witrynie sieci web programu ASP.NET.
- [Tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Przedstawiono również sposób dostosować dane profilu użytkownika.
- [Interfejs API sieci Web - zewnętrznych usług uwierzytelniania](../../../web-api/overview/security/external-authentication-services.md)
- [Dodawania logowań zewnętrznych do aplikacji platformy ASP.NET w programie Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Konta organizacyjne

Jeśli wybierzesz **kont organizacyjnych**, przykładowa aplikacja zostanie skonfigurowana w celu Windows Identity Foundation (WIF) jest używany do uwierzytelniania, na podstawie kont użytkowników w usłudze Azure Active Directory (Azure AD, która obejmuje usługi Office 365) lub Windows Server Active Directory. Aby uzyskać więcej informacji, zobacz [opcje uwierzytelniania konto organizacyjne](#orgauthoptions) w dalszej części tego tematu.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Jeśli wybierzesz **uwierzytelniania Windows**, przykładowa aplikacja zostanie skonfigurowana w celu użyć modułu programu Windows usługi IIS do uwierzytelniania. Aplikacja wyświetla identyfikator domeny i użytkownika usługi Active directory lub konto komputera lokalnego, który jest zalogowany do Windows, ale nie obejmują rejestracja użytkownika lub zalogować się w interfejsie użytkownika. Ta opcja jest przeznaczona dla witryn intranetowych.

Alternatywnie można utworzyć witryny intranetowej, która korzysta z uwierzytelniania AD, wybierając [On-Premises opcję w obszarze konta organizacyjne](#orgauthonprem). Opcja lokalnej przy użyciu Windows Identity Foundation (WIF) zamiast moduł uwierzytelniania Windows. Dodatkowe kroki są wymagane do skonfigurowania opcji lokalnego, ale program WIF umożliwia funkcje, które nie są dostępne za pomocą modułu uwierzytelniania Windows. Na przykład za pomocą programu WIF, można skonfigurować dostęp do aplikacji w usłudze Active Directory i zapytań danych katalogu.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opcje uwierzytelniania konto organizacyjne

**Konfigurowanie uwierzytelniania** okna dialogowego zapewnia kilka opcji usługi Azure Active Directory (Azure AD, która obejmuje usługi Office 365) lub uwierzytelnianie za pomocą konta systemu Windows Server Active Directory (AD):

- [Chmura — pojedyncza organizacja](#orgauthsingle) (Azure AD lub AD za pomocą integracji katalogu z usługą Azure AD)
- [Chmura — wiele organizacji](#orgauthmulti) (Azure AD lub AD za pomocą integracji katalogu z usługą Azure AD)
- [W środowisku lokalnym](#orgauthonprem) (AD)

Jeśli chcesz wypróbować jedną z opcji usługi Azure AD, ale nie masz jeszcze konta [kliknij tutaj, aby utworzyć konto usługi Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Jeśli zostanie wybrana jedna z opcji usługi Azure AD, Twój projekt wymaga bazy danych i trzeba zalogować się do konta administratora globalnego dla dzierżawy usługi Azure AD. Wprowadź nazwę i hasło dla konta organizacji (na przykład admin@contoso.onmicrosoft.com), ma uprawnienia administratora dla dzierżawy usługi Azure AD.
> 
> **Nie należy wprowadzić poświadczenia dla konta Microsoft (na przykład contoso@hotmail.com) w oknie dialogowym logowania.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Chmura — uwierzytelnianie jednej organizacji

![Uwierzytelnianie jednej organizacji](creating-web-projects-in-visual-studio/_static/image24.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w jednej usłudze Azure AD [dzierżawy](https://technet.microsoft.com/library/jj573650.aspx). Na przykład witryna jest contoso.com i będzie on dostępny dla pracowników firmy Contoso, którzy są w dzierżawie contoso.onmicrosoft.com. Nie można skonfigurować usługę Azure AD, aby umożliwić użytkownikom z innych dzierżaw, aby uzyskać dostęp do aplikacji.

#### <a name="domain"></a>Domain

Wprowadź domenę usługi Azure AD, który chcesz skonfigurować aplikację, na przykład: `contoso.onmicrosoft.com`. Jeśli masz [domeny niestandardowej](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), takich jak `contoso.com` zamiast `contoso.onmicrosoft.com`, można wprowadzić, w tym miejscu.

#### <a name="access-level"></a>Poziom dostępu

Jeśli aplikacja musi wysyłać zapytania lub zaktualizować informacje o katalogu przy użyciu interfejsu API programu Graph, wybierz **logowanie jednokrotne, Czytaj dane katalogu** lub **logowanie jednokrotne, Odczyt i zapis danych katalogu**. W przeciwnym razie wybierz **logowania jednokrotnego**. Aby uzyskać więcej informacji, zobacz [poziomów dostępu aplikacji](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) i [przy użyciu interfejsu API programu Graph do wykonywania zapytań w usłudze Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Identyfikator URI Identyfikatora aplikacji

Domyślnie szablon tworzy identyfikator URI aplikacji dla Ciebie przez dodanie nazwy projektu do domeny usługi Azure AD. Na przykład, jeśli nazwa projektu jest `Example` a domena nazywa się `contoso.onmicrosoft.com`, aplikacja stanie się identyfikator URI `https://contoso.onmicrosoft.com/Example`. Jeśli chcesz ręcznie określić identyfikator URI aplikacji, rozwiń **więcej opcji** sekcji, a następnie wprowadź identyfikator URI aplikacji w polu tekstowym. Aplikacji, identyfikator URI musi zaczynać się od `https://`.

Domyślnie jeśli aplikacja, która została już przydzielona w usłudze Azure AD ma ten sam identyfikator URI aplikacji jako jeden, który używa programu Visual Studio dla projektu, projekt będzie połączony z istniejącej aplikacji zamiast inicjowania obsługi administracyjnej nowej. Nowa aplikacja do zainicjowania obsługi administracyjnej w takiej sytuacji należy wyczyścić **Zastąp wpis aplikacji, jeśli o tym samym identyfikatorze już istnieje** pole wyboru.

Jeśli **Zastąp** pole wyboru jest wyczyszczone, a program Visual Studio znajdzie istniejącej aplikacji za pomocą tej samej aplikacji, identyfikator URI, tworzy nowy identyfikator URI, dołączając do identyfikatora URI został przejściem do użycia. Załóżmy, że nazwa projektu jest `Example`, pole będzie puste pole tekstowe, możesz wyczyścić **Zastąp** pole wyboru i dzierżawy usługi Azure AD ma już aplikacja o identyfikatorze URI `https://contoso.onmicrosoft.com/Example`. W takim przypadku nowych aplikacji będą udostępniane z aplikacji, takich jak identyfikator URI `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Inicjowanie obsługi administracyjnej aplikacji w usłudze Azure AD

Aby aprowizować aplikację w usłudze Azure AD lub połączyć projekt z istniejących aplikacji, program Visual Studio wymaga poświadczeń administratora globalnego dla domeny. Po kliknięciu **OK** w **Konfigurowanie uwierzytelniania** okno dialogowe, zostanie wyświetlony monit o nazwę użytkownika i hasło administratora globalnego dla domeny określonej. Później, po kliknięciu **Tworzenie projektu** w **nowy projekt ASP.NET** okno dialogowe, Visual Studio obsługuje aplikacji w usłudze Azure AD. Należy pamiętać, że w ramach tego procesu programu Visual Studio osadza wartości klucza tajnego klienta w pliku Web.config, który wygasa rok po utworzeniu.

Aby uzyskać informacje o sposobie tworzenia aplikacji, które używają **chmura — pojedyncza organizacja** uwierzytelniania, zobacz następujące zasoby:

- [Uwierzytelnianie usługi Azure](../2012/windows-azure-authentication.md)
- [Dodawanie logowania do aplikacji sieci Web przy użyciu usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Tworzenie aplikacji ASP.NET z wykorzystaniem usługi Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Zabezpieczanie wzorca ASP.NET Web API z usługą Azure AD i składniki Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Samouczki jeszcze nie zostało zaktualizowane dla programu Visual Studio 2013; Zautomatyzowana jest część jakie samouczków bezpośrednie wykonywanie ręcznie w programie Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Chmura — wiele organizacji uwierzytelniania

![Wiele organizacji uwierzytelnień](creating-web-projects-in-visual-studio/_static/image25.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w usłudze Azure AD w wielu [dzierżaw](https://technet.microsoft.com/library/jj573650.aspx). Na przykład witryna jest contoso.com i będzie on dostępny do pracowników firmy Contoso, którzy są w dzierżawie contoso.onmicrosoft.com i pracowników firmy Fabrikam, którzy są w dzierżawie fabrikam.onmicrosoft.com.

Ustawienia, które należy wprowadzić i aprowizacji kroku aplikacji są podobne do [uwierzytelniania jednej organizacji](#orgauthsingle).

Aby uzyskać informacje o sposobie tworzenia aplikacji, które używają **chmura — wiele organizacji** uwierzytelniania, zobacz następujące zasoby:

- [Łatwa integracja aplikacji sieci Web za pomocą usługi Azure Active Directory, ASP.NET &amp; programu Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) na blogu zespołu usługi Active Directory.
- [Tworzenie wielodostępnych aplikacji sieci Web z usługą Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) samouczka. Samouczek jeszcze nie zostało zaktualizowane dla programu Visual Studio 2013; Zautomatyzowana jest część co samouczka kieruje, można wykonywać ręcznie w programie Visual Studio 2013.
- [Musisz zarejestrować się przy użyciu własnej wielu aplikacji ASP.NET organizacji przed zarejestrowaniem się](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog autorstwa Vittorio Bertocci, który objaśnia, jak rozwiązywać typowe osób problem wystąpi podczas tworzenia projektu, który korzysta z uwierzytelniania dla wielu organizacji.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Uwierzytelnianie organizacyjne w środowisku lokalnym

![Uwierzytelnianie organizacyjne w środowisku lokalnym](creating-web-projects-in-visual-studio/_static/image26.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelniania dla kont użytkowników, które są zdefiniowane w systemie Windows Server Active Directory (AD), a nie chcesz używać usługi Azure AD. Ta opcja służy do tworzenia witryny intranetowej lub witryny internetowej. Dla witryny internetowej należy użyć Active Directory Federation Services (ADFS) pozwala zapewnić dostęp do usługi AD. Aby uzyskać więcej informacji, zobacz [korzystać z opcji uwierzytelniania lokalnej organizacji (ADFS) za pomocą programu ASP.NET w programie Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

W przypadku witryny intranetowej, jako alternatywę można wybrać [uwierzytelniania Windows](#winauth) zamiast tej opcji. Dla opcji uwierzytelniania Windows nie trzeba podać adres URL dokumentu metadanych. Jednak uwierzytelniania Windows zapewnia możliwości kontroli dostępu aplikacji w usłudze Active Directory lub katalogu zapytania o dane.

#### <a name="on-premises-authority"></a>Urząd lokalny

Wprowadź adres URL wskazujący dokument metadanych. Ten dokument metadanych zawiera współrzędne urzędu. Aplikacja będzie używać tych współrzędnych można dostarczać przepływ logowania jednokrotnego w sieci web.

#### <a name="application-id-uri"></a>Identyfikator URI Identyfikatora aplikacji

Podaj unikatowy identyfikator URI, używanego przez usługi AD do identyfikowania tej aplikacji, lub pozostaw puste, aby umożliwić programowi Visual Studio, utwórz ją.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

Ten dokument udostępnia podstawowe pomocy do tworzenia nowego projektu sieci web platformy ASP.NET w programie Visual Studio 2013. Aby uzyskać więcej informacji o korzystaniu z programu Visual Studio do tworzenia aplikacji internetowych, zobacz [ https://www.asp.net/visual-studio/ ](../../index.md).
