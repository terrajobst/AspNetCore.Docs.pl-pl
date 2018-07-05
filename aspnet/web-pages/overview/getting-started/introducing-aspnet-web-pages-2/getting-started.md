---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Wprowadzenie do składnika ASP.NET Web Pages — wprowadzenie | Dokumentacja firmy Microsoft
author: tfitzmac
description: Program WebMatrix nie jest już zalecany jako zintegrowane środowisko projektowe dla stron ASP.NET Web Pages. Użyj programu Visual Studio lub Visual Studio Code. Niniejsze wskazówki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 3717fe230f818f251ac5d0378701c28f6b427df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382075"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Wprowadzenie do wzorca ASP.NET Web Pages — wprowadzenie
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > Program WebMatrix nie jest już zalecany jako zintegrowane środowisko projektowe dla stron ASP.NET Web Pages. Użyj [programu Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) lub [programu Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Tej wskazówki i aplikacji umożliwia przegląd programu ASP.NET Web Pages (w wersji 2 lub nowszego) i składni Razor, uproszczone framework do tworzenia dynamicznych witryn sieci Web. Przedstawia on również programu WebMatrix, narzędzie do tworzenia stron i witryn.
> 
> **Poziom**: nowe strony sieci Web ASP.NET.  
> **Umiejętności zakłada, że**: HTML, podstawowe kaskadowych arkuszy stylów (CSS).
> 
> Zawartość w pierwszym samouczku zestawu:
> 
> - Jakich technologii ASP.NET Web Pages jest i co to jest.
> - Co to jest program WebMatrix.
> - Jak zainstalować wszystkie elementy.
> - Jak utworzyć witrynę sieci Web za pomocą programu WebMatrix.
>   
> 
> Funkcje/technologie omówione:
> 
> - Instalator platformy sieci Web firmy Microsoft.
> - WebMatrix.
> - *.cshtml* stron
>   
> 
> Mike Pope pierwotnie napisane w tym samouczku. Tom FitzMacken aktualizacji dla programu Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Program WebMatrix 3


## <a name="what-should-you-know"></a>Co należy wiedzieć?

Zakłada się, że znasz:

- **HTML**. Nie doświadczenia szczegółowe jest wymagana. Nie możemy wyjaśnić HTML, ale nie są używane również jakiekolwiek złożone. Firma Microsoft udostępni linki do samouczków HTML, w którym uważamy, że są one przydatne.
- **Kaskadowych arkuszy stylów (CSS)**. Tak samo jak z kodem HTML.
- **Pomysły podstawowej bazy danych**. Jeśli już używać arkusza kalkulacyjnego w przypadku danych i sortowanie i filtrowanie danych, to jest poziom doświadczenia ogólnie zakłada się na tym zestawie samouczków.

Zakłada się również, czy interesuje Cię nauka programowania basic. Strony ASP.NET Web Pages Użyj język programowania C#. Nie masz żadnych tło w programowaniu, po prostu zainteresowanie go. Jeśli kiedykolwiek napisanych dowolnego języka JavaScript na stronie sieci web przed, masz wystarczająco dużo tła.

Należy pamiętać, że osoby zaznajomione z programowaniem może się okazać, że w tym samouczku ustawiany początkowo przemieszczany powoli korzystamy początkujących programistów się wszystkiego. Po pierwsze kilka samouczków, uzyskując jednak będą dostępne programowanie mniej basic, aby wyjaśnić, i czynności będzie przesunąć wzdłuż na szybsze klipu.

## <a name="what-do-you-need"></a>Co jest potrzebne?

Oto, co jest potrzebne:

- Komputer z systemem Windows 8, Windows 7, Windows Server 2008 lub Windows Server 2012.
- Połączenie internetowe na żywo.
- Uprawnienia administratora (wymaganych podczas procesu instalacji).

## <a name="what-is-aspnet-web-pages"></a>Co to jest wzorca ASP.NET Web Pages?

Strony sieci Web ASP.NET to platforma, która umożliwia tworzenie dynamicznych stron sieci web. Prostą stronę sieci web HTML jest statyczny; jego zawartość jest określana przez stały kod znaczników HTML, który znajduje się w stronę. Strony dynamiczne, takie jak te, które tworzysz przy użyciu stron ASP.NET Web Pages pozwalają tworzyć zawartość strony na bieżąco, za pomocą kodu.

Dynamiczne strony umożliwiają wykonywanie różnych interesujących rzeczy. Można poprosić użytkownika o danych wejściowych za pomocą formularza, a następnie zmień wyświetla stronę lub jego wygląd. Może zająć informacji od użytkownika, zapisz go w bazie danych i umieść je później. Możesz wysłać wiadomość e-mail z witryny. Możesz wchodzić w interakcje z innymi usługami w sieci web (na przykład usługa mapowania) i tworzenia stron, które integrują się informacji z tych źródeł.

## <a name="what-is-webmatrix"></a>Co to jest program WebMatrix?

Program WebMatrix to narzędzie, które integruje edytora stron sieci web, narzędzia bazy danych, serwer sieci web do testowania stron i funkcji do publikacji witryny sieci Web w Internecie. Program WebMatrix to bezpłatne i jest łatwa instalacja i łatwych w użyciu. (Działa tylko zwykły strony HTML, a także innych technologii, takich jak PHP.)

Nie zostanie faktycznie *mają* do pracy przy użyciu stron ASP.NET Web Pages przy użyciu programu WebMatrix. Można utworzyć stron przy użyciu tekstu edytora, na przykład i przetestować strony dostępne za pomocą którego można uzyskać dostęp do serwera sieci web. Jednak program WebMatrix ułatwia wszystkie bardzo, dlatego w tych samouczkach będzie przy użyciu programu WebMatrix w całym.

## <a name="about-these-tutorials"></a>Informacje te samouczki

Ten zestaw samouczek stanowi wprowadzenie do sposobu używania stron ASP.NET Web Pages. Są całkowite w tym zestawie samouczków wprowadzających samouczków 9. Tego częścią serii samouczków zestawy, od nowicjuszy strony sieci Web ASP.NET do tworzenia rzeczywistych, profesjonalnych witryn sieci Web.

Ten pierwszy samouczek nastavit koncentraty zawierający podstawowe informacje dotyczące sposobu pracy przy użyciu stron ASP.NET Web Pages. Gdy wszystko będzie gotowe, można pracować dodatkowe zestawy samouczek, które przejmą, gdzie kończy się w niej i eksplorowanie, stron sieci Web, aby bardziej szczegółowo.

Firma Microsoft celowo Przejdź łatwe szczegółowe wyjaśnienia. I w każdym przypadku, gdy coś pokazujemy, dla tego zestawu samouczków firma Microsoft zawsze wybierz sposób, który naszym zdaniem jest łatwiej zrozumieć. Nowsze zestawy samouczek bardziej szczegółowo i dowiesz się efektywniejsze i bardziej elastyczne podejście (również fajne). Jednak te samouczki trzeba najpierw poznać podstawy.

Zestawie samouczków, po prostu rozpoczętej obejmuje następujące funkcje dla stron sieci Web platformy ASP.NET:

- Wprowadzenie i rozpoczynanie wszystko, co jest zainstalowany. (Który jest w tym samouczku, który czytasz).
- Podstawy programowania w produkcie ASP.NET Web Pages.
- Tworzenie bazy danych.
- Tworzenia i przetwarzania formularz wprowadzania użytkownika.
- Dodawanie, aktualizowanie i usuwanie danych w bazie danych.

## <a name="what-will-you-create"></a>Co spowoduje utworzenie?

Ustaw w tym samouczku, a kolejne koncentrują się wokół witryny sieci Web, gdzie możesz wyświetlić listę filmów, które chcesz. Będzie można wprowadź filmy, edytować je, a ich listę. Oto kilka stron, które zostaną utworzone w tym zestawie samouczków. Pierwszy pokazuje film stronie, którą utworzysz listy:

![Aplikacji zakończył Film przedstawiający listę filmu](getting-started/_static/image1.png)

A Oto strona która umożliwia dodawanie nowych informacji film do swojej witryny:

![Aplikacja Zakończono Film przedstawiający stronę dodać film](getting-started/_static/image2.png)

Kolejne samouczek Zestawy kompilacji na tym zestawu i Dodaj więcej funkcji, takich jak przekazywania obrazów, umożliwiając osób, zaloguj się, wysyłania wiadomości e-mail i integracji z mediów społecznościowych.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz już konto, masz następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

## <a name="installing-everything"></a>Instalowanie wszystko

Wszystko, czego można zainstalować za pomocą Instalatora platformy sieci Web firmy Microsoft. W efekcie Zainstaluj Instalator, a następnie użyć go do zainstalowania całą resztą.

Używać stron sieci Web, musisz mieć co najmniej Windows XP z dodatkiem SP3 zainstalowane lub Windows Server 2008 lub nowszym.

Na [stron sieci Web stronę](../../../index.md) witryny sieci Web platformy ASP.NET, kliknij przycisk **zainstalować**.

![Wyświetlanie witryny sieci Web platformy ASP.NET &quot;Zainstaluj program WebMatrix&quot; przycisku](getting-started/_static/image3.png)

Zostanie wyświetlony monit o zaakceptowanie postanowień licencyjnych i zasady zachowania poufności informacji przed zainstalowaniem programu WebMatrix.

![Zaakceptuj termin, aby rozpocząć instalację](getting-started/_static/image4.png)

Kliknij przycisk **Uruchom** aby rozpocząć instalację. (Jeśli chcesz zapisać Instalator, kliknij przycisk **Zapisz** , a następnie uruchom Instalatora z folderu, w którym został zapisany.)

![](getting-started/_static/image5.png)

Zostanie wyświetlony Instalator platformy sieci Web, gotowe do zainstalowania programu WebMatrix. Kliknij przycisk **zainstalować**.

![](getting-started/_static/image6.png)

Proces instalacji wpadł na pomysł posiada do zainstalowania na komputerze i rozpoczyna proces. W zależności od tego, co dokładnie musi być zainstalowany proces może potrwać od kilku minut do kilku minut. Wybierz **akceptuję** do akceptowania postanowień licencyjnych.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Gdy wszystko będzie gotowe, proces instalacji można uruchomić program WebMatrix automatycznie. W przeciwnym razie w Windows, od **Start** menu, uruchamiania **Microsoft WebMatrix**.

Po uruchomieniu programu WebMatrix po raz pierwszy podano Państwo zalogować się do Microsoft Azure z Twoim kontem Microsoft. Logując się, zostanie wyświetlony 10 aplikacji internetowych bezpłatne za pośrednictwem platformy Azure. Aplikacje sieci web bezpłatna zapewnia wygodny sposób do testowania aplikacji. Jeśli nie masz jeszcze konta platformy Azure, ale masz subskrypcję MSDN, możesz to zrobić [Aktywuj korzyści z subskrypcji MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). W przeciwnym razie można utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nie masz Zaloguj się w tej chwili kontynuować z tego samouczka. Jeśli nie zarejestrujesz teraz, nadal masz opcję, aby zalogować się później. Ostatni [tematu](publishing.md) w tym samouczku omówiono sposób wdrażania witryny sieci Web na platformie Azure; w związku z tym, należy zalogować się do wykonania zadań opisanych w tym temacie.

Na tym etapie Zaloguj się przy użyciu konta Microsoft lub wybierz **nie teraz** w prawym dolnym rogu.

![Rejestrowanie](getting-started/_static/image7.png)

Aby rozpocząć, możesz utworzyć pustą witrynę sieci Web i Dodawanie strony. Później w samouczku w tym zestawie odtwarzane od jednego z szablonów wbudowanych witryny sieci Web.

W oknie start kliknij **New**.

![Ekran startowy programu WebMatrix](getting-started/_static/image8.png)

Szablony to wstępnie utworzone pliki i strony dla różnych typów witryn sieci Web. Aby wyświetlić wszystkie szablony, które są domyślnie dostępne, wybierz opcję galerii szablonów.

![Wybierz galerię szablonów](getting-started/_static/image9.png)

W **— Szybki Start** wybierz **pusta witryna** z **ASP.NET** grupy i nazwij nową witrynę "WebPagesMovies".

![Program WebMatrix — Szybki Start okna z wybraniu szablonu Pusta witryna](getting-started/_static/image10.png)

Kliknij przycisk **Dalej**.

Jeśli zalogowaniu się do swojego konta Microsoft, użytkownik otrzyma możliwość utworzenia witryny na platformie Azure. Na podstawie nazwy witryny, domyślna nazwa **WebPagesMovies.azurewebsites.net** proponowaną; jednak wykrzyknik wskazuje, że ta nazwa nie jest dostępna na platformie Windows Azure. Dla uproszczenia wybierz **Pomiń** Pomijanie teraz tworzenie witryny sieci web na platformie Azure. W dalszej części tej serii opublikujemy odpowiednie witryny na platformie Azure.

![Tworzenie witryny systemu azure](getting-started/_static/image11.png)

Program WebMatrix tworzy i otwiera witrynę:

![Nowa witryna WebPagesMovies otwartych w programie WebMatrix](getting-started/_static/image12.png)

U góry strony istnieje paska narzędzi szybkiego dostępu i Wstążki. W lewym dolnym rogu, zostanie wyświetlony selektor obszarów gdy przełączasz się między zadaniami (**witryny**, **pliki**, **baz danych**, **raporty**). Po prawej stronie jest okienko zawartość edytora i raportów. I w dół od czasu do czasu pojawi się na pasku powiadomień dla wiadomości.

Dowiesz się więcej na temat programu WebMatrix i jej funkcji podczas wykonywania kroków w tych samouczkach.

## <a name="creating-a-web-page"></a>Tworzenie strony sieci Web

Aby zapoznać się z programu WebMatrix i stron ASP.NET Web Pages, należy utworzyć prostą stronę.

W selektorze obszaru roboczego wybierz **pliki** obszaru roboczego. Ten obszar roboczy pozwala pracować z plikami i folderami. W okienku po lewej stronie pokazuje strukturę pliku witryny. Zmiany wstążki, aby wyświetlić zadania związane z pliku.

![Plik obszaru roboczego w programie WebMatrix](getting-started/_static/image13.png)

Na wstążce kliknij strzałkę w obszarze **New** a następnie kliknij przycisk **nowy plik**.

![Za pomocą &quot;New&quot; polecenia na Wstążce, aby utworzyć nowy plik](getting-started/_static/image14.png)

Program WebMatrix Wyświetla listę typów plików. Wybierz **CSHTML**, a następnie w **nazwa** wpisz "nazwę HelloWorld". Strona CSHTML jest stron ASP.NET Web Pages.

![Tworzenie nowej strony CSHTML o nazwie HelloWorld.cshtml](getting-started/_static/image15.png)

Kliknij przycisk **OK**.

Program WebMatrix tworzy stronę i otwiera go w edytorze.

![Nowa strona HelloWorld w edytorze programu WebMatrix](getting-started/_static/image16.png)

Jak widać, ta strona zawiera głównie zwykły kod znaczników HTML, z wyjątkiem bloku u góry strony, który wygląda tak:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

To dodawania kodu, ponieważ pojawi się wkrótce.

Należy zauważyć, że różne części strony &mdash; nazwy elementów, atrybutów i tekstu, a także bloku u góry — są w różnych kolorach. Jest to nazywane *wyróżnianie składni*, i ułatwia zapewnienie Wyczyść wszystko. Jest to jeden z funkcji, które można łatwo pracować ze stronami sieci web w programie WebMatrix.

Dodaj zawartość `<head>` i `<body>` elementów, takich jak w poniższym przykładzie. (Jeśli chcesz, można po prostu skopiuj następujący blok i Zastąp cały istniejącej strony przy użyciu tego kodu.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Do paska narzędzi szybkiego dostępu lub w **pliku** menu, kliknij przycisk **Zapisz**.

![Przycisk Zapisz w narzędzi Szybki dostęp programu WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testowanie strony

W **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy *HelloWorld.cshtml* strony, a następnie kliknij przycisk **Uruchom w przeglądarce**.

![Uruchomienie strony, przy użyciu przycisku Uruchom na wstążce programu WebMatrix](getting-started/_static/image18.png)

Program WebMatrix uruchamia wbudowanego serwera sieci web (IIS Express) używanej do testowania stron na komputerze. (Bez usług IIS Express w programie WebMatrix, trzeba się gdzieś opublikować stronę serwera sieci web, zanim można go przetestować.) Ta strona jest wyświetlana w domyślnej przeglądarce.

![&quot;Witaj, świecie&quot; strony uruchomionej w przeglądarce](getting-started/_static/image19.png)

Zwróć uwagę, czy podczas testowania strony w programie WebMatrix, adres URL w przeglądarce jest podobny do `http://localhost:33651/HelloWorld.cshtml.` nazwę *localhost* odwołuje się do serwera lokalnego, co oznacza, że strona jest obsługiwana przez serwer sieci web, która znajduje się na swoim komputerze. Jak wspomniano, program WebMatrix zawiera program serwera sieci web o nazwie usług IIS Express, który jest wykonywany po uruchomieniu strony.

Liczba po *localhost* (na przykład *localhost:33651*) odwołuje się do *numer portu* na tym komputerze. Jest to liczba "kanał" używającej usług IIS Express dla tej konkretnej witryny sieci Web. Numer portu jest wybranych losowo z zakresu od 1024 do 65536 podczas tworzenia witryny i różni się dla każdej lokacji, którą tworzysz. (Podczas testowania do własnej witryny numer portu prawie na pewno będzie inny numer niż 33561). Przy użyciu innego portu dla każdej witryny sieci Web, usług IIS Express można utrzymania który mówi się do witryn.

Nowszym w przypadku publikowania witryny na serwerze sieci web publiczne, nie są już wyświetlane *localhost* w adresie URL. W tym momencie zostanie wyświetlony bardziej typowy adres URL, takich jak `http://myhostingsite/mywebsite/HelloWorld.cshtml` lub dowolną stronę. Dowiesz się więcej o publikowaniu lokację w dalszej części tej serii samouczków.

## <a name="adding-some-server-side-code"></a>Dodawanie kodu po stronie serwera

Zamknij przeglądarkę i przejdź wstecz do strony w programie WebMatrix.

Dodaj linię do bloku kodu, tak że wygląda podobnie do poniższego kodu:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Jest to trochę kodu Razor. To prawdopodobnie oczywiste, pobiera bieżącą datę i godzinę i umieszcza tę wartość do *zmiennej* o nazwie `currentDateTime`. Użytkownik będzie Dowiedz się więcej o składni Razor w następnym samouczku.

W treści strony po `<p>Hello World!</p>` elementu, Dodaj następujący kod:

[!code-html[Main](getting-started/samples/sample4.html)]

Ten kod pobiera wartość, która umieszczonego w `currentDateTime` zmiennej u góry i wstawia je do znaczników strony. `@` Znak oznacza kodu strony sieci Web ASP.NET na stronie.

Uruchom ponownie strony (program WebMatrix zapisuje zmiany za Ciebie przed uruchomieniem strony). Tym razem zostanie wyświetlony, datę i godzinę na stronie.

![&quot;Witaj, świecie&quot; uruchomiony w przeglądarce, za pomocą wyświetlacza dynamicznie generowanym czasu strony](getting-started/_static/image20.png)

Poczekaj chwilę, a następnie odśwież stronę w przeglądarce. Wyświetlanie daty i godziny jest aktualizowana.

W przeglądarce Przyjrzyj się źródła strony. Wygląda następujące znaczniki:

[!code-html[Main](getting-started/samples/sample5.html)]

Należy zauważyć, że `@{ }` u góry nie będzie dostępne. Należy również zauważyć, że data i godzina pokazuje rzeczywistego ciągu znaków (`1/18/2012 2:49:50 PM` lub niezależnie od), a nie `@currentDateTime` tak jak gdyby *.cshtml* strony. Co się stało, w tym miejscu jest, że po uruchomieniu strony, ASP.NET przetwarzane cały kod (bardzo małe w tym przypadku) oznaczoną jako zaniechana `@`. Kod generuje dane wyjściowe, a te dane wyjściowe został wstawiony do strony.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Jest to, o ile wzorca ASP.NET Web Pages

Podczas odczytywania się, że stron ASP.NET Web Pages tworzy dynamicznej zawartości internetowej, co przedstawiono w tym miejscu jest pomysł. Strony, do której właśnie utworzony zawiera ten sam znacznik HTML, który już wcześniej odwrócony. Może również zawierać kod, który można wykonywać różnorodne zadania. W tym przykładzie jak prostym zadaniem uzyskania bieżącej daty i godziny. Jak możesz rozdzielać kodu HTML do generowania danych wyjściowych strony. Gdy ktoś żąda *.cshtml* strony w przeglądarce, platformy ASP.NET przetwarza strony jest nadal w ręce serwer sieci web. ASP.NET wstawia dane wyjściowe dla kodu (jeśli istnieje) do strony jako kod HTML. Po zakończeniu przetwarzania kodu ASP.NET wysyła wynikowy strony do przeglądarki. To wszystko, czego nigdy nie pobiera przeglądarki HTML. Poniżej przedstawiono diagram:

![Jak ASP.NET generuje kod HTML dynamicznie koncepcyjny przepływu](getting-started/_static/image21.png)

Koncepcja jest prosta, ale istnieje wiele interesujących zadań, które mogą wykonywać kod, a istnieje wiele różnych sposobów, w których można dynamicznie dodać zawartość HTML do strony. I ASP.NET *.cshtml* stron, podobnie jak dowolnej strony HTML może również zawierać kod, który działa w przeglądarce, sama (kod JavaScript i jQuery w aplikacji). Dowiesz się, wszystkie te czynności w tym zestawie samouczków i kolejne.

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku z tej serii możesz eksplorować, ASP.NET Web Pages programowania nieco bardziej.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Utwórz witrynę sieci Web platformy ASP.NET od podstaw](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Jest to samouczek, który jest specjalnie o przy użyciu programu WebMatrix (nie stron ASP.NET Web Pages). Usługa zostanie wprowadzona do nieco więcej szczegółów na temat niektóre dodatkowe funkcje programu WebMatrix, które nie zostaną przedstawione w tym zestawie samouczków.

> [!div class="step-by-step"]
> [Next](intro-to-web-pages-programming.md)
