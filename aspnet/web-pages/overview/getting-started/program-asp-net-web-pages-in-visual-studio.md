---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programowanie ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio | Dokumentacja firmy Microsoft
author: tfitzmac
description: Niniejszym załączniku opisano, jak można użyć Visual Studio 2010 ani Visual Web Developer 2010 Express do programu ASP.NET Web Pages o składni Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 41cb1048b9dab21516e38cfff0772b8b690d474f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753718"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programowanie wzorca ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak można użyć programu Visual Studio lub Visual Web Developer Express do programu ASP.NET Web Pages (Razor) witryn sieci Web.
> 
> Dowiesz się
> 
> - Co jest potrzebne do zainstalowania (jeśli nic) do pracy przy użyciu stron ASP.NET Web Pages w używanej wersji programu Visual Studio.
> - Jak dodać obsługę dla stron sieci Web platformy ASP.NET do programu Visual Web Developer 2010 Express.
> - Jak używać funkcji w programie Visual Studio do pracy ze stronami Razor aplikacji ASP.NET, w tym funkcji IntelliSense i debugowania.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - Program WebMatrix 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 programu Visual Studio 2012, Visual Studio 2010 i programu WebMatrix 2.


Można programować wzorca ASP.NET Web pages o składni Razor za pomocą programu WebMatrix lub innych edytorów kodu. Można również użyć programu Microsoft Visual Studio, który jest w pełni funkcjonalne zintegrowanym środowisku programistycznym (IDE), która udostępnia zaawansowany zestaw narzędzi do tworzenia wielu rodzajach aplikacji (nie tylko witryn sieci Web). Do pracy ze stronami Razor aplikacji ASP.NET, można albo użyj jednego z pełnej wersji programu Visual Studio lub bezpłatną [programu Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Dostępne są następujące dwie szczególnie przydatne funkcje, które program Visual Studio udostępnia do programowania w języku ASP.NET Razor strony sieci web:

- *Funkcja IntelliSense*. Funkcja IntelliSense, wbudowany w program Visual Studio jest większe niż funkcja IntelliSense w programie WebMatrix.
- *Debuger*. Debuger umożliwia rozwiązywanie problemów z kodu przez zatrzymanie programu, gdy jest uruchomiona, badanie zmiennych i krokowe wykonywanie kodu wiersz po wierszu.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Za pomocą programu Visual Studio z użyciem różnych wersji wzorca ASP.NET Web Pages

Visual Studio 2012 i Visual Studio 2013 obejmują obsługę składnika ASP.NET Web Pages. (Pakiety, które są wymagane do obsługi stron ASP.NET Web Pages są instalowane podczas instalowania programu Visual Studio).

Program Visual Studio 2010 nie obejmuje pomocy technicznej domyślnie dla stron ASP.NET Web Pages. Strony sieci Web ASP.NET za pomocą programu Visual Studio 2010, należy zainstalować pakiet platformy ASP.NET MVC. Aby uzyskać ASP.NET Web Pages 2, należy zainstalować platformy ASP.NET MVC 4.

Poniższa tabela zawiera podsumowanie obsługi dla stron ASP.NET Web Pages w różnych wersjach programu Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Instalowanie platformy ASP.NET MVC 4 | (Dołączone) | (Dołączone) |
| **ASP.NET Web Pages 3** |  | Aktualizacja sieci Web platformy ASP.NET strony 3 za pomocą NuGet | (Dołączone) |

Aby pracować z usługą Visual Studio 2010, zobacz [instalowania obsługi dla stron ASP.NET Web Pages w programie Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Uruchamianie programu Visual Studio z programu WebMatrix

Jeśli rozpoczęto projektu w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, program WebMatrix udostępnia przycisku umożliwiającego łatwe otwieranie projektu w programie Visual Studio. Konieczne jest posiadanie zainstalowanego programu Visual Studio na komputerze przez ten przycisk, aby włączyć. Na poniższej ilustracji przedstawiono przycisku w programie WebMatrix.

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Po kliknięciu przycisku, projekt jest otwierany w programie Visual Studio. Możesz przełączać się i z powrotem między programu WebMatrix i Visual Studio bez problemów. Użytkownik będzie powiadamiany, jeśli pliki zostały zmienione w innym środowisku, a trzeba będzie ponownie załadować, aby pobrać najnowsze zmiany.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Tworzenie witryny ASP.NET Razor w programie Visual Studio

Aby utworzyć witrynę sieci Web ASP.NET Razor, w programie Visual Studio:

1. Uruchom program Visual Studio lub Visual Web Developer.
2. W **pliku** menu, kliknij przycisk **nową witrynę sieci Web**.

    ![Utwórz nową witrynę sieci web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. W **nową witrynę sieci Web** okna dialogowego Wybierz język (Visual C# lub Visual Basic).
4. Wybierz **witryny sieci Web platformy ASP.NET (Razor)** szablonu.

    ![Witryna razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Kliknij przycisk **OK**.

Nowy projekt istnieje i jest wypełniana przy użyciu niektóre domyślne strony sieci web, aby pomóc Ci rozpocząć pracę.

### <a name="using-intellisense"></a>Korzystanie z IntelliSense

Teraz, po utworzeniu witryny, możesz zobaczyć, jak technologia IntelliSense działa w programie Visual Studio.

1. W witrynie sieci Web został utworzony, otwórz *Default.cshtml* strony.
2. Po `<h3>` tagów na stronie, wpisz `@ServerInfo.` (w tym kropki (.)). Zwróć uwagę, jak technologia IntelliSense wyświetla dostępne metody `ServerInfo` pomocnika na liście rozwijanej. 

    ![funkcja IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Wybierz `GetHtml` metodę z listy, a następnie naciśnij klawisz Enter. Funkcja IntelliSense automatycznie wypełnia metody. (Przy użyciu dowolnej metody w języku C#, podczas dodawania `()` znaków po metodzie.)  
   Kompletny kod dla `GetHtml` metoda wygląda następująco:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Naciśnij klawisze Ctrl + F5, aby uruchomić stronę. Jest to, jak wygląda podczas wyświetlania w przeglądarce strony: 

    ![domyślną stronę w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Zamknij przeglądarkę.

### <a name="using-the-debugger"></a>Za pomocą debugera

1. W górnej części *Default.cshtml* strony po wierszu, który rozpoczyna się od `Page.Title`, Dodaj następujący wiersz kodu: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Szare marginesie po lewej stronie kodu w edytorze, kliknij przycisk Dalej, ten nowy wiersz. Aby można było dodać *punktu przerwania*. Punkt przerwania jest znacznik, który informuje debuger, aby zatrzymać program w tym momencie, dzięki czemu można zobaczyć, co się dzieje.

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Usuń wywołanie funkcji `ServerInfo.GetHtml` metodę i dodaj wywołanie do `@myTime` zmiennej w tym miejscu. To wywołanie powoduje wyświetlenie bieżącej wartości godziny, który jest zwracany przez nowy wiersz kodu.
4. Naciśnij klawisz F5, aby uruchomić stronę w debugerze. Strona zatrzymuje się na punkcie przerwania, który został ustawiony. Na poniższej ilustracji przedstawiono, jak strona wygląda w edytorze za pomocą punktu przerwania (w kolorze żółtym). 

    ![punkt przerwania debugowania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na pasku narzędzi debugowania kliknij **Step Into** przycisk (lub naciśnij klawisz F11) do uruchamiania następnego wiersza kodu. Każde kliknięcie tego przycisku, wykonanie przejdź do następnego wiersza kodu.

    ![Wejdź do przycisku](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Sprawdź wartość `myTime` zmiennej, przytrzymując wskaźnik myszy nad nim lub sprawdzając wartości wyświetlane w **lokalne** i **stos wywołań** systemu windows. Program Visual Studio wyświetlić wartość zmiennej.

    ![wartość czasu show](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Gdy skończysz, sprawdzając zmienną i krokowe wykonywanie kodu, naciśnij klawisz F5, aby kontynuować wykonywanie strony bez zatrzymywania w każdym wierszu. Po zakończeniu przechodzeniu przez cały kod w przeglądarce pojawi się strona.

Aby dowiedzieć się więcej na temat debugera i o tym, jak można debugować kodu w programie Visual Studio, zobacz [wskazówki: debugowanie stron sieci Web w Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Korzystanie z aparatu Razor w projektach ASP.NET MVC z programem Visual Studio

Składnia Razor jest również używany często w projektach ASP.NET MVC. MVC to zaawansowany, bazujący na wzorcach sposób tworzenia dynamicznych witryn sieci Web. Witryna ASP.NET Web Pages staje się trudne w utrzymaniu, warto wziąć pod uwagę podczas konwertowania go do aplikacji ASP.NET MVC. Aby uzyskać przykład tworzenia aplikacji MVC, zobacz [wprowadzenie do ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalowanie obsługi dla stron sieci Web platformy ASP.NET w programie Visual Studio 2010

W tej sekcji przedstawiono sposób instalowania narzędzia stron sieci Web platformy ASP.NET i Visual Web Developer Express 2010 dla programu Visual Studio.

1. Jeśli nie masz jeszcze Instalatora platformy sieci Web, należy ją pobrać z następującego adresu URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Uruchom Instalatora platformy sieci Web.
3. Kliknij przycisk **produktów** kartę.

    ![Karta produktów Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Wyszukaj **platformy ASP.NET MVC 4** (dla programu ASP.NET Web Pages 2) a następnie kliknij przycisk **Dodaj**. Produkty te obejmują narzędzia programu Visual Studio do tworzenia witryn sieci Web ASP.NET Razor.

    ![Opcje instalacji Instalatora WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Kliknij przycisk **zainstalować** do ukończenia instalacji.
