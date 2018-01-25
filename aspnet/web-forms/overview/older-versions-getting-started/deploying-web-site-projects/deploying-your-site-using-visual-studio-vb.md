---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: "Wdrażanie witryny za pomocą programu Visual Studio (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Visual Studio zawiera narzędzia do wdrażania witryny sieci Web. Dowiedz się więcej o tych narzędzi, w tym samouczku."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 362f8391f3352b3abf00045bca0c212cd850b17f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Wdrażanie witryny za pomocą programu Visual Studio (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio zawiera narzędzia do wdrażania witryny sieci Web. Dowiedz się więcej o tych narzędzi, w tym samouczku.


## <a name="introduction"></a>Wprowadzenie

Samouczek poprzedniego przeglądał wdrażanie prostej aplikacji sieci web ASP.NET do dostawcy hosta sieci web. W szczególności samouczku pokazano, jak używać klient FTP, takich jak FileZilla do przesyłania niezbędne pliki środowiska programowania do środowiska produkcyjnego. Program Visual Studio oferuje również wbudowane narzędzia ułatwiające wdrażanie u dostawcy hosta sieci web. W tym samouczku sprawdza, czy dwa z tych narzędzi: narzędzie kopiowanie witryny sieci Web, gdzie przenoszenia plików do i z serwera zdalnego w sieci web przy użyciu protokołu FTP lub rozszerzeń serwera FrontPage; i narzędzie publikowania, które kopiuje całej witryny sieci Web w określonej lokalizacji.


> [!NOTE]
> Inne narzędzia dotyczący wdrażania oferowane przez program Visual Studio obejmują [projekty Instalatora sieci Web](https://msdn.microsoft.com/library/wx3b589t.aspx) i [projekty sieci Web wdrożenia](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) dodatku. Projekty sieci Web Instalatora pakietu zawartości witryny sieci Web i informacje o konfiguracji w jednym pliku MSI. Ta opcja jest najbardziej przydatne dla witryn sieci Web, które są wdrożone w intranecie lub dla firm, które sprzedawać aplikacji sieci web opakowanych zainstalować klientów na serwerach sieci web. Dodatek projekty wdrażania sieci Web jest Visual Studio dodatku ułatwiający określenie konfiguracji różnice między kompilacji dla środowisk projektowych oraz środowisk produkcyjnych. Projekty Instalatora sieci Web nie zostały omówione w tym samouczku; Projekty wdrażania w sieci Web zostały podsumowane w [ *typowych konfiguracji różnice między rozwoju i produkcji* ](common-configuration-differences-between-development-and-production-vb.md) samouczka.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Wdrażanie witryny za pomocą narzędzia Kopia witryny sieci Web

Narzędzie kopiowanie witryny sieci Web programu Visual Studio jest podobne do autonomicznej klienta FTP. Mówiąc narzędzie kopiowanie witryny sieci Web umożliwia łączenie z do zdalnej witryny sieci web za pomocą protokołu FTP lub rozszerzeń serwera FrontPage. Podobnie jak w FileZilla interfejsu użytkownika, interfejs użytkownika kopiowanie witryny sieci Web zawiera dwa okienka: okienka po lewej stronie wymieniono pliki lokalne, a w okienku po prawej stronie listy tych plików na serwerze docelowym.

> [!NOTE]
> Narzędzie kopiowanie witryny sieci Web jest dostępna tylko dla projektów witryny sieci Web. Visual Studio oferują tego narzędzia, podczas pracy z projektu aplikacji sieci Web.


Spójrzmy na przy użyciu narzędzia kopiowanie witryny sieci Web do publikowania aplikacji przejrzyj książki do środowiska produkcyjnego. Ponieważ narzędzie kopiowanie witryny sieci Web działa tylko w przypadku projektów, w których jest używany model projekt witryny sieci Web można tylko omówione w projekcie BookReviewsWSP za pomocą tego narzędzia. Otwórz ten projekt.

Uruchamianie projektu narzędzia kopiowanie witryny sieci Web, klikając ikonę kopiowanie witryny sieci Web w Eksploratorze rozwiązań (Ta ikona jest zaznaczona kółkiem na rysunku 1); Alternatywnie można wybrać opcję Kopiuj witrynę sieci Web z menu witryny sieci Web. Każda metoda uruchamia kopiowanie witryny sieci Web interfejsu użytkownika pokazano na rysunku 1; tylko lewym okienku na rysunku nr 1 jest wypełniana ponieważ jeszcze mamy się nawiązać połączenia z serwerem zdalnym.


[![Interfejs użytkownika narzędzia kopiowania witryny sieci Web jest podzielony na dwa okienka](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Rysunek 1**: kopiowanie witryny sieci Web narzędzia interfejsu użytkownika jest podzielony na dwa okienka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image3.png))


W celu wdrożenia naszej witrynie należy najpierw połączyć się z dostawcą hosta sieci web. Kliknij przycisk Połącz w górnej części interfejsu użytkownika kopiowanie witryny sieci Web. Zostaną wyświetlone okno dialogowe Otwórz witrynę sieci Web pokazany na rysunku 2.

Docelowej witryny sieci Web można nawiązać, wybierając jedną z czterech opcji z lewej strony:

- **System plików** — wybierz to do wdrożenia witryny do folderu lub udziału sieciowego dostępny z komputera.
- **Lokalne usługi IIS** — ta opcja służy do wdrażania witryny na serwerze sieci web usług IIS zainstalowana na danym komputerze.
- **Witryny FTP** — Połącz ze zdalną stroną sieci web za pomocą protokołu FTP.
- **Lokacji zdalnej** -połączenia do zdalnej witryny sieci Web używa rozszerzeń serwera FrontPage.

Większość dostawców usług hosta sieci web obsługuje FTP, ale mniej oferują obsługę rozszerzenia serwera FrontPage. Z tego powodu I została wybrana opcja witryny FTP i następnie wprowadzić informacje o połączeniu, jak pokazano na rysunku 2.


[![Określ miejsce docelowe witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Rysunek 2**: Określ miejsce docelowe witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Po nawiązaniu połączenia narzędzia kopiowanie witryny sieci Web ładuje plików w lokacji zdalnej w okienku po prawej stronie i wskazuje stan każdego pliku: nowe, Deleted, zmienione lub Unchanged. Można skopiować pliku z lokacji lokalnej do zdalnej witryny lub odwrotnie a.

Dodajmy nowej strony do projektu BookReviewsWSP, a następnie wdrożyć go, dzięki czemu możemy stwierdzić, narzędzie kopiowanie witryny sieci Web w akcji. Utwórz nową stronę ASP.NET w programie Visual Studio w katalogu głównym o nazwie `Privacy.aspx`. Strona, użyj strony wzorcowej `Site.master` i Dodaj zasady zachowania poufności informacji witryny do tej strony. Rysunek 3 przedstawia Visual Studio po utworzeniu tej strony.


[![Dodaj nowy składnik o nazwie strony &lt;kod&gt;Privacy.aspx&lt;/kod&gt; folder główny witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Rysunek 3**: Dodaj nazwę nowej strony `Privacy.aspx` folder główny witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Następnie wróć do interfejsu użytkownika kopiowanie witryny sieci Web. Jak pokazano na rysunku 4, w okienku po lewej stronie zawiera teraz nowe pliki - `Policy.aspx` i `Policy.aspx.vb`. Co więcej pliki te są oznaczone ikoną strzałkę oraz stanu z nowej, wskazujący, że istnieją one w lokalnej witrynie, ale nie na zdalnej witrynie.


[![Narzędzie kopii witryny sieci Web zawiera nowe &lt;kod&gt;Privacy.aspx&lt;/kod&gt; strony w jej lewe okienko](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Rysunek 4**: narzędzie kopii witryny sieci Web zawiera nowe `Privacy.aspx` strony w jej lewe okienko ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Aby wdrożyć nowe pliki, wybierz je, a następnie kliknij ikonę strzałki, aby przenieść je do zdalnej witryny. Po ukończeniu transferu `Policy.aspx` i `Policy.aspx.vb` pliki istnieją w obu lokacjach lokalnych i zdalnych w stanie Unchanged.

Wraz z listę nowych plików, narzędzie kopiowanie witryny sieci Web prezentuje wszystkie pliki, które różnią się między lokacjami lokalnymi i zdalnymi. Aby wyświetlić to działanie, wróć do `Privacy.aspx` i dodać kilka słów więcej do zasady zachowania poufności informacji. Zapisz stronę, a następnie wróć do narzędzia kopiowanie witryny sieci Web. Jak pokazano na rysunku nr 5, `Privacy.aspx` strony w okienku po lewej stronie ma stan zmienione co oznacza, że synchronizowane ze zdalnej witryny.


[![Narzędzie kopii witryny sieci Web oznacza to, że &lt;kod&gt;Privacy.aspx&lt;/kod&gt; strony został zmieniony.](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Rysunek 5**: narzędzia witryny sieci Web kopiowania wskazuje, że `Privacy.aspx` strony zostało zmienione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Narzędzie kopiowanie witryny sieci Web wskazuje także, czy plik został usunięty od czasu ostatniej operacji kopiowania. Usuń `Privacy.aspx` z lokalnym projektu i Odśwież narzędzia kopiowanie witryny sieci Web. `Privacy.aspx` i `Privacy.aspx.vb` pliki pozostają wymienionych w lewym okienku, ale musi mieć stan usunięte wskazujący, że te zostały usunięte od czasu ostatniej operacji kopiowania.

## <a name="publishing-a-web-application"></a>Publikowanie aplikacji sieci Web

Innym sposobem wdrożenia aplikacji sieci web w programie Visual Studio jest opcja publikowania, który jest dostępny za pośrednictwem menu Kompiluj. Polecenie Publikuj jawnie kompiluje aplikacji, a następnie kopiuje wszystkie pliki niezbędne do określonej lokacji zdalnej. Jak zajmiemy się wkrótce, opcja publikowania jest bardziej stożkowa niż przy użyciu narzędzi kopiowanie witryny sieci Web. Narzędzie kopiowanie witryny sieci Web umożliwia Sprawdź pliki w lokacjach zdalnych i lokalnych i zezwala na przekazywanie lub pobieranie pojedyncze pliki, zgodnie z potrzebami, opcja publikowania wdraża całej aplikacji sieci web.


Oprócz kopiowania wszystkie wymagane pliki do określonej lokacji zdalnej, opcja publikowania kompiluje również jawnie aplikacji. Biorąc pod uwagę, że projekty aplikacji sieci Web muszą być jawnie skompilowany powinna ona jako nie niespodziewanego, że opcja publikowania jest dostępna dla projektów aplikacji sieci Web. Które mogą być nieco zaskakująco jest opcja publikowania jest również dostępne dla projektów witryny sieci Web. Zgodnie z opisem w [ *określania co pliki potrzebne do wdrożenia* ](determining-what-files-need-to-be-deployed-vb.md) samouczek, projektów witryny sieci Web może zostać jawnie skompilowany przez proces, nazywany *prekompilacja*. Ten samouczek koncentruje się na użycie opcji publikowania z projektów aplikacji sieci Web; Samouczek przyszłych zastosuje wstępnej kompilacji, po czym zostanie zwrócona aby przyjrzeć się z projektami witryn sieci Web przy użyciu opcji publikowania.

> [!NOTE]
> Gdy opcja publikowania jest dostępna w programie Visual Studio dla projektów witryny sieci Web i projekty aplikacji sieci Web, Visual Web Developer oferuje tylko opcja publikowania dla projektów aplikacji sieci Web.


Przyjrzyjmy się wdrożenie aplikacji przeglądami książki przy użyciu opcji publikowania. Rozpocznij od otwierania BookReviewsWAP (projekt aplikacji sieci Web) w programie Visual Studio. Z menu publikowanie wybierz BookReviewsWAP kompilacji projektu. Wywołuje okno dialogowe z monitem o lokalizacji docelowej, wśród innych opcji konfiguracji (patrz rysunek 6). Podobnie jak za pomocą narzędzia kopiowanie witryny sieci Web można wprowadzić lokalizacji, która wskazuje na folder lokalny, lokalną witrynę sieci Web w usługach IIS, zdalnej witryny sieci Web, który obsługuje rozszerzenia serwera FrontPage lub adresu serwera FTP. Możesz wybrać, czy należy zastąpić pliki na serwerze sieci web do zdalnego wdrożonych plików lub usunąć całą zawartość na zdalnej witrynie przed opublikowaniem. Można również określić, czy do skopiowania:

- Tylko pliki w projekcie potrzebne do uruchomienia aplikacji, które pomija kod źródłowy niepotrzebnych i plików związanych z projektem.
- Wszystkie pliki projektu, takich jak pliki kodu źródłowego i pliki projektu programu Visual Studio, takie jak plik rozwiązania.
- Wszystkie pliki w źródłowym folderze projektu, która kopiuje wszystkie pliki w folderze projektu źródłowego niezależnie od tego, czy są one dołączone do projektu.

Istnieje również opcja przekazywania zawartość `App_Data` folderu.


[![Określ miejsce docelowe witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Rysunek 6**: Określ miejsce docelowe witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Dla aplikacji przejrzyj książki lokacji zdalnej zawiera pliki wdrożone podczas kopiowania projektu BookReviewsWSP za pomocą narzędzia kopiowanie witryny sieci Web. Załóżmy więc opcji Publikuj uruchomienia przez usunięcie wszystkich istniejących zawartości. Ponadto umożliwia po prostu skopiuj niezbędne pliki, a nie przeładowania z niepotrzebnych źródła kodu i pliki projektu w środowisku produkcyjnym. Po określeniu tych opcji, kliknij przycisk Publikuj. Za pośrednictwem dalej kilka sekund Visual Studio wdroży pliki niezbędne do lokacji docelowej, wyświetlanie postęp w oknie danych wyjściowych.

Rysunek nr 7 przedstawia pliki w witrynie FTP po wykonaniu operacji publikowania. Należy pamiętać, że zostały przekazane na stronach znaczników i pliki pomocnicze niezbędne sever — i po stronie klienta.


[![Opublikowano tylko pliki potrzebne do środowiska produkcyjnego](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Rysunek 7**: tylko niezbędne pliki zostały opublikowane do środowiska produkcyjnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image21.png))


Polecenie Publikuj jest narzędziem nuanced mniej niż przy użyciu narzędzi kopiowanie witryny sieci Web. Narzędzie kopiowanie witryny sieci Web umożliwia przeprowadzanie inspekcji plików w lokacjach zdalnych i lokalnych i zobacz, jak będą się różnić, opcja publikowania stanowi taki interfejs. Ponadto narzędzie kopiowanie witryny sieci Web umożliwia zmianę jednorazowe, przekazywanie lub usuwanie poszczególnych plików. Polecenie Publikuj nie zezwala na takie szczegółową kontrolę; Zamiast tego publikuje *cały* aplikacji. To zachowanie jest jego zalet i wad. Na tej stronie plus wiadomo, korzystając z opcji publikowania, nie będzie można włączaniu przekazywanie ważnych plików. Jednak rozważyć, co się stanie, jeśli wprowadzono niewielkie zmiany do bardzo dużych witryny sieci Web — z opcją publikowania nie może zaktualizować strony lub dwa, który został zmodyfikowany, ale zamiast tego należy poczekać, program Visual Studio wdroży całej witryny.

Nie jest rzadko w celu zapewnienia określonych plików, których zawartość różni się od produkcyjnego i środowisk deweloperskich. Przykład klawisza jest pliku konfiguracji aplikacji, `Web.config`. Ponieważ polecenie Publikuj ślepo kopiuje pliki aplikacji sieci web zastępuje pliki dostosowanej konfiguracji środowiska produkcyjnego z wersją w środowisku programistycznym. Samouczek kolejnych Eksploruje dalej w tym temacie i zawiera wskazówki dotyczące wdrażania aplikacji sieci web, gdy istnieją różnice.

## <a name="summary"></a>Podsumowanie

Wdrażanie witryny sieci Web obejmuje kopiowanie niezbędne pliki środowiska programowania do środowiska produkcyjnego. Poprzednie samouczku przedstawiono sposób transferu plików przy użyciu klienta FTP, takich jak FileZilla. W tym samouczku zbadać dwa narzędzia wdrażania w programie Visual Studio: narzędzie kopiowanie witryny sieci Web i Publikuj. Narzędzie kopiowanie witryny sieci Web jest podobny do klienta FTP, w tym składa się z dwóch paned interfejsu, wyświetlanie listy plików na komputerze lokalnym i określonego komputera zdalnego, który ułatwia przekazywanie lub pobieranie plików między dwoma komputerami. Opcja publikowania jest bardziej stożkowa narzędzie, które jawnie kompiluje projektu, a następnie wdraża całej aplikacji do określonej lokalizacji docelowej.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Kopiowanie witryny sieci Web za pomocą narzędzia Kopia witryny sieci Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Jak I: wdrażać witryny sieci Web za pomocą narzędzia Kopia witryny sieci Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (klip wideo)
- [Porady: Publikowanie projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Porady: Publikowanie witryn sieci Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Projekty instalacji i wdrażania w programie Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[Poprzednie](deploying-your-site-using-an-ftp-client-vb.md)
[dalej](common-configuration-differences-between-development-and-production-vb.md)
