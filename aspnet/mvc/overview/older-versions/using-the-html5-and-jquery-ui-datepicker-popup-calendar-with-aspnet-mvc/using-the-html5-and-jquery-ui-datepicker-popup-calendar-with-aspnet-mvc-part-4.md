---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: c6df727107b0a045341badefbf99eec773cd4eff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 4
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web programu ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Dodać szablon do edycji dat

W tej sekcji utworzysz szablon do edycji daty, które będą stosowane, gdy ASP.NET MVC wyświetla interfejsu użytkownika do edycji właściwości modelu, które są oznaczone ikoną z **data** wyliczenie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attibute. Szablon będą zawierały tylko data; czas nie będą wyświetlane. W szablonie użyjesz [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego w sposób, aby edytować daty.

Aby rozpocząć, otwórz *Movie.cs* plik i dodać [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutem **data** wyliczeniu, aby `ReleaseDate` właściwości, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Powoduje, że ten kod `ReleaseDate` pola, które mają być wyświetlane bez czasu w obu szablonów edycji i Wyświetl szablony. Jeśli aplikacja zawiera *date.cshtml* szablonu w *Views\Shared\EditorTemplates* folderu lub *Views\Movies\EditorTemplates* folder, w tym szablonie będzie używany do renderowania żadnego `DateTime` właściwości podczas edytowania. W przeciwnym razie wbudowanych systemu tworzenia szablonów ASP.NET wyświetli właściwość jako data.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Wybierz łącza edycji, aby sprawdzić, czy pole wejściowe dla Data wydania jest wyświetlany tylko data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

W **Eksploratora rozwiązań**, rozwiń węzeł *widoków* folder, rozwiń węzeł *Shared* folder, a następnie kliknij prawym przyciskiem myszy *Views\Shared\EditorTemplates* folderu.

Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **widoku**. **Dodaj widok** zostanie wyświetlone okno dialogowe.

W **nazwy widoku** wpisz &quot;data&quot;.

Wybierz **Utwórz jako widok częściowy** pole wyboru. Upewnij się, że **Użyj układ strony wzorcowej** i **utworzyć widok jednoznacznie** nie są zaznaczone pola wyboru.

Kliknij przycisk **Dodaj**. *Views\Shared\EditorTemplates\Date.cshtml* tworzenia szablonu.

Dodaj następujący kod do *Views\Shared\EditorTemplates\Date.cshtml* szablonu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje modelu, który ma być `DateTime` typu. Mimo że nie należy zadeklarować typ modelu w edycji i Wyświetl szablony, najlepszym rozwiązaniem jest, aby pobrać kompilacji sprawdzanie modelu są przekazywane do widoku. (Kolejna korzyść związana jest następnie Uzyskaj IntelliSense dla modelu w widoku w programie Visual Studio). Jeśli nie jest zadeklarowany jako typ modelu, ASP.NET MVC uzna [dynamiczne](https://msdn.microsoft.com/library/dd264741.aspx) typu i nie ma żadnych kompilacji sprawdzania typu. W przypadku modelu, który ma być `DateTime` typu, staje się silnie typizowane.

Drugi wiersz jest tylko literału kod znaczników HTML, który wyświetla &quot;przy użyciu szablonu data&quot; przed pole daty. Ten wiersz będzie używany tymczasowo, aby sprawdzić, czy ta data szablon jest używany.

Następnego wiersza jest [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) pomocnika, który renderuje `input` pola, które jest polem tekstowym. Trzeci parametr pomocnika używa typu anonimowego można ustawić klasy dla pola tekstowego `datefield` i typ do `date`. (Ponieważ `class` jest zarezerwowana w języku C#, musisz użyć `@` znaku ucieczki `class` atrybutu w analizatorze składni języka C#.)

`date` Typ jest typ danych wejściowych HTML5, umożliwiającą przeglądarki obsługującej HTML5 do renderowania formantu kalendarza HTML5. Później należy dodać niektóre JavaScript, aby przyłączyć selektora daty jQuery do `Html.TextBox` przy użyciu elementu `datefield` klasy.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Sprawdź, czy `ReleaseDate` właściwości w widoku edycji jest przy użyciu szablonu edycji, ponieważ Wyświetla szablon &quot;przy użyciu szablonu data&quot; tuż przed `ReleaseDate` tekst wejściowy pola, jak pokazano w tym obrazie:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

W przeglądarce Wyświetl źródło strony. (Na przykład, kliknij prawym przyciskiem myszy strony i wybierz **Wyświetl źródło**.) Poniższy przykład przedstawia niektóre z kodu znaczników dla strony, pokazujący `class` i `type` atrybuty HTML renderowanych.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Wróć do *Views\Shared\EditorTemplates\Date.cshtml* szablon i Usuń &quot;przy użyciu szablonu data&quot; znaczników. Teraz zakończone szablonu wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Dodawanie jQuery kalendarza podręcznego selektora daty interfejsu użytkownika przy użyciu narzędzia NuGet

W tej sekcji dodasz [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego do szablonu datę edycji. [Interfejsu użytkownika jQuery](http://jqueryui.com/) biblioteki zapewnia obsługę zaawansowana efekty i dostosowywalne elementy widget animacji. Jest on oparty na bibliotece jQuery JavaScript. Kalendarza podręcznego selektora daty ułatwia fizycznych za pomocą kalendarza zamiast wprowadzania ciąg daty wprowadzenia. Kalendarza podręcznego ogranicza także użytkownikom prawne dat — zwykły tekst wpis daty będzie można wprowadzić wyglądać mniej więcej tak `2/33/1999` (lutego 33rd, 1999), ale [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego nie zezwala.

Najpierw należy zainstalować biblioteki interfejsu użytkownika jQuery. W tym celu użyjesz NuGet, który jest Menedżera pakietów, który znajduje się w wersji dodatku SP1 dla programu Visual Studio 2010 oraz Visual Web Developer.

W programie Visual Web Developer z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie wybierz **Zarządzaj pakietami NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Uwaga: Jeśli **narzędzia** nie zostanie wyświetlone menu **Menedżer pakietów biblioteki** polecenia, musisz zainstalować NuGet zgodnie z instrukcjami [instalowania NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) strony Witryna sieci Web NuGet.   
  
Jeśli używasz programu Visual Studio zamiast programu Visual Web Developer, z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie wybierz **Dodaj odwołanie do biblioteki pakietu**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

W **MVCMovie — Zarządzanie pakietami NuGet** okno dialogowe, kliknij przycisk **Online** karcie po lewej stronie, a następnie wprowadź &quot;jQuery.UI&quot; w polu wyszukiwania. Wybierz j **selektora zapytania interfejsu użytkownika elementy widget: daty**, a następnie wybierz pozycję **zainstalować** przycisku.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet dodaje te wersje debugowania i zminimalizowany Core interfejsu użytkownika jQuery i selektora daty interfejsu użytkownika jQuery do projektu:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Uwaga: Wersje debugowania (plików bez *. min.js* rozszerzenia) są przydatne do debugowania, ale w lokacji produkcyjnej, można dołączyć tylko wersje zminimalizowany.

Użyć jQuery wyboru daty, należy utworzyć skrypt jQuery, który posłuży się widżet kalendarza do edycji szablonu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *skryptów* i wybierz polecenie **Dodaj**, następnie **nowy element**, a następnie **języka JScript Plik**. Nadaj nazwę plikowi *DatePickerReady.js*.

Dodaj następujący kod do *DatePickerReady.js* pliku:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Jeśli nie masz doświadczenia w obsłudze jQuery, w tym miejscu jest to jest krótki opis: jest to pierwszy wiersz &quot;jQuery gotowe&quot; funkcji, które jest wywoływane, gdy wszystkie elementy modelu DOM strony zostały załadowane. Drugi wiersz wybiera wszystkie elementy modelu DOM, które mają nazwę klasy `datefield`, następnie wywołuje `datepicker` funkcja dla każdego z nich. (Należy pamiętać, że dodane `datefield` klasy do *Views\Shared\EditorTemplates\Date.cshtml* szablonu wcześniej w samouczku.)

Następnie otwórz folder *Views\Shared\\_Layout.cshtml* pliku. Konieczne jest dodanie odwołania do następujących plików, które są wszystkie wymagane, dzięki czemu można użyć selektora dat:

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

W poniższym przykładzie pokazano kod należy dodać u dołu `head` element *Views\Shared\\_Layout.cshtml* pliku.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Pełną `head` sekcji jest następujący:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[Zawartości Pomocnika adresu URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) metoda konwertuje ścieżka zasobu na ścieżkę bezwzględną. Należy użyć `@URL.Content` poprawnie odwołanie do tych zasobów, gdy aplikacja jest uruchomiona na serwerze IIS.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Wybierz łącze edycji, a następnie umieść punkt wstawiania w **ReleaseDate** pola. Kalendarza podręcznego interfejsu użytkownika jQuery jest wyświetlany.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Podobnie jak większość formantów jQuery selektora daty pozwala dostosować go często. Informacje, zobacz [dostosowywania Visual: Projektowanie motyw interfejsu użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) na [interfejsu użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/) lokacji.

### <a name="supporting-the-html5-date-input-control"></a>Obsługa kontrolki wprowadzania daty HTML5

Jak więcej przeglądarki obsługują HTML5, należy użyć natywnego HTML5, wprowadzania, takich jak `date` elemencie wejściowym, a nie używać kalendarza interfejsu użytkownika jQuery. Możesz dodać logikę do aplikacji, aby automatycznie używać formantów HTML5, jeśli przeglądarka obsługuje je. Aby to zrobić, Zamień zawartość *DatePickerReady.js* pliku następującym kodem:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Pierwszy wiersz ten skrypt używa Modernizr do Sprawdź, czy data HTML5 w danych wejściowych jest obsługiwana. Jeśli nie jest obsługiwana, selektora daty interfejsu użytkownika jQuery jest podłączonymi zamiast tego. ([Modernizr](http://www.modernizr.com/docs/) biblioteka języka JavaScript open source, która wykrywa dostępność natywnych implementacji HTML5 i CSS3. Modernizr znajduje wszystkie nowe projekty składnika ASP.NET MVC, utworzonych przez Ciebie.)

Po wprowadzeniu tej zmiany należy przetestować go za pomocą przeglądarki, która obsługuje protokół HTML5, takie jak Opera 11. Uruchom aplikację za pomocą przeglądarki zgodnej HTML5 i edytować wpis filmu. Kontrola daty HTML5 służy zamiast kalendarza podręcznego interfejsu użytkownika jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Nowe wersje przeglądarek przyrostowo wdrażania HTML5, dobrym podejście obecnie jest dodawanie kodu do witryny sieci Web, uwzględniający szerokiej gamy HTML5 pomocy technicznej. Na przykład bardziej niezawodne *DatePickerReady.js* skryptu są wyświetlane poniżej umożliwiająca przeglądarek obsługi lokacji obsługujące tylko częściowo kontroli daty HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Ten skrypt wybiera HTML5 `input` elementów typu `date` które nie obsługują w pełni kontroli daty HTML5. Dla tych elementów przechwytuje kalendarza podręcznego interfejsu użytkownika jQuery, a następnie zmienia `type` atrybutu z `date` do `text`. Zmieniając `type` atrybutu z `date` do `text`, wyeliminowania częściowej obsługi Data HTML5. Bardziej niezawodna *DatePickerReady.js* skryptu można znaleźć w folderze [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Dodawanie daty wartości null do szablonów

Jeśli Użyj jednego z istniejących szablonów daty i przekazać null daty, zostanie wyświetlony błąd w czasie wykonywania. Aby szablony Data bardziej niezawodne, użytkownik będzie je zmienić zgodnie ze Obsługa wartości zerowych. Aby obsługiwać daty wartości null, Zmień kod w *Views\Shared\DisplayTemplates\DateTime.cshtml* do następującego:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Ten kod zwraca pusty ciąg, jeśli model jest **null**.

Zmień kod w *Views\Shared\EditorTemplates\Date.cshtml* pliku do następującego:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Gdy ten kod jest uruchamiany, jeśli model nie ma wartości null, modelu `DateTime` wartość jest używana. Jeśli model ma wartość null, zamiast niego jest używana bieżącą datę.

### <a name="wrapup"></a>Wrapup

W tym samouczku pokrywającego podstawy pomocników szablonu platformy ASP.NET i przedstawiono sposób użycia kalendarza podręcznego selektora daty interfejsu użytkownika jQuery w aplikacji platformy ASP.NET MVC. Aby uzyskać więcej informacji Wypróbuj następujące zasoby:

- Uzyskać informacji o lokalizacji, zobacz blog firmy Rajeesh [selektora daty JQueryUI na platformie ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Informacje dotyczące interfejsu użytkownika jQuery, zobacz [interfejsu użytkownika jQuery](http://docs.jquery.com/UI).
- Aby uzyskać informacje na temat do zlokalizowania formant selektora daty, zobacz [interfejsu użytkownika/selektora daty/lokalizacja](http://docs.jquery.com/UI/Datepicker/Localization).
- Aby uzyskać więcej informacji na temat szablonów ASP.NET MVC, zobacz serii blogu Brada Wilsona na [ASP.NET MVC 2 szablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Mimo że serii dotyczy programu ASP.NET MVC 2, materiałów nadal ma zastosowanie do bieżącej wersji programu ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
