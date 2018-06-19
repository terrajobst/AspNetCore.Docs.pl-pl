---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Tworzenie projektu | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 7cfceb38204b6cfd3589a082761273e54ac122ca
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740455"
---
<a name="create-the-project"></a>Tworzenie projektu
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


W tym samouczku utworzysz, przejrzyj i uruchom domyślny projekt w programie Visual Studio, który umożliwia zapoznanie się z funkcjami programu ASP.NET. Ponadto należy przejrzeć środowiska Visual Studio.

## <a name="what-youll-learn"></a>Zawartość:

- Jak utworzyć nowy projekt formularzy sieci Web.
- Struktura pliku projektu formularzy sieci Web.
- Jak uruchomić projekt w programie Visual Studio.
- Różne funkcje domyślnej aplikacji formularzy sieci Web.
- Niektóre podstawowe informacje o sposobie używania środowiska Visual Studio.

## <a name="creating-the-project"></a>Tworzenie projektu

1. Otwórz program Visual Studio.
2. Wybierz **nowy projekt** z **pliku** menu w programie Visual Studio. 

    ![Tworzenie projektu - nowy element Menu projektu](create-the-project/_static/image1.png)
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.  
 Ten samouczek serii używanej platformy .NET Framework 4.5.2.
5. Nazwij swój projekt *WingtipToys* i wybierz polecenie **OK** przycisku. 

    ![Tworzenie projektu — okno dialogowe nowego projektu](create-the-project/_static/image2.png)

    > [!NOTE]
    > Nazwa projektu w tym samouczku jest **WingtipToys**. Zaleca się korzystanie z tej *dokładnie* Nazwa projektu, dzięki czemu kod podane w serii samouczek funkcji zgodnie z oczekiwaniami.

6. Kliknij przycisk **Zmień uwierzytelnianie** przycisku. Wybierz **indywidualnych kont użytkowników** i kliknij przycisk **OK** przycisku.

7. Wybierz **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisku.

    ![Utwórz projekt — nowy szablon projektu](create-the-project/_static/image3.png)

Projekt zajmie trochę czasu do utworzenia. Gdy będzie gotowy, otwórz **Default.aspx** strony.

![Utwórz projekt — nowy szablon projektu](create-the-project/_static/image4.png)

Można przełączać się między **projekt** widoku i **źródła** widoku, zaznaczając odpowiednią opcję w dolnej części okna Centrum. **Projekt** Wyświetla widok strony sieci Web ASP.NET, stron wzorcowych, strony z zawartością, stron HTML i za pomocą widoku w pobliżu WYSIWYG formanty użytkownika. **Źródło** widok przedstawia kod znaczników HTML dla strony sieci Web, który można edytować.

> [!TIP] 
> 
> **Opis platformy ASP.NET**
> 
> Formularze sieci Web ASP.NET umożliwia kompilacji dynamicznych witryn sieci Web przy użyciu znanego modelu przeciągania i upuszczania, sterowane zdarzeniami. Powierzchni projektowej oraz setkom kontrolek i składników pozwalają szybko tworzyć złożone, zaawansowane witryny sterowane za pośrednictwem interfejsu użytkownika z dostępem do danych. Magazynu zabawka Wingtip opiera się na formularzy sieci Web ASP.NET, ale pojęcia, które użytkownik pozna w tym samouczku są stosowane do wszystkich aplikacji ASP.NET.
> 
> Program ASP.NET oferuje cztery środowiska deweloperskie głównej:
> 
> - [Formularze sieci Web ASP.NET](../../../index.md)  
>  Framework formularzy sieci Web jest przeznaczony dla deweloperów, którzy preferowane jest systemem kontroli i deklaratywne programowania, takich jak Microsoft Windows Forms (WinForms) i Silverlight-WPF/XAML. Zapewnia model projektanta driven development, WYSIWYG, tak aby zawierała popularnych z deweloperami wyszukiwanie Środowisko deweloperskie (RAD) szybkie aplikacji do tworzenia aplikacji sieci web. Jeśli dopiero zaczynasz korzystać z sieci web programowania i zapoznaniu się z tradycyjnego narzędzia programistyczne klienta RAD firmy Microsoft (na przykład dla programu Visual Basic i Visual C#), można szybko tworzyć aplikacji sieci web bez konieczności obsługi w kodzie HTML i JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC jest przeznaczony dla deweloperów, którzy interesujących wzorców i zasady, takie jak test-driven development, separacji, Inwersja kontroli (IoC) i iniekcji zależności (Podpisane). Ta struktura zachęca oddzielanie warstwy logiki biznesowej aplikacji sieci web z jej warstwy prezentacji.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  Strony sieci Web ASP.NET jest przeznaczony dla deweloperów, którzy chcą wątku programowanie prostego, wzdłuż linii PHP. W modelu stron sieci Web można tworzyć strony HTML, a następnie dodaj kod na serwerze do strony aby dynamicznie kontrolować sposób renderowania tego kodu znaczników. Strony sieci Web jest specjalnie przeznaczona do jest lekki struktura i jest najprostszym punktem wejścia do platformy ASP.NET dla osób, które znasz HTML, ale może nie mieć szerokie środowisko programowania — na przykład, studentów lub hobbystów. Jest również dobrym sposobem dla deweloperów sieci web, którzy wiedzą, PHP lub podobne struktury, aby rozpocząć korzystanie z platformy ASP.NET.
> - [ASP.NET pojedynczej strony aplikacji](../../../../single-page-application/index.md)  
>  ASP.NET pojedynczej strony aplikacji JEDNOSTRONICOWEJ pomaga tworzyć aplikacje, które zawierają istotne interakcji po stronie klienta, za pomocą HTML 5, CSS 3 i JavaScript. ASP.NET i zaktualizować 2012.2 narzędzia sieci Web jest dostarczany nowego szablonu do tworzenia aplikacji jednej strony przy użyciu knockout.js i interfejsu API sieci Web platformy ASP.NET. Oprócz nowy szablon SPA nowe szablony utworzone społeczności SPA są również dostępne do pobrania.
> 
> Oprócz czterech struktur głównych programowanie program ASP.NET oferuje również dodatkowych technologii, które są należy pamiętać o i doświadczenia w obsłudze, ale nie zostały omówione w tym samouczku:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) — struktura umożliwiająca tworzenie usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.
> - [Biblioteka SignalR platformy ASP.NET](../../../../signalr/index.md) -biblioteki, która ułatwia tworzenie funkcji sieci web w czasie rzeczywistym.


### <a name="reviewing-the-project"></a>Przeglądanie projektu

W programie Visual Studio **Eksploratora rozwiązań** okno umożliwia zarządzanie plikami dla projektu. Spójrzmy na foldery, które zostały dodane do aplikacji w **Eksploratora rozwiązań**. Szablon aplikacji sieci web dodaje strukturę folderów podstawowe:

![Tworzenie projektu - Eksploratora rozwiązań](create-the-project/_static/image5.png)

Program Visual Studio tworzy niektórych początkowej folderów i plików dla projektu. Pierwszy pliki, które będzie można stosowaniu później w tym samouczku są następujące:

| **Plik** | **Cel** |
| --- | --- |
| *Default.aspx* | Zwykle pierwszej strony wyświetlany, gdy aplikacja jest uruchamiana w przeglądarce. |
| *Site.Master* | Strona, która pozwala utworzyć spójne zachowanie standardowe układ i użyj stron w aplikacji. |
| *Global.asax* | Opcjonalny plik zawierający kod reagowanie na poziomie aplikacji i poziomu sesji zdarzenia wywoływane przez platformę ASP.NET lub przez moduły HTTP. |
| *Web.config* | Dane konfiguracji dla aplikacji. |

### <a name="running-the-default-web-application"></a>Uruchomiona domyślnej aplikacji sieci Web

Domyślnej aplikacji sieci Web udostępnia bogate środowisko na podstawie wbudowanej funkcji i pomocy technicznej. Bez wprowadzania żadnych zmian do projektu domyślnego formularzy sieci Web aplikacja jest gotowa do uruchomienia w przeglądarce sieci Web w lokalnej.

1. Naciśnij klawisz ***F5*** klawisz w programie Visual Studio.   
 Aplikacja kompilacji i wyświetlania w przeglądarce sieci Web.  

    ![Tworzenie projektu — strona domyślna](create-the-project/_static/image6.png)
2. Po utworzeniu ukończone przeglądu działającej aplikacji, zamknij okno przeglądarki.

Istnieją trzy główne strony w tej domyślnej aplikacji sieci Web: *Default.aspx* (Narzędzia główne), *About.aspx*, i *Contact.aspx*. Każdy z tych stron jest osiągalna z górnym pasku nawigacyjnym. Istnieją również dwie dodatkowe strony znajdujące się w folderze konta, strona Register.aspx i strony Login.aspx. Te dwie strony umożliwiają używanie funkcji członkostwa programu ASP.NET można tworzyć, przechowywać i sprawdzanie poprawności poświadczeń użytkownika.

## <a name="aspnet-web-forms-background"></a>Tło formularzy sieci Web ASP.NET

Formularze sieci Web ASP.NET są stron, które są oparte na technologii Microsoft ASP.NET, w którym kod, który działa na serwerze dynamicznie generuje dane wyjściowe strony sieci Web na urządzeniu klienta lub dotyczących przeglądarki. Strony formularzy sieci Web ASP.NET jest automatycznie renderuje poprawne zgodny z przeglądarki HTML dla funkcji, takich jak style, układu i tak dalej. Formularze sieci Web są zgodne z dowolnego języka, obsługiwane przez .NET środowisko uruchomieniowe języka wspólnego, takich jak Microsoft Visual Basic i Microsoft Visual C#. Ponadto formularzy sieci Web są tworzone [programu Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), która zapewnia korzyści, takich jak zarządzanego środowiska, zabezpieczeń i dziedziczenia.

Po uruchomieniu strony formularzy sieci Web ASP.NET, strona przechodzi przez cyklu życia, w której wykonuje serię kroków przetwarzania. Kroki te obejmują inicjowania uruchamianiu formantów, przywracania i zachowanie stanu, uruchomiony kod obsługi zdarzenia i renderowania. W bardziej zapoznanie się z możliwości formularzy sieci Web ASP.NET jest ważne zrozumieć [cyklu życia strony ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) , dzięki czemu można napisać kod na etapie cyklu życia odpowiednie mają wpływ.

Gdy serwer sieci Web otrzymuje żądanie strony, jego znajduje stronę, procesy wysyła go do przeglądarki i odrzuca wszystkie informacje ze strony. Jeśli użytkownik ponownie żąda tej samej stronie, serwer powtarza całą sekwencję ponowne przetwarzanie strony, od początku. Innymi słowy, serwer ma Brak pamięci stron, czy ma ona przetworzyć strony są bezstanowe. Struktury strony ASP.NET obsługuje automatycznie zadanie obsługi stan strony i jego formantów i umożliwia jawne sposoby zarządzania stanem informacje specyficzne dla aplikacji.

> [!TIP] 
> 
> **Funkcje aplikacji sieci Web w szablonie aplikacji formularzy sieci Web**
> 
> Szablon aplikacji formularzy sieci Web platformy ASP.NET zawiera bogaty zestaw wbudowanych funkcji. Nie tylko umożliwia on *Home.aspx* strony, *About.aspx* strony, *Contact.aspx* strony, ale także funkcje członkostwo, które rejestruje użytkowników i zapisuje swoje poświadczenia, aby ich zalogować się do witryny sieci Web. Ten przegląd zawiera więcej informacji na temat niektóre funkcje zawarte w szablonie aplikacji formularzy sieci Web ASP.NET i sposób ich użycia w aplikacji Wingtip Toys.
> 
> **Członkostwo**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) tożsamości przechowuje poświadczenia użytkownika w bazie danych utworzonego przez aplikację. Gdy użytkownicy zalogują się, aplikacji sprawdza poprawność poświadczeń za Odczytywanie bazy danych. Projektu *konta* folder zawiera pliki, które implementuje różne części członkostwa: rejestracji, logowania, zmiana haseł i autoryzowania dostępu. Ponadto formularzy sieci Web ASP.NET obsługuje OAuth i OpenID. Te ulepszenia uwierzytelniania zezwolić użytkownikom na logowanie się do witryny przy użyciu istniejących poświadczeń z konta, takich jak Facebook, Twitter, Windows Live i Google.
> 
> ![Tworzenie projektu - Eksploratorze rozwiązań (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Domyślnie szablon tworzy bazę danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu programu SQL Server Express LocalDB, serwer bazy danych programowanie dołączoną do programu Visual Studio Express 2013 for Web.
> 
> **SQL Server Express LocalDB.**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) jest to Uproszczona wersja programu SQL Server, który ma wiele funkcje programowania bazy danych programu SQL Server. SQL Server Express LocalDB zostanie uruchomiony w trybie użytkownika i ma szybką, niewymagającą konfiguracji instalacji, który ma krótką listę wymagań wstępnych instalacji. W programie Microsoft SQL Server, bazy danych lub kodu języka Transact-SQL można przenieść z programu SQL Server Express LocalDB programu SQL Server i SQL Azure bez wszystkie kroki uaktualniania. Tak SQL Server Express LocalDB może służyć jako Środowisko deweloperskie dla aplikacji przeznaczonych dla wszystkich wersji programu SQL Server. SQL Server Express LocalDB włącza funkcje takie jak procedur składowanych, funkcje zdefiniowane przez użytkownika i agregacji, integracja .NET Framework, typy przestrzenne i inne osoby, które nie są dostępne w programie SQL Server Compact.
> 
> **Strony wzorcowe**
> 
> [Strony wzorcowej ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definiuje spójny wygląd i zachowanie dla wszystkich stron w aplikacji. Układ strony wzorcowej łączy się z zawartości z jednej strony zawartości do produkcji na ostatniej stronie, które użytkownik widzi. W aplikacji Wingtip Toys zmodyfikować *Site.master* strony wzorcowej, dzięki czemu wszystkich stron w witrynie sieci Web Wingtip Toys udostępnianie tego samego charakterystyczne paska logo i nawigacji.
> 
> **HTML5**
> 
> Szablon aplikacji formularzy sieci Web ASP.NET obsługuje [HTML5](http://www.w3schools.com/html/html5_intro.asp), która jest najnowsza wersja języka znaczników HTML. HTML5 obsługuje nowe elementy i funkcje, które ułatwiają tworzenie witryn sieci Web.
> 
> **Modernizr**
> 
> W przypadku przeglądarek, które nie obsługują HTML5, można użyć [Modernizr](http://www.modernizr.com/). Modernizr jest biblioteka języka JavaScript open source, który może wykryć, czy przeglądarka obsługuje funkcje HTML5 i je włączyć, jeśli nie. W szablonie aplikacji formularzy sieci Web ASP.NET Modernizr jest instalowany jako pakietu NuGet.
> 
> **Bootstrap**
> 
> Szablony projektu Visual Studio 2013 użyj [Bootstrap](http://getbootstrap.com/), ramy układ i motywów utworzonych przez usługi Twitter. Bootstrap używa CSS3, aby zapewnić elastyczny projekt, co oznacza, że układów dynamicznie można dostosować do rozmiarów okna innej przeglądarki. Funkcja tworzenia motywów ładowania początkowego na umożliwia również łatwe wpływ zmian w aplikacji wyglądu i działania. Domyślnie szablonu aplikacji sieci Web ASP.NET w programie Visual Studio 2013 obejmują Bootstrap jako pakietu NuGet.
> 
> **NuGet Packages**
> 
> Szablon aplikacji formularzy sieci Web platformy ASP.NET zawiera zbiór [NuGet](http://www.nuget.org/) pakietów. Te pakiety udostępnia składnikowa funkcji w formie typu open source, biblioteki i narzędzia. Istnieje szereg pakiety, które ułatwiają tworzenie i testowanie aplikacji. Visual Studio ułatwia dodawanie, usuwanie i aktualizację pakietów NuGet. Deweloperzy mogą utworzyć i również dodawać pakiety NuGet.
> 
> ![Tworzenie projektu — okno dialogowe NuGet](create-the-project/_static/image8.png)
> 
> Po zainstalowaniu pakietu NuGet kopiuje pliki do rozwiązania i automatycznie sprawia, że wszelkie zmiany są potrzebne, takie jak dodawanie odwołań i zmienianie konfiguracji skojarzone z aplikacją sieci Web. Jeśli zdecydujesz się usunąć biblioteki, NuGet usuwa pliki i odwraca wszelkie zmiany wprowadzone w projekcie, dzięki czemu pozostanie bez bałaganu. NuGet jest dostępna z **narzędzia** menu w programie Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) jest szybkie i zwięzłe biblioteki JavaScript, która upraszcza przechodzenie dokumentu HTML, obsługa zdarzeń animacji i interakcje Ajax dla rozwoju szybkiej sieci web. Biblioteka JavaScript jQuery jest zawarta w szablonie aplikacji formularzy sieci Web ASP.NET jako pakietu NuGet.
> 
> **Sprawdzania poprawności dyskretnego kodu**
> 
> Formanty wbudowany moduł sprawdzania poprawności zostały skonfigurowane dyskretny kod JavaScript na potrzeby logikę weryfikacji po stronie klienta. To znacznie zmniejsza ilość kodu JavaScript renderowane w tekście w znaczniku strony i zmniejsza całkowity rozmiar strony. Sprawdzania poprawności dyskretnego kodu jest dodawana globalnie do szablonu aplikacji formularzy sieci Web ASP.NET, na podstawie ustawienia w &lt;appSettings&gt; elementu *Web.config* pliku w katalogu głównym aplikacji.
> 
> **Entity Framework Code First**
> 
> Oprócz funkcji w szablonie aplikacji formularzy sieci Web platformy ASP.NET używa aplikacji Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), która jest biblioteki NuGet, który umożliwia programowanie skoncentrowane kodu podczas pracy z danymi. Po prostu umieść, tworzy bazy danych części aplikacji na podstawie kodu, który można zapisać. Przy użyciu programu Entity Framework, możesz pobrać i manipulowanie danymi jako silnie typizowanych obiektów. Dzięki temu można skupić się na logice biznesowej w aplikacji, a nie szczegółowe informacje o sposobie jest uzyskiwany dostęp do danych.
> 
> Aby uzyskać dodatkowe informacje dotyczące zainstalowanych bibliotek i pakietów zawartych w szablonie formularzy sieci Web ASP.NET zobacz listę zainstalowanych pakietów NuGet. Aby to zrobić, w programie Visual Studio Utwórz nowy projekt formularzy sieci Web, wybierz opcję **narzędzia**  - &gt; **Menedżer pakietów biblioteki**  - &gt; **Zarządzaj Pakiety NuGet dla rozwiązania**i wybierz **zainstalowane pakiety** w **Zarządzaj pakietami NuGet** okno dialogowe.


### <a name="touring-visual-studio"></a>Touring programu Visual Studio

Podstawowy system windows w programie Visual Studio zawiera **Eksploratora rozwiązań**, **Eksploratora serwera** (**Eksploratora bazy danych** w Express), **właściwości Okno**, **przybornika**, **narzędzi**i **dokument**.

![Tworzenie projektu — okno dialogowe NuGet](create-the-project/_static/image9.png)

Aby uzyskać więcej informacji na temat programu Visual Studio, zobacz [Visual Przewodnik po Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku utworzeniu, przejrzeć i uruchom domyślnej aplikacji formularzy sieci Web. Zostały sprawdzone różne funkcje domyślnej aplikacji formularzy sieci Web i przedstawiono niektóre podstawowe informacje o sposobie używania środowiska Visual Studio. W następujących samouczkach utworzysz Warstwa dostępu do danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wybieranie odpowiedniej Model programowania](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projekty aplikacji sieci Web i projektów witryny sieci Web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Omówienie stron formularzy sieci Web ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Poprzednie](introduction-and-overview.md)
> [dalej](create_the_data_access_layer.md)
