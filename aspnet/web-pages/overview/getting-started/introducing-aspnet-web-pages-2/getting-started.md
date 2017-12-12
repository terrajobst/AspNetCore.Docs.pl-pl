---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "Wprowadzenie do składnika ASP.NET Web Pages — wprowadzenie | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Program WebMatrix jest już zalecane jako zintegrowane środowisko programistyczne dla stron sieci Web programu ASP.NET. Za pomocą programu Visual Studio lub Visual Studio Code. W tych wskazówkach..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 615ddc31d0d857e5bf9a7f372b7efcf67d185905
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Wprowadzenie do strony sieci Web ASP.NET — wprowadzenie
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > Program WebMatrix jest już zalecane jako zintegrowane środowisko programistyczne dla stron sieci Web programu ASP.NET. Użyj [programu Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) lub [kodu programu Visual Studio](https://code.visualstudio.com/).
> 
> 
> Tej wskazówki i aplikacji zapewnia przegląd z stron sieci Web platformy ASP.NET (wersja 2 lub nowszego) oraz składni Razor, lekkie framework do tworzenia dynamicznych witryn sieci Web. Podaj programu WebMatrix, narzędzie do tworzenia stron i witryn.
> 
> **Poziom**: nowe do stron sieci Web programu ASP.NET.  
> **Umiejętności zakłada, że**: HTML, podstawowe kaskadowych arkuszy stylów (CSS).
> 
> Zawartość w pierwszym samouczku zestawu:
> 
> - Jakie technologii ASP.NET Web Pages jest i co to jest.
> - Co to jest program WebMatrix.
> - Jak zainstalować wszystkie elementy.
> - Jak utworzyć witrynę sieci Web za pomocą programu WebMatrix.
>   
> 
> Funkcje/technologie omówione:
> 
> - Instalator platformy sieci Web firmy Microsoft.
> - Program WebMatrix.
> - *.cshtml* stron
>   
> 
> Jan Pope pierwotnie został napisany w tym samouczku. Tomasz FitzMacken aktualizacji dla programu Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Program WebMatrix 3


## <a name="what-should-you-know"></a>Co należy wiedzieć?

Zakłada się, że znasz:

- **HTML**. Nie szczegółowych doświadczenia jest wymagana. Firma Microsoft nie będzie wyjaśnić HTML, ale również nie używamy niczego złożonych. Firma Microsoft udostępni łącza do samouczków HTML, w którym mamy nadzieję, że są one przydatne.
- **Kaskadowych arkuszy stylów (CSS)**. Taka sama jak HTML.
- **Koncepcje podstawowe bazy danych**. Jeśli już używane dla danych arkusza kalkulacyjnego i posortowane i przefiltrowane dane, które jest poziom wiedzy zakłada się, ogólnie dla tego samouczka zestawu.

Zakłada się również, że jesteś w podstawy programowania. Strony sieci Web ASP.NET, użyj język programowania C#. Nie masz żadnych tło w programowaniu, po prostu zainteresowanie go. Jeśli kiedykolwiek napisanych żadnych JavaScript na stronie sieci web przed, masz dużo tła.

Należy pamiętać, że jeśli znasz programowania, może się okazać, że w tym samouczku początkowo ustawiona przemieszczany powoli możemy Przełącz nowy programistów szybko. Jak możemy pokonać pierwszy samouczki kilka, będą mniej podstawowe programowania wyjaśnić i rzeczy przeniesie na szybsze klip.

## <a name="what-do-you-need"></a>Czego potrzebujesz?

Oto, co jest potrzebne:

- Komputer z systemem Windows 8, Windows 7, Windows Server 2008 lub Windows Server 2012.
- Połączenie internetowe na żywo.
- Uprawnienia administratora (wymagane dla procesu instalacji).

## <a name="what-is-aspnet-web-pages"></a>Co to jest strony sieci Web ASP.NET?

Strony sieci Web ASP.NET to platforma, która umożliwia tworzenie dynamicznych stron sieci web. Proste strony sieci web HTML jest statyczny; jego zawartość jest określana przez stały znacznik HTML strony. Strony dynamiczne jak utworzony z strony sieci Web ASP.NET umożliwiają tworzenie zawartości strony na bieżąco, przy użyciu kodu.

Strony dynamiczne pozwalają na wykonywanie szerokiej gamy rzeczy. Można uzyskać danych wejściowych użytkownika za pomocą formularza, a następnie zmień wyświetla stronę lub jak wygląda. Można podjąć informacje od użytkownika, zapisz go w bazie danych i wyświetlić ją później. Możesz wysłać wiadomość e-mail z witryny. Mogą współpracować z innymi usługami w sieci web (na przykład usługa mapowania) i utworzyć stron, które integrują informacje z tych źródeł.

## <a name="what-is-webmatrix"></a>Co to jest program WebMatrix?

Program WebMatrix to narzędzie, które integruje edytora stron sieci web, narzędzia bazy danych, serwer sieci web do testowania stron i funkcji do publikacji witryny sieci Web w Internecie. Program WebMatrix to bezpłatne, i jest łatwy do zainstalowania i łatwy w użyciu. (Działa tylko zwykły stron HTML, a także innych technologii, takich jak PHP.)

Nie zostanie faktycznie *ma* Aby pracować ze strony sieci Web ASP.NET za pomocą programu WebMatrix. Można utworzyć strony przy użyciu tekstu edytor, na przykład i stron w teście za pomocą którego można uzyskać dostęp do serwera sieci web. Jednak program WebMatrix ułatwia wszystkie bardzo, więc te samouczki będzie używać programu WebMatrix w całej.

## <a name="about-these-tutorials"></a>Te samouczki — informacje

Ten samouczek zestaw jest wprowadzenie do jak używać stron sieci Web ASP.NET. Całkowita liczba w tym zestawie Samouczek wprowadzający są samouczki 9. Tego częścią serii samouczek zestawów życia, od nowicjuszy stron ASP.NET Web Pages do tworzenia rzeczywistych, profesjonalnych witryn sieci Web.

W tym pierwszym samouczku ustawiać koncentraty pokazujące, podstawowe informacje o pracy z ASP.NET Web Pages. Gdy wszystko będzie gotowe, możesz pracować z zestawami samouczek dodatkowe, które wybierze, którym ta kończy się i który eksplorować stron sieci Web bardziej szczegółowo.

Firma Microsoft celowo Przejdź łatwe szczegółowych wyjaśnień. I zawsze, gdy zostanie przedstawiony coś, dla tego samouczka zestawu możemy zawsze wybierz sposób, w jaki naszym zdaniem jest łatwiej zrozumieć. Nowsze zestawy samouczek bardziej szczegółowo i wyświetlić efektywniejsze lub bardziej elastyczne podejścia (również fun więcej). Jednak te samouczki trzeba najpierw poznać podstawy.

Samouczek zestawu, po prostu uruchomienia obejmuje funkcje te strony sieci Web ASP.NET:

- Wprowadzenie i pobiera wszystkie zainstalowane. (To w instrukcji, które czytanej zawartości).
- Podstawy programowania w języku ASP.NET Web Pages.
- Tworzenie bazy danych.
- Tworzenia i przetwarzania formularza danych wejściowych użytkownika.
- Dodawanie, aktualizowanie i usuwanie danych w bazie danych.

## <a name="what-will-you-create"></a>Co spowoduje utworzenie?

Ustaw w tym samouczku, a kolejne obracać wokół witryny sieci Web, w którym można wyświetlić listę filmów, które chcesz. Można wprowadzić filmy je edytować i ich. Oto kilka stron, które zostaną utworzone w tym zestawie samouczka. Pierwsza z nich zawiera filmu wyświetlania strony, które zostaną utworzone:

![Aplikacji Finshed Film przedstawiający listę film](getting-started/_static/image1.png)

A Oto strony, które umożliwia dodanie nowe informacje do swojej witryny:

![Aplikacji Zakończono Film przedstawiający stronę dodać film](getting-started/_static/image2.png)

Kolejne samouczek Zestawy kompilacji na tym ustawić i dodać więcej funkcji, takich jak przekazywanie obrazów, umożliwiając osób, zaloguj się za wysyłanie poczty e-mail i integrowanie z mediami społecznościowymi.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure. Jeśli nie masz już konto, dostępne są następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

## <a name="installing-everything"></a>Instalowanie wszystko

Wszystko, co można zainstalować za pomocą Instalatora platformy sieci Web firmy Microsoft. W efekcie Zainstaluj Instalator, a następnie użyć, aby zainstalować wszystkie inne elementy.

Używać stron sieci Web, musisz mieć co najmniej Windows XP z dodatkiem SP3 zainstalowane lub Windows Server 2008 lub nowszym.

Na [strony dla stron sieci Web](../../../index.md) witryny sieci Web ASP.NET, kliknij przycisk **zainstalować**.

![Wyświetlanie witryny sieci Web ASP.NET &quot;Zainstaluj program WebMatrix&quot; przycisku](getting-started/_static/image3.png)

Należy zaakceptować postanowienia licencyjne i zasady zachowania poufności informacji przed zainstalowaniem programu WebMatrix.

![Zaakceptuj termin do rozpoczęcia instalacji](getting-started/_static/image4.png)

Kliknij przycisk **Uruchom** do rozpoczęcia instalacji. (Jeśli chcesz zapisać Instalatora, kliknij przycisk **zapisać** , a następnie uruchom Instalatora z folderu, w którym został zapisany.)

![](getting-started/_static/image5.png)

Instalator platformy sieci Web zostanie wyświetlona, gotowe do zainstalowania programu WebMatrix. Kliknij przycisk **zainstalować**.

![](getting-started/_static/image6.png)

Proces instalacji określa posiada do zainstalowania na komputerze i rozpoczyna proces. W zależności od tego, co dokładnie musi być zainstalowany proces może potrwać od kilku minut do kilku minut. Wybierz **akceptuję** do akceptowania postanowień licencyjnych.

## <a name="hello-webmatrix"></a>Witaj, programu WebMatrix

Po zakończeniu procesu instalacji można uruchomić program WebMatrix automatycznie. W przeciwnym razie w systemie Windows, z **Start** menu, uruchom **Microsoft WebMatrix**.

Po uruchomieniu programu WebMatrix po raz pierwszy podana jest stosowany do logowania do systemu Microsoft Azure z Twoim kontem Microsoft. Logując się, zostanie wyświetlony 10 aplikacje wolnej sieci web za pośrednictwem platformy Azure. Aplikacje sieci web wolnej umożliwiają wygodny do testowania aplikacji. Jeśli nie masz jeszcze konta platformy Azure, ale masz subskrypcję MSDN, możesz [aktywować korzyści dla subskrypcji MSDN](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). W przeciwnym razie możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nie masz teraz zalogować się do kontynuowania tego samouczka. Jeśli nie zalogujesz teraz, konieczne będzie nadal opcję, aby zalogować się później. Ostatni [tematu](publishing.md) w tym samouczku serii opisano sposób wdrażania witryny sieci Web na platformie Azure; w związku z tym musisz zalogować się do ukończenia tego tematu.

W tym momencie albo Zaloguj się przy użyciu konta Microsoft lub wybierz **nie teraz** w prawym dolnym rogu.

![Rejestrowanie](getting-started/_static/image7.png)

Aby rozpocząć, możesz utworzyć pusty witrynę sieci Web i Dodaj stronę. W późniejszym instrukcji w tym zestawie odtwarzane z jednego z szablonów wbudowane witrynę sieci Web.

W oknie start kliknij **nowy**.

![Ekran startowy programu WebMatrix](getting-started/_static/image8.png)

Szablony są wbudowane pliki i strony dla różnych typów witryn sieci Web. Aby wyświetlić wszystkie szablony, które są dostępne jako domyślne, wybierz opcję galerii szablonów.

![Wybierz szablon galerii](getting-started/_static/image9.png)

W **Szybki Start** wybierz **pusta witryna** z **ASP.NET** grupy i nadaj nazwę nowej witryny "WebPagesMovies".

![Program WebMatrix Szybki Start okna z wybrany szablon pustej witryny](getting-started/_static/image10.png)

Kliknij przycisk **Dalej**.

Jeśli zalogowaniu się do swojego konta Microsoft, użytkownik otrzyma możliwość utworzenia witryny na platformie Azure. Na podstawie nazwy witryny domyślnej nazwy **WebPagesMovies.azurewebsites.net** sugerowanego; jednak wykrzyknik wskazuje, że ta nazwa nie jest dostępna w systemie Windows Azure. Dla uproszczenia, wybierz **Pomiń** Aby pominąć tworzenie witryny sieci web na platformie Azure w tej chwili. W dalszej części tej serii firma Microsoft opublikuje witryny na platformie Azure.

![Utwórz witrynę azure](getting-started/_static/image11.png)

Program WebMatrix tworzy i otwiera witrynę:

![Otwórz nową witrynę WebPagesMovies w programie WebMatrix](getting-started/_static/image12.png)

U góry jest pasek narzędzi Szybki dostęp i Wstążki. W lewym dolnym rogu, zostanie wyświetlony selektor obszaru roboczego gdzie przełączania między zadaniami (**lokacji**, **pliki**, **baz danych**, **raporty**). Po prawej stronie jest okienko zawartość edytora i raportów. I u dołu czasami pojawi się na pasku powiadomień dla wiadomości.

Więcej informacji na temat programu WebMatrix i jego funkcje podczas wykonywania kroków samouczka, dowiesz się.

## <a name="creating-a-web-page"></a>Tworzenie strony sieci Web

Aby zapoznać się z programem WebMatrix i stron ASP.NET Web Pages, należy utworzyć prostą stronę.

W selektorze obszaru roboczego wybierz **pliki** obszaru roboczego. Ten obszar roboczy umożliwia pracę z plikami i folderami. W okienku po lewej stronie zostaną wyświetlone struktury plików witryny sieci. Zmiany wstążki Pokaż zadania związane z pliku.

![Obszar roboczy plików w programie WebMatrix](getting-started/_static/image13.png)

Na wstążce kliknij strzałkę w obszarze **nowy** , a następnie kliknij przycisk **nowy plik**.

![Przy użyciu &quot;nowy&quot; poleceń na Wstążce, aby utworzyć nowy plik](getting-started/_static/image14.png)

Program WebMatrix Wyświetla listę typów plików. Wybierz **CSHTML**, a następnie w **nazwa** wpisz "HelloWorld". Strona CSHTML jest stron ASP.NET Web Pages.

![Tworzenie nowej strony CSHTML o nazwie HelloWorld.cshtml](getting-started/_static/image15.png)

Kliknij przycisk **OK**.

Program WebMatrix tworzy stronę i otwarcie go w edytorze.

![Nowa strona HelloWorld w edytorze programu WebMatrix](getting-started/_static/image16.png)

Jak widać, strona zawiera głównie zwykłej kod znaczników HTML, z wyjątkiem blok u góry, która wygląda następująco:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

To jest do dodawania kodu, jak można zauważyć wkrótce.

Zwróć uwagę, że różne części strony &mdash; nazwy elementów, atrybutów i tekstu, a także u góry bloku — są w różnych kolorach. Ta metoda jest wywoływana *wyróżnianie składni*, oraz jej ułatwia zapewnienie Wyczyść wszystko. Jest jednym z funkcji, które można łatwo pracować ze stronami sieci web w programie WebMatrix.

Dodaj zawartość `<head>` i `<body>` elementów, takich jak w poniższym przykładzie. (Jeśli chcesz, można po prostu skopiuj następujący blok i Zastąp całą stronę istniejących tego kodu.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Do paska narzędzi Szybki dostęp lub w **pliku** menu, kliknij przycisk **zapisać**.

![Zapisz przycisku w narzędzi Szybki dostęp do programu WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testowanie strony

W **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy *HelloWorld.cshtml* a następnie kliknij przycisk **Uruchom w przeglądarce**.

![Uruchomienie strony, za pomocą przycisku Uruchom na wstążce programu WebMatrix](getting-started/_static/image18.png)

Program WebMatrix uruchamia wbudowanego serwera sieci web (IIS Express) używanej do testowania stron na komputerze. (Bez usług IIS Express w programie WebMatrix, czy należy opublikować stronę na serwerze sieci web gdzieś przed można przetestować go.) Ta strona jest wyświetlana w domyślnej przeglądarce.

![&quot;Witaj świecie&quot; strony uruchomionej w przeglądarce](getting-started/_static/image19.png)

Zauważ, że podczas testowania strony w programie WebMatrix, adres URL w przeglądarce jest podobny do `http://localhost:33651/HelloWorld.cshtml.` nazwa *localhost* odwołuje się do serwera lokalnego, co oznacza, że strona jest obsługiwana przez serwer sieci web, który znajduje się na komputerze. Jak wspomniano, program WebMatrix zawiera program serwera sieci web o nazwie usług IIS Express, która jest uruchamiana podczas uruchamiania strony.

Liczba po *localhost* (na przykład *localhost:33651*) odwołuje się do *numer portu* na tym komputerze. Jest to liczba "kanał" używającej usług IIS Express dla określonej witryny sieci Web. Numer portu jest zaznaczone losowo z zakresu od 1024 do 65536 podczas tworzenia witryny i różni się dla każdej witryny. (Podczas testowania własnej witryny, numer portu prawie na pewno będzie inny numer niż 33561). Przy użyciu innego portu dla każdej witryny sieci Web, usługi IIS Express można zachować proste którym rozmawia się do witryn.

Nowsze podczas publikowania witryny na serwerze sieci web publiczny nie są już wyświetlane *localhost* w adresie URL. W tym momencie zostanie wyświetlony bardziej typowego adresu URL, takie jak `http://myhostingsite/mywebsite/HelloWorld.cshtml` lub dowolnej strony. Dowiesz się więcej o publikowaniu witryny w dalszej części tego samouczka serii.

## <a name="adding-some-server-side-code"></a>Dodawanie kodu po stronie serwera

Zamknij przeglądarkę i wróć do strony w programie WebMatrix.

Dodaj linię do bloku kodu, aby wyglądały one podobnie do następującego kodu:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Jest to niewielki kodu Razor. Jest prawdopodobnie jasne, pobiera bieżącą datę i godzinę i umieszcza tę wartość do *zmiennej* o nazwie `currentDateTime`. Użytkownik będzie Dowiedz się więcej o składni Razor w następnym samouczku.

W treści strony po `<p>Hello World!</p>` elementu, Dodaj następujący kod:

[!code-html[Main](getting-started/samples/sample4.html)]

Ten kod pobiera wartość, która je `currentDateTime` zmiennej u góry i wstawia go w znaczniku strony. `@` Znak oznacza kod ASP.NET Web Pages na stronie.

Uruchom ponownie strony (program WebMatrix umożliwia zapisanie zmian dla Ciebie przed uruchomieniem strony). Teraz zobaczyć datę i godzinę na stronie.

![&quot;Witaj świecie&quot; strony uruchomionej w przeglądarce z dynamicznie generowanym godzinę](getting-started/_static/image20.png)

Poczekaj chwilę, a następnie odśwież stronę w przeglądarce. Wyświetlanie daty i godziny jest aktualizowana.

Obejrzyj źródło strony w przeglądarce. Wygląda następujący kod:

[!code-html[Main](getting-started/samples/sample5.html)]

Zwróć uwagę, że `@{ }` bloku u góry nie istnieje. Także zauważyć, że data i godzina pokazuje rzeczywisty ciąg znaków (`1/18/2012 2:49:50 PM` lub niezależnie od), a nie `@currentDateTime` tak, jak gdyby *.cshtml* strony. Co się stało, Oto po uruchomieniu strony ASP.NET przetworzenia przez cały kod (bardzo podobne w tym przypadku), która została oznaczona atrybutem `@`. Kod generuje dane wyjściowe i wstawiono te dane wyjściowe do strony.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Jest to stron ASP.NET Web Pages są — informacje

Podczas odczytu, czy strony sieci Web ASP.NET tworzy dynamicznej zawartości sieci web, co przedstawiono w tym miejscu jest. Strona, którą właśnie utworzony zawiera samej znacznik HTML, który już wcześniej odwrócony. Może również zawierać kod, który można wykonywać różne rodzaje zadań. W tym przykładzie jak zadanie trivial uzyskiwanie aktualnej daty i godziny. Jak widać, należy intersperse kodu HTML do generowania danych wyjściowych strony. Gdy ktoś żąda *.cshtml* strony w przeglądarce, platformy ASP.NET przetwarza stronę go w trakcie ręce serwera sieci web. ASP.NET wstawia dane wyjściowe kodu (jeśli istnieje) do strony HTML. Po zakończeniu przetwarzania kodu ASP.NET wysyła wynikowej strony w przeglądarce. Wszystkie przeglądarki kiedykolwiek pobiera jest HTML. Poniżej przedstawiono diagram:

![Jak ASP.NET generuje kod HTML dynamicznie koncepcyjny przepływu](getting-started/_static/image21.png)

Koncepcja jest prosta, ale jest wiele interesujące zadań, które mogą wykonywać kod, a istnieje wiele różnych sposobów, w których można dynamicznie dodać zawartość HTML do strony. I ASP.NET *.cshtml* stron, podobnie jak wszystkie strony HTML, mogą również obejmować kodu uruchamianego w przeglądarce (kod JavaScript i jQuery). Będzie Eksploruj wszystkie czynności, w tym zestawie samouczka i kolejne.

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku tej serii eksplorowania stron ASP.NET Web Pages nieco programowania.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Tworzenie witryny sieci Web platformy ASP.NET od początku](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Jest to samouczek, który jest specjalnie o za pomocą programu WebMatrix (nie stron sieci Web programu ASP.NET). Umieszczanej w kodzie nieco więcej szczegółów na temat niektóre dodatkowe funkcje programu WebMatrix, który firma Microsoft nie będzie obejmować w tym zestawie samouczka.

>[!div class="step-by-step"]
[Dalej](intro-to-web-pages-programming.md)
