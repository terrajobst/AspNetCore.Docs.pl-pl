---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Podaj CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) danych tworzą Obsługa wpis | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 5 pokazano, jak wykonać klasy DinnersController nasze dalsze przez włączanie obsługi edycji, tworzenie i usuwanie kolacji z nim również.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876751"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Podaj CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) danych tworzą wpis pomocy technicznej
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 5 z bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 5 pokazano, jak wykonać klasy DinnersController nasze dalsze przez włączanie obsługi edycji, tworzenie i usuwanie kolacji z nim również.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner krok 5: Tworzenie, aktualizowanie i usuwanie scenariusze formularza

Wprowadzeniu wprowadzone kontrolery i widoki i jak ich używać do implementowania listę/szczegóły obsługę kolacji w lokacji objętych. Nasze następnym krokiem jest podjęcie dalszych klasy Nasze DinnersController i włączyć obsługę edytowanie, tworzenie i usuwanie kolacji również z nim.

### <a name="urls-handled-by-dinnerscontroller"></a>Adresy URL obsługiwany przez DinnersController

Dodaliśmy wcześniej metod akcji do DinnersController wprowadzonym obsługę adresów URL: */Dinners* i */Dinners/szczegóły / [id]*.

| **ADRES URL** | **VERB** | **Cel** |
| --- | --- | --- |
| */Dinners/* | GET | Wyświetl listę nadchodzących kolacji HTML. |
| */Dinners/szczegóły / [id]* | GET | Wyświetlanie szczegółów dotyczących określonego obiad. |

Teraz dodamy metod akcji, aby zaimplementować trzy dodatkowe adresy URL: <em>/Dinners/Edit / [id], / kolacji/Create,</em>i<em>/Dinners/Delete / [id]</em>. Te adresy URL zostanie włączona obsługa edycji istniejących kolacji, tworzenie nowych kolacji i usuwanie kolacji.

Obsługujemy interakcji zlecenie HTTP GET i POST protokołu HTTP z tych nowych adresów URL. Żądania HTTP GET do tych adresów URL wyświetli początkowej widok HTML danych (formularza wypełniane przy użyciu danych obiad w przypadku "edit", pusty formularz w przypadku "Utwórz" i ekran potwierdzenia usunięcia w przypadku "delete"). Żądania HTTP POST do tych adresów URL będzie save/aktualizowania/usuwania danych obiad w naszym DinnerRepository (i z tego miejsca w bazie danych).

| **ADRES URL** | **VERB** | **Cel** |
| --- | --- | --- |
| */Dinners/edit / [id]* | GET | Wyświetlanie edytowalnego formularza HTML wypełniane przy użyciu danych obiad. |
| POST | Zapisz zmiany formularza dla określonego obiad do bazy danych. |
| */Dinners/Create* | GET | Wyświetlanie pustego formularza HTML, który pozwala użytkownikom na definiowanie nowych kolacji. |
| POST | Utwórz nowy obiad i zapisz go w bazie danych. |
| */Dinners/delete / [id]* | GET | Wyświetl usuwanie ekran potwierdzenia. |
| POST | Usuwa określony obiad z bazy danych. |

### <a name="edit-support"></a>Edytuj pomocy technicznej

Zacznijmy zaimplementowanie scenariusza "edit".

#### <a name="the-http-get-edit-action-method"></a>Metody akcji HTTP GET edycji

Zaczniemy zaimplementowanie HTTP "GET" zachowanie naszych edycji metody akcji. Ta metoda jest wywoływana, gdy */Dinners/Edit / [id]* wymagany jest adres URL. Implementacja naszej będą wyglądać jak:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Powyższy kod używa DinnerRepository można pobrać obiektu obiad. Szablon widoku przy użyciu obiektu obiad następnie renderowania. Ponieważ firma Microsoft nie zostały jawnie przekazany nazwę szablonu w celu *View()* metody pomocniczej, zostanie użyty oparte na konwencji domyślnej ścieżki rozwiązywać Wyświetl szablon: /Views/Dinners/Edit.aspx.

Teraz Utwórzmy ten szablon widoku. Spowoduje to wykonanie prawym przyciskiem myszy w obrębie metody edycji i wybierając polecenie "Dodaj widok" z menu kontekstowego:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przekazywane do naszej szablon widoku jako swojego modelu obiektu obiad i chce automatycznie szkieletu "Edytuj w" szablonie:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Kliknięcie przycisku "Dodaj", Visual Studio Dodaj nowy plik szablonu widoku "Edit.aspx" firmie Microsoft w ramach katalogu "\Views\Dinners". Otworzy się również się nowy szablon widoku "Edit.aspx" edytora kodu-— wypełniane przy użyciu początkowej "Edytuj" szkieletu implementację podobnie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Umożliwia zmianę kilka wygenerowany domyślny "Edytuj" szkieletu i aktualizację szablonu widok edycji poniżej zawartości (co spowoduje usunięcie kilka właściwości, które nie chcemy ujawniać):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Firma Microsoft uruchamiania aplikacji i żądania *"/ 1/Edit/kolacji"* adresu URL zostanie wyświetlone następujące strony:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Kod znaczników HTML, generowane przez naszych widoku wygląda jak poniżej. Jest standardowe HTML — z &lt;formularza&gt; element, który wykonuje akcję POST protokołu HTTP do */Dinners/Edit/1* adres URL po "Zapisz" &lt;wprowadzania type = "submit" /&gt; przycisk jest naciśnięty. HTML &lt;wprowadzania type = "text" /&gt; element został danych wyjściowych dla każdej właściwości można edytować:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() i Html.TextBox() metody pomocnika kodu Html

Nasze "Edit.aspx" Wyświetl szablon korzysta kilka metod "Pomocnika kodu Html": Html.ValidationSummary(), Html.BeginForm() Html.TextBox() i Html.ValidationMessage(). Oprócz generowania kod znaczników HTML dla nas, te metody pomocnika zapewnienia obsługi błędów wbudowanych i weryfikacja obsługi.

##### <a name="htmlbeginform-helper-method"></a>Metoda pomocnicza Html.BeginForm()

Metoda pomocnicza Html.BeginForm() jest co danych wyjściowych HTML &lt;formularza&gt; element w naszym znaczników. W naszym Edit.aspx Wyświetl szablon można zauważyć, że jest stosowane C# "" instrukcję using przy użyciu tej metody. Otwórz nawias klamrowy wskazuje na początek &lt;formularza&gt; zawartości i zamykający nawias klamrowy jest, co wskazuje koniec &lt;/form&gt; elementu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternatywnie Jeśli okaże się instrukcji "using" podejścia nienaturalnej scenariusz podobny do tego, możesz użyć połączenia Html.BeginForm() i Html.EndForm() (która jest samo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Wywoływanie Html.BeginForm() bez parametrów spowoduje jego dane wyjściowe element formularza, który wykonuje akcję POST protokołu HTTP do adresu URL bieżącego żądania. Oznacza to, dlaczego naszych widoku edycji generuje *&lt;akcji formularza = "/ kolacji/Edit/1" metody = "post"&gt;* elementu. Firma Microsoft może również przekazano jawne parametry do Html.BeginForm() czy możemy post do innego adresu URL.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() helper method

Nasze widoku Edit.aspx używa metody pomocnika Html.TextBox() do wyjściowego &lt;wprowadzania type = "text" /&gt; elementy:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Metoda Html.TextBox() powyżej przyjmuje jeden parametr —, który jest używany do określenia atrybutów id/nazwa z &lt;wprowadzania type = "text" /&gt; element danych wyjściowych, a także aby wypełnić pole tekstowe wartości z właściwości modelu. Na przykład obiekt obiad możemy przekazany do widoku edycji ma wartość właściwości "Title" "Prognoz .NET" i tak wywołanie dane wyjściowe w naszym metody Html.TextBox("Title"): *&lt;wejściowy identyfikator = "Title" name = "Title" type = wartość "text" = "Prognoz .NET" /&gt;*.

Firma Microsoft można również użyć pierwszy parametr Html.TextBox(), aby określić identyfikator/nazwę elementu, a następnie jawnie przekazać wartości do użycia jako drugiego parametru:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Często chcemy wykonać niestandardowe formatowanie na wartość, która jest danych wyjściowych. Metoda statyczna String.Format() wbudowane .NET jest przydatne w przypadku tych scenariuszy. Szablon widoku naszych Edit.aspx jest za pomocą tej do formatowania wartości EventDate (która jest typu DateTime) tak, aby nie wyświetla czas w sekundach:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Trzeci parametr Html.TextBox() opcjonalnie może służyć do wyjściowego dodatkowe atrybuty kodu HTML. Poniższy fragment kodu przedstawia sposób renderowania dodatkowe rozmiar = "30" atrybutu i klasę = "mycssclass" atrybutu na &lt;wprowadzania type = "text" /&gt; elementu. Należy zwrócić uwagę, jak są anulowanie nazwę przy użyciu atrybutu klasy "@" character because "klasy" jest zarezerwowanym słowem kluczowym języka C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementacja metody akcji edycji POST protokołu HTTP

Mamy teraz wersji HTTP GET naszych zaimplementowano metody akcji edycji. Gdy użytkownik zażąda */Dinners/Edit/1* adres URL otrzymują stronę HTML, takie jak następujące:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Naciśnięcie przycisku "Zapisz" powoduje, że post formularza */Dinners/Edit/1* adres URL, i przesyła HTML &lt;wejściowych&gt; wartości przy użyciu zlecenie HTTP POST formularza. Umożliwia teraz implementuje zachowanie HTTP POST naszych metody akcji edycji — która obsłuży zapisywania obiad.

Rozpocznie się przez dodanie przeciążonej metody akcji "Edytuj" do naszej DinnersController, który ma atrybut "AcceptVerbs" z który wskazuje, że obsługi scenariuszy HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Gdy atrybut [AcceptVerbs] jest stosowany do metod akcji przeciążona, ASP.NET MVC automatycznie obsługuje wysyłania żądań do metody odpowiednie działania w zależności od przychodzące zlecenie HTTP. Żądania HTTP POST do <em>/Dinners/Edit / [id]</em> adresy URL zostanie umieszczona na powyższej metody edycji podczas wszystkich innych żądaniach zlecenie HTTP <em>/Dinners/Edit / [id]</em>przejdzie adresy URL pierwsza metodę edycji (które wprowadziliśmy atrybut nie jest [AcceptVerbs]).

| **Temat po stronie: Dlaczego rozróżnianie za pomocą polecenia HTTP?** |
| --- |
| Użytkownik może poprosić o — dlaczego są możemy przy użyciu jednego adresu URL i rozróżnianie jego zachowanie za pośrednictwem zlecenie HTTP? Dlaczego nie można mieć dwa oddzielne adresy URL do obsługi ładowania i zapisywania zmian edycji? Na przykład: /Dinners/Edit / [id] do wyświetlania formularza początkowego i /Dinners/Save / [id] do obsługi post formularza je zapisać? Wadą interfejsu z dwóch oddzielnych adresy URL publikowania jest, że w przypadku, gdy firma Microsoft ogłosić /Dinners/Save/2 i następnie trzeba ponownie wyświetlić formularza HTML ze względu na błąd danych wejściowych, użytkownik końcowy zakończą się o 2-kolacji/Zapisz adres URL na pasku adresu w przeglądarce (od momentu, która jest adres URL publikowanego formularza). Jeśli użytkownik końcowy zakładki na tej stronie redisplayed do ich listy ulubionych w przeglądarce, lub kopiowania/past adres URL, wysłanie wiadomości e-mail do znajomego one zakończą się zapisywania adresów URL, które nie będą działać w przyszłości (ponieważ tego adresu URL zależy od wartości post). W przypadku wystawianego pojedynczego adresu URL (takich jak: /Dinners/Edit/[id]) i rozróżnianie przetwarzania go przez zlecenie HTTP, jest bezpieczne dla użytkowników końcowych do zakładki edycji strony i/lub wysłać adres URL do innych osób. |

#### <a name="retrieving-form-post-values"></a>Pobieranie wartości Post formularza

Istnieją różne sposoby, które firma Microsoft mogą uzyskiwać dostęp do zaksięgowany parametrów formularza w naszym "Edytuj" metodą HTTP POST. Jednym z podejść proste jest używać tylko właściwość Request w klasie podstawowej kontrolera dostępu do kolekcji formularza i bezpośrednio pobrać opublikowanych wartości:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Powyższe podejście jest jednak nieco verbose, szczególnie w przypadku, gdy możemy dodać logikę obsługi błędów.

A lepiej rozwiązań dla tego scenariusza jest wykorzystanie przez wbudowane *UpdateModel()* metody pomocnika dla klasy podstawowej kontrolera. Obsługuje ona aktualizowanie właściwości obiektu, który go za pomocą przychodzące parametrów formularza jest przekazywana. Używa odbicia w celu określenia nazwy właściwości w obiekcie, a następnie automatycznie konwertuje i przypisuje wartości do nich na podstawie wartości wejściowych przesłany przez klienta.

Możemy użyć metody UpdateModel(), aby uprościć naszych HTTP POST Edytuj akcji przy użyciu tego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Firma Microsoft może teraz odwiedzić */Dinners/Edit/1* adres URL, a następnie zmień tytuł naszych obiad:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Kliknięcie przycisku "Zapisz", firma Microsoft będzie wykonywać post formularza do naszej akcji edycji, a zaktualizowane wartości zostaną utrwalone w bazie danych. Firma Microsoft będzie następnie przekierowywany do adresu URL szczegóły na obiad, (co spowoduje wyświetlenie nowo zapisanych wartości):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Obsługa błędów edycji

Nasze bieżącego działania implementacji HTTP POST drobne — z wyjątkiem sytuacji gdy występują błędy.

Gdy użytkownik tworzy błędu edycji formularza, musimy upewnić się, że formularza zostanie wyświetlony ponownie szczegółowy komunikat o błędzie, który poprowadzi ich, aby go rozwiązać. Dotyczy to przypadków, gdy użytkownik końcowy zapisuje niepoprawne dane wejściowe (na przykład: ciągu daty źle sformułowane), jak również przypadków, gdy format wejściowy jest prawidłowy, ale stanowi to naruszenie reguły biznesowej. Jeśli błędy występują się, że formularz powinien zachować dane wejściowe użytkownika wprowadzona, dzięki czemu nie trzeba ręcznie uzupełnić swoje zmiany. Ten proces należy powtórzyć tyle razy, w razie potrzeby zakończenia formularza pomyślnie.

Platforma ASP.NET MVC zawiera niektóre nieuprzywilejowany wbudowane funkcje, które łatwo Obsługa błędów i ponowne wyświetlanie formularza. Aby zobaczyć te funkcje w akcji teraz zaktualizować metody akcji naszych edycji z następującym kodem:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Powyższy kod jest podobny do naszej poprzedniej implementacji —, z tą różnicą, że firma Microsoft są teraz otaczającym bloku try/catch obsługi błędów pracę. Jeśli wystąpi wyjątek podczas wywoływania metody UpdateModel() lub możemy spróbuj i Zapisz DinnerRepository (który zgłosi wyjątek, jeśli obiekt obiad, który próbujemy zapisywania jest nieprawidłowy z powodu naruszenia reguły w modelu), będzie naszych bloku catch błąd obsługi wykonanie. Znajdujące się w nim możemy pętli wszelkie naruszenia reguły, które istnieją w obiekcie obiad i dodaj je do obiektu ModelState (co omówiono wkrótce). Następnie możemy ponownie wyświetlić widoku.

Aby wyświetlić to działa teraz ponownie uruchomić aplikację, Edytuj obiad i zmień, aby mieć pustym tytułem EventDate "BOGUS" i używać numeru telefonu na Brytyjski o wartości kraju USA. Gdy firma Microsoft, naciśnij przycisk "Zapisz" Nasze metody HTTP POST Edytuj nie będzie można zapisać obiad (ponieważ występują błędy) i zostanie ponownie wyświetlić formularza:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Naszej aplikacji ma środowisko zadowalający błędu. Elementy tekstu nieprawidłowe dane wejściowe są wyróżnione kolorem czerwonym i komunikatów o błędach są wyświetlane użytkownikowi końcowemu o nich. Formularz również zachowuje dane wejściowe użytkownika wprowadzona — tak, aby nie trzeba niczego uzupełniania.

Jak to zrobić może poprosić o zostało to przeprowadzone? Jak pól tekstowych Tytuł, EventDate i ContactPhone wyróżnić się na czerwono i wiedzieć zwracać wartości pierwotnie wprowadzonego użytkownika? I jak wyświetlane komunikaty o błędach pobrać na liście u góry? Dobre wieści jest czy nie dzieje przez magic — zamiast został ponieważ użyliśmy niektóre wbudowane funkcje platformy ASP.NET MVC, które łatwo sprawdzania poprawności danych wejściowych i błąd obsługi scenariuszy.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Opis ModelState i sprawdzanie poprawności metody pomocnika kodu HTML

Klasy kontrolera mają kolekcji właściwości "ModelState", który umożliwia wskazująca, czy występują błędy były przekazywane do widoku obiektu modelu. Zapisy błędów w kolekcji ModelState zidentyfikować nazwę właściwości modelu do wydawania (na przykład: "Title", "EventDate" lub "ContactPhone") i umożliwić komunikat o błędzie przyjaznych dla człowieka, należy określić (na przykład: "Tytuł jest wymagane").

*UpdateModel()* metody pomocnika automatycznie wypełni kolekcji ModelState po napotkaniu błędy podczas próby przypisania wartości formularza do właściwości obiektu modelu. Na przykład obiekt naszych obiad EventDate właściwość jest typu DateTime. Gdy metoda UpdateModel() nie można przypisać wartości ciągu "BOGUS" do niego w powyższym scenariuszu, metoda UpdateModel() dodać wpis do kolekcji ModelState, co wskazuje na błąd przypisania wystąpiły z tą właściwością.

Programiści mogą również pisać kod, aby jawnie dodać zapisy błędów do kolekcji ModelState, takich jak robimy poniżej naszych "catch" Błąd obsługi bloku, który jest zapełnianie kolekcji ModelState zapisy w oparciu active naruszeń zasady w Obiekt obiad:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integracja pomocnika kodu HTML z ModelState

Metody pomocnicze HTML — podobnie jak Html.TextBox() - sprawdź kolekcji ModelState podczas renderowania danych wyjściowych. Jeśli wystąpi błąd elementu, renderowania wartości wprowadzonych przez użytkownika i błąd klasę CSS.

Na przykład w naszym "Edytuj w" widoku metody pomocnika Html.TextBox() jest używany do renderowania EventDate naszych obiad obiektu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Widok był renderowany w scenariuszu błąd, metoda Html.TextBox() zaznaczone kolekcji ModelState, aby zobaczyć, czy wystąpiły błędy skojarzone z właściwością "EventDate" naszych obiad obiektu. Jeśli okaże się, że wystąpił błąd go renderowane przesłanych danych wejściowych użytkownika ("BOGUS") jako wartość i dodaje klasę css błąd do &lt;wprowadzania type = "pole tekstowe" /&gt; on generowany kod znaczników:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Można dostosować wygląd błąd klasy css do wyszukiwania, ale ma. Domyślna klasa błąd CSS — "input błędzie sprawdzania poprawności" — jest zdefiniowany w *\content\site.css* arkusza stylów oraz wygląda podobnie jak poniżej:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Ta reguła CSS jest, co spowodowało naszych nieprawidłowe elementy wejściowe być wyróżniane podobnie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metoda pomocnicza Html.ValidationMessage()

Metoda pomocnicza Html.ValidationMessage() może służyć do danych wyjściowych ModelState komunikat o błędzie skojarzony z właściwością określonego modelu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Generuje kod powyżej:  *&lt;span klasy = "pola błędzie sprawdzania poprawności"&gt; wartość "BOGUS" jest nieprawidłowa &lt; /span&gt;*

Metoda pomocnicza Html.ValidationMessage() obsługuje również drugi parametr, który umożliwia deweloperom Zastąp tekst komunikat o błędzie jest wyświetlany:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Generuje kod powyżej:  <em>&lt;span klasy = "pola błędzie sprawdzania poprawności"&gt;\*&lt;/span&gt;</em>zamiast domyślny tekst błędu w przypadku błędu dla Właściwość EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metoda pomocnicza Html.ValidationSummary()

Metoda pomocnika Html.ValidationSummary() może zostać użyty do renderowania komunikat błędu podsumowania, wraz z &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; listę wszystkie szczegółowe informacje o błędzie w wiadomości Element ModelState kolekcji:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Metoda pomocnika Html.ValidationSummary() przyjmuje parametr opcjonalny ciąg — definiuje podsumowania komunikat do wyświetlenia powyżej listy szczegółowe błędy:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Można opcjonalnie użyć CSS do zastąpienia, jak wygląda na liście błędów.

#### <a name="using-a-addruleviolations-helper-method"></a>Przy użyciu metody pomocnika AddRuleViolations

Nasze początkowej implementacji Edytuj HTTP POST używane instrukcji foreach jego bloku catch do pętli obiektu obiad naruszeń zasady i dodaj je do kolekcji ModelState kontrolera:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Firma Microsoft może wprowadzać tego kodu małego czyściciel przez dodanie "ControllerHelpers" klas do projektu NerdDinner i implementować "AddRuleViolations" metody rozszerzenia w niej dodające metody pomocnika do klasy platformy ASP.NET MVC ModelStateDictionary. Ta metoda rozszerzenia umożliwiająca Hermetyzowanie logiki niezbędne do wypełnienia ModelStateDictionary z listą błędów RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Firma Microsoft następnie aktualizować nasze metody akcji Edytuj HTTP POST do używania tej metody rozszerzenia do wypełniania kolekcji ModelState z naszych obiad naruszenia reguły.

#### <a name="complete-edit-action-method-implementations"></a>Zakończenie implementacje metod akcji edycji

Poniższy kod implementuje wszystkie niezbędne do naszego scenariusza edycji logiką kontrolera:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Dodatkową korzyścią o naszych implementacji edycji jest czy naszej klasy kontrolera ani naszych szablon widoku nie posiada nic wiedzieć o określonych weryfikacji lub reguły biznesowe są wymuszane przez nasz model obiad. Firma Microsoft może w przyszłości dodać dodatkowe reguły do naszego modelu i nie trzeba wprowadzać żadnych zmian w kodzie naszych kontrolera lub widok, aby je do obsługi. To zapewnia elastyczność w łatwy sposób rozwijać naszej aplikacji wymagania w przyszłości z co najmniej zmian w kodzie.

### <a name="create-support"></a>Tworzenie pomocy technicznej

Gdy już zakończyliśmy implementacja naszej klasy DinnersController zachowanie "Edit". Teraz Przyjrzyjmy się zaimplementować go — co umożliwi użytkownikom dodawanie nowych kolacji pomocy technicznej "Utwórz".

#### <a name="the-http-get-create-action-method"></a>Metody akcji tworzenia GET protokołu HTTP

Rozpocznie się z zastosowaniem protokołu HTTP "GET" zachowanie naszych Utwórz metody akcji. Ta metoda zostanie wywołana, gdy ktoś odwiedzi */kolacji/Utwórz* adresu URL. Implementacja naszej wygląda następująco:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Powyższy kod tworzy nowy obiekt obiad i przypisuje jej właściwość EventDate się jeden tydzień w przyszłości. Następnie renderowania widoku, który jest oparty na nowy obiekt obiad. Ponieważ firma Microsoft nie zostały jawnie przekazany nazwę *View()* metody pomocniczej, zostanie użyty oparte na konwencji domyślnej ścieżki rozwiązywać Wyświetl szablon: /Views/Dinners/Create.aspx.

Teraz Utwórzmy ten szablon widoku. Firma Microsoft to zrobić, klikając prawym przyciskiem myszy w metodzie akcji tworzenia i wybierając polecenie "Dodaj widok" z menu kontekstowego. W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przekazywane do szablonu widoku obiektu obiad i wybrać do automatycznego szkieletu szablon "Utwórz":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Kliknięcie przycisku "Dodaj", Visual Studio Zapisz nowy widok "Create.aspx" na podstawie szkieletu katalogu "\Views\Dinners" i otwórz go w środowisku IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Załóżmy zmiany kilka do domyślnego "Utwórz" szkieletu pliku, który został wygenerowany firmie Microsoft, a zmodyfikuj go, aby wyglądały jak poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

I teraz możemy uruchomienia naszych aplikacji i dostępu *"/ kolacji/Create"* adres URL w przeglądarce wyświetli interfejsu użytkownika, takie jak poniżej z naszych Utwórz wykonania akcji:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementowanie POST protokołu HTTP Utwórz metody akcji

Mamy wersji HTTP GET naszych zaimplementowano Utwórz metody akcji. Gdy użytkownik kliknie przycisk "Zapisz" wykonuje post formularza */kolacji/Utwórz* adres URL, i przesyła HTML &lt;wejściowych&gt; wartości przy użyciu zlecenie HTTP POST formularza.

Umożliwia teraz zaimplementować zachowanie HTTP POST z naszych Utwórz metody akcji. Rozpocznie się przez dodanie przeciążonej metody akcji "Utwórz" do naszej DinnersController, który ma atrybut "AcceptVerbs" z który wskazuje, że obsługi scenariuszy HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Istnieją różne sposoby, firma Microsoft mogą uzyskiwać dostęp do parametrów przesłanego formularza w naszym metody "Utwórz" włączone POST protokołu HTTP.

Jednym z podejść jest utworzyć nowy obiekt obiad, a następnie użyć *UpdateModel()* metody pomocniczej (takich jak robiliśmy za pomocą operacji Edit) można umieścić w nim wartości przesłanego formularza. Firma Microsoft następnie dodaj go do naszego DinnerRepository, jego utrwalenia w bazie danych i przekieruje użytkownika do naszej akcji Details pokazanie obiad nowo utworzony przy użyciu kodu poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternatywnie możemy użyć metody gdzie mamy naszych metody akcji Create() zająć obiektu obiad jako parametru metody. ASP.NET MVC zostanie następnie automatycznie wystąpienia nowego obiektu obiad firmie Microsoft, wypełnić jego właściwości za pomocą formularza danych wejściowych i przekaż go do naszego metody akcji:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nasze metody akcji powyżej sprawdza, czy obiekt obiad został pomyślnie wypełnione wartościami post formularza przez sprawdzenie właściwości ModelState.IsValid. To zwróci wartość false, jeśli istnieją wprowadzania problemów dotyczących konwersji (na przykład: ciąg dla właściwości EventDate "BOGUS"), i jeśli występują problemy naszych metody akcji ładowaniu formularza.

Jeśli wartości wejściowe są prawidłowe, metoda akcji podejmuje próbę dodania i Zapisz nowe obiad DinnerRepository. Opakowuje tej pracy w bloku try/catch, a ładowaniu formularza, jeśli istnieją wszelkie naruszenia reguły biznesowej (które spowodowałoby metody dinnerRepository.Save() zgłosić wyjątek).

Aby wyświetlić ten błąd obsługi zachowanie w akcji, firma Microsoft może żądać */kolacji/Utwórz* adresu URL i wypełnij szczegółowe informacje o nowych obiad. Niepoprawne dane wejściowe lub wartości spowoduje, że formularza Utwórz, aby być wyświetlane ponownie z powodu błędów wyróżnione podobnie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Zwróć uwagę, jak naszym Utwórz formularz jest ramach dokładnie tego samego reguły sprawdzania poprawności i business jako naszego formularza edycji. To dlatego nasze zasady sprawdzania poprawności i business zostały zdefiniowane w modelu, a nie zostały osadzone w obrębie interfejsu użytkownika lub kontrolera aplikacji. Oznacza to, firma Microsoft może później zmiany/rozwijać naszych weryfikacji lub reguły biznesowe w jednej Umieść i ich zastosowania w naszej aplikacji. Firma Microsoft nie będzie miał zmiana kodu w ramach jednej naszych edycji lub tworzenia metod akcji automatycznie uwzględnić wszelkie nowe reguły lub modyfikacji istniejących.

Gdy firma Microsoft rozwiązać wartości wejściowe i kliknij przycisk "Zapisz" ponownie, naszych oprócz DinnerRepository powiedzie się, a nowe obiad zostaną dodane do bazy danych. Firma Microsoft będzie być przekierowywane do */Dinners/szczegóły / [id]* adres URL — gdzie możemy zostaną wyświetlone szczegółowe informacje o nowo utworzony obiad:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Usuń pomocy technicznej

Teraz Dodajmy "Delete" pomocy technicznej do naszej DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metody akcji Delete HTTP GET

Rozpocznie się zaimplementowanie zachowanie naszych metody akcji delete HTTP GET. Ta metoda będzie pobrać wywoływana, gdy ktoś odwiedzi */Dinners/Delete / [id]* adresu URL. Poniżej znajdują się implementacji:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Metody akcji próbuje pobrać obiad do usunięcia. Jeśli obiad istnieje renderuje widok oparte na obiekt obiad. Jeśli obiekt nie istnieje (lub został już usunięty) zwraca widok, który renderuje "NotFound" Wyświetl szablonu utworzonego wcześniej dla metody akcji naszych "Szczegóły".

Można utworzyć szablon widoku "Delete" prawym przyciskiem myszy w metodzie akcji usuwania i wybierając polecenie "Dodaj widok" z menu kontekstowego. W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przekazywane do naszej szablon widoku jako swojego modelu obiektu obiad i wybrać opcję utworzenia pustego szablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Kliknięcie przycisku "Dodaj", Visual Studio Dodaj nowy plik szablonu widoku "Delete.aspx" firmie Microsoft w naszym katalogu "\Views\Dinners". Niektóre HTML i kod zostanie dodany do szablonu, aby zaimplementować ekran potwierdzenia usunięcia, takich jak poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Powyższy kod Wyświetla tytuł obiad do usunięcia i dane wyjściowe &lt;formularza&gt; element, który wykonuje POST /Dinners/Delete / [id] adres URL, jeśli użytkownik końcowy kliknie przycisk "Usuń" znajdujące się w nim.

Gdy przeprowadzana naszych aplikacji i dostępu *"/ kolacji/Delete / [id]"* adres URL dla obiad nieprawidłowy obiekt go renderuje interfejsu użytkownika, takich jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Temat po stronie: Dlaczego robimy ogłoszenie (POST)?** |
| --- |
| Użytkownik może poprosić o — Dlaczego rozszerzana za pomocą nakładu pracy tworzenia &lt;formularza&gt; w naszym ekran potwierdzenia usunięcia? Dlaczego nie tylko umożliwia standardowe hyperlink łącze do metody akcji, który wykonuje operację usuwania rzeczywiste? Dzieje się tak, ponieważ chcemy należy uważać, aby zabezpieczyć się przed przeszukiwarki sieci web i wyszukiwarki odnajdowania zmieniliśmy adresy URL i przypadkowo powoduje dane są usuwane, gdy ich skorzystaj z łączy. GET protokołu HTTP, na podstawie adresów URL są traktowane jako "bezpieczne" im dostępu/przeszukiwania i mają wykonuj HTTP POST z nich. Rozsądną regułą jest upewnij się, że należy zawsze umieszczać destrukcyjnego lub operacje modyfikowania danych za żądania HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementacja metody akcji POST protokołu HTTP Delete

Mamy teraz wersji HTTP GET naszych zaimplementowano metody akcji usuwania, która wyświetla ekran potwierdzenia usunięcia. Gdy użytkownik końcowy kliknie przycisk "Usuń" wykona post formularza */Dinners/obiad / [id]* adresu URL.

Umożliwia teraz zaimplementować zachowanie "POST" HTTP delete metody akcji przy użyciu kodu poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Wersja HTTP POST naszych metody akcji usuwania podejmie próbę pobrania obiektu obiad do usunięcia. Nie można go znaleźć (ponieważ został już usunięty) renderowania szablonu naszych "NotFound". W przypadku odnalezienia obiad, usuwa ją z DinnerRepository. Następnie renderowania szablonu "Usunięte".

Do wdrożenia szablonu "Usunięte" Nasz kliknij prawym przyciskiem myszy w metodzie akcji i wybierz menu kontekstowe "Dodaj widok". Firma Microsoft będzie nazwy naszych widok "Usunięte" i została ona być pustego szablonu (i nie przyjmować obiekt jednoznacznie modelu). Następnie dodamy zawartość HTML do niej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

I teraz możemy uruchomienia naszych aplikacji i dostępu *"/ kolacji/Delete / [id]"* adres URL na prawidłową obiad, potwierdzenie usunięcia obiektu będą zawierały naszych obiad ekranu podobnie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Gdy firma Microsoft, kliknij przycisk "Usuń" wykona akcję POST protokołu HTTP, aby */Dinners/Delete / [id]* adresu URL, który spowoduje to usunięcie obiad z naszej bazie danych i wyświetlić naszych "Usunięte" Wyświetl szablon:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Zabezpieczenia wiązania modelu

Zostały omówione dwa różne sposoby korzystania z wbudowanych funkcji wiązania modelu platformy ASP.NET MVC. Najpierw przy użyciu metody UpdateModel() można zaktualizować właściwości istniejącego obiektu modelu, a drugi przy użyciu platformy ASP.NET MVC obsługę przekazywania obiekty modelu jako parametry metody akcji. Obie te metody są bardzo zaawansowane i bardzo użyteczny.

Ta zasilania również łączy się z nim odpowiedzialności. Ważne jest, aby zawsze być paranoidalne o zabezpieczeniach podczas akceptowania podawanych przez użytkownika, a dotyczy to również powiązanie obiektów danych wejściowych z formularza. Należy pamiętać, aby zawsze wartości wprowadzonych przez użytkownika, aby uniknąć ataków iniekcji kodu HTML i JavaScript oraz szczególną uwagę na ataki kodowanie HTML (Uwaga: użyto LINQ do SQL dla aplikacji, które automatycznie koduje parametrów, aby uniknąć tych rodzaje ataków). Należy nigdy nie zależą od samego weryfikacji po stronie klienta i zawsze stosować weryfikacji po stronie serwera w celu ochrony przed hakerami próby wysłać sfałszowany wartości.

Jeden element dodatkowe zabezpieczenia, aby upewnić się, że pomyśleć, korzystając z funkcji powiązania platformy ASP.NET MVC jest zakres obiektów, które są powiązania. W szczególności chcesz upewnij się, że rozumiesz konsekwencje zabezpieczeń właściwości, które umożliwi można powiązać, i upewnij się, że Zezwalaj tylko te właściwości, które naprawdę powinna zezwalać na aktualizacje przez użytkownika końcowego do zaktualizowania.

Domyślnie metoda UpdateModel() będzie podejmować próby modyfikowanie wszystkich właściwości obiektu modelu, która pasuje do przychodzącego wartości parametru formularza. Ponadto obiekty przekazywane jako parametry metody akcji również domyślnie może mieć wszystkich ich właściwości, za pomocą parametrów formularza.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blokowanie powiązania w zasadzie na użycie

Możesz zablokować powiązanie zasady na podstawie na użycie przez udostępnienie explicit "Lista dołączania" właściwości, które mogą zostać zaktualizowane. Można to zrobić przez przekazanie ciągu dodatkowy parametr tablicy do metody UpdateModel() podobnie jak poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Obiekty przekazywane jako parametry metody akcji również obsługuje atrybutu [Bind], który umożliwia "obejmują listy" dozwolone właściwości, które mają być określane jak poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blokowanie powiązania na podstawie typu

Możesz również zablokować powiązanie reguły na podstawie-type. Dzięki temu można określić zasady powiązanie tylko raz, a następnie ich zastosować we wszystkich scenariuszach (w tym zarówno UpdateModel, jak i akcja scenariusze parametru metody) we wszystkich kontrolerów i metod akcji.

Reguły na typ powiązania można dostosować przez dodanie atrybutu [Bind] na typ lub rejestrując go w pliku Global.asax aplikacji (przydatne w przypadku scenariuszy, w których nie jesteś właścicielem tego typu). Można użyć atrybutu Bind dołączania i wykluczania właściwości, aby kontrolować właściwości, które są powiązania dla określonej klasy lub interfejsu.

Firma Microsoft będzie użyć tej metody klasy obiad w naszej aplikacji NerdDinner, a następnie dodaj atrybut [Bind] do niego, który ogranicza listy właściwości do następujących:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Należy zauważyć, że firma Microsoft nie zezwalają na kolekcji zbędne ustawianych przez powiązanie, nie możemy umożliwia DinnerID lub HostedBy właściwości można ustawić za pomocą powiązania. Ze względów bezpieczeństwa firma Microsoft będzie zamiast tego jedynie manipulować te właściwości określonym za pomocą jawnego kodu w ramach naszych metod akcji.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC zawiera wiele wbudowanych funkcji, które ułatwiają wdrażanie formularza scenariuszy księgowanie. Zapewnia obsługę interfejsu użytkownika CRUD na naszych DinnerRepository My używamy różnych te funkcje.

Podejście fokus modelu jest używany do wdrożenia aplikacji. Oznacza to, że nasze sprawdzania poprawności i reguły biznesowej logiki jest definiowana naszych warstwy modelu — i nie w ramach naszych kontrolery i widoki. Klasy Nasze kontrolera ani naszym szablony widoku niczego wiedzieć o reguły biznesowe określonych wymuszany przez naszych obiad klasę modelu.

Spowoduje to zachować czystą Nasza architektura aplikacji i ułatwić testowanie. Można dodać dodatkowe reguły biznesowe do naszej warstwy modelu w przyszłości i *nie muszą wprowadzać żadnych zmian w kodzie* do naszej kontrolera lub widok, aby je do obsługi. To będzie uzyskaliśmy dużą elastyczność rozwijać i zmieniać naszej aplikacji w przyszłości.

Nasze DinnersController teraz umożliwia obiad list/szczegółów, oraz tworzenia, edytowania i usuwania pomocy technicznej. Kompletny kod dla klasy znajduje się poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Następny krok

Mamy teraz podstawowej obsługi CRUD (tworzenia, odczytu, aktualizacji i usuwania), wdrożenie w naszej klasy DinnersController.

Teraz Przyjrzyjmy się jak możemy użyć klasy ViewData i ViewModel włączyć bardziej szczegółowego interfejsu użytkownika na naszych formularzy.

> [!div class="step-by-step"]
> [Poprzednie](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [dalej](use-viewdata-and-implement-viewmodel-classes.md)
