---
title: "Tworzenie pomocników tagów w platformy ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak tworzyć pomocników tagów w ASP.NET Core."
keywords: "Platformy ASP.NET Core pomocników tagów"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cbe46ee1d3cd9f7a30a87d364074f1302f9af7ab
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Tworzenie pomocników tagów w ASP.NET Core wskazówki próbki

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="getting-started-with-tag-helpers"></a>Wprowadzenie do korzystania z pomocników tagów

Ten samouczek zawiera wprowadzenie do programowania pomocników tagów. [Wprowadzenie do pomocników tagów](intro.md) opisuje korzyści, które zapewniają pomocników tagów.

Obiekt pomocnika tagów jest każda klasa implementująca `ITagHelper` interfejsu. Jednak podczas tworzenia pomocnika tagów można zwykle pochodzi od `TagHelper`, wykonanie tak zapewnia dostęp do `Process` metody.

1. Utwórz nowy projekt platformy ASP.NET Core o nazwie **AuthoringTagHelpers**. Dla tego projektu nie ma potrzeby uwierzytelniania.

2. Utwórz folder do przechowywania pomocników tagu o nazwie *TagHelpers*. *TagHelpers* folder jest *nie* wymagane, ale jest uzasadnione Konwencję. Teraz do dzieła pisania pomocników niektórych prostych tagów.

## <a name="a-minimal-tag-helper"></a>Minimalny pomocnika tagów

W tej sekcji możesz zapisać pomocnika tagów, która aktualizuje tag poczty e-mail. Na przykład:

```html
<email>Support</email>
   ```

Nasze pomocnika tagów poczty e-mail będzie używany przez serwer programu można przekonwertować tego znacznika do następujących czynności:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

Oznacza to, że tag kotwicy dzięki temu to łącze w wiadomości e-mail. Można to zrobić, jeśli pisania aparat blogu i potrzebny do wysyłania wiadomości e-mail sprzedaży, pomocy technicznej i inne kontaktów wszystko do tej samej domenie.

1.  Dodaj następujące `EmailTagHelper` klasy do *TagHelpers* folderu.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Uwagi:**
    
    * Pomocników tagów używać konwencji nazewnictwa, która dotyczy elementów Nazwa klasy głównym (minus *pomocnika tagów* część nazwy klasy). W tym przykładzie nazwa głównego **E-mail**pomocnika tagów jest *e-mail*, więc `<email>` tag będą stosowane. Konwencja nazewnictwa powinien działać w przypadku większości pomocników tagów, później zostanie omówiony sposób jej zastąpienie.
    
    * `EmailTagHelper` Pochodną klasy `TagHelper`. `TagHelper` Klasa udostępnia metody i właściwości do pisania pomocników tagów.
    
    * Zastąpione `Process` kontrolki metody pomocnika tagów czego po wykonaniu. `TagHelper` Klasa udostępnia także asynchroniczną wersję (`ProcessAsync`) z takimi samymi parametrami.
    
    * Parametr kontekstowy do `Process` (i `ProcessAsync`) zawiera informacje związane z wykonywaniem bieżącego tagu HTML.
    
    * Parametr wyjściowy do `Process` (i `ProcessAsync`) zawiera element HTML stanowe reprezentatywny z oryginalnego źródła służącego do generowania tagu HTML i zawartości.
    
    * Nasze Nazwa klasy zawiera sufiks **pomocnika tagów**, który jest *nie* wymagane, ale uwzględniono Konwencji najlepsze praktyki. Można zadeklarować tej klasy jako:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Aby `EmailTagHelper` klasy dostępne dla wszystkich naszych widokami Razor, Dodaj `addTagHelper` dyrektywy do *Views/_ViewImports.cshtml* pliku:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Powyższy kod używa składni symboli wieloznacznych do określenia tagów pomocników w naszym zestawie będą dostępne. Pierwszy ciąg po `@addTagHelper` określa pomocnika tagów można załadować (Użyj "*" dla pomocników tagów), a drugi ciąg "AuthoringTagHelpers" Określa zestaw Pomocnik ten tag. Należy również zauważyć, że drugi wiersz zaimportowanie pomocników tagów platformy ASP.NET Core MVC przy użyciu składni symbolu wieloznacznego (pomocników te zostały omówione w [wprowadzenie do pomocników tagów](intro.md).) Jest `@addTagHelper` dyrektywy, która udostępnia pomocnika tagów do widoku Razor. Alternatywnie można podać w pełni kwalifikowana nazwa (FQN) pomocniczych znaczników, jak pokazano poniżej:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Aby dodać pomocnika tagów do widoku, używając FQN, należy najpierw dodać FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), a następnie nazwy zestawu (*AuthoringTagHelpers*). Większość deweloperów będzie wolą używać składni symboli wieloznacznych. [Wprowadzenie do pomocników tagów](intro.md) określa szczegółowo składni Dodawanie, usuwanie, hierarchii i symboli wieloznacznych pomocnika tagów.
    
3.  Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* pliku z tych zmian:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Uruchom aplikację i użyj ulubionej przeglądarce, aby wyświetlić źródło HTML, aby zweryfikować tagi poczty e-mail są zastępowane znacznika zakotwiczenia (na przykład `<a>Support</a>`). *Pomocy technicznej* i *Marketing* są renderowane jako łącza, ale nie mają `href` atrybutu w celu zapewnienia ich funkcjonalności. Firma Microsoft będzie rozwiązać ten problem w następnej sekcji.

Uwaga: Jak tagów HTML i atrybutów, tagi, nazwy klas i atrybutów w Razor i C# nie jest rozróżniana.

## <a name="setattribute-and-setcontent"></a>SetAttribute i SetContent

W tej sekcji, będziemy informować `EmailTagHelper` tak, aby tworzy tag kotwicy prawidłowy do obsługi poczty e-mail. Będziemy informować podjęcia informacji z widoku Razor (w postaci `mail-to` atrybut) i używać go do generowania zakotwiczenia.

Aktualizacja `EmailTagHelper` klasy następującym kodem:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Uwagi:**

* Pascal — z uwzględnieniem wielkości liter nazwy klasy i właściwości pomocników tagów są przekształcane na ich [obniżyć przypadku kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). W związku z tym Aby użyć `MailTo` użyjesz atrybutu, `<email mail-to="value"/>` równoważne.

* Ostatni wiersz ustawia ukończone zawartość dla naszych pomocnika tag minimalny funkcjonalności.

* Wyróżniony wiersz przedstawiono składnię dodawania atrybutów:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Takie podejście działa dla atrybutu "href", tak długo, jak obecnie nie istnieje w kolekcji atrybutów. Można również użyć `output.Attributes.Add` metody, aby dodać atrybut pomocnika tagów do końca kolekcji atrybutów tagu.

1.  Aktualizowanie kodu znaczników w *Views/Home/Contact.cshtml* pliku z tych zmian:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Uruchom aplikację i sprawdź generuje poprawne łącza.
    
    > [!NOTE]
    >Gdyby zapisu poczty e-mail tag własnym zamknięcia (`<email mail-to="Rick" />`), również będą samozamykającego ostateczne dane wyjściowe. Aby włączyć możliwość zapisu tag z tagiem początkowym (`<email mail-to="Rick">`) musi dekoracji klasy następującym kodem:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Z samozamykającego pomocnika tagów poczty e-mail, dane wyjściowe będą `<a href="mailto:Rick@contoso.com" />`. Samozamykającego Tag kotwicy nie są prawidłowe HTML, więc nie chcesz utworzyć, ale warto utworzyć pomocnika tag, który jest samozamykającego. Pomocników tagów Ustaw typ `TagMode` właściwości po odczytaniu tag.
    
### <a name="processasync"></a>ProcessAsync

W tej sekcji firma Microsoft będzie zapisu pomocnika asynchroniczne poczty e-mail.

1.  Zastąp `EmailTagHelper` klasy następującym kodem:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Uwagi:**

    * Ta wersja używa asynchroniczną `ProcessAsync` metody. Asynchroniczną `GetChildContentAsync` zwraca `Task` zawierający `TagHelperContent`.

    * Użyj `output` parametr, aby pobrać zawartość elementu HTML.

2.  Wprowadź następujące zmiany do *Views/Home/Contact.cshtml* plików, dlatego pomocnika tagów można pobrać docelowych wiadomości e-mail.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Uruchom aplikację i sprawdź, czy generuje łącza prawidłowy adres e-mail.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent i PostContent.SetHtmlContent

1.  Dodaj następujące `BoldTagHelper` klasy do *TagHelpers* folderu.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Uwagi:**
    
    * `[HtmlTargetElement]` Atrybutu przekazuje parametr atrybutu, który określa, czy dowolnego elementu HTML zawierającego atrybut HTML o nazwie "bold" jest zgodny, i `Process` metoda przesłonięcia w klasie zostanie uruchomiony. W naszym przykładzie `Process` metoda usuwa atrybut "bold" i otaczający zawierającego znaczników z `<strong></strong>`.
    
    * Ponieważ nie chcesz zastąpić istniejący znacznik zawartości, należy napisać otwarcia `<strong>` tagu z `PreContent.SetHtmlContent` — metoda i zamykania `</strong>` Oznacz za pomocą `PostContent.SetHtmlContent` — metoda.
    
2.  Modyfikowanie *About.cshtml* widok ma zawierać `bold` wartość atrybutu. Poniżej przedstawiono kompletny kod.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Uruchom aplikację. Ulubionej przeglądarce służy do kontroli źródła i sprawdź kod znaczników.

    `[HtmlTargetElement]` Atrybut powyżej dotyczy tylko kod znaczników HTML, który zawiera nazwę atrybutu "bold". `<bold>` Element nie został zmodyfikowany przez pomocnika tagów.

4. Komentarz `[HtmlTargetElement]` wiersza atrybutu i domyślnie przeznaczonych dla `<bold>` tagów, oznacza to, że kod znaczników HTML w formie `<bold>`. Należy pamiętać, że domyślną konwencję nazewnictwa będzie zgodna z nazwą klasy **Bold**pomocnika tagów do `<bold>` tagów.

5. Uruchom aplikację i sprawdź, czy `<bold>` tag są przetwarzane przez pomocnika tagów.

Klasa z wieloma dekoracji `[HtmlTargetElement]` atrybutów wyników w logiczne lub elementy docelowe. Na przykład przy użyciu poniższy kod, bold tag lub atrybut pogrubienia będą zgodne.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Jeśli wiele atrybutów zostaną dodane do tej samej instrukcji, środowisko uruchomieniowe traktuje je jako logicznego koniunkcji binarnej. Na przykład w poniższym kodzie HTML element muszą nosić nazwy "bold" z atrybutem o nazwie "bold" (`<bold bold />`) do dopasowania.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Można również użyć `[HtmlTargetElement]` Aby zmienić nazwę elementu docelowego. Na przykład jeśli potrzebujesz `BoldTagHelper` do docelowego `<MyBold>` tagi, należy użyć następującego atrybutu:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a>Przekazywanie modelu do pomocnika tagów

1.  Dodaj *modele* folderu.

2.  Dodaj następujące `WebsiteContext` klasy do *modele* folderu:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Dodaj następujące `WebsiteInformationTagHelper` klasy do *TagHelpers* folderu.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Uwagi:**
    
    * Jak wspomniano wcześniej, pomocników tagów tłumaczy liter Pascal C#, nazwy klas i właściwości dla pomocników tagów do [obniżyć przypadku kebab](http://wiki.c2.com/?KebabCase). W związku z tym Aby użyć `WebsiteInformationTagHelper` w elemencie Razor, przedstawiono tworzenie `<website-information />`.
    
    * Nie są jawnie identyfikujący element docelowy z `[HtmlTargetElement]` atrybutu to domyślne ustawienie `website-information` będą stosowane. Po zastosowaniu następującego atrybutu (Uwaga nie jest kebab, ale jest zgodna z nazwą klasy):
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Niższe znacznika skrzynki kebab `<website-information />` nie będą zgodne. Jeśli chcesz używać `[HtmlTargetElement]` atrybutu, należy użyć przypadku kebab, jak pokazano poniżej:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Elementy, które są samozamykającego nie ma zawartości. Na przykład znaczników Razor użyje tagu samozamykającego, ale zostanie utworzony pomocnika tagów [sekcji](http://www.w3.org/TR/html5/sections.html#the-section-element) elementu (który nie jest samozamykającego i pisania zawartości wewnątrz `section` elementu). W związku z tym należy ustawić `TagMode` do `StartTagAndEndTag` do zapisywania danych wyjściowych. Alternatywnie komentarz ustawienia linii `TagMode` i pisanie kodu znaczników przy użyciu tagu zamykającego. (Znaczników przykład znajduje się w dalszej części tego samouczka).
    
    * `$` (Dolar) w następującym wierszu używa [interpolowane ciąg](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Dodaj następujący kod do *About.cshtml* widoku. Wyróżnione znaczników zawiera informacje dotyczące witryny sieci web.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > W znaczniku Razor, pokazano poniżej:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Zna razor `info` atrybut jest klasa, nie ciągu i chcesz zapisać kod w języku C#. Wszelkie atrybut pomocnika tagów innych niż ciąg powinien być zapisywany bez `@` znaków.
    
5.  Uruchom aplikację i przejdź do widoku informacje, zapoznaj się z informacjami w witrynie sieci web.

    >[!NOTE]
    >Można użyć następującego kodu znaczników przy użyciu tagu zamykającego i Usuń wiersz z `TagMode.StartTagAndEndTag` w pomocnika tagów:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Warunek pomocnika tagów

Warunek pomocnika tagów renderuje dane wyjściowe po upływie wartość true.

1.  Dodaj następujące `ConditionTagHelper` klasy do *TagHelpers* folderu.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Zastąp zawartość *Views/Home/Index.cshtml* pliku z następujący kod:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Zastąp `Index` metoda `Home` kontrolera następującym kodem:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Uruchom aplikację i przejdź do strony głównej. Znaczniki w warunkowej `div` nie będzie renderowany. Dołącz ciągu zapytania `?approved=true` do adresu URL (na przykład `http://localhost:1235/Home/Index?approved=true`). `approved`ma ustawioną wartość true a warunkowe zostanie wyświetlony kod znaczników.

>[!NOTE]
>Użyj [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatora, aby określić atrybut docelowy zamiast określania ciąg, jak w przypadku Pomocnik bold tagu:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatora będzie chronić kod powinien on kiedykolwiek można refaktorować (może chcemy zmienić nazwę na `RedCondition`).

### <a name="avoiding-tag-helper-conflicts"></a>Unikanie konfliktów pomocnika tagów

W tej sekcji należy napisać parę automatyczne łączenie pomocników tagów. Pierwszy spowoduje zastąpienie znaczników zawierający adres URL rozpoczynający się protokołu HTTP z HTML zakotwiczenia tag zawierający ten sam adres URL (i w związku z tym reaguje łącze do adresu URL). Drugi będzie wykonywać takie same dla danego adresu URL począwszy WWW.

Ponieważ te dwa pomocników są ściśle powiązane i mogą je w przyszłości Refaktoryzuj, firma Microsoft będzie przechowywać w tym samym pliku.

1.  Dodaj następujące `AutoLinkerHttpTagHelper` klasy do *TagHelpers* folderu.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper` Klas docelowych `p` elementów i używa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) utworzyć zakotwiczenia.

2.  Dodaj następujący kod na końcu *Views/Home/Contact.cshtml* pliku:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Uruchom aplikację i sprawdź pomocnika tagów poprawnie renderuje zakotwiczenia.

4.  Aktualizacja `AutoLinker` klasy, aby uwzględnić `AutoLinkerWwwTagHelper` który przekonwertuje tekst www tag kotwicy zawierający oryginalny tekst www. Zaktualizowany kod zostanie wyróżniona poniżej:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Uruchom aplikację. Zwróć uwagę, www tekst jest traktowany jako łącze, ale nie jest tekst HTTP. Jeśli umieścisz punkt przerwania w obu klasach widać, czy klasa pomocnika tagów HTTP jest uruchamiany pierwszy. Problem polega na dane wyjściowe pomocnika tagów są buforowane, czy po uruchomieniu pomocnika tagów WWW, zastępuje on dane wyjściowe pamięci podręcznej pomocnika tagów HTTP. W dalszej części samouczka przedstawiono będzie sterowanie pomocników tagów uruchamiane w kolejności. Firma Microsoft będzie naprawić kod z następujących czynności:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >W pierwszym wydaniu pomocników tagów automatyczne łączenie masz zawartość elementu docelowego z następującym kodem:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >Oznacza to, należy wywołać `GetChildContentAsync` przy użyciu `TagHelperOutput` przekazany `ProcessAsync` metody. Jak wspomniano wcześniej, ponieważ dane wyjściowe są buforowane, ostatniego tagu pomocnika do uruchamiania usługi wins. Został rozwiązany problem z następującym kodem:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Powyższy kod sprawdza, czy zawartość została zmodyfikowana, a jeśli tak, pobiera zawartość z buforu wyjściowego.

6.  Uruchom aplikację i sprawdzić, czy dwa łącza działają zgodnie z oczekiwaniami. Gdy wydaje się, że nasze pomocnika tagów konsolidatora automatycznie jest prawidłowe i kompletne ma niewielkie problem. Jeśli pierwszy uruchamia pomocnika tagów WWW, linki sieci Web nie będą poprawne. Zaktualizuj kod, dodając `Order` przeciążenia można sterować kolejnością działającym w tagu. `Order` Właściwość określa kolejność wykonywania względem innych pomocników tagów przeznaczonych dla tego samego elementu. Wartość domyślna kolejności to zero, a wystąpienia o niższych wartościach jest wykonywany jako pierwszy.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Powyższy kod zagwarantuje, że pomocnika tagów HTTP jest uruchamiany przed pomocnika tagów WWW. Zmień `Order` do `MaxValue` i sprawdź, czy znacznika generowany dla tagu WWW jest niepoprawny.

## <a name="inspecting-and-retrieving-child-content"></a>Sprawdzanie i pobierania zawartość elementu podrzędnego

Pomocników tagów podać kilka właściwości, aby pobrać zawartość.

-  Wynik `GetChildContentAsync` można dołączać do `output.Content`.
-  Możesz sprawdzić wynik `GetChildContentAsync` z `GetContent`.
-  Jeśli zmodyfikujesz `output.Content`, treści pomocnika tagów nie zostanie wykonana ani renderowane, chyba że wywołujesz `GetChildContentAsync` jak naszej próbki automatycznie konsolidatora:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Wiele wywołań `GetChildContentAsync` zwróci taką samą wartość i nie zostanie wykonany ponownie `TagHelper` body, chyba że przekazujesz w parametrze false wskazujący używaj buforowanego zestawu wyników.
