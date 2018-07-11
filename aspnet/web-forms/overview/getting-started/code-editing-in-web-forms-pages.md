---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edytowanie kodu ASP.NET Web Forms w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: ''
ms.author: aspnetcontent
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: c8f0184ffcec0a988dc8bc814e16936c173659c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815976"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Kod edycji ASP.NET Web Forms w programie Visual Studio 2013
====================
przez [Erik Reitan](https://github.com/Erikre)

Na wielu stronach formularz sieci Web ASP.NET piszesz kod w języku Visual Basic, C# lub innego języka. Edytor kodu w programie Visual Studio może pomóc szybko Pisz kod, a jednocześnie pomaga uniknąć błędów. Ponadto Edytor umożliwia sposobów zmniejszyć ilość pracy, które należy wykonać w celu utworzenia kodu wielokrotnego użytku.

W tym instruktażu pokazano różne funkcje edytora kodu Visual Studio.

Z tego instruktażu dowiesz się jak:

- Poprawianie błędów kodowania w tekście.
- Refaktoryzacja i zmiana nazwy kodu.
- Zmiana nazwy zmiennych i obiektów.
- Wstawianie wstawek kodu.

## <a name="prerequisites"></a>Wymagania wstępne


Aby ukończyć ten przewodnik, potrzebne są:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 i Microsoft Visual Studio Express 2013 for Web będzie często określane jako programu Visual Studio w całej tej serii samouczków.  
    >   
    > Jeśli używasz programu Visual Studio, tym przewodniku przyjęto założenie, że wybrana **programowania dla sieci Web** zbiór ustawień podczas pierwszego uruchomienia programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybieranie ustawienia środowiska programowania sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).

  Wprowadzenie do programu Visual Studio i platformy ASP.NET, zobacz [Tworzenie podstawowych strony ASP.NET 4.5 Web Forms w programie Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Tworzenie projektu aplikacji sieci Web i strony

<a id="sectionToggle0"></a>

W tej części instruktażu spowoduje utworzenie projektu aplikacji sieci Web i Dodaj nową stronę do niego.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz program Microsoft Visual Studio.
2. Na **pliku** menu, wybierz opcję **nowy projekt**.  
    ![Menu Plik](code-editing-in-web-forms-pages/_static/image1.png)

    **Nowy projekt** pojawi się okno dialogowe.
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.
5. Nazwij swój projekt ***BasicWebApp*** i kliknij przycisk **OK** przycisku.   
![Okno dialogowe Nowy projekt](code-editing-in-web-forms-pages/_static/image2.png)
6. Następnie wybierz pozycję **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisk, aby utworzyć projekt.  
![Okno dialogowe Nowy projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera funkcje wstępnie utworzone na podstawie szablonu formularzy sieci Web.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web platformy ASP.NET


Podczas tworzenia nowej aplikacji formularzy sieci Web, za pomocą **aplikacji sieci Web ASP.NET** szablon projektu Visual Studio dodaje strony ASP.NET (strony formularzy sieci Web) o nazwie *Default.aspx*, jak również jako kilka innych plików i foldery. Możesz użyć *Default.aspx* strony jako stronę główną dla aplikacji sieci Web. Jednak w tym przewodniku utworzysz i pracować z nowej strony.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web


1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku Nazwa aplikacji jest **BasicWebSite**), a następnie kliknij przycisk **Dodaj**  - &gt; **Nowy element**.   
**Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz **formularz sieci Web** ze środka listy i nadaj mu nazwę *FirstWebPage.aspx*.   
    ![Dodaj nowy element, okno dialogowe](code-editing-in-web-forms-pages/_static/image4.png)
3. Kliknij przycisk **Dodaj** można dodać strony formularzy sieci Web do projektu.  
 Visual Studio tworzy nową stronę i otwiera go.
4. Następnie można ustawić tej nowej strony jako stronę startową domyślne. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nową stronę o nazwie *FirstWebPage.aspx* i wybierz **Ustaw jako stronę startową**. Podczas następnego uruchomienia tej aplikacji, aby przetestować nasze postępy automatycznie zobaczą tej nowej strony w przeglądarce.


## <a name="correcting-inline-coding-errors"></a>Poprawianie wbudowane błędy kodowania


Edytor kodu w programie Visual Studio pomaga uniknąć błędów, jak napisać kod, a jeśli utworzono błąd, Edytor kodu umożliwia użytkownikom do naprawienia błędu. W tej części instruktażu pisania wiersza kodu, które ilustrują funkcje korekcji błędów w edytorze.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Aby poprawić proste błędów kodowania w programie Visual Studio


1. W **projektowania** wyświetlić, kliknij dwukrotnie pustą stronę, aby utworzyć program obsługi **obciążenia** zdarzeń dla strony.   
   Używasz programu obsługi zdarzeń tylko jako miejsce do napisania kodu.
2. Wewnątrz procedury obsługi, wpisz następujący wiersz, który zawiera błąd i naciśnij klawisz **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Po naciśnięciu klawisza **ENTER**, Edytor kodu umieszcza zielonego i czerwonego podkreśleń (często wywołać &quot;falista&quot; wierszy) w ramach obszarów kodu, które mają problemy z. Ostrzeżenie wskazuje, zielonym podkreśleniem. Czerwoną linią wskazuje na błąd, który należy rozwiązać. 

    Umieść wskaźnik myszy nad `myStr` Aby wyświetlić etykietkę narzędzia, która informuje o ostrzeżenia. Ponadto wskaźnik myszy nad czerwonym podkreśleniem, aby wyświetlić komunikat o błędzie.

    Na poniższej ilustracji przedstawiono kod za pomocą podkreślenia.

    ![Tekst powitania w widoku Projekt](code-editing-in-web-forms-pages/_static/image5.png "tekst powitania w widoku projektu")  
   Należy naprawić błąd, dodając je średnikiem `;` do końca wiersza. Ostrzeżenie po prostu powiadamia użytkownika, które nie zostały użyte `myStr` jeszcze zmiennej.  

    > [!NOTE] 
    > 
    > Możesz wyświetlić bieżący kod formatowania ustawień w programie Visual Studio, wybierając **narzędzia**  - &gt; **opcje**  - &gt; **czcionek i Kolory**.


## <a name="refactoring-and-renaming"></a>Refaktoryzacja i zmiana nazwy

Refaktoryzacja jest metodologia oprogramowania, która obejmuje restrukturyzacji swój kod, aby ułatwić do zrozumienia i utrzymania, zachowując przy tym swoje funkcje. Prostym przykładem może być napisać kod w obsłudze zdarzeń można pobrać danych z bazy danych. Podczas opracowywania strona odnajdywania, trzeba uzyskać dostęp do danych z kilku różnych procedur obsługi. W związku z tym Refaktoryzacja kodu strony, tworząc metody dostępu do danych na stronie, a następnie wstawianie wywołania metody w programów obsługi.

Edytor kodu zawiera narzędzia, które ułatwiają wykonywanie różnych zadań refaktoryzacji. W tym instruktażu będą działać z dwóch technik refaktoryzacji: zmiana nazwy zmiennych i wyodrębniania metody. Inne opcje refaktoryzacji obejmują hermetyzowania pola, podwyższanie poziomu zmiennych lokalnych do parametrów metod i zarządzania parametrów metody. Dostępność te opcje refaktoryzacji, zależy od lokalizacji w kodzie.

### <a name="refactoring-code"></a>Refaktoryzacja kodu

Typowy scenariusz refaktoryzacji jest utworzenie (extract) metody z kodu, który znajduje się wewnątrz innego elementu członkowskiego, takie jak metody. Zmniejsza rozmiar oryginalny element członkowski i sprawia, że wyodrębnione kodu wielokrotnego użytku.

W tej części instruktażu będą pisania prostego kodu, a następnie Wyodrębnij metodę z niego. Refaktoryzacja jest obsługiwana dla języka C#, więc będzie można utworzyć stronę, która używa języka C# jako języka programowania.

### <a name="to-extract-a-method-in-a-c-page"></a>Aby wyodrębnić metody na stronie C#

1. Przełącz się do **projektowania** widoku.
2. W **przybornika**, z **standardowa** kartę, przeciągnij [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontroli na stronę.
3. Kliknij dwukrotnie **przycisk** formantu, aby utworzyć procedury obsługi dla jego [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzenia, a następnie dodaj następujący wyróżniony kod:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Ten kod tworzy **ArrayList** , używa pętli, aby załadować do niej wartości, a następnie używa innej pętli, aby wyświetlić zawartość **ArrayList** obiektu.
4. Naciśnij klawisz **kombinację klawiszy CTRL + F5** uruchomienia strony, a następnie kliknij przycisk **przycisk** aby upewnić się, że są widoczne następujące wyniki:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Powrót do edytora kodu, a następnie wybierz następujące wiersze, w obsłudze zdarzeń.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Kliknij prawym przyciskiem myszy zaznaczenie, kliknij przycisk **Refaktoryzuj**, a następnie wybierz **Wyodrębnij metodę**. 

    **Wyodrębnij metodę** pojawi się okno dialogowe.
7. W **nową nazwę metody** wpisz **DisplayArray**, a następnie kliknij przycisk **OK**. 

    Edytor kodu tworzy nową metodę o nazwie `DisplayArray`i umieszcza wywołanie do nowej metody w **kliknij** program obsługi, gdzie pierwotnie pętli.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Naciśnij klawisz **kombinację klawiszy CTRL + F5** ponownie uruchomić stronę, a następnie kliknij przycisk **przycisk**.

    Strona działa tak samo, tak jak poprzednio. `DisplayArray` Metody mogą być teraz wywołania z dowolnego miejsca w klasie strony.

## <a name="renaming-variables"></a>Zmiana nazwy zmiennych

Podczas pracy z zmienne, a także obiekty, można zmienić ich nazwy, po istnieją już odwołania w kodzie. Jednak zmiana nazwy zmiennych i obiektów może spowodować kod, aby przerwać Jeśli pominiesz, zmiana nazwy jedno z odwołań. W związku z tym służy refaktoryzacji do wykonania, zmiana nazwy.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Na potrzeby Refaktoryzacja zmiany nazwy zmiennej


1. W **kliknij** procedura obsługi zdarzeń, znajdź następujący wiersz:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Kliknij prawym przyciskiem myszy nazwę zmiennej `alist`, wybierz **Refaktoryzuj**, a następnie wybierz **Zmień nazwę**.

    **Zmień nazwę** pojawi się okno dialogowe.
3. W **nową nazwę** wpisz **ArrayList1** i upewnij się, że **podgląd zmian odwołania** pole wyboru zostało zaznaczone. Następnie kliknij przycisk **OK**.

    **Podgląd zmian** okno dialogowe pojawia się i wyświetla drzewa, która zawiera wszystkie odwołania do zmiennej, która jest zmieniana.
4. Kliknij przycisk **Zastosuj** zamknąć **podgląd zmian** okno dialogowe.

    Zmienne, które odwołują się do wystąpienia, które wybrano są zmieniane. Zauważ, że zmienna `alist` w następującym wierszu nie została zmieniona.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Zmienna `alist` w danym wierszu nie została zmieniona, ponieważ nie reprezentuje tę samą wartość jak zmienna `alist` którego nazwa została zmieniona. Zmienna `alist` w `DisplayArray` deklaracja jest zmienną lokalną dla tej metody. Obrazuje to, że za pomocą refaktoryzacji można zmienić nazwy zmiennych różni się od po prostu wykonywania działań Znajdź i Zamień w edytorze; Refaktoryzacja zmiany nazw zmienne przy zachowaniu wiedzy o semantykę zmiennej, która pracuje się z.


## <a name="inserting-snippets"></a>Wstawianie fragmentów kodu

Ponieważ istnieje wiele zadań kodowania, które deweloperzy formularzy sieci Web często muszą wykonywać, Edytor kodu zawiera bibliotekę fragmentów kodu lub bloki kodu przedpisanych. Te fragmenty kodu można wstawić do strony.

Każdy język, którego używasz w programie Visual Studio ma niewielkie różnice w taki sposób, wstawki kodu. Aby dowiedzieć się, jak wstawianie fragmentów kodu, zobacz [fragmenty kodu IntelliSense w języku Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Aby dowiedzieć się, jak wstawianie fragmentów kodu w języku Visual C#, zobacz [Visual C# — wstawki](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Następne kroki

W tym przewodniku ma ilustruje podstawowe funkcje edytora kodu programu Visual Studio 2010 do poprawiania błędów w kodzie, Refaktoryzacja kodu, zmiana nazwy zmiennych i wstawianie fragmentów kodu do kodu. Dodatkowe funkcje w edytorze ułatwia rozwój aplikacji jest łatwe i szybkie. Na przykład możesz chcieć:

- Dowiedz się więcej na temat funkcji IntelliSense, takich jak modyfikowanie Opcje IntelliSense, zarządzanie fragmenty kodu i wyszukiwanie wstawek kodu w trybie online. Aby uzyskać więcej informacji, zobacz [za pomocą funkcji IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Dowiedz się, jak utworzyć własne fragmenty kodu. Aby uzyskać więcej informacji, zobacz [tworzyć i za pomocą fragmenty kodu IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Dowiedz się więcej na temat funkcji specyficznych dla języka Visual Basic fragmenty kodu IntelliSense, takie jak dostosowywanie fragmenty kodu i rozwiązywania problemów. Aby uzyskać więcej informacji, zobacz [fragmenty kodu IntelliSense w języku Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Dowiedz się więcej na temat języka C# — określonych funkcji IntelliSense, takie jak Refaktoryzacja i fragmentów kodu. Aby uzyskać więcej informacji, zobacz [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
