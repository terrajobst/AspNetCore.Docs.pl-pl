---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Warsztaty: One ASP.NET: integrowanie wzorca ASP.NET Web Forms, MVC i interfejs API sieci Web | Dokumentacja firmy Microsoft'
author: rick-anderson
description: ASP.NET to architektura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalne, np. MVC, interfejs API sieci Web i innych. Przy użyciu rozszerzeń ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: b6ac0dca92ab3d75eb871099882dcea549264354
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755056"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Warsztaty: One ASP.NET: integrowanie wzorca ASP.NET Web Forms, MVC i Web API
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET to architektura służąca do tworzenia witryn sieci Web, aplikacji i usług przy użyciu technologii specjalne, np. MVC, interfejs API sieci Web i innych. Z rozszerzeniem ASP.NET obserwowała, jak od jego utworzenia i zaakceptowania muszą być tych technologii zintegrowane, działania w kierunku miały miejsce ostatnie wysiłków **One ASP.NET**.
> 
> Visual Studio 2013 wprowadza nowy jednolity system, który umożliwia tworzenie aplikacji i korzystać z technologii ASP.NET w jednym projekcie. Ta funkcja eliminuje konieczność pobrania jedna technologia na początku projektu i hokejowego z nim, a zamiast tego zaleca się korzystanie z wielu platform ASP.NET, w ramach jednego projektu.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Tworzenie witryny sieci Web, na podstawie **One ASP.NET** typ projektu
- Użyj innych **ASP.NET** platform, na przykład **MVC** i **interfejsu API sieci Web** w tym samym projekcie
- Identyfikowanie główne składniki **ASP.NET** aplikacji
- Skorzystaj z zalet **funkcja tworzenia szkieletu ASP.NET** framework, automatyczne tworzenie widoków i kontrolerów w celu wykonywania operacji CRUD, oparte na klasach modeli
- Udostępnianie tego samego zestawu danych w formatach machine - i czytelny dla człowieka, za pomocą właściwych narzędzi dla każdego zadania

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.
3. Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Za pomocą fragmentów kodu

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania. Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Tworzenie nowego projektu formularzy sieci Web](#Exercise1)
2. [Tworzenie szkieletów kontrolera MVC](#Exercise2)
3. [Tworzenie szkieletów kontrolera interfejsu API sieci Web](#Exercise3)

Szacowany czas do ukończenia tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień. Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web

W tym ćwiczeniu utworzysz nową lokację formularzy sieci Web przy użyciu programu Visual Studio 2013 **One ASP.NET** ujednolicone środowisko projektu, który pozwoli łatwo zintegrować składniki formularzy sieci Web, MVC i interfejs API sieci Web w tej samej aplikacji. Będzie Poznaj rozwiązanie wygenerowany i zidentyfikować jej części, a na koniec zobaczysz witryny sieci Web w działaniu.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Zadanie 1 — Tworzenie nowej lokacji za pomocą jednego środowiska ASP.NET

W ramach tego zadania, użytkownik rozpocznie Tworzenie nowej witryny sieci Web w programie Visual Studio na podstawie **One ASP.NET** typ projektu. **One ASP.NET** łączy wszystkie technologie ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami. Następnie rozpoznają różnych składników Web Forms, MVC i interfejs API sieci Web, znajdujące się obok siebie w ramach aplikacji.

1. Otwórz **Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** kartę i upewnij się, **.NET Framework 4.5** jest zaznaczone. Nadaj projektowi nazwę *MyHybridSite*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **formularzy sieci Web** szablonu, a następnie wybierz **MVC** i **interfejsu API sieci Web** opcje. Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualne konta użytkowników**. Kliknij przycisk **OK** aby kontynuować.

    ![Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składników Web API i MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składników Web API i MVC*
4. Teraz możesz eksplorować strukturę wygenerowanego rozwiązania.

    ![Eksplorowanie wygenerowanej rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Eksplorowanie wygenerowanej rozwiązania*

    1. **Konto:** ten folder zawiera strony formularzy sieci Web do rejestracji, zaloguj się do i zarządzanie kontami użytkowników w aplikacji. Ten folder jest dodawany, gdy **indywidualne konta użytkowników** wybrano opcję uwierzytelniania podczas konfigurowania szablonu projektu formularzy sieci Web.
    2. **Modele:** ten folder zawiera klasy reprezentujące dane aplikacji.
    3. **Kontrolery** i **widoków**: te foldery są wymagane dla **platformy ASP.NET MVC** i **ASP.NET Web API** składników. Przedstawimy technologii MVC i interfejs API sieci Web w następnym ćwiczeniach.
    4. **Default.aspx**, **Contact.aspx** i **About.aspx** pliki są wstępnie zdefiniowane stron formularzy sieci Web, które służy jako punkt wyjścia do tworzenia stron, które są specyficzne dla użytkownika aplikacja. Logikę programistyczną tych plików znajduje się w oddzielnym pliku, nazywane &quot;związanym z kodem&quot; pliku, który ma &quot;. aspx.vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od język używany). Logika związanym z kodem działa na serwerze i dynamicznie generuje dane wyjściowe HTML dla strony.
    5. **Site.Master** i **Site.Mobile.Master** stron zdefiniować wygląd i działanie i standardowe zachowanie wszystkich stron w aplikacji.
5. Kliknij dwukrotnie **Default.aspx** plik, aby eksplorować zawartość strony.

    ![Eksplorowanie strony Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Eksplorowanie strony Default.aspx*

    > [!NOTE]
    > **Strony** dyrektywę w górnej części pliku definiuje atrybuty strony formularzy sieci Web. Na przykład **MasterPageFile** atrybut określa ścieżkę do poziomu głównego stronie — w tym przypadku *Site.Master* stronie — i **Inherits** definiuje atrybut osobna klasa kodu strony dziedziczenia. Ta klasa znajduje się w pliku ustalany na podstawie **CodeBehind** atrybutu.
    > 
    > **Asp: Content** kontroli rzeczywiste zawartością strony (tekst, znaczników i kontrolki) i jest mapowany na **asp: ContentPlaceHolder** kontrolki na stronie głównej. W tym przypadku będzie renderowana zawartość strony wewnątrz *MainContent* kontroli *Site.Master* strony.
6. Rozwiń **aplikacji\_Start** folderu i zwróć uwagę, **WebApiConfig.cs** pliku. Programu Visual Studio tego pliku są zawarte w wygenerowanym rozwiązaniu, ponieważ dołączył interfejsu API sieci Web podczas konfigurowania projektu z szablonu aplikacji One ASP.NET.
7. Otwórz **WebApiConfig.cs** pliku. W *WebApiConfig* klasy można znaleźć konfiguracji skojarzone z internetowego interfejsu API, który mapuje HTTP kieruje do **kontrolerów internetowych interfejsów API**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Otwórz **RouteConfig.cs** pliku. Wewnątrz *RegisterRoutes* metody, można znaleźć konfiguracji skojarzone z MVC, która mapuje trasy HTTP **kontrolerów MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — uruchamianie rozwiązania

W ramach tego zadania będzie uruchomić wygenerowanego rozwiązanie, zapoznaj się z aplikacji i część jej dostępnych funkcji, takich jak ponownego zapisywania adresów URL i wbudowanego uwierzytelniania za pomocą.

1. Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** znajdujący się na pasku narzędzi. Strona główna aplikacji powinna zostać otwarta w przeglądarce.

    ![Uruchamianie rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Sprawdź, czy strony formularzy sieci Web są wywoływane. Aby to zrobić, należy dołączyć **/contact.aspx** do adresu URL w pasku adresu i naciśnij klawisz **Enter**.

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Przyjazne adresy URL*

    > [!NOTE]
    > Jak widać, zmiana adresu URL do **/skontaktuj się z pomocą**. Począwszy od **platformy ASP.NET 4**możliwości routingu adresów URL zostały dodane do formularzy sieci Web, dzięki czemu można napisać adresy URL, takich jak *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* zamiast  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Aby uzyskać więcej informacji zobacz [routingu adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Może teraz zapoznać się przepływ uwierzytelniania w aplikacji. Aby to zrobić, kliknij przycisk **zarejestrować** w prawym górnym rogu strony.

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Rejestrowanie nowego użytkownika*
4. W **zarejestrować** wpisz **nazwa_użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Strona rejestracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Strona rejestracji*
5. Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniony.

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Użytkownik uwierzytelniony*
6. Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ćwiczenie 2: Tworzenie szkieletów kontrolera MVC

W tym ćwiczeniu będzie korzystać z platformę ASP.NET tworzenie szkieletów udostępniane przez program Visual Studio, aby utworzyć kontroler składnika ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania nawet jednego wiersza kodu. Proces tworzenia szkieletów użyje Entity Framework Code First generowanie kontekstu danych i schemat bazy danych w bazie danych SQL.

**O programu Entity Framework Code najpierw**

Entity Framework (EF) to maper obiektowo relacyjny (ORM), który pozwala na tworzenie aplikacji dostęp do danych przez programowania, korzystając z modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny.

Entity Framework Code First modelowania przepływu pracy pozwala na używanie własnych klas domeny do reprezentowania model, który EF opiera się na podczas wykonywania zapytania, funkcji śledzenia zmian i aktualizacji. Za pomocą Code First przepływ pracy tworzenia oprogramowania, nie trzeba zacząć aplikację przez utworzenie bazy danych lub określenie schematu. Zamiast tego można napisać standardowych klas platformy .NET, które określają najbardziej odpowiednie obiekty modelu domeny dla swojej aplikacji i Entity Framework spowoduje utworzenie bazy danych dla Ciebie.

> [!NOTE]
> Dowiedz się więcej na temat platformy Entity Framework [tutaj](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Zadanie 1 — Tworzenie nowego modelu

Teraz określi **osoby** klasy, która będzie model używany przez proces tworzenia szkieletu do tworzenia widoków i kontrolerów MVC. Rozpocznie się przez utworzenie **osoby** klasy modelu, a operacje CRUD na kontrolerze zostaną automatycznie utworzone przy użyciu funkcji tworzenia szkieletów.

1. Otwórz **Visual Studio Express 2013 for Web** i **MyHybridSite.sln** rozwiązanie znajduje się w **/Ex2-MvcScaffolding/początkowy w źródle** folderu. Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modeli** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Klasa...** .

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Dodawanie klasy modelu osoby*
3. W **Dodaj nowy element** okno dialogowe, nazwij plik *osoba.cs* i kliknij przycisk **Dodaj**.

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Tworzenie klasy modelu osoby*
4. Zastąp zawartość **osoba.cs** pliku następującym kodem. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

    (Code Snippet — *PersonClass BringingTogetherOneAspNet - Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MyHybridSite** projektu, a następnie wybierz **kompilacji**, lub naciśnij **CTRL + SHIFT + B** do skompilowania projektu.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Zadanie 2 — Tworzenie kontrolera MVC

Teraz, gdy **osoby** model został utworzony, ponieważ będziesz korzystać scaffolding ASP.NET MVC za pomocą platformy Entity Framework do tworzenia akcji kontrolera CRUD i widoków **osoby**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Nowy element szkieletowy...** .

    ![Tworzenie nowego szkieletu kontrolera](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Tworzenie nowego kontrolera szkielet*
2. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC 5 z widokami używający narzędzia Entity Framework** a następnie kliknij przycisk **Dodaj.**

    ![Wybieranie kontroler MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Wybieranie kontroler MVC 5 z widokami i Entity Framework*
3. Ustaw *MvcPersonController* jako **nazwy kontrolera**, wybierz opcję **używać asynchronicznych akcji kontrolera** opcji, a następnie wybierz pozycję **osoby (MyHybridSite.Models)**  jako **klasa modelu**.

    ![Dodawanie kontrolera MVC za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Dodawanie kontrolera MVC za pomocą tworzenia szkieletów*
4. W obszarze **klasa kontekstu danych**, kliknij przycisk **nowy kontekst danych...** .

    ![Tworząc nowy kontekst danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Tworząc nowy kontekst danych*
5. W **nowy kontekst danych** okno dialogowe, nazwa nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Tworzenie nowego typu PersonContext*
6. Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla **osoby** z tworzenia szkieletów. Program Visual Studio wygeneruje wtedy akcji kontrolera, kontekst danych osoby i widokami Razor.

    ![Po utworzeniu kontroler MVC z tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Po utworzeniu kontroler MVC z tworzenia szkieletów*
7. Otwórz **MvcPersonController.cs** w pliku **kontrolerów** folderu. Należy zauważyć, że metody akcji CRUD został wygenerowany automatycznie.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Wybierając **używać asynchronicznych akcji kontrolera** pole wyboru szkieletu opcje w poprzednich krokach, program Visual Studio generuje metod asynchronicznych akcji dla wszystkich akcji, które obejmują dostęp do kontekstu danych osoby. Zalecane jest, że używasz metod asynchronicznych akcji dla długotrwałych, innego niż procesor CPU powiązane żądania w celu unikania blokowania serwera sieci Web z wykonywania pracy, podczas przetwarzania żądania.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchamianie rozwiązania

W tym zadaniu uruchomisz rozwiązanie ponownie, aby sprawdzić, czy widoków dla **osoby** działają zgodnie z oczekiwaniami. Należy dodać nową osobę, aby sprawdzić, czy zostaje pomyślnie zapisane w bazie danych.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Przejdź do **/MvcPerson**. Widok szkieletu, który pokazuje listę osób, powinny być wyświetlane.
3. Kliknij przycisk **Utwórz nową** na dodawanie nowej osoby.

    ![Przejdź do szkieletu widoków MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Przejdź do szkieletu widoków MVC*
4. W **Utwórz** wyświetlanie, podaj **nazwa** i **wiek** osoby, a następnie kliknij przycisk **Utwórz**.

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Dodawanie nowej osoby*
5. Nowej osoby zostanie dodany do listy. Na liście elementów kliknij **szczegóły** do wyświetlenia widoku szczegółów osoby. Następnie w **szczegóły** wyświetlić, kliknij przycisk **powrót do listy** aby powrócić do widoku listy.

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Widok szczegółów osoby*
6. Kliknij przycisk **Usuń** łącze, aby usunąć osoby. W **Usuń** wyświetlić, kliknij przycisk **Usuń** o potwierdzenie operacji.

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Usuwanie osoby*
7. Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ćwiczenie 3: Tworzenie szkieletów kontrolera interfejsu API sieci Web

Strukturę interfejsu API sieci Web jest częścią stosu ASP.NET i mają ułatwić Implementowanie usług HTTP, zwykle wysyłania i odbierania danych w formacie JSON lub XML za pomocą interfejsu API RESTful.

W tym ćwiczeniu funkcja tworzenia szkieletu ASP.NET będzie użyć ponownie, aby wygenerować kontroler Web API. Będzie używać tego samego **osoby** i **PersonContext** klas z poprzednim ćwiczeniu, aby zapewnić te same dane osoby w formacie JSON. Zobaczysz, jak udostępnić te same zasoby na różne sposoby, w ramach tej samej aplikacji ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web

W tym zadaniu zostanie utworzony nowy **kontroler internetowego interfejsu API** , spowodują ujawnienie danych osobowych w formacie usprawnia maszyny, takich jak JSON.

1. Jeśli nie jest jeszcze otwarty, otwórz **Visual Studio Express 2013 for Web** , a następnie otwórz **MyHybridSite.sln** rozwiązanie znajduje się w **/Ex3-WebAPI/początkowy w źródle** folderu. Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.

    > [!NOTE]
    > W przypadku uruchomienia za pomocą rozwiązania Rozpocznij od 3 do wykonywania, naciśnij klawisz **CTRL + SHIFT + B** do skompilowania rozwiązania.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **MyHybridSite** projektu, a następnie wybierz **Dodaj | Nowy element szkieletowy...** .

    ![Tworzenie nowego szkieletu kontrolera](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Tworzenie nowego szkieletu kontrolera*
3. W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **interfejsu API sieci Web** w lewym okienku, a następnie **kontroler internetowego interfejsu API 2 z akcjami używający narzędzia Entity Framework** w środkowym okienku, a następnie kliknij przycisk  **Dodaj.**

    ![Wybieranie kontroler 2 internetowego interfejsu API z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "wybierając kontroler internetowego interfejsu API 2 z akcjami i Entity Framework")

    *Wybieranie kontroler internetowego interfejsu API 2 z akcjami i Entity Framework*
4. Ustaw *ApiPersonController* jako **nazwy kontrolera**, wybierz opcję **używać asynchronicznych akcji kontrolera** opcji, a następnie wybierz pozycję **osoby (MyHybridSite.Models)**  i **PersonContext (MyHybridSite.Models)** jako **modelu** i **kontekstu danych** klasy odpowiednio. Następnie kliknij przycisk **Dodaj**.

    ![Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów")

    *Dodawanie kontrolera interfejsu API sieci Web za pomocą tworzenia szkieletów*
5. Program Visual Studio wygeneruje wtedy **ApiPersonController** klasie z atrybutem cztery akcje CRUD do pracy z danymi.

    ![Po utworzeniu kontroler Web API za pomocą tworzenia szkieletów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po utworzeniu kontroler Web API za pomocą tworzenia szkieletów")

    *Po utworzeniu kontroler Web API za pomocą tworzenia szkieletów*
6. Otwórz **ApiPersonController.cs** plik i sprawdzić *GetPeople* metody akcji. Ta metoda wysyła zapytanie pola db **PersonContext** typu, aby pobrać dane osoby.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Teraz zauważyć komentarz powyżej definicję metody. Zawiera identyfikator URI, który udostępnia tę akcję, który będzie używany w ramach następnego zadania.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Domyślnie interfejs API sieci Web jest skonfigurowany do zapytań, aby przechwycić */interfejs API* ścieżki w celu uniknięcia kolizji z kontrolerów MVC. Jeśli potrzebujesz zmienić to ustawienie, zapoznaj się [routingu ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — uruchamianie rozwiązania

W ramach tego zadania będzie używać programu Internet Explorer **narzędzi deweloperskich F12** Aby sprawdzić pełną odpowiedź z kontrolera interfejsu API sieci Web. Zobaczysz, jak przechwytywać ruch sieciowy, aby uzyskać lepszy wgląd w dane aplikacji.

> [!NOTE]
> Upewnij się, że **programu Internet Explorer** wybrano **Start** znajdujący się na pasku narzędzi programu Visual Studio.
> 
> ![Opcja program Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **Narzędzi deweloperskich F12** mają szeroki zestaw funkcji, która nie została uwzględniona w tym hands-na-laboratorium. Aby dowiedzieć się więcej na ten temat, zapoznaj się [przy użyciu narzędzi deweloperskich F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.

    > [!NOTE]
    > Aby można było wykonać to zadanie prawidłowo, aplikacja musi mieć danych. Jeśli baza danych jest pusta, można wrócić do zadanie 3 w wersji 2 wykonywania i postępuj zgodnie z instrukcjami na temat tworzenia nowej osoby za pomocą widoków MVC.
2. W przeglądarce naciśnij **F12** otworzyć **narzędzi deweloperskich** panelu. Naciśnij klawisz **CTRL** + **4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie interfejsu API sieci Web, przechwytywanie sieci](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "przechwytywania sieci Inicjowanie interfejsu API sieci Web")

    *Inicjuje Przechwytywanie sieci interfejsu API sieci Web*
3. Dołącz **interfejsu api/ApiPerson** do adresu URL w pasku adresu przeglądarki. Teraz Sprawdź szczegóły odpowiedzi z **ApiPersonController**.

    ![Trwa pobieranie danych osobowych za pomocą interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "podczas pobierania danych osobowych za pomocą interfejsu API sieci Web")

    *Trwa pobieranie danych osobowych za pomocą interfejsu API sieci Web*

    > [!NOTE]
    > Po zakończeniu pobierania, pojawi się akcję przy użyciu pobranego pliku. Pozostaw to okno dialogowe otwarte, aby móc obejrzeć zawartość odpowiedzi za pomocą okna narzędzia dla deweloperów.
4. Teraz Sprawdź treść odpowiedzi. Aby to zrobić, kliknij przycisk **szczegóły** kartę, a następnie kliknij przycisk **treść odpowiedzi**. Możesz sprawdzić, czy pobrane dane znajduje się lista obiektów z właściwościami **identyfikator**, **nazwa** i **wiek** odpowiadające **osoby** Klasa.

    ![Przeglądanie sieci Web w treści odpowiedzi interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "przeglądania sieci Web w treści odpowiedzi interfejsu API")

    *Treść odpowiedzi interfejsu API sieci Web wyświetlania*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Zadanie 3 — Dodawanie stron pomocy interfejsu API sieci Web

Podczas tworzenia internetowego interfejsu API, warto utworzyć stronę pomocy, aby inni deweloperzy powinni wiedzieć, jak wywołać interfejs API. Należy utworzyć i ręcznie zaktualizuj strony dokumentacji, ale zaleca się automatyczne generowanie je, aby uniknąć konieczności wykonaj prac konserwacyjnych. W tym zadaniu użyjesz pakietu Nuget do automatycznego generowania strony pomocy internetowego interfejsu API do rozwiązania.

1. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**.
2. W **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** pakiet instaluje konieczne zestawy i dodaje widoków MVC dla stron pomocy, w obszarze **obszarów/HelpPage** folderu.

    ![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage obszaru")

    *Obszar HelpPage*
3. Domyślnie pomocy strony zawierają symbol zastępczy parametrów dla dokumentacji. Komentarze dokumentacji XML umożliwia tworzenie w dokumentacji. Aby włączyć tę funkcję, otwórz **HelpPageConfig.cs** plik znajdujący się w **obszarów/HelpPage/aplikacji\_Start** folder i usuń znaczniki komentarza następujący wiersz:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz opcję **właściwości** i kliknij przycisk **kompilacji** kartę.

    ![Tworzenie karty](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Tworzenie sekcji")

    *Tworzenie karty*
5. W obszarze **dane wyjściowe**, wybierz opcję **pliku dokumentacji XML**. Wpisz w polu edycji **aplikacji\_Data/XmlDocument.xml**.

    ![Dane wyjściowe sekcji na karcie kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "danych wyjściowych sekcji na karcie kompilacja")

    *Sekcja danych wyjściowych na karcie kompilacja*
6. Naciśnij klawisz **CTRL** + **S** Aby zapisać zmiany.
7. Otwórz **ApiPersonController.cs** plik wchodzącej w skład **kontrolerów** folderu.
8. Wprowadź nowy wiersz między *GetPeople* podpis metody i */ / GET interfejsu api/ApiPerson* komentarz, a następnie wpisz trzy ukośników.

    > [!NOTE]
    > Program Visual Studio automatycznie wstawia elementy XML, które definiują w dokumentacji metody.
9. Dodawanie tekstu podsumowania i wartość zwracana dla *GetPeople* metody. Powinno to wyglądać następująco.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
11. Dołącz **/help** do adresu URL w pasku adresu, aby przejść do strony pomocy.

    ![Strona pomocy interfejsu API sieci Web platformy ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "strona pomocy interfejsu API sieci Web platformy ASP.NET")

    *Strona pomocy interfejsu API sieci Web platformy ASP.NET*

    > [!NOTE]
    > Główną zawartością strony znajduje się tabela interfejsów API, pogrupowane według kontrolera. Wpisy tabeli są generowane dynamicznie przy użyciu **IApiExplorer** interfejsu. Jeśli dodasz lub zaktualizować Kontroler interfejsu API tabeli zostaną automatycznie zaktualizowane przy następnej kompilacji aplikacji.
    > 
    > **API** kolumna zawiera metodę HTTP oraz względnego identyfikatora URI. **Opis** kolumna zawierała informacje, który został wyodrębniony z dokumentacji metody.
12. Należy zauważyć, że opis, który dodaje powyżej definicję metody jest wyświetlana w kolumnie Opis.

    ![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "opis metody interfejsu API")

    *Opis metody interfejsu API*
13. Kliknij jeden z metody interfejsu API, aby przejść do strony z bardziej szczegółowe informacje, w tym przykładowe ciała odpowiedzi.

    ![Strona informacji o szczegółów](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "szczegółowe informacje o stronie")

    *Szczegółowe informacje na stronie*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktycznego wiesz już, jak:

- Utwórz nową aplikację sieci Web za pomocą jednego środowiska ASP.NET w programie Visual Studio 2013
- Integrowanie wiele technologii ASP.NET z jednego pojedynczego projektu
- Generowanie widoków i kontrolerów MVC z klas modelu za pomocą programu ASP.NET tworzenie szkieletów
- Generowanie kontrolerów internetowych interfejsów API, które używają funkcji, takich jak programowanie Async oraz dostęp do danych za pomocą platformy Entity Framework
- Automatyczne generowanie stron pomocy interfejsu API sieci Web dla kontrolerów
