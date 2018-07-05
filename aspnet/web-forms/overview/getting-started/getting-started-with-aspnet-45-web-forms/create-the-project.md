---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Utwórz projekt | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: e5d6fa312ce0375eee5ca456aaea9f088d806cfc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384491"
---
<a name="create-the-project"></a>Tworzenie projektu
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku zostanie tworzenia, przejrzyj i uruchamiania domyślny projekt w programie Visual Studio, która pozwala zapoznać się z funkcjami platformy ASP.NET. Ponadto należy przejrzeć środowiska Visual Studio.

## <a name="what-youll-learn"></a>Zawartość:

- Jak utworzyć nowy projekt formularzy sieci Web.
- Struktura pliku projektu formularzy sieci Web.
- Jak uruchomić projekt w programie Visual Studio.
- Różne funkcje domyślnej aplikacji formularzy sieci Web.
- Pewne podstawowe informacje o sposobie używania środowiska Visual Studio.

## <a name="creating-the-project"></a>Tworzenie projektu

1. Otwórz program Visual Studio.
2. Wybierz **nowy projekt** z **pliku** menu w programie Visual Studio. 

    ![Utwórz projekt — nowy element Menu projektu](create-the-project/_static/image1.png)
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.  
 W tej serii samouczków jest przy użyciu platformy .NET Framework 4.5.2.
5. Nazwij swój projekt *WingtipToys* i wybierz polecenie **OK** przycisku. 

    ![Tworzenie projektu — okno dialogowe nowego projektu](create-the-project/_static/image2.png)

    > [!NOTE]
    > Nazwa projektu w tej serii samouczków jest **WingtipToys**. Zaleca się, że możesz użyć tego *dokładnie* nazwy projektu, dzięki czemu kod przedstawionych w tej serii samouczka funkcji zgodnie z oczekiwaniami.

6. Kliknij przycisk **Zmień uwierzytelnianie** przycisku. Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK** przycisku.

7. Wybierz **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisku.

    ![Utwórz projekt — nowy szablon projektu](create-the-project/_static/image3.png)

Projekt zajmie trochę czasu, aby utworzyć. Gdy wszystko będzie gotowe, otwórz **Default.aspx** strony.

![Utwórz projekt — nowy szablon projektu](create-the-project/_static/image4.png)

Możesz przełączać się między **projektowania** widoku i **źródła** widoku, zaznaczając odpowiednią opcję w dolnej części okna center. **Projekt** widoku są wyświetlane strony ASP.NET Web pages, strony wzorcowe, zawartości strony, strony HTML oraz formanty użytkownika przy użyciu widoku w trybie WYSIWYG. **Źródło** widok przedstawia kod znaczników HTML dla strony sieci Web, który można edytować.

> [!TIP] 
> 
> **Omówienie platformy ASP.NET**
> 
> ASP.NET Web Forms umożliwia kompilacji dynamicznych witryn sieci Web przy użyciu znanego modelu przeciągania i upuszczania, oparte na zdarzeniach. Powierzchni projektowej oraz setkom kontrolek i składników pozwalają szybko tworzyć złożone, zaawansowane witryny opartej na interfejsie użytkownika z dostępem do danych. Store zabawki Wingtip opiera się na formularzach sieci Web platformy ASP.NET, ale wiele koncepcji, których można dowiedzieć się w tej serii samouczków mają zastosowanie do wszystkich aplikacji ASP.NET.
> 
> Program ASP.NET oferuje cztery podstawowego rozwoju platform:
> 
> - [ASP.NET Web Forms](../../../index.md)  
>  Środowiska formularzy sieci Web jest przeznaczony dla deweloperów, którzy wolą opartych na kontroli i deklaratywne programowania, takich jak Microsoft Windows Forms (WinForms) i WPF/XAML/Silverlight. Oferuje ona modelem WYSIWYG projektowania opartego na projektanta, więc popularne wśród deweloperów szukających środowisko projektowe (RAD) szybkie aplikacji służące do tworzenia aplikacji sieci web. Jeśli rozpoczynasz programowanie sieci web i zapoznać się z tradycyjnych narzędzi deweloperskich klienta RAD firmy Microsoft (na przykład w przypadku języka Visual Basic i Visual C#), można szybko zbudować aplikację sieci web bez doświadczenia w kodzie HTML i JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC jest przeznaczony dla deweloperów, którzy są zainteresowani wzorców i zasad, takich jak programowania sterowanego testami, separacji Inwersja kontroli (IoC) i wstrzykiwanie zależności (DI). Ta struktura zachęca oddzielenie warstwy logiki biznesowej aplikacji sieci web od jej warstwy prezentacji.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET Web Pages jest przeznaczony dla deweloperów, którzy chcą wątku rozwoju prostą, wzdłuż linii PHP. W modelu stron sieci Web tworzyć strony HTML, a następnie dodać kod na serwerze do strony, aby dynamicznie kontrolować sposób renderowania tego znacznika. Strony sieci Web została opracowana jako lekka framework i jest najprostszym punkt wejścia do programu ASP.NET dla osób, które znasz HTML, ale może nie mieć szerokie możliwości programowania — na przykład, studentów lub hobbystów. Jest również dobrym sposobem dla deweloperów sieci web, którzy nie znają języka PHP lub podobnych platform, aby rozpocząć korzystanie z platformy ASP.NET.
> - [ASP.NET pojedynczej strony aplikacji](../../../../single-page-application/index.md)  
>  ASP.NET pojedynczej strony aplikacji (SPA) pomaga w tworzeniu aplikacji, które zawierają istotne interakcji po stronie klienta przy użyciu języków HTML 5, CSS 3 i JavaScript. Program ASP.NET i Web Tools 2012.2 Update jest dostarczany nowy szablon do tworzenia aplikacji jednostronicowej przy użyciu struktura knockout.js i Web API platformy ASP.NET. Oprócz nowy szablon SPA nowych szablonów utworzonych przez społeczność SPA są także dostępne do pobrania.
> 
> Oprócz cztery struktur głównych rozwoju program ASP.NET oferuje również dodatkowych technologii, które są ważne, aby mieć świadomość i zapoznać się z, ale nie zostały omówione w tej serii samouczków:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -umożliwiająca tworzenie usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.
> - [ASP.NET SignalR](../../../../signalr/index.md) -bibliotekę, która ułatwia opracowywanie funkcji sieci web w czasie rzeczywistym.


### <a name="reviewing-the-project"></a>Przeglądanie projektu

W programie Visual Studio **Eksploratora rozwiązań** okno umożliwia zarządzanie plikami projektu. Przyjrzyjmy się w folderach, które zostały dodane do aplikacji w **Eksploratora rozwiązań**. Szablon aplikacji sieci web dodaje strukturę folderów podstawowe:

![Utwórz projekt — Eksplorator rozwiązań](create-the-project/_static/image5.png)

Program Visual Studio tworzy niektóre początkowej foldery i pliki w projekcie. Pierwszy pliki, które będziesz pracować z później w tym samouczku są następujące:

| **Plik** | **Cel** |
| --- | --- |
| *Default.aspx* | Zazwyczaj pierwsza strona wyświetlana, gdy aplikacja jest uruchamiana w przeglądarce. |
| *Site.Master* | Strona, która pozwala na tworzenie spójne zachowanie standardowego układu i użycie stron w aplikacji. |
| *Global.asax* | Opcjonalny plik, który zawiera kod w odpowiedzi na poziomie aplikacji i poziom sesji zdarzenia wywoływane przez platformę ASP.NET lub przez moduły HTTP. |
| *Web.config* | Dane konfiguracji dla aplikacji. |

### <a name="running-the-default-web-application"></a>Uruchamianie domyślnej aplikacji sieci Web

Domyślnej aplikacji sieci Web zapewnia bogate, na podstawie wbudowanej funkcjonalności i pomocy technicznej. Bez wprowadzania zmian w domyślny projekt formularzy sieci Web aplikacja jest gotowa do uruchomienia w przeglądarce sieci Web w lokalnych.

1. Naciśnij klawisz ***F5*** klucza podczas w programie Visual Studio.   
 Aplikacja będzie tworzyć i wyświetlać w przeglądarce sieci Web.  

    ![Tworzenie projektu — strona domyślna](create-the-project/_static/image6.png)
2. Po utworzeniu przeglądu ukończone w uruchomionej aplikacji, zamknij okno przeglądarki.

Istnieją trzy główne strony w tej domyślnej aplikacji sieci Web: *Default.aspx* (dom), *About.aspx*, i *Contact.aspx*. Każda z tych stron jest osiągalna z górnego paska nawigacyjnego. Dostępne są także dwie dodatkowe strony znajdujące się w folderze konta, strona Register.aspx i strony Login.aspx. Te dwie strony pozwalają na wykorzystanie możliwości członkostwa programu ASP.NET do tworzenia, przechowywania i sprawdzanie poprawności poświadczeń użytkownika.

## <a name="aspnet-web-forms-background"></a>Tła formularzy sieci Web ASP.NET

ASP.NET Web Forms są stron, które są oparte na technologii Microsoft ASP.NET, w którym kod, który działa na serwerze dynamicznie generuje danych wyjściowych strony sieci Web do przeglądarki lub klienta urządzenia. Na stronie ASP.NET Web Forms automatycznie renderuje poprawne zgodne z przeglądarki HTML, dla funkcji, takich jak style, układ i tak dalej. Formularze sieci Web są zgodne z dowolnym językiem, obsługiwane przez .NET środowisko uruchomieniowe języka wspólnego, takich jak Microsoft Visual Basic i Microsoft Visual C#. Ponadto formularzy sieci Web są oparte na [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), który zapewnia korzyści w środowisku zarządzanym, bezpieczeństwo typów i dziedziczenia.

Po uruchomieniu strony formularzy sieci Web platformy ASP.NET, strona przechodzi przez cyklu życia, w którym wykonuje szereg kroków przetwarzania. Te kroki obejmują inicjowania wystąpienia kontrolki, przywracania i zachowaniem stanu, uruchomiony kod procedury obsługi zdarzeń, a renderowania. Zgodnie z bardziej zapoznanie się z możliwościami formularzy sieci Web ASP.NET jest ważne zrozumieć [cyklu życia strony ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) tak, aby na etapie odpowiednie cyklu życia dla efektu planowane można napisać kod.

Gdy serwer sieci Web odbiera żądanie dla strony, jego znajduje stronę, przetwarza je, wysyła go do przeglądarki i odrzuca wszystkie informacje ze strony. Jeśli użytkownik zażąda tej samej stronie ponownie, serwer powtarza całą sekwencję, ponownego przetworzenia strony od podstaw. Innymi słowy, serwer ma Brak pamięci stron, że ma on przetworzony strony są bezstanowe. Architektura strony ASP.NET automatycznie obsługuje zadanie obsługi stanu strony i jego formantów i umożliwia jawne sposoby zarządzania stanem informacji specyficznych dla aplikacji.

> [!TIP] 
> 
> **Funkcje aplikacji sieci Web w szablonie aplikacji formularzy sieci Web**
> 
> Szablon aplikacji formularzy sieci Web ASP.NET udostępnia bogaty zestaw wbudowanych funkcji. Nie tylko zapewnia użytkownikowi *Home.aspx* stronie *About.aspx* stronie *Contact.aspx* strony, ale oferuje także funkcjonalności członkostwa, który rejestruje użytkowników i zapisuje swoje poświadczenia, aby ich zalogować się do witryny sieci Web. Ten przegląd zawiera więcej informacji na temat niektóre funkcje zawarte w szablonie aplikacji formularzy sieci Web ASP.NET i jak są używane w aplikacji Wingtip Toys.
> 
> **Członkostwo**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) tożsamości przechowuje poświadczenia użytkownika w bazie danych utworzonych przez aplikację. Podczas logowania się użytkowników aplikacji sprawdza poprawność poświadczeń, zapoznając się bazy danych. Projektu *konta* folder zawiera pliki, które implementują różne części członkostwa: rejestracji, logowania, zmiana haseł i autoryzowanie dostępu. Ponadto formularzy sieci Web programu ASP.NET obsługuje OAuth i OpenID. Te ulepszenia uwierzytelniania zezwolić użytkownikom na logowanie się do witryny przy użyciu bieżących poświadczeń z konta, takie jak Facebook, Twitter, Windows Live i Google.
> 
> ![Utwórz projekt — Eksploratora rozwiązań (produktu ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Domyślnie ten szablon tworzy bazy danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu programu SQL Server Express LocalDB, serwer bazy danych rozwoju, dostarczanego z Visual Studio Express 2013 for Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) to Uproszczona wersja programu SQL Server, który ma wiele funkcji programowania bazy danych programu SQL Server. SQL Server Express LocalDB działa w trybie użytkownika i ma szybką, niewymagającą konfiguracji instalację, który ma krótką listę wymagań wstępnych instalacji. W programie Microsoft SQL Server, bazę danych lub kod języka Transact-SQL może zostać przeniesiona z programu SQL Server Express LocalDB do programu SQL Server i SQL Azure bez konieczności uaktualnień. Tak SQL Server Express LocalDB może służyć jako środowiska deweloperskiego dla aplikacji przeznaczonych dla wszystkich wersji programu SQL Server. SQL Server Express LocalDB włącza funkcje, takie jak procedury składowane, funkcje zdefiniowane przez użytkownika i agregacji, integracji .NET Framework, typów przestrzennych i inne, które nie są dostępne w dokumentacji programu SQL Server Compact.
> 
> **Strony wzorcowe**
> 
> [Strona wzorcowa ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definiuje spójny wygląd i zachowanie dla wszystkich stron w aplikacji. Układ strony wzorcowej jest scalana z zawartości z poszczególnych stron zawartości, aby wygenerować na ostatniej stronie, widziany przez użytkownika. W aplikacji Wingtip Toys zmodyfikujesz *Site.master* strony wzorcowej, tak aby wszystkich stron w witrynie sieci Web firmy Wingtip Toys udostępnianie tego samego szczególne paska logo i nawigacji.
> 
> **HTML5**
> 
> Szablon aplikacji formularzy sieci Web programu ASP.NET obsługuje [HTML5](http://www.w3schools.com/html/html5_intro.asp), który jest najnowszą wersję języka znaczników HTML. HTML5 obsługuje nowe elementy i funkcje, które ułatwiają tworzenie witryn sieci Web.
> 
> **Modernizr**
> 
> Dla przeglądarek, które nie obsługują HTML5, możesz użyć [Modernizr](http://www.modernizr.com/). Modernizr to biblioteka języka JavaScript typu open source, która może wykryć, czy przeglądarka obsługuje funkcje HTML5 i je włączyć, jeśli nie jest. W szablonie aplikacji formularzy sieci Web ASP.NET Modernizr jest instalowany jako pakiet NuGet.
> 
> **Bootstrap**
> 
> Szablony projektu Visual Studio 2013 korzystają [Bootstrap](http://getbootstrap.com/), układ i motywów framework, utworzone przez usługi Twitter. Usługa ładowania początkowego używa CSS3, aby zapewnić elastyczne, co oznacza, że układy dynamicznie dostosowują się do innej przeglądarki rozmiary okna. Można również użyć funkcji motywów Bootstrap firmy, można łatwo dokonać zmian w aplikacji wyglądu i działania. Domyślnie szablon aplikacji sieci Web ASP.NET w programie Visual Studio 2013 zawiera narzędzia Bootstrap jako pakiet NuGet.
> 
> **NuGet Packages**
> 
> Szablon aplikacji formularzy sieci Web ASP.NET zawiera zbiór [NuGet](http://www.nuget.org/) pakietów. Te pakiety oferuje składającej w formularzu, narzędzi i bibliotek typu open source. Istnieje szereg pakietów, aby ułatwić tworzenie i testowanie aplikacji. Program Visual Studio ułatwia dodawanie, usuwanie i aktualizowanie pakietów NuGet. Deweloperzy mogą tworzyć i dodawać pakiety NuGet także.
> 
> ![Tworzenie projektu — okno dialogowe NuGet](create-the-project/_static/image8.png)
> 
> Podczas instalowania pakietu NuGet kopiuje pliki do rozwiązania i automatycznie tworzy wszelkie potrzebne są zmiany, takie jak dodawanie odwołania i zmiany konfiguracji skojarzone z aplikacją sieci Web. Jeśli zdecydujesz się usunąć biblioteki NuGet usuwa pliki i odwraca wszelkie zmiany wprowadzone w projekcie, dzięki czemu pozostanie nie zaśmiecania. NuGet jest dostępne z **narzędzia** menu w programie Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) jest szybkie i zwięzłe biblioteka języka JavaScript, która upraszcza przechodzenie dokumentu HTML, obsługa zdarzeń, animowanie i interakcje Ajax do tworzenia aplikacji internetowych szybkie. Biblioteka JavaScript jQuery znajduje się w szablonie aplikacji formularzy sieci Web platformy ASP.NET jako pakiet NuGet.
> 
> **Sprawdzania poprawności dyskretnego kodu**
> 
> Wbudowany moduł sprawdzania poprawności formantów zostały skonfigurowane na potrzeby dyskretny kod JavaScript logiki weryfikacji po stronie klienta. To znacznie zmniejsza ilość kodu JavaScript renderowane w tekście w znaczniku strony i zmniejsza całkowity rozmiar strony. Szablon aplikacji formularzy sieci Web platformy ASP.NET na podstawie ustawienia w dodaje się globalnie sprawdzania poprawności dyskretnego kodu &lt;appSettings&gt; elementu *Web.config* pliku w katalogu głównym aplikacji.
> 
> **Entity Framework Code pierwszy**
> 
> Oprócz funkcji w szablonie aplikacji formularzy sieci Web ASP.NET, używane przez aplikację Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), czyli biblioteki NuGet, która umożliwia programowanie skoncentrowane na kodzie podczas pracy z danymi. Najprościej mówiąc, tworzy bazy danych części aplikacji na podstawie kodu, który należy napisać. Używający narzędzia Entity Framework, możesz pobrać i manipulowanie danymi jako silnie typizowanych obiektów. Pozwala to skupić się na logice biznesowej w aplikacji, a nie szczegóły sposób dostęp do danych.
> 
> Aby uzyskać dodatkowe informacje o bibliotekach zainstalowanych i pakietów zawartych w szablonie formularzy sieci Web ASP.NET zobacz listę zainstalowanych pakietów NuGet. Aby to zrobić, w programie Visual Studio Utwórz nowy projekt formularzy sieci Web, wybierz opcję **narzędzia**  - &gt; **Menedżer pakietów biblioteki**  - &gt; **Zarządzanie Pakiety NuGet dla rozwiązania**i wybierz **zainstalowanych pakietów** w **Zarządzaj pakietami NuGet** okno dialogowe.


### <a name="touring-visual-studio"></a>Podróżujący programu Visual Studio

Podstawowy system windows w programie Visual Studio zawiera **Eksploratora rozwiązań**, **Eksploratora serwera** (**Eksplorator bazy danych** w wersji Express), **właściwości Okno**, **przybornika**, **narzędzi**i **okna dokumentu**.

![Tworzenie projektu — okno dialogowe NuGet](create-the-project/_static/image9.png)

Aby uzyskać więcej informacji na temat programu Visual Studio, zobacz [przewodnik wizualny po Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Podsumowanie

W ramach tego samouczka utworzone, przejrzeniu i uruchomić domyślnej aplikacji formularzy sieci Web. Przejrzeniu różne funkcje domyślnej aplikacji formularzy sieci Web i przedstawiono pewne podstawowe informacje o sposobie używania środowiska Visual Studio. W ramach następujących samouczków utworzysz warstwy dostępu do danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wybieranie odpowiedniego modelu programowania](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web Application Projects versus Web Site Projects](https://msdn.microsoft.com/library/dd547590.aspx)   
[Omówienie stron formularzy sieci Web ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Poprzednie](introduction-and-overview.md)
> [dalej](create_the_data_access_layer.md)
