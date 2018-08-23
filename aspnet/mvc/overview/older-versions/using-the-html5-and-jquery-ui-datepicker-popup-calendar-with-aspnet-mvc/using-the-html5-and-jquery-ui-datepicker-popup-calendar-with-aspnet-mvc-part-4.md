---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 58c0ae54e31c2cb4e85da11f7402ea248d9dfb3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755358"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 4
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web platformy ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Dodawanie szablonu do edycji daty

W tej sekcji utworzysz szablon do edycji daty, które będą stosowane w przypadku platformy ASP.NET MVC pojawi się interfejs użytkownika do edycji właściwości modelu, które są oznaczone **data** wyliczenie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu. Szablon będą renderowane tylko datę. czas, nie będą wyświetlane. W szablonie użyjesz [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego, aby umożliwić edytowanie daty.

Aby rozpocząć, otwórz *Movie.cs* pliku i Dodaj [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutem **data** wyliczeniu, aby `ReleaseDate` właściwości, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Ten kod powoduje `ReleaseDate` pola, które mają być wyświetlane bez czasu, zarówno dotyczące szablonów edycji i Wyświetl szablony. Jeśli aplikacja zawiera *date.cshtml* szablonu w *Views\Shared\EditorTemplates* folderu lub *Views\Movies\EditorTemplates* folder, w tym szablonie będzie używany do renderowania dowolne `DateTime` właściwości podczas edytowania. W przeciwnym razie wbudowany system szablonów platformy ASP.NET wyświetli się właściwość jako wartość typu date.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Wybierz link edycji, aby sprawdzić, czy pole wejściowe dla daty wydania są wyświetlane tylko data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

W **Eksploratora rozwiązań**, rozwiń węzeł *widoków* folder, rozwiń węzeł *Shared* folder, a następnie kliknij prawym przyciskiem myszy *Views\Shared\EditorTemplates* folderu.

Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **widoku**. **Dodaj widok** zostanie wyświetlone okno dialogowe.

W **nazwy widoku** wpisz &quot;data&quot;.

Wybierz **Utwórz jako widok częściowy** pole wyboru. Upewnij się, że **Użyj układ strony wzorcowej** i **utworzyć widok silnie typizowane** nie są zaznaczone pola wyboru.

Kliknij przycisk **Dodaj**. *Views\Shared\EditorTemplates\Date.cshtml* szablon został utworzony.

Dodaj następujący kod do *Views\Shared\EditorTemplates\Date.cshtml* szablonu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje modelu, który ma być `DateTime` typu. Mimo że nie trzeba deklarować typ modelu, w trakcie edycji i Wyświetl szablony, jest najlepszym rozwiązaniem, aby zapewnić kompilacji sprawdzania modelu są przekazywane do widoku. (Kolejną korzyścią jest następnie Pobierz technologii IntelliSense dla modelu w widoku w programie Visual Studio). Jeśli nie został zadeklarowany typ modelu, ASP.NET MVC traktuje [dynamiczne](https://msdn.microsoft.com/library/dd264741.aspx) typu i nie ma żadnych kompilacji sprawdzania typu. Jeśli zadeklarujesz modelu, który ma być `DateTime` typu, staje się silnie typizowane.

Drugi wiersz jest po prostu literału kod znaczników HTML, który wyświetla &quot;przy użyciu szablonu data&quot; przed pole daty. Ten wiersz będzie tymczasowo użyć, aby zweryfikować, że używany jest ten szablon daty.

Następny wiersz jest [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) pomocnika, który renderuje `input` pola, które jest polem tekstowym. Trzeci parametr pomocnika używa typ anonimowy w celu ustawienia klasy dla tego pola tekstowego do `datefield` i typ do `date`. (Ponieważ `class` jest zarezerwowana w języku C#, należy użyć `@` znaku ucieczki `class` atrybutu w analizatorze składni języka C#.)

`date` Typ jest typem danych wejściowych języka HTML5, umożliwiająca przeglądarki obsługującej HTML5 do renderowania kontrolki kalendarza HTML5. Później dodasz fragmentów kodu JavaScript, aby zaczepić datepicker jQuery do `Html.TextBox` elementu za pomocą `datefield` klasy.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Sprawdź, czy `ReleaseDate` właściwości w widoku do edycji jest przy użyciu szablonu edycji, ponieważ szablon Wyświetla &quot;przy użyciu szablonu data&quot; tuż przed `ReleaseDate` polu wprowadzania tekstu, jak pokazano na tej ilustracji:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

W przeglądarce Wyświetl źródło strony. (Na przykład, kliknij prawym przyciskiem myszy strony i wybierz **Wyświetl źródło**.) Poniższy przykład przedstawia niektóre z kodu znaczników dla strony, pokazujący `class` i `type` atrybutów w postaci HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Wróć do *Views\Shared\EditorTemplates\Date.cshtml* szablonu i usunąć &quot;przy użyciu szablonu data&quot; znaczników. Ukończone szablon wygląda teraz następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Dodawanie jQuery kalendarza podręcznego selektora daty interfejsu użytkownika za pomocą narzędzia NuGet

W tej sekcji dodasz [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) Kalendarz podręczny do szablonu datę edycji. [Interfejs użytkownika jQuery](http://jqueryui.com/) Biblioteka zapewnia obsługę zaawansowana efekty i możliwe do dostosowania elementy widget animacji. Jest on oparty na podstawie biblioteki JavaScript jQuery. Kalendarza podręcznego selektora daty ułatwia naturalnych wprowadzanie daty przy użyciu kalendarza, a nie numerem ciągu. Kalendarz podręczny także ogranicza użytkownikom dostęp do dat prawne — zwykły tekst wpisu dla daty, będzie można wprowadzić podobny do `2/33/1999` (lutego 33rd, 1999), ale [selektora daty interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) kalendarza podręcznego nie zezwala.

Najpierw musisz zainstalować biblioteki interfejsu użytkownika jQuery. W tym celu użyjemy NuGet, czyli Menedżer pakietów, który znajduje się w wersji dodatku SP1 dla programu Visual Studio 2010 i Visual Web Developer.

W Visual Web Developer z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie wybierz **Zarządzaj pakietami NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Uwaga: Jeśli **narzędzia** nie wyświetla menu **Menedżer pakietów biblioteki** polecenia, musisz zainstalować NuGet, postępując zgodnie z instrukcjami wyświetlanymi [Instalowanie systemu NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) strony Witryna sieci Web NuGet.   
  
Jeśli używasz programu Visual Studio zamiast programu Visual Web Developer, z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** , a następnie wybierz **Dodaj odwołanie do pakietu biblioteki**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

W **MVCMovie — Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **Online** karcie po lewej stronie, a następnie wprowadź &quot;jQuery.UI&quot; w polu wyszukiwania. Wybierz pozycję "j" **zapytania elementy widget: selektora daty interfejsu użytkownika**, a następnie wybierz **zainstalować** przycisku.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet dodaje te wersje do debugowania i zminimalizowany wersje Core interfejsu użytkownika jQuery i selektora daty interfejsu użytkownika jQuery do projektu:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Uwaga: Wersje do debugowania (plików bez *. min.js* rozszerzenia) są przydatne do debugowania, ale witryny produkcyjnej, można dołączyć tylko wersje zminimalizowana.

Użyć selektora daty jQuery, musisz utworzyć skrypt jQuery, który będzie obsługiwać widżet kalendarza do edycji szablonu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *skrypty* i wybierz polecenie **Dodaj**, następnie **nowy element**, a następnie **JScript Plik**. Nadaj plikowi nazwę *DatePickerReady.js*.

Dodaj następujący kod do *DatePickerReady.js* pliku:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Jeśli nie znasz jQuery, Oto zwięzłe wyjaśnienie, jak to działa: pierwszy wiersz jest &quot;jQuery gotowe&quot; funkcji, która jest wywoływana, gdy wszystkie elementy modelu DOM na stronie zostały załadowane. Drugi wiersz wybiera wszystkie elementy modelu DOM, które mają taką nazwę klasy `datefield`, następnie wywołuje `datepicker` funkcji dla każdego z nich. (Należy pamiętać, że dodano `datefield` klasy *Views\Shared\EditorTemplates\Date.cshtml* szablonu we wcześniejszej części tego samouczka.)

Następnie otwórz *Views\Shared\\_Layout.cshtml* pliku. Należy dodać odwołania do następujących plików, które są wszystkie wymagane tak, aby można było używać selektor daty:

- *Content/Themes/Base/jquery.UI.Core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/Base/jquery.UI.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

W poniższym przykładzie pokazano rzeczywisty kod należy dodać u dołu `head` element *Views\Shared\\_Layout.cshtml* pliku.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Pełne `head` sekcji jest następująca:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[Zawartości obiekt pomocnika adresu URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) metoda konwertuje ścieżka zasobu na ścieżkę bezwzględną. Należy użyć `@URL.Content` poprawnie odwołanie do tych zasobów, gdy aplikacja jest uruchomiona na serwerze IIS.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Wybierz łącze edycji, a następnie umieść punkt wstawiania do **ReleaseDate** pola. Kalendarz podręczny interfejsu użytkownika jQuery jest wyświetlany.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Tak, jak większości formanty jQuery datepicker umożliwia dostosowanie go często. Aby uzyskać informacje, zobacz [dostosowanie programu Visual: Projektowanie motyw interfejsu użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) na [interfejs użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/) lokacji.

### <a name="supporting-the-html5-date-input-control"></a>Obsługa kontrolki wprowadzania daty HTML5

Ponieważ coraz więcej przeglądarek obsługuje HTML5, będziesz chciał użyć natywnej obsłudze języka HTML5, dane wejściowe, takie jak `date` elementu wejściowego i nie używać kalendarza interfejsu użytkownika jQuery. Można dodać logikę do aplikacji automatycznie korzystanie z kontrolek HTML5 Jeśli przeglądarka obsługuje je. Aby to zrobić, Zastąp zawartość *DatePickerReady.js* pliku następującym kodem:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Pierwszy wiersz ten skrypt używa Modernizr, aby sprawdzić, czy dane wejściowe daty HTML5 jest obsługiwana. Jeśli nie jest obsługiwany, selektora daty interfejsu użytkownika jQuery jest podłączany zamiast tego. ([Modernizr](http://www.modernizr.com/docs/) to biblioteka JavaScript typu open source, który wykrywa dostępność implementacji języka HTML5 i CSS3. Modernizr jest umieszczana w żadnych nowych projektach programu ASP.NET MVC, utworzone).

Po wprowadzeniu tej zmiany, można ją przetestować przy użyciu przeglądarki, która obsługuje protokół HTML5, takie jak Opera 11. Uruchom aplikację za pomocą przeglądarki zgodnej z HTML5 i zmodyfikuj wpis filmu. Kontrolki daty HTML5 jest używana zamiast kalendarza podręcznego interfejsu użytkownika jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Ponieważ nowe wersje przeglądarek wdrażają przyrostowo HTML5, dobra metoda teraz jest dodawanie kodu do witryny sieci Web, który obsługuje szeroką gamę obsługą języka HTML5. Na przykład, bardziej niezawodne *DatePickerReady.js* skryptów znajdują się poniżej umożliwiająca przeglądarek obsługi lokacji, które obsługują tylko częściowo kontrolę daty HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Ten skrypt wybiera HTML5 `input` elementów typu `date` , nie obsługują w pełni kontrolę daty HTML5. Dla tych elementów przechwytuje się kalendarza podręcznego interfejsu użytkownika jQuery, a następnie zmienia `type` atrybut z `date` do `text`. Zmieniając `type` atrybut z `date` do `text`, wyeliminowania częściową obsługę Data HTML5. Jeszcze bardziej niezawodny *DatePickerReady.js* skrypt znajduje się w temacie [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Dodawanie daty dopuszcza wartości null do szablonów

Jeśli użyj jednej z istniejących szablonów daty i przekazać wartość typu date o wartości null, zostanie wyświetlony błąd czasu wykonywania. Aby szablony Data bardziej niezawodne, zmienisz ich Obsługa wartości zerowych. Aby zapewnić obsługę daty dopuszczającego wartość null, zmiany kodu w *Views\Shared\DisplayTemplates\DateTime.cshtml* do następującego:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Ten kod zwraca pusty ciąg, jeśli model jest **null**.

Zmień kod w *Views\Shared\EditorTemplates\Date.cshtml* pliku do następującego:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Kiedy ten kod zadziała, jeśli model nie ma wartość null, model `DateTime` wartość jest używana. Jeśli model ma wartość null, w zamian jest używana bieżąca data.

### <a name="wrapup"></a>Wrapup

W tym samouczku ma omówione podstawy pomocników szablonu platformy ASP.NET, a dowiesz się, jak używać kalendarza podręcznego selektora daty interfejsu użytkownika jQuery w aplikacji ASP.NET MVC. Aby uzyskać więcej informacji Wypróbuj następujące zasoby:

- Dla informacji o lokalizacji, zobacz blog firmy Rajeesh [JQueryUI Datepicker we wzorcu ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Aby dowiedzieć się, interfejs użytkownika jQuery, zobacz [interfejs użytkownika jQuery](http://docs.jquery.com/UI).
- Aby uzyskać informacje o sposobie lokalizowania kontrolki datepicker, zobacz [interfejsu użytkownika/Datepicker/lokalizacja](http://docs.jquery.com/UI/Datepicker/Localization).
- Aby uzyskać więcej informacji na temat szablonów platformy ASP.NET MVC, zobacz serię wpisów w blogu Brad Wilson na [szablony programu ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Mimo że serii został napisany dla programu ASP.NET MVC 2, materiał nadal obowiązuje ograniczenie dla bieżącej wersji platformy ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
