---
title: Dodawanie widoku do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie widoku do prostej aplikacji ASP.NET Core MVC
ms.author: riande
ms.date: 8/04/2019
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 5510fb6844452571ca764e21640f0bd16444c782
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660210"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>Dodawanie widoku do aplikacji ASP.NET Core MVC

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

W tej sekcji zmodyfikujesz klasę `HelloWorldController`, aby używać plików widoku [Razor](xref:mvc/views/razor) do czystego hermetyzacji procesu generowania odpowiedzi HTML na klienta.

Tworzysz plik szablonu widoku przy użyciu Razor. Szablony widoku oparte na Razor mają rozszerzenie *. cshtml* . Zapewniają elegancki sposób tworzenia danych wyjściowych HTML za pomocą C#.

Obecnie Metoda `Index` zwraca ciąg z komunikatem, który jest zakodowany w klasie Controller. W klasie `HelloWorldController` Zastąp metodę `Index` następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Poprzedni kod wywołuje metodę <xref:Microsoft.AspNetCore.Mvc.Controller.View*> kontrolera. Używa szablonu widoku do wygenerowania odpowiedzi HTML. Metody kontrolera (znane również jako *metody akcji*), takie jak powyższa metoda `Index`, zazwyczaj zwracają <xref:Microsoft.AspNetCore.Mvc.IActionResult> (lub klasę pochodną <xref:Microsoft.AspNetCore.Mvc.ActionResult>), a nie typ podobny do `string`.

## <a name="add-a-view"></a>Dodawanie widoku

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy folder *widoki* , a następnie **Dodaj > nowy folder** i nadaj mu nazwę folder *HelloWorld*.

* Kliknij prawym przyciskiem myszy folder *widoki/HelloWorld* , a następnie **Dodaj > nowy element**.

* W oknie dialogowym **Dodaj nowy element — MvcMovie**

  * W polu wyszukiwania w prawym górnym rogu wprowadź *Widok*

  * Wybieranie **widoku Razor**

  * Zachowaj wartość pola **Nazwa** , *index. cshtml*.

  * Wybierz pozycję **Dodaj**.

![Okno dialogowe Dodawanie nowego elementu](adding-view/_static/add_view.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Dodaj widok `Index` dla `HelloWorldController`.

* Dodaj nowy folder o nazwie *viewss/HelloWorld*.
* Dodaj nowy plik do pliku *viewss/HelloWorld* Name *index. cshtml*.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy folder *widoki* , a następnie **Dodaj > nowy folder** i nadaj mu nazwę folder *HelloWorld*.
* Kliknij prawym przyciskiem myszy folder *widoki/HelloWorld* , a następnie **Dodaj > nowy plik**.
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **ASP .NET Core** .
  * Wybierz **stronę widok MVC** w środkowym okienku.
  * Wpisz *indeks* w polu **Nazwa** .
  * Wybierz pozycję **Nowy**.

![Okno dialogowe Dodawanie nowego elementu](adding-view/_static/add_view_mac.png)

---

Zastąp zawartość pliku widoku Razor *widoków/HelloWorld/index. cshtml* następującym:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

Przejdź do adresu `https://localhost:{PORT}/HelloWorld`. Metoda `Index` w `HelloWorldController` nie zrobiła wiele; uruchomiono instrukcję `return View();`, która określa, że metoda powinna używać pliku szablonu widoku, aby renderować odpowiedź do przeglądarki. Ponieważ nazwa pliku szablonu widoku nie została określona, MVC domyślnie używa domyślnego pliku widoku. Domyślny plik widoku ma taką samą nazwę jak Metoda (`Index`), więc jest używana */views/HelloWorld/index.cshtml* . Na poniższej ilustracji przedstawiono ciąg "Hello z naszego szablonu widoku!" zakodowane w widoku.

![Okno przeglądarki](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Zmień widoki i strony układu

Wybierz linki menu (**MvcMovie**, **Home**i **privacy**). Każda Strona wyświetla ten sam układ menu. Układ menu jest implementowany w pliku *views/Shared/_Layout. cshtml* . Otwórz plik *views/Shared/_Layout. cshtml* .

Szablony [układów](xref:mvc/views/layout) umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie. Znajdź wiersz `@RenderBody()`. `RenderBody` jest symbolem zastępczym, w którym wszystkie utworzone strony specyficzne dla widoku są *widoczne na stronie* układ. Na przykład po wybraniu linku **prywatności** widok **widoki/główna/prywatność. cshtml** jest renderowany wewnątrz metody `RenderBody`.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Zmień tytuł, stopkę i łącze menu w pliku układu

Zastąp zawartość pliku *viewss/Shared/_Layout. cshtml* następującym znacznikiem. Zmiany są wyróżnione:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Views/Shared/_Layout.cshtml?highlight=6,14,40)]

Poprzednia Adiustacja wprowadziła następujące zmiany:

* 3 wystąpienia `MvcMovie` do `Movie App`.
* Element zakotwiczenia `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>`, aby `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.

W powyższym znaczniku [atrybut pomocnika `asp-area=""` tagu zakotwiczonego](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) został pominięty, ponieważ ta aplikacja nie korzysta z [obszarów](xref:mvc/controllers/areas).

**Uwaga**: kontroler `Movies` nie został zaimplementowany. W tym momencie łącze `Movie App` nie działa.

Zapisz zmiany i wybierz łącze **prywatność** . Zwróć uwagę, jak tytuł na karcie Przeglądarka wyświetla **zasady zachowania poufności informacji — aplikacja dla filmów** zamiast **zasad ochrony prywatności — film MVC**:

![Karta prywatność](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Wybierz link **domowy** i zwróć uwagę na to, że tytuł i tekst zakotwiczenia również wyświetlają **aplikację Movie**. Udało nam się wprowadzić zmiany w szablonie układu i wszystkie strony w witrynie będą odzwierciedlały nowy tekst linku i nowy tytuł.

Przejrzyj plik *views/_ViewStart. cshtml* :

```cshtml
@{
    Layout = "_Layout";
}
```

Plik *views/_ViewStart. cshtml* umieszcza w pliku *views/Shared/_Layout. cshtml* w każdym widoku. Właściwość `Layout` może służyć do ustawiania innego widoku układu lub ustawiania jej na `null`, co spowoduje, że żaden plik układu nie zostanie użyty.

Zmień tytuł i element `<h2>` w pliku widoku */HelloWorld/index. cshtml* :

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Tytuł i element `<h2>` są nieco inne, więc można zobaczyć, który bit kodu zmienia ekran.

`ViewData["Title"] = "Movie List";` w powyższym kodzie ustawia właściwość `Title` słownika `ViewData` na "Lista filmów". Właściwość `Title` jest używana w elemencie `<title>` HTML na stronie układ:

```cshtml
<title>@ViewData["Title"] - Movie App</title>
```

Zapisz zmianę i przejdź do `https://localhost:{PORT}/HelloWorld`. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i pomocnicze nagłówki zostały zmienione. (Jeśli w przeglądarce nie są widoczne zmiany, może być wyświetlana zawartość z pamięci podręcznej. Naciśnij kombinację klawiszy CTRL + F5 w przeglądarce, aby wymusić załadowanie odpowiedzi z serwera. Tytuł przeglądarki jest tworzony przy użyciu `ViewData["Title"]` ustawionych w szablonie *index. cshtml* , a dodatkowa "-Movie App" dodana w pliku układu.

Zawartość szablonu widoku *index. cshtml* jest scalana z szablonem widok *widoki/udostępnione/_Layout. cshtml* . Pojedyncza odpowiedź HTML jest wysyłana do przeglądarki. Szablony układów ułatwiają wprowadzanie zmian, które są stosowane na wszystkich stronach w aplikacji. Aby dowiedzieć się więcej, zobacz [Układ](xref:mvc/views/layout).

![Widok listy filmów](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nasz mały bit "Data" (w tym przypadku "Hello z naszego szablonu widoku!") komunikat) jest zakodowany na stałe. Aplikacja MVC ma "V" (widok) i masz "C" (kontroler), ale nie jest jeszcze "M" (model).

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Akcje kontrolera są wywoływane w odpowiedzi na żądanie przychodzącego adresu URL. Klasa kontrolera to miejsce, w którym zapisano kod obsługujący żądania przeglądarki przychodzącej. Kontroler pobiera dane ze źródła danych i decyduje o typie odpowiedzi wysyłanej z powrotem do przeglądarki. Szablony widoków mogą być używane z poziomu kontrolera do generowania i formatowania odpowiedzi HTML w przeglądarce.

Kontrolery są odpowiedzialne za dostarczanie danych wymaganych w celu renderowania odpowiedzi przez szablon widoku. Najlepsze rozwiązanie: szablony widoków **nie** powinny wykonywać logiki biznesowej ani bezpośrednio korzystać z bazy danych. Zamiast tego szablon widoku powinien współpracować tylko z danymi, które są udostępniane przez kontroler. Utrzymywanie tego "separacji zagadnień" pomaga zachować kod, weryfikowalne i łatwość utrzymania.

Obecnie Metoda `Welcome` w klasie `HelloWorldController` przyjmuje `name` i `ID` parametr, a następnie wyprowadza wartości bezpośrednio do przeglądarki. Zamiast przetworzyć tę odpowiedź jako ciąg, należy zmienić kontroler tak, aby używał szablonu widoku. Szablon widoku generuje odpowiedź dynamiczną, co oznacza, że odpowiednie bity danych muszą zostać przesłane z kontrolera do widoku w celu wygenerowania odpowiedzi. W tym celu należy mieć kontroler umieszczający dane dynamiczne (parametry) wymagane przez szablon widoku w słowniku `ViewData`, do którego ma dostęp szablon widoku.

W *HelloWorldController.cs*zmień metodę `Welcome`, aby dodać `Message` i `NumTimes` wartość do słownika `ViewData`. Słownik `ViewData` jest obiektem dynamicznym, co oznacza, że można użyć dowolnego typu; Obiekt `ViewData` nie ma zdefiniowanych właściwości, dopóki nie umieścisz w nim elementu. [System powiązania modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania na pasku adresu do parametrów w metodzie. Pełny plik *HelloWorldController.cs* wygląda następująco:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

Obiekt słownika `ViewData` zawiera dane, które zostaną przesłane do widoku.

Utwórz szablon widoku powitalnego o nazwie *przeglądający/HelloWorld/Welcome. cshtml*.

Utworzysz pętlę w szablonie widoku *Welcome. cshtml* , który wyświetla "Hello" `NumTimes`. Zastąp zawartość *widoków/HelloWorld/Welcome. cshtml* następującym:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Zapisz zmiany i przejdź do następującego adresu URL:

`https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

Dane są pobierane z adresu URL i przesyłane do kontrolera przy użyciu [spinacza modelu MVC](xref:mvc/models/model-binding) . Kontroler pakuje dane do słownika `ViewData` i przekazuje ten obiekt do widoku. Widok następnie renderuje dane jako HTML do przeglądarki.

![Widok prywatności pokazujący etykietę powitalną i frazę Hello Rick pokazywane cztery razy](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

W powyższym przykładzie słownik `ViewData` był używany do przekazywania danych z kontrolera do widoku. W dalszej części tego samouczka model widoku służy do przekazywania danych z kontrolera do widoku. Podejście model widoku do przekazywania danych jest ogólnie preferowane przez podejście `ViewData` słownika. Aby uzyskać więcej informacji [, zobacz Kiedy używać ViewBag, ViewData lub TempData](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) .

W następnym samouczku zostanie utworzona baza danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-controller.md)
> [dalej](adding-model.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tej sekcji zmodyfikujesz klasę `HelloWorldController`, aby używać plików widoku [Razor](xref:mvc/views/razor) do czystego hermetyzacji procesu generowania odpowiedzi HTML na klienta.

Tworzysz plik szablonu widoku przy użyciu Razor. Szablony widoku oparte na Razor mają rozszerzenie *. cshtml* . Zapewniają elegancki sposób tworzenia danych wyjściowych HTML za pomocą C#.

Obecnie Metoda `Index` zwraca ciąg z komunikatem, który jest zakodowany w klasie Controller. W klasie `HelloWorldController` Zastąp metodę `Index` następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Poprzedni kod wywołuje metodę <xref:Microsoft.AspNetCore.Mvc.Controller.View*> kontrolera. Używa szablonu widoku do wygenerowania odpowiedzi HTML. Metody kontrolera (znane również jako *metody akcji*), takie jak powyższa metoda `Index`, zazwyczaj zwracają <xref:Microsoft.AspNetCore.Mvc.IActionResult> (lub klasę pochodną <xref:Microsoft.AspNetCore.Mvc.ActionResult>), a nie typ podobny do `string`.

## <a name="add-a-view"></a>Dodawanie widoku

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy folder *widoki* , a następnie **Dodaj > nowy folder** i nadaj mu nazwę folder *HelloWorld*.

* Kliknij prawym przyciskiem myszy folder *widoki/HelloWorld* , a następnie **Dodaj > nowy element**.

* W oknie dialogowym **Dodaj nowy element — MvcMovie**

  * W polu wyszukiwania w prawym górnym rogu wprowadź *Widok*

  * Wybieranie **widoku Razor**

  * Zachowaj wartość pola **Nazwa** , *index. cshtml*.

  * Wybierz pozycję **Dodaj**.

![Okno dialogowe Dodawanie nowego elementu](adding-view/_static/add_view.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Dodaj widok `Index` dla `HelloWorldController`.

* Dodaj nowy folder o nazwie *viewss/HelloWorld*.
* Dodaj nowy plik do pliku *viewss/HelloWorld* Name *index. cshtml*.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy folder *widoki* , a następnie **Dodaj > nowy folder** i nadaj mu nazwę folder *HelloWorld*.
* Kliknij prawym przyciskiem myszy folder *widoki/HelloWorld* , a następnie **Dodaj > nowy plik**.
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **Sieć Web** .
  * W środkowym okienku wybierz pozycję **pusty plik HTML** .
  * Wpisz *index. cshtml* w polu **Nazwa** .
  * Wybierz pozycję **Nowy**.

![Okno dialogowe Dodawanie nowego elementu](adding-view/_static/add_view_mac.png)

---

Zastąp zawartość pliku widoku Razor *widoków/HelloWorld/index. cshtml* następującym:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

Przejdź do adresu `https://localhost:{PORT}/HelloWorld`. Metoda `Index` w `HelloWorldController` nie zrobiła wiele; uruchomiono instrukcję `return View();`, która określa, że metoda powinna używać pliku szablonu widoku, aby renderować odpowiedź do przeglądarki. Ponieważ nazwa pliku szablonu widoku nie została określona, MVC domyślnie używa domyślnego pliku widoku. Domyślny plik widoku ma taką samą nazwę jak Metoda (`Index`), więc jest używana */views/HelloWorld/index.cshtml* . Na poniższej ilustracji przedstawiono ciąg "Hello z naszego szablonu widoku!" zakodowane w widoku.

![Okno przeglądarki](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Zmień widoki i strony układu

Wybierz linki menu (**MvcMovie**, **Home**i **privacy**). Każda Strona wyświetla ten sam układ menu. Układ menu jest implementowany w pliku *views/Shared/_Layout. cshtml* . Otwórz plik *views/Shared/_Layout. cshtml* .

Szablony [układów](xref:mvc/views/layout) umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie. Znajdź wiersz `@RenderBody()`. `RenderBody` jest symbolem zastępczym, w którym wszystkie utworzone strony specyficzne dla widoku są *widoczne na stronie* układ. Na przykład po wybraniu linku **prywatności** widok **widoki/główna/prywatność. cshtml** jest renderowany wewnątrz metody `RenderBody`.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Zmień tytuł, stopkę i łącze menu w pliku układu

* W elementach tytuł i stopka Zmień `MvcMovie` na `Movie App`.
* Zmień `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` elementu zakotwiczenia, aby `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.

Następujące znaczniki pokazują wyróżnione zmiany:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24,51)]

W poprzednim znaczniku [atrybut pomocnika `asp-area` tagu zakotwiczenia](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) został pominięty, ponieważ ta aplikacja nie korzysta z [obszarów](xref:mvc/controllers/areas).

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

**Uwaga**: kontroler `Movies` nie został zaimplementowany. W tym momencie łącze `Movie App` nie działa.

Zapisz zmiany i wybierz łącze **prywatność** . Zwróć uwagę, jak tytuł na karcie Przeglądarka wyświetla **zasady zachowania poufności informacji — aplikacja dla filmów** zamiast **zasad ochrony prywatności — film MVC**:

![Karta prywatność](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Wybierz link **domowy** i zwróć uwagę na to, że tytuł i tekst zakotwiczenia również wyświetlają **aplikację Movie**. Udało nam się wprowadzić zmiany w szablonie układu i wszystkie strony w witrynie będą odzwierciedlały nowy tekst linku i nowy tytuł.

Przejrzyj plik *views/_ViewStart. cshtml* :

```cshtml
@{
    Layout = "_Layout";
}
```

Plik *views/_ViewStart. cshtml* umieszcza w pliku *views/Shared/_Layout. cshtml* w każdym widoku. Właściwość `Layout` może służyć do ustawiania innego widoku układu lub ustawiania jej na `null`, co spowoduje, że żaden plik układu nie zostanie użyty.

Zmień tytuł i element `<h2>` w pliku widoku */HelloWorld/index. cshtml* :

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Tytuł i element `<h2>` są nieco inne, więc można zobaczyć, który bit kodu zmienia ekran.

`ViewData["Title"] = "Movie List";` w powyższym kodzie ustawia właściwość `Title` słownika `ViewData` na "Lista filmów". Właściwość `Title` jest używana w elemencie `<title>` HTML na stronie układ:

```cshtml
<title>@ViewData["Title"] - Movie App</title>
```

Zapisz zmianę i przejdź do `https://localhost:{PORT}/HelloWorld`. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i pomocnicze nagłówki zostały zmienione. (Jeśli w przeglądarce nie są widoczne zmiany, może być wyświetlana zawartość z pamięci podręcznej. Naciśnij kombinację klawiszy CTRL + F5 w przeglądarce, aby wymusić załadowanie odpowiedzi z serwera. Tytuł przeglądarki jest tworzony przy użyciu `ViewData["Title"]` ustawionych w szablonie *index. cshtml* , a dodatkowa "-Movie App" dodana w pliku układu.

Zwróć uwagę na to, jak zawartość w szablonie widoku *index. cshtml* została scalona z szablonem *widoków/Shared/_Layout. cshtml* . do przeglądarki została WYSŁANA pojedyncza odpowiedź html. Szablony układów ułatwiają wprowadzanie zmian, które są stosowane do wszystkich stron w aplikacji. Aby dowiedzieć się więcej, zobacz [Układ](xref:mvc/views/layout).

![Widok listy filmów](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nasz mały bit "Data" (w tym przypadku "Hello z naszego szablonu widoku!") komunikat) jest zakodowany na stałe. Aplikacja MVC ma "V" (widok) i masz "C" (kontroler), ale nie jest jeszcze "M" (model).

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Akcje kontrolera są wywoływane w odpowiedzi na żądanie przychodzącego adresu URL. Klasa kontrolera to miejsce, w którym zapisano kod obsługujący żądania przeglądarki przychodzącej. Kontroler pobiera dane ze źródła danych i decyduje o typie odpowiedzi wysyłanej z powrotem do przeglądarki. Szablony widoków mogą być używane z poziomu kontrolera do generowania i formatowania odpowiedzi HTML w przeglądarce.

Kontrolery są odpowiedzialne za dostarczanie danych wymaganych w celu renderowania odpowiedzi przez szablon widoku. Najlepsze rozwiązanie: szablony widoków **nie** powinny wykonywać logiki biznesowej ani bezpośrednio korzystać z bazy danych. Zamiast tego szablon widoku powinien współpracować tylko z danymi, które są udostępniane przez kontroler. Utrzymywanie tego "separacji zagadnień" pomaga zachować kod, weryfikowalne i łatwość utrzymania.

Obecnie Metoda `Welcome` w klasie `HelloWorldController` przyjmuje `name` i `ID` parametr, a następnie wyprowadza wartości bezpośrednio do przeglądarki. Zamiast przetworzyć tę odpowiedź jako ciąg, należy zmienić kontroler tak, aby używał szablonu widoku. Szablon widoku generuje odpowiedź dynamiczną, co oznacza, że odpowiednie bity danych muszą zostać przesłane z kontrolera do widoku w celu wygenerowania odpowiedzi. W tym celu należy mieć kontroler umieszczający dane dynamiczne (parametry) wymagane przez szablon widoku w słowniku `ViewData`, do którego ma dostęp szablon widoku.

W *HelloWorldController.cs*zmień metodę `Welcome`, aby dodać `Message` i `NumTimes` wartość do słownika `ViewData`. Słownik `ViewData` jest obiektem dynamicznym, co oznacza, że można użyć dowolnego typu; Obiekt `ViewData` nie ma zdefiniowanych właściwości, dopóki nie umieścisz w nim elementu. [System powiązania modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania na pasku adresu do parametrów w metodzie. Pełny plik *HelloWorldController.cs* wygląda następująco:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

Obiekt słownika `ViewData` zawiera dane, które zostaną przesłane do widoku.

Utwórz szablon widoku powitalnego o nazwie *przeglądający/HelloWorld/Welcome. cshtml*.

Utworzysz pętlę w szablonie widoku *Welcome. cshtml* , który wyświetla "Hello" `NumTimes`. Zastąp zawartość *widoków/HelloWorld/Welcome. cshtml* następującym:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Zapisz zmiany i przejdź do następującego adresu URL:

`https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`

Dane są pobierane z adresu URL i przesyłane do kontrolera przy użyciu [spinacza modelu MVC](xref:mvc/models/model-binding) . Kontroler pakuje dane do słownika `ViewData` i przekazuje ten obiekt do widoku. Widok następnie renderuje dane jako HTML do przeglądarki.

![Widok prywatności pokazujący etykietę powitalną i frazę Hello Rick pokazywane cztery razy](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

W powyższym przykładzie słownik `ViewData` był używany do przekazywania danych z kontrolera do widoku. W dalszej części tego samouczka model widoku służy do przekazywania danych z kontrolera do widoku. Podejście model widoku do przekazywania danych jest ogólnie preferowane przez podejście `ViewData` słownika. Aby uzyskać więcej informacji [, zobacz Kiedy używać ViewBag, ViewData lub TempData](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) .

W następnym samouczku zostanie utworzona baza danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-controller.md)
> [dalej](adding-model.md)

::: moniker-end
