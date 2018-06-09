---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Wskazówki laboratorium: Jeden ASP.NET: integrowanie formularzy sieci Web ASP.NET, MVC i interfejs API sieci Web | Dokumentacja firmy Microsoft'
author: rick-anderson
description: ASP.NET to struktura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalnych, takich jak MVC, Web API i inne. Z rozszerzeniem ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566441"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Wskazówki laboratorium: Jeden ASP.NET: integrowanie formularzy sieci Web ASP.NET, MVC i interfejs API sieci Web
====================
Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET to struktura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalnych, takich jak MVC, Web API i inne. Z rozszerzeniem ASP.NET ma odebrane od czasu jego utworzenia i zaakceptowania muszą mieć te technologie zintegrowane, zaszły ostatnie wysiłków w prace nad **ASP.NET jeden**.
> 
> Visual Studio 2013 wprowadzenie nowego systemu ujednoliconego projektu, który umożliwia tworzenie aplikacji i korzystać z technologii ASP.NET w jednym projekcie. Ta funkcja eliminuje konieczność pobrania jednej technologii na początku projektu i dysku z nim, a zamiast tego zaleca się korzystanie z wielu platform ASP.NET w jednym projekcie.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Tworzenie witryny sieci Web na podstawie **ASP.NET jeden** typ projektu
- Użyj innego **ASP.NET** platform, takich jak **MVC** i **interfejsu API sieci Web** w tym samym projekcie
- Identyfikacja głównych składników **ASP.NET** aplikacji
- Korzystać z **szkieletów ASP.NET** framework do automatycznego tworzenia widoków i kontrolerów w celu wykonywania operacji CRUD oparte na klasach modeli
- Udostępnianie ten sam zestaw informacji w formatach machine - i zrozumiałą dla użytkownika za pomocą właściwych narzędzi do każdego zadania

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.
3. Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.

> [!NOTE]
> Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Korzystania z wstawek kodu

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania. W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Tworzenie nowego projektu formularzy sieci Web](#Exercise1)
2. [Tworzenie przy użyciu funkcji szkieletów kontrolera MVC](#Exercise2)
3. [Tworzenie kontrolera interfejsu API sieci Web przy użyciu funkcji szkieletów](#Exercise3)

Szacowany czas trwania tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji. Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web

W tym ćwiczeniu utworzysz nową lokację formularzy sieci Web w programie Visual Studio 2013 za pomocą **ASP.NET jeden** unified doświadczenie w projekcie, co pozwoli łatwo zintegrować składników Web Forms, MVC i interfejsu API sieci Web w tej samej aplikacji. Będą Eksploruj wygenerowanego rozwiązania i zidentyfikować jego części, a na koniec zostanie zobacz witrynę sieci Web w akcji.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Zadanie 1 — Tworzenie nowej lokacji za pomocą jednego środowiska ASP.NET

To zadanie będzie rozpocząć tworzenia nowej witryny sieci Web w programie Visual Studio na podstawie **ASP.NET jeden** typu projektu. **Jeden ASP.NET** łączy wszystkie technologii ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami. Następnie rozpoznania różnych składników Web Forms, MVC i interfejsu API sieci Web, które na żywo jednocześnie w aplikacji.

1. Otwórz **programu Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** i upewnij się, że **.NET Framework 4.5** jest zaznaczone. Nazwij projekt *MyHybridSite*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu, a następnie wybierz **MVC** i **interfejsu API sieci Web** opcje. Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualnych kont użytkowników**. Kliknij przycisk **OK** aby kontynuować.

    ![Tworzenie nowego projektu z szablonu formularzy sieci Web, w tym składniki interfejsu API sieci Web i MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Tworzenie nowego projektu z szablonu formularzy sieci Web, w tym składniki interfejsu API sieci Web i MVC*
4. Teraz można sprawdzić strukturę wygenerowanego rozwiązania.

    ![Eksploracja wygenerowanego rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Eksploracja wygenerowanego rozwiązania*

    1. **Konto:** ten folder zawiera na stronach formularzy sieci Web, aby zarejestrować, zaloguj się i zarządzanie kontami użytkowników aplikacji. Ten folder jest dodana gdy **indywidualnych kont użytkowników** wybrano opcję uwierzytelniania podczas konfigurowania szablonu projektu formularzy sieci Web.
    2. **Modele:** ten folder będzie zawierać klasy, które przedstawiają dane aplikacji.
    3. **Kontrolery** i **widoków**: te foldery są wymagane dla **ASP.NET MVC** i **ASP.NET Web API** składników. Użytkownik może zapoznać technologii MVC i interfejsu API sieci Web w następnym ćwiczeniach.
    4. **Default.aspx**, **Contact.aspx** i **About.aspx** pliki są wstępnie zdefiniowane stron formularzy sieci Web, które służy jako punkt wyjścia do tworzenia strony specyficzne dla użytkownika aplikacja. Programowania logiki tych plików znajduje się w oddzielnym pliku nazywane &quot;CodeBehind&quot; pliku, który ma &quot;. aspx.vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od język używany). Logika CodeBehind działa na serwerze i dynamicznie tworzy danych wyjściowych HTML strony.
    5. **Site.Master** i **Site.Mobile.Master** stron zdefiniuj wyglądu i działania i standardowe zachowanie wszystkich stron w aplikacji.
5. Kliknij dwukrotnie **Default.aspx** plik, aby zapoznać się z zawartości strony.

    ![Na stronie Default.aspx eksploracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Na stronie Default.aspx eksploracji*

    > [!NOTE]
    > **Strony** dyrektywy w górnej części pliku definiuje atrybuty strony formularzy sieci Web. Na przykład **MasterPageFile** atrybut określa ścieżkę do wzorca strony — w takim przypadku *Site.Master* stronie — i **Inherits** definiuje atrybut klasy związane z kodem strony dziedziczenia. Ta klasa znajduje się w pliku ustaleniami **plik CodeBehind** atrybutu.
    > 
    > **Asp: Content** formant zawiera rzeczywistej zawartości strony (tekst, znaczniki i kontrolki) i jest mapowany na **asp: ContentPlaceHolder** kontrolki na stronie głównej. W takim przypadku zawartość strony będzie renderowany wewnątrz *znacznika* kontroli *Site.Master* strony.
6. Rozwiń węzeł **aplikacji\_Start** folderu i zwróć uwagę, **WebApiConfig.cs** pliku. Visual Studio zawarte tego pliku w wygenerowanym rozwiązania, ponieważ dołączony interfejsu API sieci Web podczas konfigurowania projektu przy użyciu szablonu platformy ASP.NET w jednym.
7. Otwórz **WebApiConfig.cs** pliku. W *WebApiConfig* klasy można znaleźć konfiguracji skojarzone z sieci Web interfejsu API, który mapuje HTTP kieruje do **kontrolerów interfejsu API sieci Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Otwórz **RouteConfig.cs** pliku. Wewnątrz *RegisterRoutes* można znaleźć konfiguracji skojarzone z MVC, który mapuje tras HTTP do metody **kontrolerów MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — rozwiązanie uruchomiona

W tym zadaniu zostanie uruchamianie wygenerowanego rozwiązania, aplikacji i niektóre jej funkcje, takie jak ponowne zapisywanie adresów URL i wbudowane uwierzytelnianie.

1. Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** przycisk znajdujący się na pasku narzędzi. Strona główna aplikacji należy otworzyć w przeglądarce.

    ![Uruchomiona rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Upewnij się, że stron formularzy sieci Web są wywoływane. Aby to zrobić, należy dołączyć **/contact.aspx** do adresu URL na pasku adresu i naciśnij klawisz **Enter**.

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Przyjazne adresy URL*

    > [!NOTE]
    > Jak widać, zmiana adresu URL do **/skontaktuj się z**. Począwszy od **programu ASP.NET 4**, adres URL funkcji routingu zostały dodane do formularzy sieci Web, więc można napisać adresów URL, takich jak *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* zamiast  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Więcej informacji można znaleźć w [routingu adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Użytkownik może teraz zapoznać przepływ uwierzytelniania zintegrowane w aplikacji. Aby to zrobić, kliknij przycisk **zarejestrować** w prawym górnym rogu strony.

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Rejestrowanie nowego użytkownika*
4. W **zarejestrować** wprowadź **nazwy użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Zarejestruj strony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Zarejestruj strony*
5. Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniony.

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Użytkownik uwierzytelniony*
6. Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ćwiczenie 2: Tworzenie kontrolera MVC przy użyciu funkcji szkieletów

W tym ćwiczeniu zostanie korzystanie z platformy ASP.NET szkieletów dostarczane przez program Visual Studio do utworzenia kontrolera ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania pojedynczy wiersz kodu. Proces tworzenia szkieletów użyje Entity Framework Code First można wygenerować kontekstu danych i schemat bazy danych w bazie danych SQL.

**O programu Entity Framework Code najpierw**

Entity Framework (EF) jest mapowania obiektów relacyjnych (ORM) umożliwiający tworzenie aplikacji dostęp do danych przez programowania za pomocą modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny.

Entity Framework Code First modelowania przepływu pracy umożliwia przy użyciu własnych klas domeny do reprezentowania EF zależy od podczas wykonywania kwerendy, model funkcji śledzenia zmian i aktualizowanie. Przy użyciu Code First przepływu pracy programowania, nie należy do rozpoczęcia tworzenia bazy danych lub określając schematem aplikacji. Zamiast tego można napisać standardowe klasy .NET, które definiują najbardziej odpowiednie obiekty modelu domeny dla aplikacji i programu Entity Framework utworzy bazę danych można.

> [!NOTE]
> Dowiedz się więcej na temat programu Entity Framework [tutaj](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Zadanie 1 — Tworzenie nowego modelu

Teraz określi **osoby** klasy, który będzie używany przez proces tworzenia szkieletów do tworzenia kontrolera MVC i widoki modelu. Rozpocznie się przez utworzenie **osoby** klasy modelu i operacje CRUD na kontrolerze zostanie automatycznie utworzony przy użyciu funkcji szkieletów.

1. Otwórz **programu Visual Studio Express 2013 for Web** i **MyHybridSite.sln** rozwiązania, znajdujących się w **źródło/Ex2-MvcScaffolding/Begin** folderu. Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modele** folderu **MyHybridSite** projekt i wybierz **Dodaj | Klasy...** .

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Dodawanie klasy modelu osoby*
3. W **Dodaj nowy element** okno dialogowe, nazwa pliku *Person.cs* i kliknij przycisk **Dodaj**.

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Tworzenie klasy modelu osoby*
4. Zamień zawartość **Person.cs** pliku następującym kodem. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

    (Fragment - kodu *PersonClass BringingTogetherOneAspNet - Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MyHybridSite** projekt i wybierz **kompilacji**, lub naciśnij klawisz **CTRL + SHIFT + B** Aby skompilować projekt.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Zadanie 2 — Tworzenie kontrolera MVC

Teraz, gdy **osoby** modelu jest tworzony, użyje szkieletów ASP.NET MVC z programu Entity Framework do utworzenia CRUD akcji kontrolera oraz widoki dla **osoby**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projekt i wybierz **Dodaj | Nowy element szkieletu...** .

    ![Tworzenie nowego kontrolera szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Tworzenie nowego kontrolera szkieletu*
2. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework** , a następnie kliknij przycisk **Dodaj.**

    ![Wybieranie kontroler MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Wybieranie kontroler MVC 5 z widokami i Entity Framework*
3. Ustaw *MvcPersonController* jako **nazwy kontrolera**, wybierz pozycję **używać asynchronicznych akcji kontrolera** opcję i zaznacz **osoby (MyHybridSite.Models)**  jako **klasa modelu**.

    ![Dodawanie kontroler MVC z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Dodawanie kontroler MVC z szkieletów*
4. W obszarze **klasa kontekstu danych**, kliknij przycisk **nowy kontekst danych...** .

    ![Tworzenie nowego kontekstu danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Tworzenie nowego kontekstu danych*
5. W **nowy kontekst danych** okno dialogowe, nazwa nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Tworzenie nowego typu PersonContext*
6. Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla **osoby** z szkieletów. Program Visual Studio wygeneruje następnie akcji kontrolera, kontekst danych osoby i widokami Razor.

    ![Po utworzeniu kontroler MVC z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Po utworzeniu kontroler MVC z szkieletów*
7. Otwórz **MvcPersonController.cs** w pliku **kontrolerów** folderu. Zwróć uwagę, że metody akcji CRUD został wygenerowany automatycznie.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Wybierając **używać asynchronicznych akcji kontrolera** pola wyboru z rusztowania opcje w poprzednich krokach, Visual Studio generuje metod asynchronicznych akcji dla wszystkich akcji, które wymagają dostępu do osoby kontekstu danych. Zaleca się, że w przypadku użycia metod asynchronicznych akcji dla długotrwałe, niezwiązane z procesorem powiązany żądania w celu unikania blokowania serwera sieci Web z wykonywania pracy podczas przetwarzania żądania.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchomiona rozwiązania

W ramach tego zadania, należy uruchomić ponownie, aby sprawdzić, czy rozwiązanie widoków **osoby** działają zgodnie z oczekiwaniami. Spowoduje dodanie nowej osoby, aby zweryfikować, że został pomyślnie zapisany w bazie danych.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Przejdź do **/MvcPerson**. Widok szkieletu, który zawiera listę osób, powinny być wyświetlane.
3. Kliknij przycisk **Utwórz nowy** można dodać nowej osoby.

    ![Nawigowanie do szkieletu widoków MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Nawigowanie do szkieletu widoków MVC*
4. W **Utwórz** wyświetlić, podaj **nazwa** i **wieku** osoby, a następnie kliknij przycisk **Utwórz**.

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Dodawanie nowej osoby*
5. Nowej osoby jest dodawany do listy. Z listy element, kliknij przycisk **szczegóły** Aby wyświetlić szczegóły danej osoby. Następnie w **szczegóły** wyświetlić, kliknij przycisk **powrót do listy** aby powrócić do widoku listy.

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Widok szczegółów osoby*
6. Kliknij przycisk **usunąć** łącze, aby usunąć osoby. W **usunąć** wyświetlić, kliknij przycisk **usunąć** o potwierdzenie operacji.

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Usuwanie osoby*
7. Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ćwiczenie 3: Tworzenie kontrolera interfejsu API sieci Web przy użyciu funkcji szkieletów

Strukturę interfejsu API sieci Web jest częścią stosu ASP.NET i przeznaczony do ułatwienia wykonawcze usługi HTTP, zazwyczaj wysyłania i odbierania danych w formacie JSON i XML za pomocą interfejsu API RESTful.

W tym ćwiczeniu będzie szkieletów ASP.NET ponownie użyć do wygenerowania kontrolera interfejsu API sieci Web. Będzie używać tego samego **osoby** i **PersonContext** klas z poprzednim ćwiczeniu, aby zapewnić tych samych danych osoby w formacie JSON. Zobaczysz, jak mogą uwidaczniać tych samych zasobów w różny sposób w tej samej aplikacji ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web

W ramach tego zadania spowoduje utworzenie nowej **Kontroler interfejsu API sieci Web** który powoduje to udostępnienie danych osoby w niestandardowe maszyny formatu JSON.

1. Jeśli nie jest jeszcze otwarty, otwórz **programu Visual Studio Express 2013 for Web** , a następnie otwórz **MyHybridSite.sln** rozwiązania, znajdujących się w **źródło/Ex3-WebAPI/Begin** folderu. Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.

    > [!NOTE]
    > Po uruchomieniu z rozwiązaniem Rozpocznij od 3 do wykonywania, naciśnij klawisz **CTRL + SHIFT + B** celu skompilowania rozwiązania.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projekt i wybierz **Dodaj | Nowy element szkieletu...** .

    ![Tworzenie nowego kontrolera szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Tworzenie nowego kontrolera szkieletu*
3. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **interfejsu API sieci Web** w okienku po lewej stronie, następnie **Web Kontroler interfejsu API 2 z akcjami używający narzędzia Entity Framework** w środkowym okienku, a następnie kliknij przycisk  **Dodaj.**

    ![Wybiera kontroler 2 interfejsu API sieci Web z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Wybieranie sieci Web Kontroler interfejsu API 2 z akcjami i Entity Framework")

    *Wybór kontroler Web API 2 z akcjami i Entity Framework*
4. Ustaw *ApiPersonController* jako **nazwy kontrolera**, wybierz pozycję **używać asynchronicznych akcji kontrolera** opcję i zaznacz **osoby (MyHybridSite.Models)**  i **PersonContext (MyHybridSite.Models)** jako **modelu** i **kontekstu danych** odpowiednio klasy. Następnie kliknij przycisk **Dodaj**.

    ![Dodawanie kontrolera interfejsu API sieci Web z szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "dodawania kontrolera interfejsu API sieci Web z szkieletów")

    *Dodawanie kontrolera interfejsu API sieci Web z szkieletów*
5. Następnie program Visual Studio wygeneruje **ApiPersonController** klasy z czterech akcjami CRUD do pracy z danymi.

    ![Po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów")

    *Po utworzeniu kontrolera interfejsu API sieci Web za pomocą szkieletów*
6. Otwórz **ApiPersonController.cs** plików i sprawdzić *GetPeople* metody akcji. Ta metoda odpytuje db zakresie **PersonContext** typu w celu pobrania danych osoby.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Teraz można zauważyć komentarz powyżej definicją metody. Zawiera identyfikator URI, który udostępnia tę akcję, która zostanie użyta w następnym zadaniem.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Domyślnie skonfigurowano catch zapytań do interfejsu API sieci Web */api* ścieżkę, aby uniknąć kolizji z kontrolerów MVC. Jeśli musisz zmienić to ustawienie, można skorzystać z [routingu na platformie ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — rozwiązanie uruchomiona

W tym zadaniu korzystasz z programu Internet Explorer **narzędzi deweloperskich F12** do zbadania pełnego odpowiedzi z kontrolera interfejsu API sieci Web. Zobaczysz, jak można przechwytywać ruch sieciowy do pobrania uzyskać lepszy wgląd w dane aplikacji.

> [!NOTE]
> Upewnij się, że **programu Internet Explorer** wybrano **Start** przycisk znajdujący się na pasku narzędzi programu Visual Studio.
> 
> ![Opcji programu Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **Narzędzi deweloperskich F12** mają szeroki zestaw funkcji, które nie są ujęte w tym hands-na-laboratorium. Aby uzyskać więcej informacji na ten temat, zapoznaj się [korzystania z narzędzi deweloperskich F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.

    > [!NOTE]
    > Aby wykonać to zadanie poprawnie, aplikacja musi mieć danych. Jeśli baza danych jest pusta, można powrócić do 3 zadania w 2 wykonywania i wykonaj kroki dotyczące tworzenia nowej osoby za pomocą widoków MVC.
2. W przeglądarce, naciśnij klawisz **F12** otworzyć **Developer Tools** panelu. Naciśnij klawisz **CTRL** + **4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "przechwytywania ruchu sieciowego Inicjowanie interfejsu API sieci Web")

    *Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web*
3. Dołącz **interfejsu api/ApiPerson** do adresu URL na pasku adresu przeglądarki. Teraz będzie przeprowadzał inspekcję szczegóły odpowiedzi z **ApiPersonController**.

    ![Pobieranie danych osoby za pomocą interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "pobierania danych osoby za pomocą interfejsu API sieci Web")

    *Pobieranie danych osoby za pomocą interfejsu API sieci Web*

    > [!NOTE]
    > Po zakończeniu pobierania pojawi się monit dokonanie akcji z pobranego pliku. Pozostaw to okno dialogowe otwarte, aby można było oglądać zawartości odpowiedzi, za pomocą okna narzędzia dla deweloperów.
4. Teraz będzie przeprowadzał inspekcję treść odpowiedzi. Aby to zrobić, kliknij przycisk **szczegóły** a następnie kliknij pozycję **treść odpowiedzi**. Można sprawdzić, czy pobrane dane mają listę obiektów z właściwościami **identyfikator**, **nazwa** i **wieku** odpowiadające **osoby** Klasa.

    ![Treść odpowiedzi interfejsu API sieci Web wyświetlanie](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "przeglądania sieci Web treść odpowiedzi interfejsu API")

    *Treść odpowiedzi interfejsu API sieci Web wyświetlania*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Zadanie 3 — Dodawanie strony pomocy interfejsu API sieci Web

Utwórz interfejs API sieci Web, jest przydatne do tworzenia strony pomocy, aby inni deweloperzy będą wiedzieli, sposób wywołania interfejsu API. Należy utworzyć i ręcznie zaktualizować stron z dokumentacją, ale zaleca się automatycznego generowania je, aby uniknąć konieczności wykonywać zadania konserwacji. W tym zadaniu użyjesz pakiet Nuget do automatycznego generowania strony pomocy interfejsu API sieci Web do rozwiązania.

1. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
2. W **Konsola Menedżera pakietów** okna, uruchom następujące polecenie:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** pakiet instaluje konieczne zestawy i dodaje widoków MVC dla strony pomocy w obszarze **obszarów/HelpPage** folderu.

    ![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage obszaru")

    *Obszar HelpPage*
3. Domyślnie pomocy strony mają ciągów symbolu zastępczego dla dokumentacji. Komentarze dokumentacji XML służy do tworzenia w dokumentacji. Aby włączyć tę funkcję, otwórz **HelpPageConfig.cs** plik znajdujący się w **App-obszarów/HelpPage\_Start** folder i Usuń komentarz następujący wiersz:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz pozycję **właściwości** i kliknij przycisk **kompilacji** kartę.

    ![Tworzenie kartę](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "kompilacji sekcji")

    *Karta kompilacji*
5. W obszarze **dane wyjściowe**, wybierz pozycję **pliku dokumentacji XML**. W polu edycji wpisz **aplikacji\_Data/XmlDocument.xml**.

    ![Dane wyjściowe części kart Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output sekcji karcie kompilacji")

    *Sekcja danych wyjściowych w karcie kompilacji*
6. Naciśnij klawisz **CTRL** + **S** Aby zapisać zmiany.
7. Otwórz **ApiPersonController.cs** plik z **kontrolerów** folderu.
8. Wprowadź nowy wiersz między *GetPeople* podpis metody i */ / api/ApiPerson GET* komentarz, a następnie wpisz trzy kreskami ukośnymi.

    > [!NOTE]
    > Visual Studio automatycznie wstawia elementy XML, które definiują dokumentacji — metoda.
9. Dodawanie tekstu podsumowania i wartość zwracana *GetPeople* metody. Jego wygląd powinien być podobne do następujących.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
11. Dołącz **/help** do adresu URL na pasku adresu, aby przejść do strony pomocy.

    ![Strona pomocy interfejsu API sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "strona pomocy interfejsu API sieci Web ASP.NET")

    *Strona pomocy interfejsu API sieci Web ASP.NET*

    > [!NOTE]
    > Główne zawartość strony znajduje się tabela interfejsów API, które są pogrupowane według kontrolera. Wpisy tabeli są generowane dynamicznie, przy użyciu **IApiExplorer** interfejsu. Jeśli musisz dodać lub zaktualizować Kontroler interfejsu API, tabeli będą automatycznie aktualizowane przy następnym kompilowania aplikacji.
    > 
    > **Interfejsu API** kolumna zawiera metody HTTP oraz względnego identyfikatora URI. **Opis** kolumna zawiera informacje, który został wyodrębniony z dokumentacji metody.
12. Należy pamiętać, że opis dodanych powyżej definicja metody jest wyświetlany w kolumnie Opis.

    ![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "opis metody interfejsu API")

    *Opis metody interfejsu API*
13. Kliknij jedną z metod interfejsu API można przejść do strony z bardziej szczegółowe informacje, w tym próbki treści odpowiedzi.

    ![Strona informacji o szczegółów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "szczegółowe informacje o strony")

    *Szczegółowe informacje na stronie*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium praktycznego wiesz już, jak:

- Tworzenie nowej aplikacji sieci Web przy użyciu jednego środowiska ASP.NET w programie Visual Studio 2013
- Integracji wiele technologii ASP.NET z jednego pojedynczego projektu
- Generowanie widoków i kontrolerów MVC z klas modelu przy użyciu funkcji szkieletów ASP.NET
- Generowanie kontrolerów interfejsu API sieci Web, korzystających z funkcji, takich jak programowania asynchronicznego i dostęp do danych za pośrednictwem programu Entity Framework
- Automatycznego generowania strony pomocy interfejsu API sieci Web dla kontrolerów
