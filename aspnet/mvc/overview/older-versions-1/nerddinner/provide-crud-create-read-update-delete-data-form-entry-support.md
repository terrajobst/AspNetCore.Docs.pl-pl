---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) danych tworzą wpis pomocy technicznej | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 5 pokazuje, jak korzystać z naszych DinnersController klasą znajdującą się, Włącz obsługę edycji, tworzenie i usuwanie kolacji z nim także.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 821684c0753967fc1a693b061d5d539951cd7c23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388811"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Operacji CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) obsługi formularzach danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 5 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.
> 
> Krok 5 pokazuje, jak korzystać z naszych DinnersController klasą znajdującą się, Włącz obsługę edycji, tworzenie i usuwanie kolacji z nim także.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner krok 5: Tworzenie, aktualizowanie i usuwanie formularza scenariuszy

Firma Microsoft została wprowadzona, widoków i kontrolerów i opisano sposób ich używać do implementowania lista/szczegóły umożliwiający kolacji w lokacji. Naszym kolejnym krokiem będzie podjęcie dalszych klasy Nasze DinnersController i włączyć obsługę edycji, tworzenie i usuwanie kolacji z nim także.

### <a name="urls-handled-by-dinnerscontroller"></a>Obsługiwane przez DinnersController adresów URL

Dodaliśmy wcześniej metod akcji do DinnersController, które zaimplementować obsługę dwa adresy URL: */Dinners* i */Dinners/szczegóły / [id]*.

| **ADRES URL** | **VERB** | **Cel** |
| --- | --- | --- |
| */Dinners/* | POBIERZ | Wyświetlona lista nadchodzących kolacji HTML. |
| */Dinners/szczegóły / [id]* | POBIERZ | Wyświetl szczegółowe informacje o określonych obiad. |

Teraz dodamy metody akcji w celu zaimplementowania trzy dodatkowe adresy URL: <em>/Dinners/Edit / [identyfikator], / kolacji/tworzenie,</em>i<em>/Dinners/Delete / [id]</em>. Te adresy URL umożliwi obsługę kolacji edycji istniejących, tworzenie nowych kolacji i usuwanie kolacji.

Firma Microsoft będzie obsługiwać interakcje czasownik HTTP GET i POST protokołu HTTP te nowe adresy URL. Żądania HTTP GET do tych adresów URL będą wyświetlane początkowa widoku HTML w danych (formularza wypełniony danymi obiad "Edytuj" w przypadku pustego formularza w przypadku "Utwórz" i ekran potwierdzenia usunięcia przypadku "delete"). Żądania HTTP POST do tych adresów URL będzie save/aktualizowanie/usuwanie danych firmy Dinner w naszym DinnerRepository (i z tego miejsca w bazie danych).

| **ADRES URL** | **VERB** | **Cel** |
| --- | --- | --- |
| */Dinners/edit / [id]* | POBIERZ | Wyświetlanie edytowalnego formularza HTML wypełniony danymi obiad. |
| POST | Zapisywanie zmian w formularzu na obiad z nich do bazy danych. |
| */Dinners/Create* | POBIERZ | Wyświetlanie pustego formularza HTML, który pozwala użytkownikom na definiowanie nowych kolacji. |
| POST | Utwórz nowy obiad i zapisz go w bazie danych. |
| */Dinners/delete / [id]* | POBIERZ | Wyświetl usuwanie ekran potwierdzenia. |
| POST | Usuwa określony obiad z bazy danych. |

### <a name="edit-support"></a>Edytuj pomocy technicznej

Zacznijmy od realizacji scenariusza "Edytuj".

#### <a name="the-http-get-edit-action-method"></a>Metody akcji edycji HTTP GET

Zaczniemy od implementacji protokołu HTTP zachowanie "GET" Nasze metody akcji edycji. Ta metoda jest wywoływana, gdy */Dinners/Edit / [id]* żądanie adresu URL. Naszej implementacji będzie wyglądać następująco:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Powyższy kod używa DinnerRepository można pobrać obiektu obiad. Wyświetlanie szablonu za pomocą obiektu obiad następnie renderowania. Ponieważ firma Microsoft nie zostały jawnie przekazano nazwę szablonu w celu *View()* metody pomocnika, użyje domyślnej ścieżki oparte na Konwencji rozpoznać Wyświetl szablon: /Views/Dinners/Edit.aspx.

Teraz Utwórzmy ten szablon widoku. Firma Microsoft będzie to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody edycji i wybierając polecenie "Dodaj widok" z menu kontekstowego:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przekazywane do widoku szablonu jako swój model obiektu obiad i możliwość automatycznego szkieletu szablonu usługi "Edit":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Kliknięcie przycisku "Dodaj", programu Visual Studio Dodaj nowy plik szablonu widoku "Edit.aspx" dla nas w katalogu "\Views\Dinners". On również zostanie otwarty nowy szablon "Edit.aspx" w widoku, w ramach edytora kodu — wypełniana początkowej "Edit" szkieletu implementacji takich jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Możemy wprowadzić kilka zmian, aby generowany domyślny "edit" szkieletu i zaktualizować szablon widoku edycji zawartości poniżej (spowoduje to usunięcie kilka właściwości, które nie chcemy ujawniać):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Firma Microsoft uruchamiania aplikacji i żądania *"/ 1/Edit/kolacji"* adresu URL, firma Microsoft zostanie wyświetlona następująca strona:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Kod znaczników HTML, generowane przez naszych widoku wygląda jak poniżej. Jest standardowa HTML — za pomocą &lt;formularza&gt; element, który wykonuje metodę POST protokołu HTTP do */Dinners/Edit/1* adres URL po "Zapisz" &lt;typu danych wejściowych = "Prześlij" /&gt; wypchnięciach przycisk. HTML &lt;typu danych wejściowych = "text" /&gt; element został danych wyjściowych dla każdej właściwości można edytować:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() oraz metody pomocnika kodu Html Html.TextBox()

Szablon widoku "Edit.aspx" jest przy użyciu kilku metod "Pomocnika kodu Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() i Html.ValidationMessage(). Oprócz generowania kod znaczników HTML dla nas, te metody pomocnika zapewniają obsługę błędów wbudowanych i sprawdzanie poprawności pomocy technicznej.

##### <a name="htmlbeginform-helper-method"></a>Metoda pomocnika Html.BeginForm()

Metoda pomocnika Html.BeginForm() to, jakie dane wyjściowe HTML &lt;formularza&gt; elementu w naszym znaczników. W naszym Edit.aspx Wyświetl szablon można zauważyć, że jest stosowane w języku C# "" instrukcji przy użyciu tej metody. Otwarty nawias klamrowy wskazuje początek &lt;formularza&gt; zawartości i zamykający nawias klamrowy to, co wskazuje koniec &lt;/form&gt; elementu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternatywnie Jeśli okaże się instrukcji "using" podejście nienaturalnym dla tego typu scenariusza, możesz użyć kombinacji Html.BeginForm() i Html.EndForm() (co działa tak samo):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Wywoływanie Html.BeginForm() bez parametrów spowoduje jego danych wyjściowych element formularza, który wykonuje metodę POST protokołu HTTP do adresu URL bieżącego żądania. Oznacza to, dlaczego nasz widoku edycji generuje *&lt;akcji formularza = "/ 1/Edit/kolacji" metoda = "post"&gt;* elementu. Firma Microsoft może mieć też przekazywane jawnych parametrów do Html.BeginForm() Chcieliśmy do wysłania do innego adresu URL.

##### <a name="htmltextbox-helper-method"></a>Metoda pomocnika Html.TextBox()

Widok naszej Edit.aspx używa metody pomocnika Html.TextBox() służący do wypełniania wyjściowego &lt;typu danych wejściowych = "text" /&gt; elementy:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Powyższej metody Html.TextBox() przyjmuje jeden parametr –, który jest używany do określania atrybutów id/nazwa elementu &lt;typu danych wejściowych = "text" /&gt; element danych wyjściowych, jak również właściwości modelu, aby wypełnić pole tekstowe wartości. Na przykład obiektu obiad, firma Microsoft jest przekazywane do widoku edycji ma wartość właściwości "Title" "Prognoz .NET" i dlatego naszych metoda Html.TextBox("Title") wywoływać dane wyjściowe: *&lt;wejściowy identyfikator = "Title" Nazwa = "Title" type = "text" value = "Prognoz .NET" /&gt;*.

Alternatywnie możemy użyć pierwszy parametr Html.TextBox() można określić id/nazwa elementu, a następnie jawnie przekazać wartości do użycia jako drugi parametr:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Często firma Microsoft będzie wykonywać niestandardowe formatowanie na wartość, która jest dane wyjściowe. Statyczna metoda String.Format() wbudowane .NET jest przydatne w przypadku tych scenariuszy. Szablon widoku Edit.aspx używa to do formatowania wartości EventDate (czyli typu Data/godzina) tak, aby nie pokazuje czas w sekundach:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Trzeci parametr Html.TextBox() opcjonalnie może służyć do wypełniania wyjściowego dodatkowe atrybuty kodu HTML. Poniższy fragment kodu przedstawia sposób renderowania rozmiarem dodatkowe = atrybut "30", a klasa = atrybut "mycssclass" on &lt;typu danych wejściowych = "text" /&gt; elementu. Należy zauważyć, jak są zmianę nazwy przy użyciu atrybutu klasy "@" character because "klasy" jest zastrzeżonym słowem kluczowym w języku C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementacja metody akcji edycji POST protokołu HTTP

Teraz mamy wersję HTTP GET naszych zaimplementowano metody akcji edycji. Gdy użytkownik zażąda */Dinners/Edit/1* otrzymują stronę HTML, podobnie do następującego adresu URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Naciśnięcie przycisku "Zapisz" powoduje, że post formularza */Dinners/Edit/1* adresu URL, i przesyła HTML &lt;wejściowych&gt; wartości czasownikiem HTTP POST formularza. Załóżmy teraz zaimplementować to zachowanie POST protokołu HTTP, nasze metody akcji edycji — które będzie obsługiwać zapisywanie Dinner.

Rozpocznie się przez dodanie przeciążonej metody akcji "Edit" do naszych DinnersController, który ma atrybut "AcceptVerbs" który wskazuje, że obsługuje ona scenariusze żądania HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Gdy atrybut [AcceptVerbs] jest stosowany do metod akcji przeciążona, ASP.NET MVC automatycznie obsługuje wysyłania żądań do metody odpowiedniej akcji w zależności od przychodzącego zlecenie HTTP. Żądania HTTP POST <em>/Dinners/Edit / [id]</em> adresy URL zostaną wysłane do powyższej metody edycji podczas wszystkich innych żądaniach czasownik HTTP <em>/Dinners/Edit / [id]</em>zaczną adresy URL do pierwszej metody edycji (które zostały zaimplementowane ma atrybut [AcceptVerbs]).

| **Temat po stronie: Dlaczego rozróżnienia przy użyciu poleceń HTTP?** |
| --- |
| Możesz zadawać — dlaczego jest efektywność za pomocą pojedynczego adresu URL i rozszerzają jego zachowanie za pośrednictwem czasownik HTTP? Dlaczego nie można mieć dwa osobne adresy URL do obsługi ładowania i zapisywania zmian edycji? Na przykład: /Dinners/Edit / [id] wyświetlania formularza początkowego i /Dinners/Save / [id] do obsługi publikacje formularzy, aby go zapisać? Wadą z dwa osobne adresy URL publikowania jest, czy w przypadkach, gdzie możemy ogłoszeniem /Dinners/Save/2, a następnie pojawi się konieczność ponownego wyświetlenia formularza HTML, ze względu na błąd danych wejściowych, użytkownik końcowy będzie znajdą się o kolacji/Save/2 adres URL na pasku adresu w przeglądarce (ponieważ jest to było adres URL formularza, opublikowane w usłudze). Jeśli użytkownik końcowy zakładki tę stronę redisplayed do listy ulubionych w swojej przeglądarce, lub kopiowania/past adres URL i wysłanie wiadomości e-mail znajomego, ich zakończą się zapisywania adresu URL, który nie będzie działać w przyszłości (ponieważ tego adresu URL, zależy od wartości post). Dzięki uwidocznieniu działania pojedynczego adresu URL (takich jak: /Dinners/Edit/[id]) i rozszerzają przetwarzania go za zlecenie HTTP, jest bezpieczny dla użytkowników końcowych do zakładki na stronie edycji i/lub wysłać adres URL do innych osób. |

#### <a name="retrieving-form-post-values"></a>Pobieranie wartości Post formularza

Istnieją różne sposoby, które firma Microsoft mogą uzyskiwać dostęp do opublikowanych parametrów formularza w ramach naszych "Edit" metodą HTTP POST. Jeden proste podejście jest po prostu użyć właściwości żądania w klasie bazowej kontrolera dostępu do kolekcji formularza i bezpośrednio pobrać opublikowanych wartości:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Powyższe podejście jest jednak nieco verbose, szczególnie w przypadku, gdy możemy dodać logikę obsługi błędu.

A lepiej podejście dla tego scenariusza jest do użycia wbudowanych *UpdateModel()* metody pomocniczej dla klasy bazowej kontrolera. Obsługuje ona, aktualizowanie właściwości obiektu, który przekażemy go za pomocą przychodzących parametrów formularza. Używa odbicia w celu określenia nazwy właściwości w obiekcie, a następnie automatycznie konwertuje i przypisuje wartości do nich na podstawie wartości wejściowych przesłany przez klienta.

Moglibyśmy użyć metody UpdateModel() uprościć naszych żądania HTTP POST Edytuj akcję przy użyciu następującego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Firma Microsoft może teraz odwiedzić */Dinners/Edit/1* adresu URL, a następnie zmień tytuł naszych obiad:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Kliknięcie przycisku "Zapisz", możemy wykonać post formularza do naszej akcji edycji, a następnie zaktualizowane wartości zostaną utrwalone w bazie danych. Firma Microsoft następnie nastąpi przekierowanie do adresu URL szczegóły na obiad, (które będą wyświetlane nowo zapisane wartości):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Program obsługi edycji błędy

Nasz bieżący działa implementacji żądania HTTP POST poprawnie — z wyjątkiem sytuacji, gdy występują błędy.

Po użytkownik popełni edytowanie formularza, musimy upewnić się, że formularz zostanie wyświetlony ponownie, szczegółowy komunikat o błędzie, który zawiera informacje na temat ich, aby rozwiązać ten problem. Obejmuje to przypadki, w których użytkownik końcowy publikuje nieprawidłowe dane wejściowe (na przykład: ciąg daty źle sformułowane), jak również przypadków, gdy ma prawidłowy format wejściowy, ale jest naruszenie reguły biznesowej. Gdy wystąpi błąd że formularza należy zachować dane wejściowe które użytkownik wprowadził pierwotnie, tak aby nie musieli ręcznie uzupełnić swoje zmiany. Ten proces należy powtórzyć tyle razy, zgodnie z potrzebami, dopóki formularza po pomyślnym ukończeniu.

Platforma ASP.NET MVC zawiera niektóre nieuprzywilejowany wbudowane funkcje, które umożliwiają łatwe Obsługa błędów i ponowne wyświetlanie formularza. Przyjrzyjmy się te funkcje w działaniu naszych metoda akcji edycji aktualizacji z następującym kodem:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Powyższy kod jest podobny do naszego poprzedniego wdrożenia —, z tą różnicą, że możemy są teraz otaczającym bloku obsługi błędów try/catch naszą pracę. Jeśli wystąpi wyjątek podczas wywoływania UpdateModel() lub gdy firma Microsoft spróbuj zapisać DinnerRepository, (które zgłosi wyjątek, jeśli obiekt obiad, których firma próbuje zapisać jest nieprawidłowa z powodu naruszenia reguły w naszym modelu), będzie naszym Blok obsługi błędów catch wykonywanie. Znajdujący się w nim możemy pętli wszelkie naruszenia reguły, które istnieją w obiekcie firmy Dinner i dodać je do obiektu ModelState, (które omówimy w dalszej części wkrótce). Następnie możemy ponownie wyświetlić widoku.

Aby to zobaczyć działa teraz ponownie uruchomić aplikację, edytować obiad i zmień go na mają pustym tytułem EventDate "BOGUS" i użyj numeru telefonu Zjednoczone Królestwo z wartością kraju w USA. Po naciśnięciu klawisza przycisk "Zapisz" Metoda naszych Edytuj POST protokołu HTTP nie będzie można zapisać Dinner (ponieważ ma błędów) i spowoduje ponowne wyświetlenie formularza:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nasza aplikacja zawiera środowisko znośnego błędu. Elementy tekstowe z nieprawidłowe dane wejściowe są wyróżnione kolorem czerwonym, a komunikaty o błędach weryfikacji są widoczne dla użytkownika końcowego o nich. Formularz również zachowuje dane wejściowe użytkownika wprowadzona — tak, aby nie musieli uzupełnienie niczego.

Jak to zrobić możesz zadawać, dzieje? Jak pola tekstowe tytuł, EventDate i ContactPhone wyróżnić się na czerwono i wiedzieć, aby dane wyjściowe wartości pierwotnie wprowadzonego użytkownika? I jak komunikaty o błędach wyświetlane na liście u góry? Dobra wiadomość jest taka, to nie występują w raporcie magic — zamiast został ponieważ użyliśmy niektóre wbudowane funkcje platformy ASP.NET MVC, które ułatwić sprawdzenie poprawności danych wejściowych i błędów scenariuszy obsługi.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Element ModelState zrozumienie i metody pomocnika kodu HTML sprawdzania poprawności

Klasy kontrolera mają Kolekcja właściwości "ModelState", która zapewnia sposób, aby wskazać, że istnieją błędy za pomocą obiektu modelu były przekazywane do widoku. Błąd wpisów w tej kolekcji element ModelState określić nazwę właściwości modelu z emisją (na przykład: "Title", "EventDate" lub "ContactPhone") i Zezwól przyjaznego dla człowieka komunikat, należy określić (na przykład: "Wymagany jest tytuł").

*UpdateModel()* metody pomocnika automatycznie wypełnia kolekcję ModelState podczas napotka błędy podczas próby przypisania wartości formularza do właściwości obiektu modelu. Na przykład obiekt naszych obiad EventDate właściwość jest typu Data/Godzina. Gdy metoda UpdateModel() nie może przypisać wartość ciągu "BOGUS" do niej w powyższym scenariuszu, metoda UpdateModel() dodać wpis do kolekcji ModelState, co wskazuje na błąd przypisania miało miejsce przy użyciu tej właściwości.

Programiści mogą również pisać kod, aby jawnie dodać wpisów błędów do kolekcji ModelState, takie jak Radzimy sobie poniżej naszej "Łap" Błąd obsługi bloku, które wypełniają z wpisami na podstawie aktywnych naruszenia reguły w kolekcji ModelState Obiekt obiad:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integracja pomocnika kodu HTML z ModelState

Metody pomocników HTML — takich jak Html.TextBox() — sprawdź kolekcji ModelState podczas renderowania danych wyjściowych. Jeśli wystąpi błąd dla elementu, są one renderowane wartości wprowadzone przez użytkownika i błąd klasę CSS.

Na przykład naszym zdaniem "Edit" użyto metody pomocnika Html.TextBox() do renderowania EventDate naszych obiektu obiad:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Gdy widok był renderowany w scenariuszu błąd, metoda Html.TextBox() sprawdzana jest kolekcji ModelState, aby zobaczyć, czy wystąpiły błędy skojarzone z właściwością "EventDate" nasze obiektu obiad. Jeśli okaże się, że wystąpił błąd jest renderowany przesłanych danych wejściowych użytkownika ("BOGUS") jako wartość i dodaje klasę css błąd do &lt;typu danych wejściowych = "w polu tekstowym" /&gt; on generowany kod znaczników:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Można dostosować wygląd błąd klasy css do wyszukiwania w dowolny sposób. Domyślna klasa błąd CSS — "danych wejściowych błędzie sprawdzania poprawności" — jest zdefiniowany w *\content\site.css* arkusza stylów i wygląda jak poniżej:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Ta reguła CSS jest, co spowodowało naszych nieprawidłowe elementy wejściowe być wyróżniony, takich jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metoda pomocnika Html.ValidationMessage()

Metody pomocnika Html.ValidationMessage() może służyć do wypełniania wyjściowego ModelState komunikat o błędzie skojarzony z właściwością określonego modelu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Powyższy kod wyświetla:  *&lt;span klasy = "pole błędzie sprawdzania poprawności"&gt; wartość "BOGUS" jest nieprawidłowa &lt; /span&gt;*

Metoda pomocnika Html.ValidationMessage() obsługuje również drugi parametr, który umożliwia deweloperom zastąpić tekst komunikatu o błędzie jest wyświetlany:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Powyższy kod wyświetla:  <em>&lt;span klasy = "pole błędzie sprawdzania poprawności"&gt;\*&lt;/span&gt;</em>zamiast domyślny tekst błędu, gdy błąd występuje Właściwość EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metoda pomocnika Html.ValidationSummary()

Metoda pomocnika Html.ValidationSummary() może zostać użyty do renderowania podsumowania komunikat o błędzie wraz z &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; listę wszystkich szczegółowy komunikat o błędzie w wiadomości Element ModelState kolekcji:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Metoda pomocnika Html.ValidationSummary() przyjmuje parametr opcjonalny ciąg — definiujący podsumowania komunikat o błędzie wyświetlany powyżej listy szczegółowe informacje o błędach:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Można opcjonalnie użyć CSS do zastąpienia, jak wygląda listy błędów.

#### <a name="using-a-addruleviolations-helper-method"></a>Za pomocą innej metody pomocnika AddRuleViolations

Nasze wstępne wdrożenie Edytuj POST protokołu HTTP używane Instrukcja foreach w ramach jego blok catch do pętli obiektu obiad naruszeń reguł i dodać je do kolekcji ModelState kontrolera:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Firma Microsoft ułatwia ten kod, nieco bardziej przejrzystą, dodając "ControllerHelpers" klasy do projektu NerdDinner i zaimplementować "AddRuleViolations" metody rozszerzenia w nim, który dodaje metody pomocnika do klasy program ASP.NET MVC ModelStateDictionary. Ta metoda rozszerzenia umożliwiająca Hermetyzowanie logikę niezbędną do wypełniania ModelStateDictionary z listą błędów RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Firma Microsoft jest następnie zaktualizuj naszych żądania HTTP POST Edytuj metodę akcji do używania tej metody rozszerzenia do wypełnienia kolekcji ModelState naruszeń reguł naszej firmy Dinner.

#### <a name="complete-edit-action-method-implementations"></a>Wykonaj implementacje metod akcji edycji

Poniższy kod implementuje wszystkie niezbędne w naszym scenariuszu edycji logiką kontrolera:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Korzyścią o naszej implementacji edycji polega na tym, czy szablon widoku ani naszych klasy kontrolera musi nic wiedzieć o określonych sprawdzania poprawności lub reguły biznesowe są wymuszane przez nasz model obiad. Firma Microsoft może w przyszłości dodać dodatkowe reguły do nasz model i nie trzeba wprowadzać żadnych zmian w kodzie naszej kontrolera lub widok, w kolejności, które mają być obsługiwane. To zapewnia nam elastyczność łatwo rozwój aplikacji wymogów w zakresie w przyszłości z co najmniej zmian w kodzie.

### <a name="create-support"></a>Tworzenie pomocy technicznej

Po zakończeniu Implementacja klasy Nasze DinnersController zachowanie "Edytuj". Teraz przejdźmy do zaimplementowania pomocy technicznej "Utwórz" na nim — które będą umożliwiać użytkownikom dodawanie nowych kolacji.

#### <a name="the-http-get-create-action-method"></a>HTTP GET metody akcji Create

Firma Microsoft rozpocznie się poprzez implementację HTTP "GET" zachowanie naszych utworzyć metody akcji. Ta metoda zostanie wywołana, gdy ktoś odwiedzi */kolacji/tworzenie* adresu URL. Naszej implementacji wygląda następująco:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Powyższy kod tworzy nowy obiekt obiad i przypisuje jej właściwości EventDate jeden tydzień w przyszłości. Go następnie renderuje widok, który opiera się na nowy obiekt obiad. Ponieważ firma Microsoft nie zostały jawnie przekazywane nazwę *View()* metody pomocnika, użyje domyślnej ścieżki oparte na Konwencji rozpoznać Wyświetl szablon: /Views/Dinners/Create.aspx.

Teraz Utwórzmy ten szablon widoku. Możemy to zrobić, kliknij prawym przyciskiem myszy w obrębie metody akcji Create i wybierając polecenie "Dodaj widok" z menu kontekstowego. W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przechodzi do szablonu widoku obiektu obiad i możliwość automatycznego szkieletu szablon "Utwórz":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Kliknięcie przycisku "Dodaj", Visual Studio Zapisz nowy widok szkieletu na podstawie "Create.aspx" w katalogu "\Views\Dinners" i otwórz go w środowisku IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Możemy wprowadzić kilka zmian do domyślnej "Utwórz" szkielet pliku, który został wygenerowany dla nas i zmodyfikuj go aby wyglądał jak poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

A teraz możemy uruchamiania naszych aplikacji i dostępu *"/ kolacji/Create"* adresu URL w przeglądarce, renderowanie interfejsu użytkownika, takich jak poniżej zostanie przeprowadzone z naszej implementacji Akcja Utwórz:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementowanie POST protokołu HTTP, utwórz metody akcji

Mamy wersję HTTP GET naszych zaimplementowano metody akcji tworzenia. Gdy użytkownik kliknie przycisk "Zapisz" wykonuje post formularza */kolacji/tworzenie* adresu URL, i przesyła HTML &lt;wejściowych&gt; wartości czasownikiem HTTP POST formularza.

Przejdźmy teraz Implementowanie zachowania żądania HTTP POST naszych utworzyć metody akcji. Rozpocznie się przez dodanie przeciążonej metody akcji "Utwórz" do naszych DinnersController, który ma atrybut "AcceptVerbs" który wskazuje, że obsługuje ona scenariusze żądania HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Istnieją różne sposoby mamy dostęp parametrach formularza przesłanych wewnątrz metody "Utwórz" nasze włączone POST protokołu HTTP.

Jednym z podejść jest, aby utworzyć nowy obiekt obiad, a następnie użyj *UpdateModel()* metodę pomocniczą (takich jak zrobiliśmy za pomocą akcji edycji) aby je zapełnić przy użyciu wartości przesłanego formularza. Firma Microsoft następnie dodać go do naszego DinnerRepository, jego utrwalenia w bazie danych i przekieruje użytkownika do akcję naszych szczegółów w taki sposób, aby wyświetlić nowo utworzony obiad, przy użyciu poniższego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternatywnie możemy użyć podejścia, w których mamy nasz metody akcji Create() pobrać obiekt obiad, jako parametr metody. ASP.NET MVC zostanie następnie automatycznie utworzenia wystąpienia nowego obiektu obiad dla nas wypełnienia jego właściwości w formie danych wejściowych i przekazać go do naszego metody akcji:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nasze powyżej metody akcji sprawdza, czy obiekt obiad został pomyślnie wypełnione przy użyciu wartości post formularza, zaznaczając właściwość ModelState.IsValid. To polecenie zwróci wartość false, jeśli istnieją dane wejściowe problemów dotyczących konwersji (na przykład: ciąg "BOGUS" dla właściwości EventDate), i jeśli występują problemy naszym metody akcji ładowaniu formularza.

Jeśli wartości wejściowe są prawidłowe, następnie metody akcji próbuje dodać, a następnie zapisz nowy obiad DinnerRepository. To opakowuje tej pracy w ramach bloku try/catch i ładowaniu formularza, jeśli ewentualne naruszenia reguły biznesowe, (które może spowodować, że metoda dinnerRepository.Save() zgłosić wyjątek).

Aby wyświetlić to zachowanie w działaniu obsługi błędów, możemy poprosić */kolacji/tworzenie* adresu URL i wprowadź szczegółowe informacje o nowych obiad. Nieprawidłowe dane wejściowe lub wartości spowoduje, że formularz Utwórz, aby być wyświetlane ponownie z błędami, które podobnie jak przedstawiono poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Zwróć uwagę, jak nasze Utwórz formularz jest zapewniane dokładnie te same reguły sprawdzania poprawności i firm jako naszego formularza edycji. Jest to spowodowane naszych reguł biznesowych i sprawdzania poprawności zostały zdefiniowane w modelu, a nie zostały osadzone w obrębie interfejsu użytkownika lub kontrolera w aplikacji. Oznacza to można później zmiany/Rozwijamy nasze sprawdzania poprawności reguły biznesowe w jednym miejscu i ich dotyczą całej naszej aplikacji. Firma Microsoft nie trzeba zmiany jakiegokolwiek kodu w ramach jednej z naszych edycji lub tworzenia metod akcji, które automatycznie uwzględnić wszelkie nowe reguły lub modyfikacje do istniejących.

Gdy możemy naprawić wartości wejściowych, a następnie kliknij przycisk "Zapisz" ponownie naszych oprócz DinnerRepository zakończy się powodzeniem i nowe obiad zostaną dodane do bazy danych. Firma Microsoft następnie nastąpi przekierowanie do */Dinners/szczegóły / [id]* adres URL — gdzie firma Microsoft zostaną wyświetlone szczegółowe informacje o nowo utworzony obiad:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Usuń pomocy technicznej

Teraz Dodajmy pomocy technicznej "Delete" do naszych DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metody akcji Delete protokołu HTTP GET

Rozpocznie się poprzez implementację zachowanie HTTP GET naszych metody akcji usuwania. Ta metoda ma zostać wywołana, gdy ktoś odwiedzi */Dinners/Delete / [id]* adresu URL. Poniżej przedstawiono implementację:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Metody akcji podejmie próbę pobrania obiad do usunięcia. Jeśli Dinner istnieje renderuje widok na podstawie obiad obiektu. Jeśli obiekt nie istnieje (lub został już usunięty) zwraca widok, który renderuje "NotFound" Wyświetl szablon utworzony wcześniej dla naszych metody akcji "Szczegóły".

Kliknij prawym przyciskiem myszy w obrębie metody akcji usuwania i wybierając polecenie "Dodaj widok" z menu kontekstowego, możemy utworzyć szablon widoku "Delete". W oknie dialogowym "Dodaj widok" Firma Microsoft będzie wskazywać, że firma Microsoft przekazywane do widoku szablonu jako swój model obiektu obiad i chce utworzyć pusty szablon:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Kliknięcie przycisku "Dodaj", programu Visual Studio Dodaj nowy plik szablonu widoku "Delete.aspx" nam naszym katalogu "\Views\Dinners". Dodamy do szablonu, aby zaimplementować ekran potwierdzenia usunięcia niektórych HTML i kodu, takich jak poniżej:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Powyższy kod Wyświetla tytuł obiad do usunięcia, a dane wyjściowe &lt;formularza&gt; element, który wykonuje WPIS /Dinners/Delete / [id] adres URL, jeśli użytkownik końcowy kliknie przycisk "Usuń" znajdujący się w nim.

Firma Microsoft uruchamiania naszych aplikacji i dostępu *"/ kolacji/Delete / [id]"* obiad prawidłowy adres URL obiektu go renderuje interfejsu użytkownika, takie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Temat po stronie: Dlaczego robimy WPIS?** |
| --- |
| Możesz zadawać — Dlaczego kursu nakład pracy tworzenia &lt;formularza&gt; w ramach naszych ekran potwierdzenia usunięcia? Dlaczego nie wystarczy użyć standardowego hiperłącze do łączenia z metodą akcji, który wykonuje operację usuwania rzeczywiste? Przyczyną jest, ponieważ chcemy Uważaj zabezpieczyć się przed przeszukiwarki sieci web i wyszukiwarki odnajdywania adresy URL i przypadkowo powodują, że dane są usuwane, gdy stosują łącza. GET protokołu HTTP, na podstawie adresów URL są traktowane jako "bezpieczne" dla nich dostępu/przeszukiwania i mają nie być zgodne z żądania HTTP POST z nich. Rozsądną regułą jest, aby upewnić się, możesz zawsze umieszczaj destrukcyjne lub operacji modyfikowania danych za żądań POST protokołu HTTP. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementacja metody akcji usuwania POST protokołu HTTP

W efekcie powstał wersji HTTP GET naszych zaimplementowano metody akcji usuwania, który zawiera informacje na ekranie potwierdzenia usunięcia. Gdy użytkownik końcowy kliknie przycisk "Usuń" wykona post formularza */Dinners/obiad / [id]* adresu URL.

Przejdźmy teraz wdrożyć HTTP zachowanie "POST" Usuń metodę akcji przy użyciu poniższego kodu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Wersja żądania HTTP POST naszych metody akcji usuwania podejmie próbę pobrania obiektu obiad do usunięcia. Nie można go znaleźć (ponieważ został już usunięty) powoduje renderowanie szablon "NotFound". Jeśli znajdzie Dinner, usuwa ją z DinnerRepository. Następnie renderowania szablonu "Usunięte".

Do wdrożenia szablonu "Usunięte" Firma Microsoft kliknij prawym przyciskiem myszy w metodzie akcji i wybierz pozycję "Dodaj widok" menu kontekstowego. Utworzymy nazwij naszych widok "Usunięte" i ma on być pusty szablon (i przyjmuje obiekt silnie typizowanym modelem). Następnie dodamy kilka zawartość HTML do niego:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

A teraz możemy uruchamiania naszych aplikacji i dostępu *"/ kolacji/Delete / [id]"* adres URL na obiad prawidłowe potwierdzenie usunięcia obiektu renderowanie zostanie przeprowadzone w naszym obiad ekranu podobnie jak poniżej:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Po kliknięciu przycisku "Usuń" wykona akcję POST protokołu HTTP, aby */Dinners/Delete / [id]* adresu URL, który usunie Dinner w naszej bazie danych i wyświetlić naszych "Usunięte" Wyświetl szablon:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Zabezpieczenia powiązania modelu

Zostały omówione dwa różne sposoby, aby skorzystać z wbudowanych funkcji wiązania modelu programu ASP.NET MVC. Najpierw za pomocą metody UpdateModel() można zaktualizować właściwości na istniejący obiekt modelu i sekundę przy użyciu platformy ASP.NET MVC obsługę przekazywania obiekty modelu jako parametry metody akcji. Obu tych technik jednocześnie są bardzo zaawansowane i bardzo użyteczne.

Te możliwości również wnosi odpowiedzialności. Jest ważne zawsze być paranoicznie o zabezpieczeniach podczas akceptowania danych podawanych przez użytkownika, i dotyczy to również podczas tworzenia powiązania obiektów na dane wejściowe formularza. Należy zachować ostrożność, aby zawsze kodowanie wartości wprowadzone przez użytkownika, które można uniknąć ataki przez iniekcję kodu HTML i JavaScript i należy zwrócić szczególną uwagę na ataki przez iniekcję SQL HTML (Uwaga: użyto LINQ to SQL dla naszej aplikacji, która automatycznie koduje parametry, aby uniknąć tych rodzaje ataków). Nigdy nie należy polegać na samodzielnie weryfikacji po stronie klienta i zawsze stosować weryfikacji po stronie serwera, aby zabezpieczyć się przed hakerami próby wysłać sfałszowany wartości.

Jeden element dodatkowe zabezpieczenia, aby upewnić się, że należy rozważać podczas używania funkcji wiązania platformy ASP.NET MVC jest zakres obiektów, które dokonywane jest wiązanie. W szczególności chcesz upewnij się, że rozumiesz implikacje zabezpieczeń właściwości, które chcesz zezwolić na można powiązać i upewnij się, że Zezwalaj tylko te właściwości, które naprawdę powinny być nadaje się do aktualizacji przez użytkownika końcowego do zaktualizowania.

Domyślnie metoda UpdateModel() będzie podejmować próby aktualizacji wszystkich właściwości obiektu modelu, która pasuje do przychodzącego wartości parametru formularza. Podobnie obiektów przekazywane jako parametry metody akcji również domyślnie może mieć wszystkich ich właściwości, za pomocą parametrów formularza.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blokowanie powiązania na podstawie za użycie

Zasady powiązania wydawane na użycie można zablokować przez udostępnienie jawne "Lista dołączania" właściwości, które mogą być aktualizowane. Można to zrobić przez przekazanie parametr tablicowy dodatkowych parametrów do metody UpdateModel() podobnie jak poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Obiekty przekazywane jako parametry metody akcji również obsługuje atrybutu [powiązania], który umożliwia "obejmują listy" dozwolone właściwości, aby określić jak poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blokowanie powiązania na podstawie typu

Można również zablokować zasad powiązania na podstawie-type. Dzięki temu można określić zasady wiążące tylko raz, a następnie ich stosowania we wszystkich scenariuszach (scenariuszy, w tym UpdateModel i akcji metoda parametr) dla wszystkich kontrolerów i metod akcji.

Reguły powiązania poszczególnych typów można dostosować przez dodanie atrybutu [powiązania] na typ lub rejestrując je w pliku Global.asax aplikacji (przydatne w scenariuszach, w której nie jesteś właścicielem tego typu). Można użyć atrybutu powiązania Include i wykluczanie właściwości do kontrolowania właściwości, które są możliwej do wiązania dla określonej klasy lub interfejsu.

Firma Microsoft będzie tej techniki należy używać klasy obiad w naszej aplikacji NerdDinner, a następnie dodaj atrybut [powiązania] do niego, który ogranicza możliwość użycia na liście właściwości możliwej do wiązania do następujących:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Należy zauważyć, że firma Microsoft nie zezwalają na kolekcji zbędne i można modyfikować za pomocą powiązania ani firma Microsoft umożliwia DinnerID lub HostedBy właściwości można ustawić za pomocą powiązania. Ze względów bezpieczeństwa firma Microsoft będzie zamiast tego jedynie manipulować te konkretnej właściwości, za pomocą jawnego kodu w ramach naszych metody akcji.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

Platforma ASP.NET MVC zawiera wiele wbudowanych funkcji pomagających z implementacji formularza, publikując scenariuszy. Użyliśmy różnych tych funkcji do obsługi operacji CRUD w interfejsie użytkownika na podstawie naszych DinnerRepository.

Używamy skoncentrowane na modelu podejścia do implementowania naszej aplikacji. Oznacza to, że nasze weryfikacji i reguły biznesowej logiki jest definiowana w naszej warstwie modelu — i nie w ramach naszych kontrolery i widoki. Nasze klasy kontrolera ani nasze szablony widoku nic wiedzieć o reguły biznesowe określonego wymuszana przez naszych obiad klasy modelu.

Spowoduje to zachować czyste naszej architektury aplikacji i ułatwić testowanie. Możemy dodać dodatkowe reguły biznesowe w przyszłości do naszych warstwy modelu i *nie musisz wprowadzać żadnych zmian w kodzie* do naszych kontrolera lub widoku w kolejności ich, które są obsługiwane. To spowoduje utworzenie Opisz dużą elastyczność rozwijać i zmieniać naszej aplikacji w przyszłości.

Nasze DinnersController teraz umożliwia obiad oferty/details, a także tworzenia, edytowania i usuwania pomocy technicznej. Kompletny kod dla klasy znajdują się poniżej:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Następny krok

W efekcie powstał podstawową obsługę CRUD (tworzenia, odczytu, aktualizowania lub usuwania) wdrożenia w ramach naszych DinnersController klasy.

Teraz Spójrzmy na zastosowanie podejścia ViewData i ViewModel klasy umożliwiające jeszcze dokładniej interfejsu użytkownika w formularzach naszych.

> [!div class="step-by-step"]
> [Poprzednie](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [dalej](use-viewdata-and-implement-viewmodel-classes.md)
