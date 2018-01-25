---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Tworzenie podstawowych programu ASP.NET 4.5 formularzy sieci Web strony w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 6b699cc939292b7ab0167dba7cfa6a00b681ef3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Tworzenie podstawowych programu ASP.NET 4.5 formularzy sieci Web strony w programie Visual Studio 2013
====================
Przez [Erik Reitan](https://github.com/Erikre)

Ten przewodnik zawiera wprowadzenie do środowiska projektowego sieci Web w [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) i [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Ten przewodnik prowadzi użytkownika przez proces tworzenia prostych strony formularzy sieci Web ASP.NET i przedstawiono podstawowe techniki tworzenia nowej strony, dodawanie formantów i pisanie kodu.

Zadania przedstawione w tym przewodniku obejmują:

- Tworzenie nowego projektu aplikacji formularzy sieci Web systemu plików.
- Zapoznawanie się z programem Visual Studio.
- Tworzenie stron ASP.NET.
- Dodawanie formantów.
- Dodawanie programów obsługi zdarzeń.
- Uruchomiona i testowania strony z programu Visual Studio.

## <a name="prerequisites"></a>Wymagania wstępne


W celu przeprowadzenia tego instruktażu potrzebne są:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 i programu Microsoft Visual Studio Express 2013 for Web będzie często określane jako Visual Studio w tej serii samouczka.  
    >   
    > Jeśli używasz programu Visual Studio w tym przewodniku przyjęto założenie, że wybrano **projektowanie witryn sieci Web** kolekcję ustawień przy pierwszym uruchomieniu programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybierz ustawienia środowiska sieci Web Development](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Tworzenie projektu aplikacji sieci Web i strony

<a id="sectionToggle0"></a>

W tej części przewodnika utworzysz projekt aplikacji sieci Web i Dodaj nową stronę do niego. Zostanie również dodać tekstu w formacie HTML i uruchomić strony w przeglądarce.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz program Microsoft Visual Studio.
2. Na **pliku** menu, wybierz opcję **nowy projekt**.  
    ![Menu Plik](creating-a-basic-web-forms-page/_static/image1.png)

    **Nowy projekt** zostanie wyświetlone okno dialogowe.
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.
5. Nazwij swój projekt ***BasicWebApp*** i kliknij przycisk **OK** przycisku.   
![Okno dialogowe Nowy projekt](creating-a-basic-web-forms-page/_static/image2.png)
6. Następnie wybierz pozycję **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisk, aby utworzyć projekt.  
![Okno dialogowe Nowy projekt ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera wbudowane funkcje, oparty na szablonie formularzy sieci Web. Nie tylko umożliwia on *Home.aspx* strony, *About.aspx* strony, *Contact.aspx* strony, ale także funkcje członkostwo, które rejestruje użytkowników i zapisuje swoje poświadczenia, aby ich zalogować się do witryny sieci Web. Po utworzeniu nowej strony, domyślnie Visual Studio zostanie wyświetlona strona w **źródła** widoku umożliwia wyświetlenie strony elementów HTML. Na poniższej ilustracji przedstawiono zobaczysz w **źródła** wyświetlić, jeśli utworzono nową stronę sieci Web o nazwie *BasicWebApp.aspx*.  
    ![Wyświetl źródło](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Samouczek środowiska Visual Studio Web Development


Przed kontynuowaniem, modyfikując strony jest warto zapoznać się z środowisko projektowe Visual Studio. Na poniższej ilustracji przedstawiono systemu windows i narzędzia, które są dostępne w programie Visual Studio i Visual Studio Express for Web.

> [!NOTE] 
> 
> Ten diagram przedstawia domyślne systemu windows i lokalizacje okna. **Widoku** menu pozwala na wyświetlanie dodatkowych okien i rozmieszczanie, a następnie zmień rozmiar windows zgodnie z preferencjami użytkownika. Układ okna już wprowadzono zmiany, które są widoczne nie będzie odpowiadała ilustracji.


 Środowiska Visual Studio

![Środowiska Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Zapoznaj się z Projektant stron sieci Web

Sprawdź ilustracji powyżej i odpowiadać tekstowi na poniższej liście, która opisuje najczęściej używane narzędzia i systemu windows. (Nie wszystkie okna i narzędzi, które są wyświetlane na liście, tylko te oznaczona na powyższej ilustracji.)

- Paski narzędzi. Podaj poleceń na potrzeby formatowania tekstu, wyszukiwanie tekstu i tak dalej. Niektóre paski narzędzi są dostępne tylko wtedy, gdy użytkownik pracuje w **projekt** widoku.
- **Eksplorator rozwiązań** okna. Wyświetla pliki i foldery w aplikacji sieci Web.
- Okno dokumentu. Umożliwia wyświetlanie dokumentów, w którym pracuje w systemie windows z kartami. Można przełączać się między dokumenty, klikając karty.
- **Właściwości** okna. Umożliwia zmianę ustawień strony, elementów HTML, formanty i inne obiekty.
- Wyświetlanie kart. Wyświetlenie różne widoki tego samego dokumentu. **Projekt** widok jest niemal WYSIWYG powierzchni edycji. **Źródło** widoku to edytor HTML strony. **Podziel** zostaną wyświetlone zarówno **projekt** widoku i **źródła** widok dokumentu. Będzie działać z **projekt** i **źródła** widoków w dalszej części tego przewodnika. Jeśli wolisz otwieranie stron internetowych w **projekt** wyświetlić na **narzędzia** menu, kliknij przycisk **opcje**, wybierz pozycję **projektanta HTML** węzeł, a następnie zmień **Rozpocznij stron w** opcji.
- **Przybornik**. Zawiera formanty i elementów HTML, które można przeciągnięcie do strony. **Przybornik** elementy są pogrupowane według wspólnych funkcji.
- S **erwera Explorer**. Wyświetla połączenia z bazą danych. Jeśli Eksplorator serwera nie jest widoczna, w menu Widok, kliknij przycisk Eksploratora serwera.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web ASP.NET


Podczas tworzenia nowej aplikacji formularzy sieci Web, przy użyciu **aplikacji sieci Web ASP.NET** szablon projektu Visual Studio dodaje strony ASP.NET (strony formularzy sieci Web), o nazwie *Default.aspx*oraz jak kilka innych plików i foldery. Można użyć *Default.aspx* strony jako stronę główną dla aplikacji sieci Web. Jednak w ramach tego przewodnika, utworzysz i pracować z nowej strony.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web


1. Zamknij *Default.aspx* strony. Aby to zrobić, kliknij kartę, która zawiera nazwę pliku, a następnie kliknij przycisk Zamknij opcji.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku, nazwa aplikacji jest **BasicWebSite**), a następnie kliknij przycisk **Dodaj**  - &gt; **Nowy element**.   
**Dodaj nowy element** zostanie wyświetlone okno dialogowe.
3. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz opcję **formularza sieci Web** ze środka listy i nadaj mu nazwę *FirstWebPage.aspx*.   
    ![Dodaj nowy element — okno dialogowe](creating-a-basic-web-forms-page/_static/image6.png)
4. Kliknij przycisk **Dodaj** do dodania do projektu strony sieci web.  
Visual Studio utworzy nową stronę i otwarcie go.


### <a name="adding-html-to-the-page"></a>Dodawanie do strony HTML


W tej części tego przewodnika, zostaną dodane do strony tekst statyczny.

### <a name="to-add-text-to-the-page"></a>Aby dodać tekst do strony


1. W dolnej części okna dokumentu, kliknij przycisk **projekt** kartę, aby przełączyć się do **projekt** widoku.

    Widok projektu wyświetla bieżącą stronę w sposób podobny WYSIWYG. W tym momencie nie masz dowolny tekst lub kontrolki na stronie, strona jest puste, z wyjątkiem linia przerywana, który zawiera prostokąta. Reprezentuje prostokąta **div** elementu na stronie.
2. Kliknij przycisk wnętrzu opisanym przez linię kropkowaną.
3. Typ **Zapraszamy Visual Web Developer** i naciśnij klawisz **ENTER** dwa razy.

    Na poniższej ilustracji przedstawiono tekstu w **projekt** widoku.

    ![Witamy tekst w widoku Projekt](creating-a-basic-web-forms-page/_static/image7.png "Witamy tekst w widoku Projekt")
4. Przełącz się do **źródła** widoku.

    Zostanie wyświetlony kod HTML w **źródła** widoku, który został utworzony, gdy zostało wpisane w **projekt** widoku.  
    ![Strony sieci Web z tekst statyczny](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Uruchomienie strony


Przed kontynuowaniem przez dodawanie formantów do strony, musisz go najpierw uruchomić.

### <a name="to-run-the-page"></a>Do uruchomienia strony


1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *FirstWebPage.aspx* i wybierz **Ustaw jako stronę startową**.
2. Naciśnij klawisz **CTRL + F5** do uruchomienia strony.

    Ta strona jest wyświetlana w przeglądarce. Mimo że strona utworzony ma rozszerzenie nazwy pliku *.aspx*, obecnie działa jak dowolnej strony HTML.

    Aby wyświetlić stronę w przeglądarce możesz można również kliknij prawym przyciskiem myszy strony w **Eksploratora rozwiązań** i wybierz **Wyświetl w przeglądarce**.
3. Zamknij przeglądarkę, aby zatrzymać aplikacji sieci Web.


## <a name="adding-and-programming-controls"></a>Dodawanie i Programowanie formantów


<a id="sectionToggle1"></a>

Na stronie zostanie teraz Dodaj formanty serwera. Formanty serwera, takie jak przyciski, etykiet pól tekstowych i innych znanych formantów zapewniają typowych możliwości przetwarzania formularza dla stron formularzy sieci Web. Można jednak program formantów kod uruchamiany na serwerze, a nie przez klienta.

Spowoduje dodanie [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli, [pole tekstowe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli i [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) do strony oraz napisz kod umożliwiający obsługę [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzenia dla [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) formantu.

### <a name="to-add-controls-to-the-page"></a>Aby dodać kontrolki do strony


1. Kliknij przycisk **projekt** kartę, aby przełączyć się do **projekt** widoku.
2. Umieść punkt wstawiania na końcu **Zapraszamy Visual Web Developer** tekstu i naciśnij klawisz **ENTER** pięć lub więcej razy, aby pomieścić w **div** pola elementu.
3. W **przybornika**, rozwiń węzeł **standardowe** grupy, jeśli nie jest już rozwinięte.  
Należy pamiętać, że trzeba rozwinąć **przybornika** oknie po lewej stronie, aby go wyświetlić.
4. Przeciągnij [pole tekstowe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontrolować na stronę i upuść go w środku elementu **div** pola elementu, który ma **Zapraszamy Visual Web Developer** w pierwszym wierszu.
5. Przeciągnij [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolować na stronę i upuść ją na prawo od [pole tekstowe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) formantu.
6. Przeciągnij [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontrolować na stronę i upuść go w osobnym wierszu poniżej [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) formantu.
7. Umieść punkt wstawiania powyżej [pole tekstowe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli, a następnie wpisz **wprowadź nazwę:** .

    Ten statyczny tekst HTML jest podpis [pole tekstowe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) formantu. Można mieszać Statycznych i kontrolki serwera na tej samej stronie. Na poniższej ilustracji przedstawiono sposób wyświetlania trzy formanty w **projekt** widoku.

    ![Trzy formanty w widoku Projekt](creating-a-basic-web-forms-page/_static/image9.png "trzy formanty w widoku Projekt")


### <a name="setting-control-properties"></a>Ustawianie właściwości formantu


Program Visual Studio oferuje różne sposoby ustawiania właściwości formantów na stronie. W tej części przewodnika będą ustawić właściwości zarówno **projekt** widoku i **źródła** widoku.

### <a name="to-set-control-properties"></a>Aby ustawić właściwości formantu


1. Najpierw należy wyświetlić **właściwości** systemu windows, wybierając z **widoku** menu -&gt; **inne okna**  - &gt; **Okna properies**. Można też wybrać **F4** do wyświetlenia **właściwości** okna.
2. Wybierz [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sterowania, a następnie w **właściwości** okna, ustaw wartość **tekst** do **Nazwa wyświetlana**. Wprowadzony tekst jest wyświetlany przycisk w projektancie, jak pokazano na poniższej ilustracji.

    ![Ustawianie tekstu przycisku](creating-a-basic-web-forms-page/_static/image10.png "przycisk Ustaw tekst")
3. Przełącz się do **źródła** widoku.

    **Źródło** widoku są wyświetlane HTML strony, w tym elementy utworzone dla kontrolki serwera Visual Studio. Formanty zadeklarowane za pomocą składni notacji języka HTML, z wyjątkiem tego, czy znaczniki Użyj prefiksu **asp:** i Uwzględnij atrybut **runat =&quot;serwera&quot;**.

    Właściwości formantów są deklarowane jako atrybuty. Na przykład po ustawieniu [tekst](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) właściwość [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolować, w kroku 1 zostały faktycznie ustawienie **tekst** atrybutów w znaczniku formantu.

    > [!NOTE] 
    > 
    > Wszystkie opcje znajdują się wewnątrz **formularza** element, który również ma atrybut **runat =&quot;serwera&quot;**. **Runat =&quot;serwera&quot;**  atrybutu i **asp:** prefiksu do tagów formantów oznaczanie kontrolek tak, aby są przetwarzane przez program ASP.NET na serwerze po uruchomieniu na stronie. Kod poza  **&lt;tworzą runat =&quot;serwera&quot; &gt;**  i  **&lt;skryptu runat =&quot;serwera&quot; &gt;**  elementy są wysyłane bez zmian w przeglądarce, dlatego kodu platformy ASP.NET musi znajdować się wewnątrz elementu, którego otwierający tag zawiera **runat =&quot;serwera&quot;**  atrybutu.
4. Następnie można dodać dodatkowych właściwości do [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu. Umieść punkt wstawiania bezpośrednio po **asp: Label** w  **&lt;asp: Label&gt;**  tag, a następnie naciśnij klawisz **spacja**.

    Listy rozwijanej zostanie wyświetlone na liście dostępnych właściwości można ustawić dla [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu. Ta funkcja określana jako **IntelliSense**, pomaga w **źródła** widoku przy użyciu składni kontrolki serwera, elementów HTML i innych elementów na stronie. Na poniższej ilustracji pokazano **IntelliSense** listy rozwijanej dla [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu.

    ![Atrybuty IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "atrybuty IntelliSense")
5. Wybierz **ForeColor** , a następnie wpisz znak równości.

    Funkcja IntelliSense wyświetla listę kolorów.

    > [!NOTE] 
    > 
    > Można wyświetlić **IntelliSense** listy rozwijanej w dowolnym momencie przez naciśnięcie przycisku **CTRL + J** podczas przeglądania kodu.
6. Wybierz kolor  **[etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  tekstu formantu. Upewnij się, że można wybrać kolor, który jest ciemny, aby przeczytać przed białe tło.

    **ForeColor** atrybutu zostało zakończone z kolor, który wybrano, włączając zamykającego znaku cudzysłowu.


### <a name="programming-the-button-control"></a>Kontrolka przycisku programowania


W ramach tego przewodnika będą pisania kodu, który odczytuje nazwę, że użytkownik wchodzi w polu tekstowym, a następnie wyświetla nazwę w [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu.

### <a name="add-a-default-button-event-handler"></a>Dodaj domyślny program obsługi zdarzeń przycisku


1. Przełącz się do **projekt** widoku.
2. Kliknij dwukrotnie [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) formantu.

    Domyślnie program Visual Studio przełącza się do pliku CodeBehind i tworzy program obsługi zdarzeń szkielet dla [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) formantu domyślne zdarzenie, [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzeń. Plik CodeBehind oddziela Twoje znaczników interfejsu użytkownika (na przykład HTML) w kodzie serwera (takich jak C#).   
Kursor znajduje się dodawać kod dla tego programu obsługi zdarzeń.

    > [!NOTE] 
    > 
    > Dwukrotne kliknięcie formantu w **projekt** widok jest tylko jeden z kilku sposobów można utworzyć procedury obsługi zdarzeń.
3. Wewnątrz **Button1\_kliknij** obsługi zdarzeń, typ **Label1** następuje okres (**.**).

    Podczas wpisywania okres po **identyfikator** etykiety (**Label1**), programu Visual Studio zostanie wyświetlona lista dostępnych elementów członkowskich [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli, jak pokazano w następującym Ilustracja. Zwykle właściwość elementu członkowskiego, metody lub zdarzenia.

    ![IntelliSense w widoku kodu](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense w widoku kodu")
4. Zakończ **kliknij** programu obsługi zdarzeń dla przycisku, tak że odczytuje, jak pokazano w poniższym przykładzie kodu.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Wrócić do wyświetlania **źródła** widoku z kod znaczników HTML, klikając prawym przyciskiem myszy *FirstWebPage.aspx* w **Eksploratora rozwiązań** i wybierając **widoku Kod znaczników**.
6. Przewiń do  **&lt;asp: Button&gt;**  elementu. Należy pamiętać, że  **&lt;asp: Button&gt;**  element ma teraz atrybut **onclick =&quot;Button1\_kliknij&quot;**.

    Ten atrybut jest powiązany przycisku [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzeń do metody obsługi kodowanych w poprzednim kroku.

    Metody obsługi zdarzeń może mieć dowolną nazwę; Nazwa widocznej jest domyślna nazwa utworzony przez program Visual Studio. Istotne jest, że nazwa używana dla **OnClick** atrybutu w kodzie HTML musi odpowiadać nazwie metody zdefiniowane w CodeBehind.


### <a name="running-the-page"></a>Uruchomienie strony


Teraz możesz przetestować kontrolki serwera na stronie.

### <a name="to-run-the-page"></a>Do uruchomienia strony


1. Naciśnij klawisz **CTRL + F5** do uruchomienia strony w przeglądarce. Jeśli wystąpi błąd, sprawdź ponownie powyższe kroki.
2. Wprowadź nazwę w polu tekstowym i kliknij przycisk **Nazwa wyświetlana** przycisku.

    Wprowadzona nazwa jest wyświetlana w [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu. Należy pamiętać, że po kliknięciu przycisku Strona jest przesyłana do serwera sieci Web. ASP.NET następnie tworzy ponownie strony, uruchamia kod (w takim przypadku [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) formantu [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) uruchamia program obsługi zdarzeń), a następnie wysyła nowej strony do przeglądarki. Obejrzyj paska stanu w przeglądarce, widać czy strona jest wprowadzenie obiegu do serwera sieci Web każdorazowo po kliknięciu przycisku.
3. W przeglądarce, Wyświetl źródło strony są uruchomione, klikając prawym przyciskiem myszy na stronie i wybierając **Wyświetl źródło**.

    W kodzie źródłowym strony widać HTML bez żadnego kodu serwera. W szczególności nie ma  **&lt;asp:&gt;**  elementów, które pracowano w **źródła** widoku. Po uruchomieniu strony ASP.NET przetwarza kontrolki serwera i renderowania elementów HTML do strony, które wykonują funkcje, które reprezentują formantu. Na przykład  **&lt;asp: Button&gt;**  renderowania formantu jako HTML  **&lt;wprowadzania type =&quot;przesłać&quot; &gt;**  element.
4. Zamknij przeglądarkę.


## <a name="working-with-additional-controls"></a>Praca z dodatkowych funkcji kontroli

<a id="sectionToggle2"></a>

W tej części przewodnika będzie działać z [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formantu, który wyświetla dat w miesiącu w czasie. [Kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli jest formantem bardziej złożone niż przycisk, pole tekstowe i etykiety, pracy w z i przedstawiono pewne dodatkowe możliwości formantów serwera.

W tej sekcji dodasz [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) do strony i sformatuj go.

### <a name="to-add-a-calendar-control"></a>Aby dodać kontrolkę kalendarza


1. W programie Visual Studio, przełącz się do **projekt** widoku.
2. Z **standardowe** sekcji **przybornika**, przeciągnij [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontrolować na stronę i upuść ją poniżej **div** elementu który zawiera inne formanty.

    Panel tagów inteligentnych kalendarza jest wyświetlany. Panel wyświetla polecenia, które ułatwiają wykonywanie typowych zadań dla zaznaczonego formantu. Na poniższej ilustracji pokazano [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli w postaci wyświetlanej w **projekt** widoku.

    ![Formantu w widoku Projekt kalendarza](creating-a-basic-web-forms-page/_static/image13.png "formantu w widoku Projekt kalendarza")
3. W panelu tagi inteligentne, wybierz **automatyczne formatowanie**.

    **Automatyczne formatowanie** zostanie wyświetlone okno dialogowe, które służy do wybierania schematu formatowania kalendarza. Na poniższej ilustracji pokazano **automatyczne formatowanie** okno dialogowe [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formantu.

    ![Okno dialogowe Format Auto (formant kalendarza)](creating-a-basic-web-forms-page/_static/image14.png "automatyczne formatowanie, okno dialogowe (formant kalendarza)")
4. Z **wybierz schemat** wybierz **proste** , a następnie kliknij przycisk **OK**.
5. Przełącz się do **źródła** widoku.

    Widać  **&lt;asp: kalendarza&gt;**  elementu. Ten element jest znacznie dłużej niż elementy prostych formantów utworzony wcześniej. Zawiera także podelementów, takich jak  **&lt;WeekEndDayStyle&gt;**, które reprezentują różne ustawienia formatowania. Na poniższej ilustracji pokazano [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli w **źródła** widoku. (Dokładny kod znaczników, który pojawi się w **źródła** widok może różnić się nieznacznie od ilustracji.)

    ![Formantu w widoku źródła kalendarza](creating-a-basic-web-forms-page/_static/image15.png "formantu w widoku źródła kalendarza")


### <a name="programming-the-calendar-control"></a>Programowanie formant kalendarza


W tej sekcji zostanie program [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formantu, aby wyświetlić aktualnie zaznaczona data.

### <a name="to-program-the-calendar-control"></a>Aby program formant kalendarza


1. W **projekt** wyświetlić, kliknij dwukrotnie [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formantu.

    Nowy program obsługi zdarzeń jest tworzone i wyświetlane w pliku związanym z kodem o nazwie *FirstWebPage.aspx.cs*.
2. Zakończ [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) obsługi zdarzeń z następującym kodem.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 Powyższy kod ustawia tekst formantu etykiety wybranego dnia formantu kalendarza.


### <a name="running-the-page"></a>Uruchomienie strony


Teraz możesz przetestować kalendarzem.

### <a name="to-run-the-page"></a>Do uruchomienia strony


1. Naciśnij klawisz **CTRL + F5** do uruchomienia strony w przeglądarce.
2. Kliknij datę w kalendarzu.

    Data została kliknięta jest wyświetlana w [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) formantu.
3. W przeglądarce wyświetlić kodu źródłowego dla strony.

    Należy pamiętać, że [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) wyrenderowaniu formantu do strony jako **tabeli**, przy każdym dniu **td** elementu.
4. Zamknij przeglądarkę.


## <a name="next-steps"></a>Następne kroki


<a id="nextStepsToggle"></a>

W tym przewodniku ma przedstawiono podstawowe funkcje programu Visual Studio Projektant strony. Teraz, gdy wiesz, jak tworzyć i edytować strony formularzy sieci Web w programie Visual Studio, można eksplorować innych funkcji. Można na przykład, wykonaj następujące czynności:

- Dowiedz się więcej o formularzy sieci Web ASP.NET, wykonując krok samouczka serii [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Dowiedz się więcej na temat usuwania kaskadowego arkuszy stylów (CSS). Aby uzyskać więcej informacji, zobacz [Praca z CSS omówienie](https://msdn.microsoft.com/library/bb398931.aspx).
