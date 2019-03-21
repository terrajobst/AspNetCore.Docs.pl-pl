---
title: Tworzenie pomocników tagów w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzenie pomocników tagów w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: ddfd7cb8c67e28709b8ce75d5a4d0a8c0c0cc43c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210083"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>Tworzenie pomocników tagów w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Rozpoczynanie pracy z usługą pomocnicy tagów

Ten samouczek zawiera wprowadzenie do programowania pomocników tagów. [Wprowadzenie do pomocników tagów](intro.md) opisuje korzyści, które dają pomocników tagów.

Pomocnik tagu jest każda klasa implementująca `ITagHelper` interfejsu. Jednak podczas tworzenia pomocnika tagów ogólnie klasy wyprowadzonej z `TagHelper`, dlatego zapewnia dostęp do wykonywania `Process` metody.

1. Utwórz nowy projekt ASP.NET Core o nazwie **AuthoringTagHelpers**. Nie wymaga uwierzytelniania dla tego projektu.

1. Utwórz folder do przechowywania pomocnicy tagów, o nazwie *TagHelpers*. *TagHelpers* folder jest *nie* wymagane, ale jest to uzasadnione Konwencji. Teraz Rozpocznijmy od pisania pomocników kilka prostych tagów.

## <a name="a-minimal-tag-helper"></a>Minimalny pomocnika tagów

W tej sekcji możesz zapisanie pomocnika tagów, która aktualizuje tag wiadomości e-mail. Na przykład:

```html
<email>Support</email>
```

Serwer zastosuje nasze Pomocnik tagu wiadomości e-mail do konwertowania tego znacznika do następujących czynności:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Oznacza to, że tag kotwicy temu to łącze w wiadomości e-mail. Można to zrobić, jeśli piszesz aparatu blogu i potrzebny do wysyłania wiadomości e-mail, marketing, pomocy technicznej i inne kontakty wszystkie do tej samej domenie.

1. Dodaj następujący kod `EmailTagHelper` klasy *TagHelpers* folderu.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * Pomocnicy tagów używać konwencji nazewnictwa, który jest przeznaczony dla elementów główna nazwa klasy (minus *pomocnika tagów* część nazwy klasy). W tym przykładzie nazwa głównego **EmailTagHelper** jest *e-mail*, więc `<email>` zostaną objęte tagu. Konwencja nazewnictwa powinny działać dla większości pomocnicy tagów, później zaprezentuję, jak go zastąpić.

   * `EmailTagHelper` Klasa pochodzi od `TagHelper`. `TagHelper` Klasa udostępnia metody i właściwości do pisania pomocników tagów.

   * Zastąpione `Process` metoda kontroluje Pomocnik tagu działania podczas wykonywania. `TagHelper` Klasa udostępnia także asynchroniczna wersja (`ProcessAsync`) z tymi samymi parametrami.

   * Parametr kontekstowy do `Process` (i `ProcessAsync`) zawiera informacje związane z wykonywaniem bieżącego tagu HTML.

   * Parametr wyjściowy do `Process` (i `ProcessAsync`) zawiera element HTML stanowych językiem oryginalne źródło, które są używane do generowania tagu HTML i zawartości.

   * Nasze Nazwa klasy ma sufiks **pomocnika tagów**, czyli *nie* wymagane, ale ma on uznawany za Konwencję najlepszym rozwiązaniem. Można zadeklarować klasy jako:

   ```csharp
   public class Email : TagHelper
   ```

1. Aby `EmailTagHelper` klasy dostępne dla wszystkich naszych widokami Razor, należy dodać `addTagHelper` dyrektywę *Views/_ViewImports.cshtml* pliku: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   Powyższy kod używa składni symbolu wieloznacznego do określania, że wszystkie pomocników tagów w naszym zestawie będą dostępne. Pierwszy ciąg po `@addTagHelper` określa Pomocnik tagu, można załadować (Użyj "*" dla pomocników tagów), i drugi ciąg "AuthoringTagHelpers" Określa zestaw Pomocnik tagu. Należy również zauważyć, że drugi wiersz wiąże pomocników tagów ASP.NET Core MVC za pomocą składni symboli wieloznacznych (pomocników te zostały omówione w [wprowadzenie do pomocników tagów](intro.md).) Jest `@addTagHelper` dyrektywy, która udostępnia Pomocnik tagu w widoku Razor. Alternatywnie można podać w pełni kwalifikowana nazwa (FQN) pomocnika tagów, jak pokazano poniżej:

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie **nazwy zestawu** (*AuthoringTagHelpers*, co niekoniecznie `namespace`). Większość programistów będą najpierw użyj składni symboli wieloznacznych. [Wprowadzenie do pomocników tagów](intro.md) określa szczegółowo składni dodawania, usuwania, hierarchii i symboli wieloznacznych pomocnika tagów.

1. Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* plików za pomocą tych zmian:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uruchom aplikację i użyj ulubionej przeglądarce, aby wyświetlić źródło HTML, aby można było sprawdzić tagi wiadomości e-mail są zastępowane znaczników zakotwiczenia (na przykład `<a>Support</a>`). *Pomocy technicznej* i *marketingu* są renderowane jako linki, ale nie mają `href` atrybutu, aby stały się funkcjonalne. Naprawimy, w następnej sekcji.

## <a name="setattribute-and-setcontent"></a>SetAttribute i SetContent

W tej sekcji dodamy `EmailTagHelper` tak, aby go utworzy tag kotwicy prawidłowy do obsługi poczty e-mail. Zaktualizujemy go, aby pobierał informacje z widoku Razor (w postaci `mail-to` atrybutu) i używać go podczas generowania zakotwiczenia.

Aktualizacja `EmailTagHelper` klasy następującym kodem:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* Pascal — z uwzględnieniem wielkości liter nazwy klasy i właściwości pomocników tagów są tłumaczone na ich [przypadek kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). W związku z tym Aby użyć `MailTo` użyjemy atrybutu `<email mail-to="value"/>` równoważne.

* Ostatni wiersz ustawia ukończone zawartości dla naszych Pomocnik tagu minimalny zestaw funkcjonalności.

* Wyróżniony wiersz pokazuje składnię na dodawanie atrybutów:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Takie podejście działa dla atrybutu "href", tak długo, jak obecnie nie istnieje w kolekcji atrybutów. Można również użyć `output.Attributes.Add` metodę, aby dodać atrybut pomocnika tagów na końcu kolekcji atrybutów tagu.

1. Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* plików za pomocą tych zmian: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. Uruchom aplikację i sprawdzić generuje poprawne linki.

<a name="self-closing"></a>

   > [!NOTE]
   > Gdyby można zapisać wiadomość e-mail tag samozamykający (`<email mail-to="Rick" />`), końcowych danych wyjściowych będą również samozamykającego. Aby włączyć możliwość pisania tag z tagu początkowego (`<email mail-to="Rick">`) musi dekoracji klasy następującym kodem:
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   Za pomocą samozamykającego pomocnika tagów poczty e-mail, dane wyjściowe będą `<a href="mailto:Rick@contoso.com" />`. Samozamykającego zakotwiczenia tagi nie są prawidłowe HTML, co nie byłoby dobrze, utwórz ją, ale możesz chcieć utworzyć pomocnika tagów, który jest samozamykającego. Pomocnicy tagów Ustaw typ `TagMode` właściwości po przeczytaniu tag.

### <a name="processasync"></a>ProcessAsync

W tej sekcji będziemy pisać pomocnika asynchronicznego wiadomości e-mail.

1. Zastąp `EmailTagHelper` klasy z następującym kodem:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Uwagi:**

   * Ta wersja wykorzystuje asynchroniczną `ProcessAsync` metody. Asynchroniczną `GetChildContentAsync` zwraca `Task` zawierający `TagHelperContent`.

   * Użyj `output` parametru, aby pobrać zawartość elementu HTML.

1. Wprowadź następującą zmianę w celu *Views/Home/Contact.cshtml* pliku, dzięki czemu pomocnika tagów można pobrać adresu e-mail docelowej.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uruchom aplikację i sprawdzić generuje łącza prawidłowy adres e-mail.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent

1. Dodaj następujący kod `BoldTagHelper` klasy *TagHelpers* folderu.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * `[HtmlTargetElement]` Atrybutu przekazuje parametr atrybutu, który określa, czy dowolnego elementu HTML zawierającego atrybut HTML o nazwie "bold" będą zgodne, a `Process` uruchomi metoda przesłonięcia w klasie. W naszym przykładzie `Process` metoda usuwa atrybut "bold" i otacza zawierającego znaczników przy użyciu `<strong></strong>`.

   * Ponieważ nie chcesz zastąpić istniejący znacznik zawartości, należy napisać otwarcia `<strong>` oznaczyć za pomocą `PreContent.SetHtmlContent` metody i zamknięcie `</strong>` oznaczyć za pomocą `PostContent.SetHtmlContent` metody.

1. Modyfikowanie *About.cshtml* widok ma zawierać `bold` wartość atrybutu. Poniżej przedstawiono kompletny kod.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. Uruchom aplikację. Ulubionej przeglądarce służy do kontroli źródła oraz sprawdź znaczników.

   `[HtmlTargetElement]` Atrybut powyżej jest przeznaczony tylko dla znaczników HTML, który zawiera nazwę "bold". `<bold>` Element nie został zmodyfikowany przez pomocnika tagów.

1. Komentarz `[HtmlTargetElement]` wiersza atrybutu i domyślnie przeznaczonych dla `<bold>` tagów, oznacza to, że kod znaczników HTML w formularzu `<bold>`. Należy pamiętać, że domyślna konwencja nazw będzie zgodną z nazwą klasy **Bold**pomocnika tagów do `<bold>` tagów.

1. Uruchom aplikację i upewnij się, że `<bold>` tag jest przetwarzany przez pomocnika tagów.

Klasa z wieloma urządzanie `[HtmlTargetElement]` atrybuty wyniki w logiczne OR elementów docelowych. Na przykład przy użyciu poniższego kodu, pogrubienie tag lub atrybut bold będą zgodne.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Gdy wiele atrybutów zostaną dodane do tej samej instrukcji, środowisko uruchomieniowe traktować je jak logicznego operatora AND. Na przykład w poniższym kodzie HTML element musi mieć nazwę "bold" przy użyciu atrybutu o nazwie "bold" (`<bold bold />`) do dopasowania.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Można również użyć `[HtmlTargetElement]` do zmiany nazwy elementu docelowego. Na przykład, jeżeli chcesz `BoldTagHelper` do obiektu docelowego `<MyBold>` tagów, należy użyć następującego atrybutu:

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a>Przekaż modelu do pomocnika tagów

1. Dodaj *modeli* folderu.

1. Dodaj następujący kod `WebsiteContext` klasy *modeli* folderu:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. Dodaj następujący kod `WebsiteInformationTagHelper` klasy *TagHelpers* folderu.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * Jak wspomniano wcześniej, dokonuje translacji pomocnicy tagów, Pascal — z uwzględnieniem wielkości liter C# klasy nazwy i właściwości dla pomocników tagów w [przypadek kebab](http://wiki.c2.com/?KebabCase). W związku z tym Aby użyć `WebsiteInformationTagHelper` w aparacie Razor, Ty napiszesz `<website-information />`.

   * Nie są jawnie identyfikuje element docelowy z `[HtmlTargetElement]` atrybutu, więc domyślną `website-information` docelowe. Jeśli zastosowano atrybut (Uwaga nie jest przypadek kebab, ale jest zgodna z nazwą klasy):

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   Tag przypadków kebab `<website-information />` nie odpowiada. Jeśli chcesz używać `[HtmlTargetElement]` atrybutu, należy użyć kebab przypadek, jak pokazano poniżej:

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * Elementy, które są samozamykającego nie ma zawartości. W tym przykładzie znaczników Razor użyje tagu samozamykającego, ale zostanie utworzona Pomocnik tagu [sekcji](http://www.w3.org/TR/html5/sections.html#the-section-element) element (który nie jest samozamykającego i pisania zawartości wewnątrz `section` elementu). W związku z tym, należy ustawić `TagMode` do `StartTagAndEndTag` do zapisywania danych wyjściowych. Alternatywnie możesz przekształcić w komentarz ustawienia linii `TagMode` i Zapisywanie kodu znaczników przy użyciu tagu zamykającego. (Przykład znaczników znajduje się w dalszej części tego samouczka).

   * `$` (Znak dolara) w następującym wierszu używa [ciągiem interpolowanym](/dotnet/csharp/language-reference/keywords/interpolated-strings):

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. Dodaj następujący kod do *About.cshtml* widoku. Wyróżnione znaczników Wyświetla informacje o witryny sieci web.

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > W znacznikach Razor, pokazano poniżej:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > Wie razor `info` atrybut jest klasą, a nie w ciągu i chcesz pisać kod w języku C#. Dowolny atrybut pomocnika tagów niebędących ciągami powinny być zapisywane bez `@` znaków.

1. Uruchom aplikację, a następnie przejdź do widoku informacje, aby wyświetlić informacje witryny sieci web.

   > [!NOTE]
   > Można użyć następujących znaczników przy użyciu tagu zamykającego i Usuń wiersz z `TagMode.StartTagAndEndTag` w Pomocnik tagu:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=20-21)]

## <a name="condition-tag-helper"></a>Pomocnik tagu warunku

Pomocnik tagu warunek renderuje dane wyjściowe przy przekazywaniu wartość true.

1. Dodaj następujący kod `ConditionTagHelper` klasy *TagHelpers* folderu.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. Zastąp `Index` method in Class metoda `Home` kontrolera, używając następującego kodu:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. Uruchom aplikację i przejdź do strony głównej. Kod znaczników w warunkowej `div` nie będą renderowane. Dołącz ciąg zapytania `?approved=true` do adresu URL (na przykład `http://localhost:1235/Home/Index?approved=true`). `approved` jest ustawiona na wartość PRAWDA, a warunkową znaczników, który będzie wyświetlany.

> [!NOTE]
> Użyj [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator, aby określić atrybut docelowy, a nie określając ciąg, jak w przypadku Pomocnik tagu pogrubienie:
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> [Nameof](/dotnet/csharp/language-reference/keywords/nameof) operator ma być chroniony kod powinien on nigdy nie być refaktoryzowany (Firma Microsoft może chcesz zmienić nazwę aby `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Unikaj konfliktów pomocnika tagów

W tej sekcji możesz zapisywać parę automatycznego łączenia pomocników tagów. Pierwszy spowoduje zastąpienie znaczników zawierający adres URL rozpoczynający się za pośrednictwem protokołu HTTP HTML zakotwiczenia tag zawierających ten sam adres URL (a zatem reaguje łącze do adresu URL). Drugi będzie tak samo dla danego adresu URL począwszy od WWW.

Ponieważ te dwa pomocników są ściśle powiązane i mogą je refaktoryzować w przyszłości, zachowamy je w tym samym pliku.

1. Dodaj następujący kod `AutoLinkerHttpTagHelper` klasy *TagHelpers* folderu.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >`AutoLinkerHttpTagHelper` Klasy obiektów docelowych `p` elementów i używa [wyrażenia regularnego](/dotnet/standard/base-types/regular-expression-language-quick-reference) utworzyć zakotwiczenia.

1. Dodaj następujący kod na końcu *Views/Home/Contact.cshtml* pliku:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. Uruchom aplikację i sprawdź Pomocnik tagu poprawnie renderowana zakotwiczenia.

1. Aktualizacja `AutoLinker` klasy, aby uwzględnić `AutoLinkerWwwTagHelper` który przekonwertuje www tekstu zawierającego oryginalny tekst www tag kotwicy. Zaktualizowany kod jest wyróżniona poniżej:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. Uruchom aplikację. Zwróć uwagę, tekst www jest renderowany jako link, ale nie jest tekst HTTP. Jeśli umieścisz punkt przerwania w obu klasach widać, że klasa pomocnika tagów HTTP jest uruchamiany pierwszego. Problem polega na że buforowania danych wyjściowych pomocnika tagów, a gdy pomocnik tagu WWW jest uruchomiona, zastępuje on buforowane dane wyjściowe z Pomocnik tagu HTTP. W dalszej części tego samouczka zobaczymy sposobu kontrolowania przez pomocników tagów uruchamiane w kolejności. Naprawimy ten kod z następujących czynności:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > W pierwszym wydaniu pomocników tagów automatycznego łączenia stało się zawartość elementu docelowego z następującym kodem:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > Oznacza to, należy wywołać `GetChildContentAsync` przy użyciu `TagHelperOutput` przekazany do `ProcessAsync` metody. Jak wspomniano wcześniej, ponieważ dane wyjściowe są buforowane, ostatni Oznacz element pomocniczy służący do uruchamiania usługi wins. Naprawiono problem z następującym kodem:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > Powyższy kod sprawdza, czy zawartość została zmodyfikowana, a jeśli tak, pobiera zawartość z buforu wyjściowego.

1. Uruchom aplikację i sprawdzić, czy dwa linki działają zgodnie z oczekiwaniami. Chociaż wydaje się, że naszych Pomocnik tagu konsolidator automatycznie jest prawidłowe i kompletne, ma drobny problem. Jeśli Pomocnik tagu WWW jest uruchamiany w pierwszy, linki sieci Web nie będą poprawne. Aktualizowanie kodu, dodając `Order` przeciążenie, można określić kolejność pracująca w tagu w. `Order` Właściwość określa kolejność wykonywania względem innych pomocników tagów przeznaczonych dla tego samego elementu. Wartość domyślna kolejności to zero, a wystąpienia o niższych wartościach są wykonywane jako pierwsze.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   Powyższy kod gwarantuje, że Pomocnik tagu HTTP jest uruchamiany przed Pomocnik tagu WWW. Zmiana `Order` do `MaxValue` i sprawdź, czy znaczniki wygenerowany dla tagu WWW jest niepoprawny.

## <a name="inspect-and-retrieve-child-content"></a>Sprawdzanie i pobrać zawartość elementu podrzędnego

Pomocnicy tagów zawierają kilka właściwości, aby pobrać zawartość.

* Wynik `GetChildContentAsync` można dołączyć do `output.Content`.
* Możesz sprawdzić wynik `GetChildContentAsync` z `GetContent`.
* Jeśli zmodyfikujesz `output.Content`, treść pomocnika tagów nie będą wykonywane lub renderowane, chyba że wywołujesz `GetChildContentAsync` tak jak w naszym przykładzie automatycznie konsolidatora:

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* Wiele wywołań `GetChildContentAsync` zwraca taką samą wartość i nie jest ponownie wykonywana `TagHelper` treści, chyba że przyjmie wartość false parametru, wskazującą, aby nie korzystała z buforowanego zestawu wyników.
