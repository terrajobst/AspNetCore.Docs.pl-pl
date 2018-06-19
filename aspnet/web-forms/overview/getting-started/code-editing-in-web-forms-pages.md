---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edytowanie kodu formularzy sieci Web ASP.NET w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880755"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Kod edycji ASP.NET Web Forms w programie Visual Studio 2013
====================
Przez [Erik Reitan](https://github.com/Erikre)

Na wielu stronach formularza sieci Web ASP.NET pisania kodu w języku Visual Basic, C# lub innego języka. Edytor kodu w programie Visual Studio może pomóc szybko napisać kod jednocześnie pomaga uniknąć błędów. Ponadto Edytor umożliwia sposoby tworzenia kodu wielokrotnego użytku pomagają zmniejszyć ilość pracy, które należy wykonać.

W tym przewodniku przedstawiono różne funkcje edytora kodu programu Visual Studio.

W tym przewodniku przedstawiono sposób:

- Popraw błędy kodowania w tekście.
- Refaktoryzuj i Zmień nazwę kodu.
- Zmiana nazwy zmiennych i obiektów.
- Wstawianie wstawek kodu.

## <a name="prerequisites"></a>Wymagania wstępne


W celu przeprowadzenia tego instruktażu potrzebne są:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 i programu Microsoft Visual Studio Express 2013 for Web będzie często określane jako Visual Studio w tej serii samouczka.  
    >   
    > Jeśli używasz programu Visual Studio w tym przewodniku przyjęto założenie, że wybrano **projektowanie witryn sieci Web** kolekcję ustawień przy pierwszym uruchomieniu programu Visual Studio. Aby uzyskać więcej informacji, zobacz [porady: Wybierz ustawienia środowiska sieci Web Development](https://msdn.microsoft.com/library/ff521558.aspx).

  Aby obejrzeć wprowadzenie do platformy ASP.NET i Visual Studio, zobacz [tworzenia podstawowego strony formularzy sieci Web programu ASP.NET 4.5 w programie Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Tworzenie projektu aplikacji sieci Web i strony

<a id="sectionToggle0"></a>

W tej części przewodnika utworzysz projekt aplikacji sieci Web i Dodaj nową stronę do niego.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz program Microsoft Visual Studio.
2. Na **pliku** menu, wybierz opcję **nowy projekt**.  
    ![Menu Plik](code-editing-in-web-forms-pages/_static/image1.png)

    **Nowy projekt** zostanie wyświetlone okno dialogowe.
3. Wybierz **szablony**  - &gt; **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie.
4. Wybierz **aplikacji sieci Web ASP.NET** szablonu w środkowej kolumnie.
5. Nazwij swój projekt ***BasicWebApp*** i kliknij przycisk **OK** przycisku.   
![Okno dialogowe Nowy projekt](code-editing-in-web-forms-pages/_static/image2.png)
6. Następnie wybierz pozycję **formularzy sieci Web** szablon i kliknij przycisk **OK** przycisk, aby utworzyć projekt.  
![Okno dialogowe Nowy projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera wbudowane funkcje, oparty na szablonie formularzy sieci Web.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web ASP.NET


Podczas tworzenia nowej aplikacji formularzy sieci Web, przy użyciu **aplikacji sieci Web ASP.NET** szablon projektu Visual Studio dodaje strony ASP.NET (strony formularzy sieci Web), o nazwie *Default.aspx*oraz jak kilka innych plików i foldery. Można użyć *Default.aspx* strony jako stronę główną dla aplikacji sieci Web. Jednak w ramach tego przewodnika, utworzysz i pracować z nowej strony.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web


1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku, nazwa aplikacji jest **BasicWebSite**), a następnie kliknij przycisk **Dodaj**  - &gt; **Nowy element**.   
**Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **Visual C#**  - &gt; **Web** grupy szablonów po lewej stronie. Następnie wybierz opcję **formularza sieci Web** ze środka listy i nadaj mu nazwę *FirstWebPage.aspx*.   
    ![Dodaj nowy element — okno dialogowe](code-editing-in-web-forms-pages/_static/image4.png)
3. Kliknij przycisk **Dodaj** można dodać strony formularzy sieci Web do projektu.  
 Visual Studio utworzy nową stronę i otwarcie go.
4. Następnie ustaw nowej strony jako stronę startową domyślne. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nową stronę o nazwie *FirstWebPage.aspx* i wybierz **Ustaw jako stronę startową**. Podczas następnego uruchomienia tej aplikacji do naszej postęp testu zostaną automatycznie wyświetlone tym nową stronę w przeglądarce.


## <a name="correcting-inline-coding-errors"></a>Korygowanie wbudowanego błędy kodowania


Edytor kodu w programie Visual Studio pomaga uniknąć błędów, jak napisać kod, a jeśli wprowadzono Błąd edytora kodu pomaga poprawić ten błąd. W tej części przewodnika zapisze wiersz kodu zilustrowanie funkcji korekcji błędów w edytorze.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Aby poprawić błędy kodowania proste w programie Visual Studio


1. W **projekt** wyświetlić, kliknij dwukrotnie pustej strony, aby utworzyć program obsługi **obciążenia** zdarzenia dla strony.   
   Używasz programu obsługi zdarzeń tylko jako miejsce do pisania kodu.
2. Wewnątrz obsługi, wpisz następujące polecenie, która zawiera błąd i naciśnij klawisz **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Po naciśnięciu **ENTER**, edytora kodu umieszcza zielonego i czerwonego podkreślenia (często wywołać &quot;dowolnym kształcie&quot; wierszy) w obszarach kodu, które mają problemy. Zielony podkreślenie wskazuje ostrzeżenie. Czerwone podkreślenie wskazuje błąd, który należy naprawić. 

    Umieść kursor `myStr` Aby wyświetlić etykietkę narzędzia, który informuje o ostrzeżenia. Ponadto wskaźnik myszy nad czerwonym podkreśleniem, aby wyświetlić komunikat o błędzie.

    Poniższa ilustracja przedstawia przykładowy kod podkreślenia.

    ![Witamy tekst w widoku Projekt](code-editing-in-web-forms-pages/_static/image5.png "Witamy tekst w widoku Projekt")  
   Błąd muszą zostać usunięte przez dodanie średnikiem `;` do końca wiersza. Ostrzeżenie po prostu powiadamia użytkownika, aby nie były używane `myStr` jeszcze zmiennej.  

    > [!NOTE] 
    > 
    > Możesz wyświetlić bieżący kod formatowania ustawień w Visual Studio, wybierając **narzędzia**  - &gt; **opcje**  - &gt; **czcionek i Kolory**.


## <a name="refactoring-and-renaming"></a>Refaktoryzacja i zmiana nazwy

Refaktoryzacja jest metodologia oprogramowania, która obejmuje restrukturyzacji swój kod, aby ułatwić zrozumienie i obsługa, zachowując jego funkcjonalność. Prosty przykład może być napisać kod w obsłudze zdarzeń można pobrać danych z bazy danych. Podczas opracowywania strony odkrywasz, trzeba uzyskać dostęp do danych z wielu różnych obsługi. W związku z tym Refaktoryzuj kodu strony tworzenia metody dostępu do danych na stronie i wstawianie wywołania metody w obsłudze.

Edytor kodu zawiera narzędzia, które ułatwiają wykonywanie różnych zadań refaktoryzacji. W tym przewodniku będzie działać z dwóch metod refaktoryzacji: zmiana nazwy zmiennych i wyodrębniania metody. Inne opcje refaktoryzacji obejmują hermetyzując pól, podwyższania poziomu zmiennych lokalnych do parametrów metod i zarządzania parametry metody. Dostępność tych opcji refaktoryzacji zależy od lokalizacji w kodzie.

### <a name="refactoring-code"></a>Refaktoryzacji kodu

Typowy scenariusz refaktoryzacji jest utworzenie (extract) metody z kodu, który znajduje się wewnątrz innego elementu członkowskiego, takich jak metody. Zmniejsza rozmiar oryginalnego elementu i sprawia, że kod wyodrębnionego wielokrotnego użytku.

W tej części przewodnika będą napisanie kodu, proste, a następnie Wyodrębnij metodę z niego. Refaktoryzacja jest obsługiwana dla C#, więc będzie można utworzyć stronę, która używa języka C# jako języka programowania.

### <a name="to-extract-a-method-in-a-c-page"></a>Aby wyodrębnić metody na stronie C#

1. Przełącz się do **projekt** widoku.
2. W **przybornika**, z **standardowe** karcie, przeciągnij [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) kontrolki na stronie.
3. Kliknij dwukrotnie **przycisk** formantu, aby utworzyć uchwytu dla jego [kliknij](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) zdarzenia, a następnie dodaj następujący wyróżniony kod:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kod tworzy **ArrayList** obiektu, używa go załadować z wartościami pętlę, a następnie używa innego pętli do wyświetlenia zawartości **ArrayList** obiektu.
4. Naciśnij klawisz **CTRL + F5** do uruchomienia strony, a następnie kliknij przycisk **przycisk** aby upewnić się, że wyświetlone następujące dane wyjściowe:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Powrócić do edytora kodu, a następnie wybierz następujące wiersze do obsługi zdarzeń.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Kliknij prawym przyciskiem myszy zaznaczenie, kliknij przycisk **Refaktoryzuj**, a następnie wybierz pozycję **Wyodrębnij metodę**. 

    **Wyodrębnij metodę** zostanie wyświetlone okno dialogowe.
7. W **nową nazwę metody** wpisz **DisplayArray**, a następnie kliknij przycisk **OK**. 

    Edytor kodu tworzy nową metodę o nazwie `DisplayArray`i umieszcza wywołanie do nowej metody w **kliknij** obsługi którym pierwotnie pętli.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Naciśnij klawisz **CTRL + F5** ponownie uruchomić stronę, a następnie kliknij przycisk **przycisk**.

    Strona działa tak samo, tak jak poprzednio. `DisplayArray` Metoda może być teraz wywołania z dowolnego miejsca w klasie strony.

## <a name="renaming-variables"></a>Zmiana nazwy zmiennych

Podczas pracy z obiektów, a także zmienne, można zmienić ich nazwy po ich są już przywoływane w kodzie. Zmiana nazwy zmiennych i obiektów może jednak spowodować kodu Jeśli pominiesz zmiana nazwy jedno z odwołań. W związku z tym umożliwia refaktoryzacji wykonaj zmianie nazwy.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Aby użyć refaktoryzacji można zmienić nazwy zmiennej


1. W **kliknij** program obsługi zdarzeń, znajdź następujący wiersz:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Kliknij prawym przyciskiem myszy nazwę zmiennej `alist`, wybierz **Refaktoryzuj**, a następnie wybierz pozycję **zmienić**.

    **Zmienić** zostanie wyświetlone okno dialogowe.
3. W **nową nazwę** wpisz **ArrayList1** i upewnij się, że **podgląd zmian odwołanie** pole wyboru zostało zaznaczone. Następnie kliknij przycisk **OK**.

    **Podgląd zmian** okno dialogowe zostanie wyświetlone i wyświetla drzewa, który zawiera wszystkie odwołania do zmiennej, która jest zmieniana.
4. Kliknij przycisk **Zastosuj** zamknąć **podgląd zmian** okno dialogowe.

    Zmienne odwołujące się specjalnie do wybranego wystąpienia są zmieniane. Zauważ, że zmienna `alist` w następującym wierszu nie zostaje zmieniona.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Zmienna `alist` w danym wierszu nie zostaje zmieniona, ponieważ nie reprezentuje taką samą wartość jak zmienna `alist` którego nazwa została zmieniona. Zmienna `alist` w `DisplayArray` deklaracja jest zmienną lokalną dla tej metody. Przedstawiono to, że można zmienić nazwy zmiennych przy użyciu refaktoryzacji różni się od po prostu wykonywanie akcji Znajdź i Zamień w edytorze; Refaktoryzacja zmiany nazwy zmiennych o wiedzy semantykę zmiennej, która działa z.


## <a name="inserting-snippets"></a>Wstawianie wstawek

Ponieważ istnieje wiele zadań kodowania, które deweloperzy formularzy sieci Web często konieczne jest wykonanie, edytora kodu zawiera bibliotekę wstawki lub bloków kodu przedpisanych. Na stronie można wstawić tych fragmentów.

Każdego języka używanego w programie Visual Studio ma niewielkie różnice w sposób wstawiania wstawki kodu. Aby dowiedzieć się, jak wstawianie fragmentów, zobacz [Visual Basic IntelliSense — wstawki programu](https://msdn.microsoft.com/library/18yz4be4.aspx). Aby dowiedzieć się, jak wstawianie fragmenty kodu języka Visual C#, zobacz [Visual C# — wstawki](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Następne kroki

W tym przewodniku ma przedstawiono podstawowe funkcje programu Visual Studio 2010 edytora kodu poprawianie błędów w kodzie, refaktoryzacji kodu, zmiana nazwy zmiennych i wstawiania wstawki kodu w kodzie. Dodatkowe funkcje w edytorze ułatwia projektowanie aplikacji szybkie i łatwe. Na przykład możesz chcieć:

- Więcej informacji na temat funkcji IntelliSense, takich jak modyfikowanie opcji IntelliSense, zarządzanie fragmentów kodu i wyszukiwanie wstawki kodu w trybie online. Aby uzyskać więcej informacji, zobacz [za pomocą funkcji IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Dowiedz się, jak utworzyć własne wstawki kodu. Aby uzyskać więcej informacji, zobacz [tworzenie i przy użyciu fragmentów kodu IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Dowiedz się więcej na temat funkcji specyficznych dla języka Visual Basic, fragmentów kodu IntelliSense, takie jak dostosowywanie fragmenty kodu i rozwiązywania problemów. Aby uzyskać więcej informacji, zobacz [Visual Basic IntelliSense — wstawki programu](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Dowiedz się więcej na temat języka C# — funkcje IntelliSense, takie jak wstawki refaktoryzacji i kod. Aby uzyskać więcej informacji, zobacz [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
