---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Wdrażanie witryny przy użyciu programu Visual Studio (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program Visual Studio zawiera narzędzia służące do wdrażania witryny sieci Web. Dowiedz się więcej na temat tych narzędzi, w tym samouczku.
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: e0dd38bc158dea37d614a48a6783525ed99cd97a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819192"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Wdrażanie witryny przy użyciu programu Visual Studio (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Program Visual Studio zawiera narzędzia służące do wdrażania witryny sieci Web. Dowiedz się więcej na temat tych narzędzi, w tym samouczku.


## <a name="introduction"></a>Wprowadzenie

Poprzedni Samouczek przyjrzano się jak wdrożyć prostą aplikację sieci web ASP.NET u dostawcy hosta sieci web. W szczególności samouczku pokazano, jak za pomocą klienta FTP, takich jak FileZilla przenosić niezbędne pliki środowiska programistycznego do środowiska produkcyjnego. Program Visual Studio udostępnia również wbudowane narzędzia ułatwiające wdrażanie dostawca hosta sieci web. Ten samouczek analizuje dwa z tych narzędzi: narzędzia kopiowania witryny sieci Web, gdzie przenoszenie plików do i z serwera sieci web do zdalnego przy użyciu protokołu FTP lub rozszerzenia serwera FrontPage; i narzędzia publikowania, która kopiuje całej witryny sieci Web do określonej lokalizacji.


> [!NOTE]
> Inne narzędzia związanych z wdrażaniem oferowanych przez program Visual Studio obejmują [projektów Instalatora sieci Web](https://msdn.microsoft.com/library/wx3b589t.aspx) i [projekty wdrażania w Internecie](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) dodatku. Projekty Instalatora sieci Web pakietu zawartości witryny sieci Web i informacje o konfiguracji w pojedynczym pliku MSI. Ta opcja jest najbardziej przydatne dla witryn sieci Web, które zostały wdrożone w sieci wewnętrznej lub dla firm, które sprzedawania aplikacji sieci web wstępnie spakowanej, instalowanego przez klientów na serwerach sieci web. Dodatek projekty wdrażania w sieci Web jest Visual Studio dodatek, który ułatwia określanie różnice konfiguracji między środowiskami kompilacji dla środowisk projektowych oraz środowisk produkcyjnych. Projekty Instalatora sieci Web nie zostały omówione w tej serii samouczków; Projekty wdrażania w Internecie są podsumowane w [ *typowych konfiguracji różnice między deweloperskim i produkcyjnym* ](common-configuration-differences-between-development-and-production-vb.md) samouczka.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Wdrażanie witryny przy użyciu narzędzia kopiowania witryny internetowej

Narzędzie kopiowania witryny sieci Web programu Visual Studio jest podobną funkcjonalność autonomicznej klienta FTP. Mówiąc narzędzia kopiowania witryny sieci Web pozwala połączyć się z witryny sieci web za pośrednictwem protokołu FTP lub rozszerzenia serwera FrontPage. Podobnie jak w interfejsie użytkownika programu FileZilla firmy, interfejs użytkownika Kopiuj witrynę sieci Web zawiera dwa okienka: okienka po lewej stronie zawiera listę plików lokalnych, a w okienku po prawej stronie zawiera listę tych plików na serwerze docelowym.

> [!NOTE]
> Narzędzie kopiowania witryny sieci Web jest dostępna tylko dla projektów witryny sieci Web. Program Visual Studio oferuje to narzędzie, jeśli pracujesz z projektu aplikacji sieci Web.


Przyjrzyjmy się przy użyciu narzędzia kopiowania witryny sieci Web, aby opublikować aplikację przeglądu książki do środowiska produkcyjnego. Ponieważ narzędzia kopiowania witryny sieci Web działa tylko w przypadku projektów używających modelu projektu witryny sieci Web firma Microsoft może sprawdzać tylko te z projektem BookReviewsWSP za pomocą tego narzędzia. Otwórz ten projekt.

Uruchom projekt narzędzia kopiowania witryny sieci Web, klikając ikonę Kopiuj witrynę sieci Web w Eksploratorze rozwiązań (Ta ikona jest zaznaczona kółkiem na rysunku 1). Alternatywnie można wybrać opcję Kopiuj witrynę sieci Web, w menu witryny sieci Web. Każda z tych metod uruchamia interfejsu użytkownika witryny sieci Web kopiowania przedstawionej na rysunku 1; tylko okienka po lewej stronie na rysunku 1 jest wypełniana, ponieważ mamy jeszcze nawiązać połączenia z serwerem zdalnym.


[![Interfejs użytkownika narzędzia kopiowania witryny internetowej jest podzielona na dwa okienka](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Rysunek 1**: interfejs użytkownika narzędzia kopiowania witryny internetowej firmy jest podzielona na dwa okienka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image3.png))


W celu wdrożenia w naszej witrynie, musimy najpierw połączyć się z dostawcą hosta sieci web. Kliknij przycisk Połącz w górnej części interfejsu użytkownika witryny sieci Web kopiowania. Zostaną wyświetlone okno dialogowe Otwórz witrynę sieci Web, pokazano na rysunku 2.

Można połączyć do docelowej witryny sieci Web, wybierając jedną z czterech opcji z lewej strony:

- **System plików** — zaznacz tutaj, aby możliwość wdrożenia witryny folderu lub udziału sieciowego dostępne z komputera.
- **Lokalne usługi IIS** — ta opcja służy do wdrażania witryny do serwera sieci web usług IIS, które są zainstalowane na komputerze.
- **Witryna FTP** — Połącz ze zdalną stroną sieci web przy użyciu protokołu FTP.
- **Zdalna witryna** — łączenie do zdalnej witryny sieci Web za pomocą rozszerzenia serwera FrontPage.

Większość dostawców usług hosta sieci web obsługuje FTP, ale mniej oferują obsługę rozszerzenia serwera FrontPage. Z tego powodu I została wybrana opcja witryny FTP i następnie wprowadzić informacje o połączeniu, jak pokazano na rysunku 2.


[![Określ miejsce docelowe witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Rysunek 2**: Określ miejsce docelowe witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Po nawiązaniu połączenia narzędzie kopiowania witryny sieci Web ładuje pliki w zdalnej lokalizacji, w okienku po prawej stronie i wskazuje stan każdego pliku: nowe, usunięte, zmieniono lub Unchanged. Możesz skopiować plik z lokalnej lokacji do lokacji zdalnej lub odwrotnie a.

Dodajmy nową strony do projektu BookReviewsWSP, a następnie wdrożysz go tak, aby widać narzędzia kopiowania witryny sieci Web w działaniu. Tworzenie nowej strony programu ASP.NET w programie Visual Studio, w katalogu głównym o nazwie `Privacy.aspx`. Strona użycia strony wzorcowej `Site.master` i Dodaj zasady zachowania poufności informacji witryny do tej strony. Rysunek 3 przedstawia programu Visual Studio, po utworzeniu tej strony.


[![Dodawanie nowej strony o nazwie &lt;kodu&gt;Privacy.aspx&lt;/code&gt; folder główny witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Rysunek 3**: Dodawanie nowej strony o nazwie `Privacy.aspx` folder główny witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Następnie wróć do interfejsu użytkownika witryny sieci Web kopiowania. Jak pokazano na rysunku 4, w okienku po lewej stronie zawiera teraz nowe pliki - `Policy.aspx` i `Policy.aspx.vb`. Co więcej te pliki są oznaczone ikoną strzałki oraz stan z nowej, wskazującą, czy znajdują się w lokacji lokalnej, ale nie w zdalnej witrynie.


[![Narzędzia kopiowania witryny internetowej zawiera nowe &lt;kodu&gt;Privacy.aspx&lt;/code&gt; strony w jej okienku po lewej stronie](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Rysunek 4**: narzędzia kopiowania witryny internetowej zawiera nowe `Privacy.aspx` strony w jej okienku po lewej stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Aby wdrożyć nowe pliki, zaznacz je, a następnie kliknij ikonę strzałki Aby przenieść je do lokacji zdalnej. Po zakończeniu transferu `Policy.aspx` i `Policy.aspx.vb` pliki znajdują się w obu lokacjach zdalnych i lokalnych przy użyciu stan Unchanged.

Wraz z listą nowych plików, narzędzia kopiowania witryny sieci Web wyróżnia wszystkie pliki, które różnią się między lokacjami lokalnymi i zdalnymi. Aby to zobaczyć w działaniu, wróć do `Privacy.aspx` strony i dodaj kilka więcej słów do zasady zachowania poufności informacji. Zapisz stronę, a następnie wrócić do narzędzia do kopiowania witryny sieci Web. Jak pokazano na rysunku 5, `Privacy.aspx` strony w okienku po lewej stronie ma stan zmieniono wskazującą, że jest zsynchronizowana z lokacji zdalnej.


[![Narzędzia kopiowania witryny internetowej wskazuje, że &lt;kodu&gt;Privacy.aspx&lt;/code&gt; strony został zmieniony.](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Rysunek 5**: narzędzia kopiowania witryny internetowej wskazuje, że `Privacy.aspx` strony został zmieniony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Narzędzie kopiowania witryny sieci Web wskazuje również, jeśli plik został usunięty od czasu ostatniej operacji kopiowania. Usuń `Privacy.aspx` lokalny projekt i Odśwież narzędzia kopiowania witryny sieci Web. `Privacy.aspx` i `Privacy.aspx.vb` pliki pozostają na liście w okienku po lewej stronie, ale musi mieć stan usunięto, wskazujący, że te zostały usunięte od czasu ostatniej operacji kopiowania.

## <a name="publishing-a-web-application"></a>Publikowanie aplikacji sieci Web

Inny sposób wdrażania aplikacji sieci web w programie Visual Studio jest do korzystania z opcji publikowania, który jest dostępny za pośrednictwem menu Kompiluj. Opcja Publikuj jawnie kompiluje aplikację, a następnie kopiuje wszystkie pliki niezbędne do określonej lokacji zdalnej. Jak zajmiemy się wkrótce, opcja Publikuj jest bardziej tępy niż narzędzia kopiowania witryny sieci Web. Narzędzie kopiowania witryny sieci Web umożliwia badanie plików na lokacjach zdalnych i lokalnych i zezwala na przekazywanie lub pobieranie pojedyncze pliki, zgodnie z potrzebami, opcja Publikuj wdraża całej aplikacji sieci web.


Oprócz kopiowania wszystkich wymaganych plików do określonej lokacji zdalnej, opcja Publikuj kompiluje również jawnie aplikacji. Biorąc pod uwagę, że projekty aplikacji sieci Web muszą być skompilowane w sposób jawny go pochodziły jako Nic dziwnego, że że opcja publikowania jest dostępna dla projektów aplikacji sieci Web. Co może być nieco Zaskakujące jest opcja publikowania jest również dostępna dla projektów witryny sieci Web. Jak wspomniano w [ *określająca, które pliki muszą zostać wdrożone* ](determining-what-files-need-to-be-deployed-vb.md) samouczku projektów witryny sieci Web może być jawnie kompilowane przez proces nazywany *wstępnej kompilacji*. Ten samouczek koncentruje się na użycie opcji publikowania z projektów aplikacji sieci Web; przyszłe samouczku przedstawimy wstępnej kompilacji, w tym momencie zostanie zwrócona do wzięcia pod przy użyciu opcji publikowania z projektami witryny sieci Web.

> [!NOTE]
> Gdy opcja publikowania jest dostępna w programie Visual Studio dla projektów witryny sieci Web i projektów aplikacji sieci Web, Visual Web Developer tylko oferuje opcję Publikuj dla projektów aplikacji sieci Web.


Spójrzmy na wdrażanie aplikacji przeglądy książki, za pomocą opcji publikowania. Rozpocznij, otwierając BookReviewsWAP (projekt aplikacji sieci Web) w programie Visual Studio. Menu publikowania wybierz projekt BookReviewsWAP kompilacji. Wywołuje okno dialogowe, zawierająca monit o podanie lokalizacji docelowej, wśród innych opcji konfiguracji (patrz rysunek 6). Znacznie takich jak za pomocą narzędzia kopiowania witryny sieci Web możesz wprowadzić lokalizację, która wskazuje na folder lokalny, lokalną witrynę sieci Web w usługach IIS, zdalnej witryny sieci Web, który obsługuje rozszerzenia serwera FrontPage lub adresu serwera FTP. Możesz wybrać, czy zastąpić pliki na serwerze sieci web do zdalnego plików do wdrożenia lub usunąć całą zawartość na zdalnej witrynie przed opublikowaniem. Można również określić, czy ma być kopiowany:

- Tylko pliki w projekcie potrzebnych do uruchomienia aplikacji, które pomija kod źródłowy niepotrzebne i pliki projektu.
- Wszystkie pliki projektu, takich jak pliki kodu źródłowego i pliki projektu programu Visual Studio, takich jak plik rozwiązania.
- Wszystkie pliki w źródłowym folderze projektu, która kopiuje wszystkie pliki w źródłowym folderze projektu, niezależnie od tego, czy są one dołączone w projekcie.

Istnieje również opcja przekazywania zawartości `App_Data` folderu.


[![Określ miejsce docelowe witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Rysunek 6**: Określ miejsce docelowe witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Przeglądanie książki aplikacji zdalnej witryny zawiera pliki wdrożone podczas kopiowania projektu BookReviewsWSP za pomocą narzędzia kopiowania witryny sieci Web. W związku z tym Przyjrzyjmy się opcji publikowania start, usuwając całą istniejącą zawartość. Ponadto możemy po prostu skopiuj niezbędne pliki, a nie zaśmiecania w środowisku produkcyjnym za pomocą niepotrzebne źródłowych kodu i pliki projektu. Po określeniu tych opcji, kliknij przycisk Publikuj. Za pośrednictwem dalej kilka sekund programu Visual Studio wdroży niezbędne pliki do lokacji docelowej, jej postęp wyświetlania w oknie danych wyjściowych.

Rysunek 7 zawiera pliki w witrynie FTP, po zakończeniu operacji publikowania. Należy pamiętać, że zostały przekazane tylko stronach adiustacji i pliki obsługi niezbędne sever — i po stronie klienta.


[![Tylko potrzebne pliki zostały opublikowane w środowisku produkcyjnym](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Rysunek 7**: tylko potrzebne pliki zostały opublikowane do środowiska produkcyjnego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image21.png))


Opcja publikowania jest narzędziem rozmowach mniej niż narzędzia kopiowania witryny sieci Web. Narzędzie kopiowania witryny sieci Web umożliwia inspekcję plików w lokacjach lokalnych i zdalnych i zobacz, jak się różnią, opcja Publikuj przewiduje taki interfejs nie jest. Narzędzie kopiowania witryny sieci Web pozwala ponadto zmiany jednorazowe, przekazywanie i usuwaniu poszczególnych plików. Opcja publikowania nie zezwala na takie szczegółową kontrolę; Zamiast tego publikuje *całego* aplikacji. To zachowanie ma swoje zalety i wady. Na tej stronie znaku plus wiesz, korzystając z opcji publikowania, użytkownik nie będzie podczas zapominania konta do przekazania ważnych plików. Jednak należy wziąć pod uwagę, co się stanie, jeśli niewielkie zmiany zostały wprowadzone do bardzo dużych witryny sieci Web — za pomocą opcji publikowania nie może zaktualizować tę stronę lub dwóch, który został zmodyfikowany, ale zamiast tego należy poczekać, program Visual Studio wdroży całej witryny.

Nie jest niczym niezwykłym tam być określone pliki, których zawartość różni się między środowiskiem produkcyjnym i środowisk deweloperskich. Przykład klawisza jest plik konfiguracji aplikacji, `Web.config`. Ponieważ opcja Publikuj bezrefleksyjne kopiuje pliki aplikacji sieci web zastępuje pliki konfiguracyjne dostosowanego środowiska produkcyjnego z wersją w środowisku programistycznym. Kolejnym samouczku przedstawiono, w tym temacie dalsze i oferuje wskazówki dotyczące wdrażania aplikacji sieci web, gdy istnieją różnice.

## <a name="summary"></a>Podsumowanie

Wdrażanie witryny sieci Web obejmuje kopiowanie wymaganych plików ze środowiska projektowego do środowiska produkcyjnego. Poprzednim samouczku pokazano, jak przesyłać pliki przy użyciu klienta FTP, takich jak FileZilla. W tym samouczku zbadane dwa narzędzia wdrażania w programie Visual Studio: narzędzia kopiowania witryny sieci Web i opcję Publikuj. Narzędzie kopiowania witryny sieci Web jest podobny do klienta FTP, że posiada dwa paned interfejsu, wyświetlanie listy plików na komputerze lokalnym i określonego komputera zdalnego, który można łatwo przekazywać i pobierać pliki między dwoma komputerami. Opcja publikowania jest bardziej tępy narzędzie, które jawnie kompiluje projekt, a następnie wdraża całej aplikacji do określonej lokalizacji docelowej.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Kopiowanie witryny sieci Web za pomocą narzędzia kopiowania witryny internetowej](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Jak należy I: wdrażać witryny sieci Web przy użyciu narzędzia kopiowania witryny internetowej](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (wideo)
- [Instrukcje: Publikowanie projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Instrukcje: Publikowanie witryn sieci Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Projekty instalacji i wdrażania w programie Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-your-site-using-an-ftp-client-vb.md)
> [dalej](common-configuration-differences-between-development-and-production-vb.md)
