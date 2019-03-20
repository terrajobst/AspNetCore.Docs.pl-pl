---
title: Dodawanie kontrolera do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak dodać kontroler do prostą aplikację platformy ASP.NET Core MVC.
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: dad9262e544fc216ddc694b11b0dfb88b58db9cd
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265011"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a>Dodawanie kontrolera do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Wzorzec architektury Model-View-Controller (MVC) dzieli aplikację na trzy główne składniki: **M**odelu, **V**idok, i **C**ontroller. Wzorzec MVC pomaga w tworzeniu aplikacji, które są bardziej zakresie testować i łatwiejszy do aktualizacji niż tradycyjne aplikacje monolityczne. Aplikacje korzystające z platformy MVC zawiera:

* **M**odels: Klasy, które reprezentują dane aplikacji. Klasy modeli Użyj logikę weryfikacji, aby wymuszać reguły biznesowe do tych danych. Zazwyczaj obiekty modelu pobierania i przechowywania stanu modelu w bazie danych. W tym samouczku `Movie` model pobiera filmu dane z bazy danych, przekazuje go do widoku lub aktualizuje. Zaktualizowane dane są zapisywane do bazy danych.

* **V**iews: Widoki są składnikami aplikacji interfejsu użytkownika (UI). Ogólnie rzecz biorąc ten interfejs użytkownika Wyświetla określone dane modelu.

* **C**ontrollers: Klasy, które obsługują żądania przeglądarki. Pobierają dane z modelu i wywołać Przeglądanie szablonów, które zwracają odpowiedzi. W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji. Przykładowo kontroler obsługuje wartości trasy danymi i ciągiem zapytania i przekazuje te wartości do modelu. Model może ich użyć do wykonywania zapytań w bazie danych. Na przykład `https://localhost:1234/Home/About` ma dane trasy `Home` (kontroler) i `About` (metoda akcji do wywołania na głównym kontrolerze). `https://localhost:1234/Movies/Edit/5` to żądanie Edytuj film o identyfikatorze = 5 za pomocą kontrolera filmu. Dane trasy zostało wyjaśnione w dalszej części tego samouczka.

Wzorzec MVC pomaga w tworzeniu aplikacji, których różnych aspektów aplikacji (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika), zapewniając tym luźne powiązanie tych elementów. Wzorzec Określa, gdzie każdy rodzaj logiki powinien znajdować się w aplikacji. Logika interfejsu użytkownika, należy w widoku. Logika danych wejściowych jest powiązana z kontrolerem. Logika biznesowa jest powiązana w modelu. Ta separacja ułatwia zarządzanie złożonością podczas tworzenia aplikacji, ponieważ umożliwia pracę na jednym aspekcie implementacji w danym momencie, bez wywierania wpływu na kod innego. Na przykład można pracować nad kodem widoku bez zależności od kodu logiki biznesowej.

Firma Microsoft obejmuje te pojęcia w tej serii samouczków i pokazują, jak ich używać do tworzenia aplikacji filmu. Projektu MVC zawiera foldery dla *kontrolerów* i *widoków*.

## <a name="add-a-controller"></a>Dodawanie kontrolera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów > Dodaj > kontrolera**
  ![menu kontekstowe](adding-controller/_static/add_controller.png)

* W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler MVC — pusty**

  ![Dodaj kontroler MVC i nadaj mu nazwę](adding-controller/_static/ac.png)

* W **okna dialogowego Dodaj pusty kontroler MVC**, wprowadź **HelloWorldController** i wybierz **Dodaj**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Wybierz **EXPLORER** ikonę i następnie kombinacji control + kliknięcie (kliknij prawym przyciskiem myszy) **kontrolerów > Nowy plik** i nadaj nowemu plikowi *HelloWorldController.cs*.

  ![Menu kontekstowe](~/tutorials/first-mvc-app-xplat/adding-controller/_static/new_file.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów > Dodaj > Nowy plik**.
![Menu kontekstowe](~/tutorials/first-mvc-app-mac/adding-controller/_static/add_controller.png)

Wybierz **platformy ASP.NET Core** i **Klasa kontrolera MVC**.

Nazwa kontrolera **HelloWorldController**.

![Dodaj kontroler MVC i nadaj mu nazwę](~/tutorials/first-mvc-app-mac/adding-controller/_static/ac.png)

---

Zastąp zawartość *Controllers/HelloWorldController.cs* następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Każdy `public` metody w kontrolerze jest wywoływany jako punktu końcowego HTTP. W powyższym przykładzie obu tych metod zwraca ciąg. Należy pamiętać, komentarze poprzedzających każdej metody.

Punkt końcowy HTTP jest targetable adres URL aplikacji sieci web, takich jak `https://localhost:5001/HelloWorld`i łączy protokół używany: `HTTPS`, lokalizacji sieciowej serwera sieci web (w tym z portem TCP): `localhost:5001` i docelowy identyfikator URI `HelloWorld`.

Pierwszy komentarz stany to [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) metodę, która jest wywoływana przez dołączenie `/HelloWorld/` do podstawowego adresu URL. Określa drugi komentarz [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) metodę, która jest wywoływana przez dołączenie `/HelloWorld/Welcome/` do adresu URL. Później w samouczku silnika tworzenia szkieletów służy do generowania `HTTP POST` metod, które aktualizacji danych.

Uruchom aplikację w trybie bez debugowania i Dołącz "nazwę HelloWorld" w ścieżce w pasku adresu. `Index` Metoda zwraca ciąg.

![Okno przeglądarki, wyświetlanie odpowiedzi aplikacji, to jest Moja Akcja domyślna](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC wywołuje klasy kontrolera (i metod akcji w nich), w zależności od przychodzącego adresu URL. Wartość domyślna [logikę routingu adresów URL](xref:mvc/controllers/routing) używany przez MVC używa formatu to w celu określenia, jakie kodu do wywołania:

`/[Controller]/[ActionName]/[Parameters]`

Ustawiono formatu routingu `Configure` method in Class metoda *Startup.cs* pliku.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

<!-- 
Add link to explain lambda.
Remove link for simplified tutorial.
-->

Podczas przechodzenia do aplikacji, a nie podasz żadnych segmentów adresu URL, jego wartość domyślna to kontroler "Home", a metoda "Index" określona w wiersza szablonu wyróżnione powyżej.

Pierwszy segment adresu URL określa klasę kontrolera do uruchomienia. Dlatego `localhost:xxxx/HelloWorld` mapuje `HelloWorldController` klasy. Druga część segment adresu URL określa metody akcji w klasie. Dlatego `localhost:xxxx/HelloWorld/Index` spowodowałoby `Index` metody `HelloWorldController` klasy do uruchomienia. Należy zauważyć, że masz aby przejść do `localhost:xxxx/HelloWorld` i `Index` wywołano metodę domyślnie. Jest to spowodowane `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nazwa metody nie jest jawnie określona. Trzecia część segment adresu URL ( `id`) dla danych trasy. Dane trasy zostało wyjaśnione w dalszej części tego samouczka.

Przejdź do `https://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda uruchamia i zwraca ciąg `This is the Welcome action method...`. Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji. Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.

![Okno przeglądarki, wyświetlanie odpowiedzi aplikacji, to jest metoda akcji-Zapraszamy!](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Zmodyfikuj kod, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera. Na przykład `/HelloWorld/Welcome?name=Rick&numtimes=4`. Zmiana `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano w poniższym kodzie.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Powyższy kod:

* Używa funkcji opcjonalny parametr języka C# w celu wskazania, że `numTimes` parametru wartość domyślna to 1, jeśli nie przekazano żadnej wartości tego parametru. <!-- remove for simplified -->
* Używa`HtmlEncoder.Default.Encode` chronić aplikację przed złośliwe dane wejściowe (to znaczy JavaScript).
* Używa [ciągi interpolowane](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings) w `$"Hello {name}, NumTimes is: {numTimes}"`. <!-- remove for simplified -->

Uruchom aplikację i przejdź do:

   `https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Zamiast xxxx numeru portu). Możesz spróbować różne wartości `name` i `numtimes` w adresie URL. MVC [wiązanie modelu](xref:mvc/models/model-binding) system automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie. Zobacz [powiązań modelu](xref:mvc/models/model-binding) Aby uzyskać więcej informacji.

![Wyświetlanie odpowiedzi aplikacji Hello Rick jest NumTimes okna przeglądarki: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na ilustracji powyżej segment adresu URL (`Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](https://wikipedia.org/wiki/Query_string). `?` (Znak zapytania) w powyższym adresie URL jest separatorem, a następnie wykonaj ciągi zapytań. `&` Rozdziela ciągi zapytań.

Zastąp `Welcome` metoda następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Uruchom aplikację, a następnie wprowadź następujący adres URL: `https://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

Tym razem trzeci segment adresu URL dopasowane parametru trasy `id`. `Welcome` Metody zawiera parametr `id` pasujących szablon adresu URL w `MapRoute` metody. Końcowe `?` (w `id?`) wskazuje `id` parametr jest opcjonalny.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

W tych przykładach kontrolera została wykonując "VC" część MVC — widok i kontroler pracy. Kontroler zwraca HTML bezpośrednio. Ogólnie nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo skomplikowane, kodu i obsługa. Zamiast tego używa się zazwyczaj oddzielny plik szablonu widoku Razor ułatwiający Generowanie odpowiedzi HTML. Można to zrobić w następnym samouczku.

> [!div class="step-by-step"]
> [Poprzednie](start-mvc.md)
> [dalej](adding-view.md)
