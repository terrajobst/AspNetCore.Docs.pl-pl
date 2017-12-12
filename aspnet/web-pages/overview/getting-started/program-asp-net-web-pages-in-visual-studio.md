---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programowania w języku ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten dodatek wyjaśniono, jak można użyć programu Visual Studio 2010 lub Visual Web Developer 2010 Express programu ASP.NET Web Pages o składni Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programowanie stron sieci Web platformy ASP.NET (Razor) za pomocą programu Visual Studio
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano, jak można użyć programu Visual Studio lub Visual Web Developer Express programu ASP.NET Web Pages (Razor) witryn sieci Web.
> 
> Dowiesz się
> 
> - Co jest potrzebne do zainstalowania (jeśli nic) do pracy z stron sieci Web ASP.NET w wersji programu Visual Studio.
> - Jak dodać obsługę dla stron sieci Web ASP.NET do programu Visual Web Developer 2010 Express.
> - Jak używać funkcji w programie Visual Studio, aby pracować ze stronami ASP.NET Razor, w tym IntelliSense i debuger.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
> - Visual Studio 2013
> - Program WebMatrix 3
>   
> 
> W tym samouczku współdziała również z ASP.NET Web Pages 2 programu Visual Studio 2012, Visual Studio 2010 i programu WebMatrix 2.


Można programu stron ASP.NET Web pages o składni Razor przy użyciu programu WebMatrix lub wiele edytorów kodu. Można również użyć programu Microsoft Visual Studio, czyli kompletne zintegrowane środowisko programistyczne (IDE), który zapewnia zaawansowany zestaw narzędzi do tworzenia wielu typów aplikacji (nie tylko witryn sieci Web). Aby pracować ze stronami ASP.NET Razor, możesz albo użyj jednego z pełnej wersji programu Visual Studio lub wolnych [programu Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Dostępne są następujące dwie szczególnie przydatne funkcje, które program Visual Studio udostępnia do programowania w języku ASP.NET Razor strony sieci web:

- *IntelliSense*. Funkcja IntelliSense wbudowanych w programie Visual Studio jest bardziej zaawansowane niż IntelliSense w programie WebMatrix.
- *Debuger*. Debuger umożliwia rozwiązywanie problemów z kodu przez zatrzymanie programu, gdy jest uruchomiona, sprawdzając zmiennych i krokowe wykonywanie kodu wiersz po wierszu.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Za pomocą programu Visual Studio z różnymi wersjami strony sieci Web ASP.NET

Visual Studio 2012 i Visual Studio 2013 obsługują dla stron sieci Web programu ASP.NET. (Pakiety, które są wymagane w celu obsługi stron ASP.NET Web Pages są instalowane podczas instalowania programu Visual Studio).

Program Visual Studio 2010 nie obsługują domyślnie dla stron sieci Web programu ASP.NET. Aby strony sieci Web ASP.NET za pomocą programu Visual Studio 2010, należy zainstalować pakiet programu ASP.NET MVC. Aby uzyskać ASP.NET Web Pages 2, należy zainstalować program ASP.NET MVC 4.

Poniższa tabela zawiera podsumowanie obsługi dla stron sieci Web ASP.NET w różnych wersjach programu Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Strony ASP.NET Web Pages 2** | Zainstaluj program ASP.NET MVC 4 | (Włączone) | (Włączone) |
| **Strony ASP.NET Web Pages 3** |  | Aktualizacja do sieci Web ASP.NET strony 3 za pomocą NuGet | (Włączone) |

Aby pracować z programu Visual Studio 2010, zobacz [Instalowanie obsługi dla stron sieci Web ASP.NET w programie Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Uruchamianie programu Visual Studio za pomocą programu WebMatrix

Jeśli rozpoczęto projekt w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, program WebMatrix udostępnia przycisk, aby łatwo Otwórz projekt w programie Visual Studio. Musi mieć zainstalowanego programu Visual Studio na komputerze na tym przycisku włączenia. Na poniższej ilustracji przedstawiono przycisku w programie WebMatrix.

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Po kliknięciu przycisku projektu jest otwarty w programie Visual Studio. Możesz można przełączać się między WebMatrix i Visual Studio bez problemów. Jeśli wszystkie pliki zostały zmienione w innym środowisku i musi być załadowany ponownie, aby pobrać najnowsze zmiany, otrzymasz powiadomienie.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Tworzenie witryny ASP.NET Razor w programie Visual Studio

Aby utworzyć witryny sieci Web platformy ASP.NET Razor w programie Visual Studio:

1. Uruchom program Visual Studio lub Visual Web Developer.
2. W **pliku** menu, kliknij przycisk **nowej witryny sieci Web**.

    ![Utwórz nową witrynę sieci web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. W **nowej witryny sieci Web** oknie dialogowym Wybierz język (Visual C# lub Visual Basic).
4. Wybierz **witryny sieci Web platformy ASP.NET (Razor)** szablonu.

    ![razor lokacji](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Kliknij przycisk **OK**.

Twój nowy projekt istnieje i jest wypełniana niektóre domyślne strony sieci web do ułatwiające rozpoczęcie pracy.

### <a name="using-intellisense"></a>Korzystanie z IntelliSense

Teraz, po utworzeniu witryny widać, jak działa funkcja IntelliSense w Visual Studio.

1. Witryna sieci Web został utworzony, otwórz *Default.cshtml* strony.
2. Po `<h3>` znaczników na stronie typu `@ServerInfo.` (w tym kropki (.)). Zwróć uwagę, jak IntelliSense wyświetla dostępne metody `ServerInfo` pomocnika na liście rozwijanej. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Wybierz `GetHtml` metodę z listy, a następnie naciśnij klawisz Enter. IntelliSense automatycznie wypełnia metody. (Przy użyciu dowolnej metody w języku C#, podczas dodawania `()` po metodzie znaki.)  
 Kompletny kod dla `GetHtml` metody wygląda następująco:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Naciśnij klawisze Ctrl + F5, aby uruchomić strony. Jest to, jak podczas wyświetlania w przeglądarce wygląda strony: 

    ![domyślną stronę w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Zamknij przeglądarkę.

### <a name="using-the-debugger"></a>Korzystanie z debugera

1. W górnej części *Default.cshtml* strony po wierszu, który rozpoczyna się od `Page.Title`, Dodaj następujący wiersz kodu: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Szara marginesie edytora kodu w lewo, kliknij przycisk Dalej, nowego wiersza w celu dodania *punktu przerwania*. Punkt przerwania jest znacznik informuje debugera przestanie działać program w tym momencie, tak aby było widać, co dzieje się.

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Usuń wywołanie `ServerInfo.GetHtml` metody i dodaj wywołanie do `@myTime` zmiennej w jego miejscu. To wywołanie Wyświetla bieżącą wartość czasu zwróconego przez nowy wiersz kodu.
4. Naciśnij klawisz F5, aby uruchomić strony w debugerze. Strona zatrzymuje punkt przerwania, które można ustawić. Na poniższej ilustracji przedstawiono strony jak wygląda w edytorze z punktu przerwania (w żółty). 

    ![punkt przerwania debugowania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na pasku narzędzi debugowania, kliknij przycisk **Step Into** przycisku (lub naciśnij klawisz F11) do uruchomienia w następnym wierszu kodu. Każdorazowo po kliknięciu tego przycisku wykonywanie przejście do następnego wiersza kodu.

    ![Krok do przycisku](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Sprawdź wartość `myTime` zmiennej przez umieszczenie wskaźnika myszy nad nim lub sprawdzając wartości wyświetlane w **zmiennych lokalnych** i **stos wywołań** systemu windows. Visual Studio wyświetlona wartość zmiennej.

    ![wartość czasu Pokaż](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Gdy wszystko będzie gotowe, sprawdzając zmiennej i krokowe wykonywanie kodu, naciśnij klawisz F5, aby kontynuować wykonywanie strony bez zatrzymywania w każdym wierszu. Po zakończeniu krokowe całego kodu, w przeglądarce zostanie wyświetlona strona.

Aby dowiedzieć się więcej o debugera i informacje dotyczące debugowania kodu w programie Visual Studio, zobacz [wskazówki: debugowanie stron sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Przy użyciu Razor do projektów platformy ASP.NET MVC programu Visual Studio

Składnia Razor jest również używany często w projekty składnika ASP.NET MVC. MVC to zaawansowane, na podstawie wzorców sposób tworzenia dynamicznych witryn sieci Web. Witryny ASP.NET Web Pages staje się trudne w utrzymaniu, warto wziąć pod uwagę konwersją do aplikacji platformy ASP.NET MVC. Na przykład tworzenia aplikacji MVC, zobacz [wprowadzenie do platformy ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalowanie obsługi dla stron sieci Web ASP.NET w programie Visual Studio 2010

W tej sekcji przedstawiono sposób instalowania narzędzia stron sieci Web ASP.NET i Visual Web Developer Express 2010 dla programu Visual Studio.

1. Jeśli nie masz jeszcze Instalatora platformy sieci Web, należy go pobrać z następującego adresu URL:

    [https://www.microsoft.com/Web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Uruchom Instalatora platformy sieci Web.
3. Kliknij przycisk **produktów** kartę.

    ![Karta WebPI produktów](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Wyszukaj **ASP.NET MVC 4** (dla programu ASP.NET Web Pages 2), a następnie kliknij przycisk **Dodaj**. Produkty te obejmują narzędzia programu Visual Studio do tworzenia witryn sieci Web ASP.NET Razor.

    ![Opcje instalacji WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Kliknij przycisk **zainstalować** do ukończenia instalacji.
