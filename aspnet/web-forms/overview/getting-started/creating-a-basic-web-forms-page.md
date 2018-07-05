---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Tworzenie podstawowych aplikacji ASP.NET 4.5 Web Forms strony w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: c2051b166b8800654a107c5100ad5ed94af93407
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394456"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Tworzenie podstawowych aplikacji ASP.NET 4.5 Web Forms strony w programie Visual Studio 2013
====================
przez [Erik Reitan](https://github.com/Erikre)

Ten przewodnik zawiera wprowadzenie do środowiska deweloperskiego w sieci Web w [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) i [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Ten przewodnik przeprowadzi Cię przez tworzenie prostego strony ASP.NET Web Forms i przedstawia podstawowe techniki tworzenia nowej strony, dodawanie kontrolek i napisanie kodu.

Zadania zilustrowane w tym przewodniku obejmują:

- Tworzenie projektu aplikacji formularzy sieci Web systemu plików.
- Zapoznawanie się z programem Visual Studio.
- Tworzenie strony ASP.NET.
- Dodawanie formantów.
- Dodawanie obsługi zdarzeń.
- Uruchamianie i testowanie strony z programu Visual Studio.

## <a name="prerequisites"></a>Wymagania wstępne


Aby ukończyć ten przewodnik, potrzebne są:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 i Microsoft Visual Studio Express 2013 for Web będzie często określane jako programu Visual Studio w całej tej serii samouczków.  
    >   
    > Jeśli używasz programu Visual Studio, tym przewodniku przyjęto założenie, że wybrana **programowania dla sieci Web** zbiór ustawień podczas pierwszego uruchomienia programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybieranie ustawienia środowiska programowania sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Tworzenie projektu aplikacji sieci Web i strony

<a id="sectionToggle0"></a>

W tej części instruktażu spowoduje utworzenie projektu aplikacji sieci Web i Dodaj nową stronę do niego. Zostanie również dodać tekst HTML i uruchomienia strony w przeglądarce.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz program Microsoft Visual Studio.
2. Na **pliku** menu, wybierz opcję **nowy projekt**.  
    ![Menu Plik](creating-a-basic-web-forms-page/_static/image1.png)

    **Nowy projekt** pojawi się okno dialogowe.
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.
5. Nazwij swój projekt ***BasicWebApp*** i kliknij przycisk **OK** przycisku.   
![Okno dialogowe Nowy projekt](creating-a-basic-web-forms-page/_static/image2.png)
6. Następnie wybierz pozycję **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisk, aby utworzyć projekt.  
![Okno dialogowe Nowy projekt ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera funkcje wstępnie utworzone na podstawie szablonu formularzy sieci Web. Nie tylko zapewnia użytkownikowi *Home.aspx* stronie *About.aspx* stronie *Contact.aspx* strony, ale oferuje także funkcjonalności członkostwa, który rejestruje użytkowników i zapisuje swoje poświadczenia, aby ich zalogować się do witryny sieci Web. Po utworzeniu nowej strony domyślnie Visual Studio zostanie wyświetlona strona w **źródła** widok, w którym można zobaczyć elementy HTML strony. Na poniższej ilustracji przedstawiono, jakie powinny zostać wyświetlone **źródła** wyświetlić, jeśli utworzono nową stronę sieci Web o nazwie *BasicWebApp.aspx*.  
    ![Widok źródła](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Przewodnik po środowisku programowania Visual Studio w sieci Web


Przed kontynuowaniem, modyfikując na stronie, warto zapoznać się z środowiska programistycznego Visual Studio. Na poniższej ilustracji przedstawiono okna i narzędzia, które są dostępne w programie Visual Studio i Visual Studio Express for Web.

> [!NOTE] 
> 
> Ten diagram przedstawia domyślne systemu windows i położenia okien. **Widoku** menu pozwala na wyświetlanie dodatkowych okien i rozmieszczanie i zmienianie rozmiaru windows zgodnie z preferencjami użytkownika. Układ okna już wprowadzono zmiany, zostanie wyświetlony nie będą zgodne ilustracji.


 W środowisku Visual Studio

![Środowisko Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Zapoznaj się z Projektant stron sieci Web

Sprawdź powyższej ilustracji i tekst na poniższej liście opisano najczęściej używane narzędzia i systemu windows. (Nie wszystkich okien i narzędzia, które zostanie wyświetlony na liście, tylko te oznaczone na poprzedniej ilustracji.)

- Paski narzędzi. Podaj poleceń formatowania tekstu, wyszukiwanie tekstu i tak dalej. Niektóre paski narzędzi są dostępne tylko wtedy, gdy użytkownik pracuje w **projektowania** widoku.
- **Eksplorator rozwiązań** okna. Wyświetla pliki i foldery w aplikacji sieci Web.
- Okno dokumentu. Wyświetla dokumenty, którą pracujesz w systemie windows z kartami. Możesz przełączać się między dokumenty, klikając karty.
- **Właściwości** okna. Umożliwia zmianę ustawień strony, elementów kodu HTML, formantów i innych obiektów.
- Wyświetlanie kart. Powodować różne widoki tego samego dokumentu. **Projekt** widok jest w trybie WYSIWYG powierzchni edycji. **Źródło** widok jest edytora HTML dla strony. **Podziel** widoku są wyświetlane zarówno **projektowania** widoku i **źródła** widoku dla dokumentu. Pracujesz z **projektowania** i **źródła** widoków w dalszej części tego przewodnika. Jeśli wolisz otwieranie stron internetowych w **projektowania** wyświetlić na **narzędzia** menu, kliknij przycisk **opcje**, wybierz opcję **projektancie HTML** węzeł, a następnie zmień **Rozpocznij strony w** opcji.
- **Przybornik**. Zawiera formanty i elementy HTML, które można przeciągnąć na stronę. **Przybornik** elementy są pogrupowane według wspólnych funkcji.
- S **erwer Explorer**. Wyświetla połączenia z bazą danych. Jeśli Eksplorator serwera nie jest widoczny, w menu Widok, kliknij Eksploratora serwera.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web platformy ASP.NET


Podczas tworzenia nowej aplikacji formularzy sieci Web, za pomocą **aplikacji sieci Web ASP.NET** szablon projektu Visual Studio dodaje strony ASP.NET (strony formularzy sieci Web) o nazwie *Default.aspx*, jak również jako kilka innych plików i foldery. Możesz użyć *Default.aspx* strony jako stronę główną dla aplikacji sieci Web. Jednak w tym przewodniku utworzysz i pracować z nowej strony.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web


1. Zamknij *Default.aspx* strony. Aby to zrobić, kliknij kartę która wyświetla nazwę pliku, a następnie kliknij opcję zamykania.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku Nazwa aplikacji jest **BasicWebSite**), a następnie kliknij przycisk **Dodaj**  - &gt; **Nowy element**.   
**Dodaj nowy element** zostanie wyświetlone okno dialogowe.
3. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz **formularz sieci Web** ze środka listy i nadaj mu nazwę *FirstWebPage.aspx*.   
    ![Dodaj nowy element, okno dialogowe](creating-a-basic-web-forms-page/_static/image6.png)
4. Kliknij przycisk **Dodaj** można dodać strony sieci web do projektu.  
Visual Studio tworzy nową stronę i otwiera go.


### <a name="adding-html-to-the-page"></a>Dodawanie HTML do strony


W tej części instruktażu jakiś tekst statyczny doda do strony.

### <a name="to-add-text-to-the-page"></a>Aby dodać tekst do strony


1. W dolnej części okna dokumentu kliknij **projektowania** kartę, aby przełączyć się do **projektowania** widoku.

    Widok projektu wyświetla bieżącą stronę w sposób przypominający WYSIWYG. W tym momencie nie masz dowolny tekst lub formantów na stronie, aby strona jest pusta, z wyjątkiem linię przerywaną, co przedstawiono prostokąta. Reprezentuje prostokąta **div** elementu na stronie.
2. Kliknij wewnątrz prostokąt, który jest opisany przez linię przerywaną, co.
3. Typ **Visual Web Developer — Zapraszamy** i naciśnij klawisz **ENTER** dwa razy.

    Na poniższej ilustracji przedstawiono tekst wpisany w **projektowania** widoku.

    ![Tekst powitania w widoku Projekt](creating-a-basic-web-forms-page/_static/image7.png "tekst powitania w widoku projektu")
4. Przełącz się do **źródła** widoku.

    Możesz zobaczyć HTML w **źródła** widoku, który został utworzony po wpisaniu w **projektowania** widoku.  
    ![Strony sieci Web za pomocą tekst statyczny](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Uruchomienie strony


Przed kontynuowaniem poprzez dodawanie formantów do strony należy najpierw uruchomić ją.

### <a name="to-run-the-page"></a>Aby uruchomić stronę


1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *FirstWebPage.aspx* i wybierz **Ustaw jako strona startowa**.
2. Naciśnij klawisz **kombinację klawiszy CTRL + F5** do uruchomienia strony.

    Ta strona jest wyświetlana w przeglądarce. Mimo, że utworzona strona ma rozszerzenie nazwy pliku *.aspx*, obecnie działa podobnie jak dowolnej strony HTML.

    Aby wyświetlić stronę w przeglądarce możesz można również prawym przyciskiem myszy na stronie w **Eksploratora rozwiązań** i wybierz **Pokaż w przeglądarce**.
3. Zamknij przeglądarkę, aby zatrzymać aplikację sieci Web.


## <a name="adding-and-programming-controls"></a>Dodawanie i programowanie kontrolek


<a id="sectionToggle1"></a>

Na stronie będzie teraz Dodaj formanty serwera. Formanty serwera, takie jak przyciski, etykiety, pola tekstowe i innych znanych formantów zapewniają typowych możliwości przetwarzania formularza stron formularzy sieci Web. Jednak można programować kontrolek z kodem, który jest uruchamiany na serwerze, a nie dla klienta.

Spowoduje dodanie [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolki, [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli i [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) do strony oraz napisz kod obsługujący [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzenia dla [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli.

### <a name="to-add-controls-to-the-page"></a>Aby dodać formanty do strony


1. Kliknij przycisk **projektowania** kartę, aby przełączyć się do **projektowania** widoku.
2. Umieść kursor na końcu **Visual Web Developer — Zapraszamy** tekst i naciśnij klawisz **ENTER** pięć lub więcej razy, aby pomieścić w **div** pola elementu.
3. W **przybornika**, rozwiń węzeł **standardowa** grupy, jeśli nie jest już rozwinięty.  
Należy zauważyć, że trzeba zwiększyć **przybornika** oknie po lewej stronie, aby go wyświetlić.
4. Przeciągnij [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli na stronę i upuść je w środku elementu **div** pola elementu, który ma **Visual Web Developer — Zapraszamy** w pierwszym wierszu.
5. Przeciągnij [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli na stronę i upuść je na prawo od [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli.
6. Przeciągnij [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli na stronę i upuść je w oddzielnym wierszu poniżej [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli.
7. Umieść punkt wstawiania powyżej [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) sterowania, a następnie wpisz **wprowadź swoją nazwę:** .

    Ten tekst statyczny HTML jest podpis [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) kontroli. Możesz mieszać statyczny kod HTML i formantów serwera na tej samej stronie. Na poniższej ilustracji przedstawiono, jak trzy kontrolki są wyświetlane w **projektowania** widoku.

    ![Trzy kontrolki w widoku Projekt](creating-a-basic-web-forms-page/_static/image9.png "trzy kontrolki w widoku projektu")


### <a name="setting-control-properties"></a>Ustawianie właściwości formantu


Visual Studio oferuje różne sposoby, aby ustawić właściwości formantów na stronie. W tej części instruktażu będzie ustawić właściwości zarówno **projektowania** widoku i **źródła** widoku.

### <a name="to-set-control-properties"></a>Aby ustawić właściwości kontrolki


1. Najpierw wyświetl **właściwości** systemu windows, wybierając z **widoku** menu —&gt; **Windows inne**  - &gt; **Okna properies**. Można też wybrać opcję **F4** do wyświetlenia **właściwości** okna.
2. Wybierz [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli, a następnie w polu **właściwości** okna, ustaw wartość **tekstu** do **nazwę wyświetlaną**. Wprowadzony tekst jest wyświetlany przycisk w projektancie, jak pokazano na poniższej ilustracji.

    ![Ustaw tekst przycisku](creating-a-basic-web-forms-page/_static/image10.png "przycisk Ustaw tekst")
3. Przełącz się do **źródła** widoku.

    **Źródło** widok wyświetla kod HTML dla strony, w tym elementy, które program Visual Studio zostało utworzone dla formantów serwera. Formanty są deklarowane przy użyciu składni notacji HTML, z tą różnicą, że tagi Użyj prefiksu **asp:** i zawierają atrybut **runat =&quot;serwera&quot;**.

    Właściwości kontrolki są deklarowane jako atrybuty. Na przykład po ustawieniu [tekstu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) właściwość [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sterowania, w kroku 1 zostały faktycznie ustawienie **tekstu** atrybutów w znaczniku formantu.

    > [!NOTE] 
    > 
    > Wszystkie kontrolki znajdują się wewnątrz **formularza** element, który również ma atrybut **runat =&quot;serwera&quot;**. **Runat =&quot;serwera&quot;**  atrybutu i **asp:** prefiks dla tagów formantów oznaczanie kontrolek tak, aby są przetwarzane przez platformę ASP.NET na serwerze po uruchomieniu na stronie. Kod poza **&lt;tworzą runat =&quot;serwera&quot; &gt;** i **&lt;skryptu runat =&quot;serwera&quot; &gt;** elementy są wysyłane bez zmian do przeglądarki, dlatego kod ASP.NET musi być wewnątrz elementu, którego otwierający tag zawiera **runat =&quot;serwera&quot;**  atrybutu.
4. Następnie możesz dodać dodatkowe właściwości [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli. Umieść kursor bezpośrednio po **asp: Label** w **&lt;asp: Label&gt;** tagu, a następnie naciśnij klawisz **spacja**.

    Listy rozwijanej wyświetlany jest wyświetlana lista dostępnych właściwości można ustawić dla [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli. Ta funkcja nazywana **IntelliSense**, pomoże Ci w **źródła** widoku przy użyciu składni kontrolek serwerowych, elementy HTML i innych elementów na stronie. Poniższa ilustracja przedstawia **IntelliSense** listy rozwijanej dla [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli.

    ![Atrybuty funkcji IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "atrybuty technologii IntelliSense")
5. Wybierz **ForeColor** , a następnie wpisz znak równości.

    Funkcja IntelliSense wyświetla listę kolorów.

    > [!NOTE] 
    > 
    > Możesz wyświetlić **IntelliSense** listy rozwijanej w dowolnym momencie, naciskając klawisz **CTRL + J** podczas przeglądania kodu.
6. Wybierz kolor **[etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** tekstu formantu. Upewnij się, że wybierasz kolor, który jest wystarczająco ciemne, można odczytać na białym tle.

    **ForeColor** atrybutów zostało zakończone z kolor, który wybrano, łącznie z cudzysłowu zamykającego.


### <a name="programming-the-button-control"></a>Kontrolka przycisku programowania


W tym przewodniku dajemy Ci szansę napisania kod, który odczytuje nazwę, że użytkownik wprowadzi w polu tekstowym, a następnie wyświetla nazwę w [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli.

### <a name="add-a-default-button-event-handler"></a>Dodaj domyślny program obsługi zdarzeń przycisku


1. Przełącz się do **projektowania** widoku.
2. Kliknij dwukrotnie [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli.

    Domyślnie program Visual Studio przechodzi w pliku związanego z kodem i tworzy obsługę zdarzeń szkieletu dla [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolki domyślne zdarzenie, [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzeń. Plik związany z kodem oddziela znaczników interfejsu użytkownika (na przykład HTML) w kodzie serwera (takiego jak C#).   
   Kursor znajduje się dodawać kod dla tego programu obsługi zdarzeń.

    > [!NOTE] 
    > 
    > Dwukrotne kliknięcie formantu w **projektowania** widok jest tylko jeden z kilku sposobów, można utworzyć procedury obsługi zdarzeń.
3. Wewnątrz **Button1\_kliknij** programu obsługi zdarzeń, typ **Label1** następuje kropka (**.**).

    Podczas wpisywania okresu po **identyfikator** etykiety (**Label1**), program Visual Studio Wyświetla listę dostępnych członków [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontrolować, jak pokazano w następującym Ilustracja. Element członkowski często właściwości, metody lub zdarzeń.

    ![Funkcja IntelliSense w widoku kodu](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense w widoku kodu")
4. Zakończ **kliknij** programu obsługi zdarzeń dla przycisku, tak że odczytuje, jak pokazano w poniższym przykładzie kodu.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Przejdź z powrotem do wyświetlania **źródła** widok w kodzie znaczników HTML, klikając prawym przyciskiem myszy *FirstWebPage.aspx* w **Eksploratora rozwiązań** i wybierając polecenie **widoku Kod znaczników**.
6. Przewiń do **&lt;asp: Button&gt;** elementu. Należy pamiętać, że **&lt;asp: Button&gt;** elementu ma atrybut **onclick =&quot;Button1\_kliknij&quot;**.

    Ten atrybut wiąże przycisku [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzeń do metody obsługi kodowane w poprzednim kroku.

    Metody obsługi zdarzeń może mieć dowolną nazwę; zostanie wyświetlona nazwa jest domyślna nazwa utworzone przez program Visual Studio. Istotną kwestią jest to, że nazwa używana dla **OnClick** atrybutu w kodzie HTML musi pasować do nazwy metody zdefiniowane w związanym z kodem.


### <a name="running-the-page"></a>Uruchomienie strony


Teraz możesz przetestować formantów serwera na stronie.

### <a name="to-run-the-page"></a>Aby uruchomić stronę


1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** do uruchomienia strony w przeglądarce. Jeśli wystąpi błąd, sprawdź ponownie powyższe kroki.
2. Wprowadź nazwę w polu tekstowym, a następnie kliknij przycisk **nazwę wyświetlaną** przycisku.

    Wprowadzona nazwa jest wyświetlana w [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli. Należy pamiętać, że po kliknięciu przycisku, strona jest przesyłana do serwera sieci Web. Następnie ASP.NET stronę odtwarza, uruchamia kod (w tym przypadku [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolki [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) uruchamia program obsługi zdarzeń), a następnie wysyła nową stronę w przeglądarce. Jeśli możesz obejrzeć paska stanu w przeglądarce, zostanie wyświetlony, że strony osiągnął komunikacji dwustronnej z serwerem sieci Web każde kliknięcie przycisku.
3. W przeglądarce, Wyświetl źródło strony są uruchomione, klikając prawym przyciskiem myszy na stronie i wybierając polecenie **Wyświetl źródło**.

    W kodzie źródłowym strony widać HTML bez konieczności wprowadzania kodu serwera. W szczególności nie ma **&lt;asp:&gt;** elementy, które pracowano w **źródła** widoku. Po uruchomieniu strony ASP.NET przetwarza formantów serwera i renderowania elementów HTML do strony, które wykonują funkcje, które reprezentują formantu. Na przykład **&lt;asp: Button&gt;** kontroli jest renderowany jako kod HTML **&lt;typu danych wejściowych =&quot;przesłać&quot; &gt;** element.
4. Zamknij przeglądarkę.


## <a name="working-with-additional-controls"></a>Praca z dodatkowych funkcji kontroli

<a id="sectionToggle2"></a>

W tej części instruktażu będą działać z [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formant, który wyświetla dat w miesiącu w danym momencie. [Kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli jest formantem bardziej skomplikowane niż przycisk, pole tekstowe i etykiety, pracy w z i przedstawia niektóre dodatkowe możliwości formantów serwera.

W tej sekcji dodasz [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) do strony i sformatować je.

### <a name="to-add-a-calendar-control"></a>Aby dodać formant kalendarza


1. W programie Visual Studio, przejdź do **projektowania** widoku.
2. Z **standardowa** części **przybornika**, przeciągnij [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli na stronę i upuść je poniżej **div** element, zawiera inne kontrolki.

    Zostanie wyświetlony panel tagu inteligentnego kalendarza. Na panelu są wyświetlane polecenia, które ułatwiają wykonywanie typowych zadań dla zaznaczonej kontrolki. Poniższa ilustracja przedstawia [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontrolować w postaci wyświetlanej w **projektowania** widoku.

    ![Kontrolki w widoku Projekt kalendarza](creating-a-basic-web-forms-page/_static/image13.png "formantu w widoku Projekt kalendarza")
3. W panelu tagi inteligentne wybierz **automatyczne formatowanie**.

    **Automatyczne formatowanie** zostanie wyświetlone okno dialogowe, które umożliwia wybranie schematu formatowania dla kalendarza. Poniższa ilustracja przedstawia **automatyczne formatowanie** okno dialogowe [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli.

    ![Formatuj automatycznie, okno dialogowe (formant kalendarza)](creating-a-basic-web-forms-page/_static/image14.png "automatyczne formatowanie, okno dialogowe (formant kalendarza)")
4. Z **wybierz schemat** , wybierz na liście **proste** a następnie kliknij przycisk **OK**.
5. Przełącz się do **źródła** widoku.

    Możesz zobaczyć **&lt;asp: kalendarza&gt;** elementu. Ten element jest znacznie dłuższy niż elementów dla prostych kontrolek, utworzonego wcześniej. Obejmuje również dozwolone podelementy, takich jak  **&lt;WeekEndDayStyle&gt;**, które reprezentują różne ustawienia formatowania. Poniższa ilustracja przedstawia [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) w kontrolce **źródła** widoku. (Dokładnie kod znaczników, który zostanie wyświetlony w **źródła** widoku mogą różnić się nieco od rysunku.)

    ![Kontrolki w widoku źródła kalendarza](creating-a-basic-web-forms-page/_static/image15.png "formantu w widoku źródła kalendarza")


### <a name="programming-the-calendar-control"></a>Programowanie kontrolki kalendarza


W tej sekcji zostanie program [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) formantu, aby wyświetlić aktualnie wybranej daty.

### <a name="to-program-the-calendar-control"></a>Program kontrolki kalendarza


1. W **projektowania** wyświetlić, kliknij dwukrotnie [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) kontroli.

    Nowy program obsługi zdarzeń jest tworzone i wyświetlane w pliku związanym z kodem o nazwie *FirstWebPage.aspx.cs*.
2. Zakończ [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) programu obsługi zdarzeń z następującym kodem.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Powyższy kod ustawia tekst kontrolki etykiety do wybranego dnia kontrolki kalendarza.


### <a name="running-the-page"></a>Uruchomienie strony


Teraz możesz przetestować kalendarza.

### <a name="to-run-the-page"></a>Aby uruchomić stronę


1. Naciśnij klawisz **kombinację klawiszy CTRL + F5** do uruchomienia strony w przeglądarce.
2. Kliknij datę w kalendarzu.

    Data została kliknięta jest wyświetlana na [etykiety](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) kontroli.
3. W przeglądarce należy wyświetlić kod źródłowy dla strony.

    Należy pamiętać, że [kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) na tej stronie, co zostało wyrenderowane kontroli **tabeli**, z każdego dnia, jak **td** elementu.
4. Zamknij przeglądarkę.


## <a name="next-steps"></a>Następne kroki


<a id="nextStepsToggle"></a>

Ten przewodnik zawiera zilustrowane podstawowych funkcji programu Visual Studio Projektant strony. Teraz, możesz dowiedzieć się, jak tworzyć i edytować strony formularzy sieci Web, w programie Visual Studio, warto zapoznać się z innymi funkcjami. Na przykład możesz wykonać następujące czynności:

- Dowiedz się więcej o wzorca ASP.NET Web Forms, wykonując instrukcje krok po kroku serii samouczków [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Dowiedz się więcej na temat usuwania kaskadowego arkuszy stylów (CSS). Aby uzyskać więcej informacji, zobacz [Praca z CSS Przegląd](https://msdn.microsoft.com/library/bb398931.aspx).
