---
title: Migracja z programu ASP.NET MVC do platformy ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak rozpocząć pracę, migracji projektu MVC programu ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a9e2b41b933ed04a23515564892ed1694a4ac4f8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899728"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migracja z programu ASP.NET MVC do platformy ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), i [Scott Addie](https://scottaddie.com)

W tym artykule pokazano, jak rozpocząć pracę w projekcie ASP.NET MVC do migrowania [ASP.NET Core MVC](../mvc/overview.md). W procesie jego wyróżnia wiele rzeczy, które zostały zmienione od platformy ASP.NET MVC. Migracja z programu ASP.NET MVC jest wiele krok procesu, a w tym artykule opisano początkowej konfiguracji, podstawowe kontrolery i widoki, zawartość statyczną i zależności po stronie klienta. Dodatkowe artykuły obejmują Migrowanie konfiguracji i kodu tożsamości w wielu projektach ASP.NET MVC.

> [!NOTE]
> Numery wersji w przykładach, mogą być nieaktualne. Konieczne może być odpowiednio zaktualizować swoje projekty.

## <a name="create-the-starter-aspnet-mvc-project"></a>Tworzenie modułu uruchamiającego projekt składnika ASP.NET MVC

Aby zademonstrować uaktualnienia, Zaczniemy od utworzenia aplikacji ASP.NET MVC. Utwórz go o nazwie *WebApp1* tak pasujących przestrzeni nazw projektu ASP.NET Core, utworzymy w następnym kroku.

![Visual Studio okna dialogowego Nowy projekt](mvc/_static/new-project.png)

![Okno dialogowe Nowy aplikacji sieci Web: Szablon projektu MVC wybranego w panelu szablony platformy ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcjonalnie:* Zmiana nazwy rozwiązania z *WebApp1* do *Mvc5*. Visual Studio Wyświetla nową nazwę rozwiązania (*Mvc5*), co pozwala łatwiej mówić tego projektu z projektu dalej.

## <a name="create-the-aspnet-core-project"></a>Tworzenie projektu platformy ASP.NET Core

Utwórz nową *pusty* aplikacji internetowej ASP.NET Core z taką samą nazwę jak poprzedni projekt (*WebApp1*), dzięki czemu odpowiada przestrzeni nazw w dwóch projektów. O tej samej przestrzeni nazw sprawia, że jest ona łatwiej będzie skopiować kod między dwa projekty. Będziesz mieć do tworzenia tego projektu w innym katalogu niż poprzednie projektu, aby użyć tej samej nazwie.

![Okno dialogowe nowego projektu](mvc/_static/new_core.png)

![Okno dialogowe Nowy aplikacji sieci Web platformy ASP.NET: Pusty szablon projektu służący zaznaczonych w panelu szablony programu ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcjonalnie:* Tworzenie nowej aplikacji platformy ASP.NET Core, za pomocą *aplikacji sieci Web* szablonu projektu. Nadaj projektowi nazwę *WebApp1*i wybierz opcję uwierzytelniania programu **indywidualne konta użytkowników**. Zmień nazwę tej aplikacji do *FullAspNetCore*. Tworzenie projektu oszczędza czas do konwersji. Można sprawdzić kod wygenerowany szablon, aby zobaczyć efekt lub skopiować kod do projektu konwersji. Jest również przydatne, gdy użytkownik zatrzymywane w kroku konwersji do porównania z wygenerowanych przez szablon projektu.

## <a name="configure-the-site-to-use-mvc"></a>Konfigurowanie lokacji w celu korzystania z aplikacji MVC

::: moniker range=">= aspnetcore-2.1"

* Podczas określania wartości docelowej platformy .NET Core [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) odwołuje się do domyślnego. Ten pakiet zawiera pakiety pakietów najczęściej używanych przez aplikacje platformy MVC. Jeśli przeznaczony dla .NET Framework, odwołania do pakietu musi być indywidualnie wymieniony w pliku projektu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Podczas określania wartości docelowej platformy .NET Core [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) odwołuje się do domyślnego. Ten pakiet zawiera pakiety pakietów najczęściej używanych przez aplikacje platformy MVC. Jeśli przeznaczony dla .NET Framework, odwołania do pakietu musi być indywidualnie wymieniony w pliku projektu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Po przeznaczonych dla platformy .NET Core lub .NET Framework, pakiety pakietów najczęściej używanych przez aplikacje platformy MVC są wyświetlane indywidualnie w pliku projektu.

::: moniker-end

`Microsoft.AspNetCore.Mvc` to platforma ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles` jest programem obsługi plików statycznych. Środowisko uruchomieniowe programu ASP.NET Core jest moduły i musi jawnie zgody na uczestnictwo w Obsługa plików statycznych (zobacz [pliki statyczne](xref:fundamentals/static-files)).

* Otwórz *Startup.cs* pliku i zmień kod, zgodnie z poniższym:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles` — Metoda rozszerzenia dodaje procedurę obsługi plików statycznych. Jak wspomniano wcześniej, środowisko uruchomieniowe ASP.NET to moduły, a musi jawnie zgody na uczestnictwo w Obsługa plików statycznych. `UseMvc` — Metoda rozszerzenia dodaje routingu. Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup) i [Routing](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Dodaj kontroler i widok

W tej sekcji dodasz minimalny kontroler i widok, aby służyć jako symbole zastępcze dla kontrolera ASP.NET MVC i widoki, że będzie migrowane w ramach następnej sekcji.

* Dodaj *kontrolerów* folderu.

* Dodaj **klasy kontrolera** o nazwie *HomeController.cs* do *kontrolerów* folderu.

![Dodaj okno dialogowe Nowy element](mvc/_static/add_mvc_ctl.png)

* Dodaj *widoków* folderu.

* Dodaj *widoków domowych* folderu.

* Dodaj **widoku Razor** o nazwie *Index.cshtml* do *widoków domowych* folderu.

![Dodaj okno dialogowe Nowy element](mvc/_static/view.png)

Struktura projektu jest pokazany poniżej:

![Eksplorator rozwiązań pliki i foldery WebApp1](mvc/_static/project-structure-controller-view.png)

Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:

```html
<h1>Hello world!</h1>
```

Uruchom aplikację.

![Aplikacja sieci Web otwórz w programie Microsoft Edge](mvc/_static/hello-world.png)

Zobacz [kontrolerów](xref:mvc/controllers/actions) i [widoków](xref:mvc/views/overview) Aby uzyskać więcej informacji.

Teraz, gdy minimalny projektu ASP.NET Core pracy, możemy rozpocząć migracji funkcji z projektu platformy ASP.NET MVC. Trzeba przenieść następujące czynności:

* zawartość po stronie klienta (CSS, czcionki i skryptów)

* kontrolery

* widoki

* modele

* Tworzenie pakietów

* filtry

* Zaloguj się we/wy tożsamości (jest to wykonywane w następnym samouczku.)

## <a name="controllers-and-views"></a>Kontrolery i widoki

* Skopiować każdą z metod z platformy ASP.NET MVC `HomeController` do nowego `HomeController`. Należy pamiętać, że we wzorcu ASP.NET MVC, typ zwracany metody akcji kontrolera wbudowany szablon [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); we wzorcu ASP.NET Core MVC, zwrotu metody akcji `IActionResult` zamiast tego. `ActionResult` implementuje `IActionResult`, więc nie trzeba zmieniać zwracany typ metody akcji.

* Kopiuj *About.cshtml*, *Contact.cshtml*, i *Index.cshtml* pliki widoku Razor z projektu platformy ASP.NET MVC do projektu programu ASP.NET Core.

* Uruchom aplikację ASP.NET Core, a każda metoda testowa. Firma Microsoft nie jeszcze plik układu lub style jeszcze zmigrowane, więc renderowanych widoków zawierać tylko zawartości w plikach widoku. Nie będziesz mieć linków do plików, wygenerowany układu dla `About` i `Contact` widoków, dzięki czemu będziesz mieć do wywołania, za pomocą przeglądarki (Zastąp **4492** przy użyciu numeru portu używanego w projekcie).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Skontaktuj się z pomocą strony](mvc/_static/contact-page.png)

Zwróć uwagę na Brak elementów menu i stylów. Naprawimy, w następnej sekcji.

## <a name="static-content"></a>Zawartość statyczna

W poprzednich wersjach programu ASP.NET MVC zawartość statyczna była hostowana w katalogu głównym projektu sieci web i został zmieszać z plikami po stronie serwera. W programie ASP.NET Core zawartości statycznej znajduje się w *wwwroot* folderu. Należy skopiować zawartość statyczną z starych aplikacji ASP.NET MVC w celu *wwwroot* folder w projekcie platformy ASP.NET Core. W tym konwersji próbki:

* Kopiuj *favicon.ico* plików ze starego projektu MVC do *wwwroot* folderu w projekcie platformy ASP.NET Core.

Stary platformy ASP.NET MVC projekt używa [Bootstrap](https://getbootstrap.com/) dla jego stylu i magazyny plików ładowania początkowego *zawartości* i *skrypty* folderów. Szablon, który jest generowany starego projektu ASP.NET MVC, odwołuje się do ładowania początkowego w pliku układu (*Views/Shared/_Layout.cshtml*). Można skopiować *bootstrap.js* i *bootstrap.css* projekt plików z platformy ASP.NET MVC *wwwroot* folderu w nowym projekcie. Zamiast tego dodamy obsługę ładowania początkowego (i inne biblioteki po stronie klienta), przy użyciu usługi CDN w następnej sekcji.

## <a name="migrate-the-layout-file"></a>Migrowanie pliku układu

* Kopia *_ViewStart.cshtml* plików ze starego projektu ASP.NET MVC *widoków* folderu do projektu programu ASP.NET Core *widoków* folderu. *_ViewStart.cshtml* plik nie został zmieniony na platformie ASP.NET Core MVC.

* Tworzenie *widoków/Shared* folderu.

* *Opcjonalnie:* Kopiuj *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *widoków* folderu do projektu programu ASP.NET Core *widoków* folder. Usuń wszelkie deklarację przestrzeni nazw w *_ViewImports.cshtml* pliku. *_ViewImports.cshtml* plik zawiera przestrzenie nazw dla wszystkich plików w widoku i wiąże [pomocników tagów](xref:mvc/views/tag-helpers/intro). Pomocnicy tagów są używane w nowym pliku układu. *_ViewImports.cshtml* nowego pliku, dla platformy ASP.NET Core.

* Kopia *_Layout.cshtml* plików ze starego projektu ASP.NET MVC *widoków/Shared* folderu do projektu programu ASP.NET Core *widoków/Shared* folderu.

Otwórz *_Layout.cshtml* plik i dokonaj następujących zmian (kompletny kod znajduje się poniżej):

* Zastąp `@Styles.Render("~/Content/css")` z `<link>` elementu, aby załadować *bootstrap.css* (patrz poniżej).

* Usuń `@Scripts.Render("~/bundles/modernizr")`.

* Komentarz `@Html.Partial("_LoginPartial")` wiersza (Otocz wiersz z `@*...*@`). Aby uzyskać więcej informacji, zobacz [migracji uwierzytelnianie i tożsamość do platformy ASP.NET Core](xref:migration/identity)

* Zastąp `@Scripts.Render("~/bundles/jquery")` z `<script>` — element (patrz poniżej).

* Zastąp `@Scripts.Render("~/bundles/bootstrap")` z `<script>` — element (patrz poniżej).

Znaczniki zastąpienia dla CSS szablonu Bootstrap dołączania:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Znaczniki zastąpienia dla jQuery i włączenia Bootstrap JavaScript:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Zaktualizowany interfejs *_Layout.cshtml* plików znajdują się poniżej:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Wyświetl witryny w przeglądarce. Teraz powinien on załadowany poprawnie, z oczekiwanym style w miejscu.

* *Opcjonalnie:* Można spróbować użyć nowego pliku układu. Dla tego projektu można skopiować plik układu z *FullAspNetCore* projektu. Nowy plik układu używa [pomocników tagów](xref:mvc/views/tag-helpers/intro) i ma inne ulepszenia.

## <a name="configure-bundling-and-minification"></a>Skonfiguruj tworzenie pakietów i minimalizowanie

Aby uzyskać informacje o sposobie konfigurowania tworzenie pakietów i minimalizowanie, zobacz [tworzenie pakietów i minimalizowanie](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Rozwiązuje błędy HTTP 500

Istnieje wiele problemów, które mogą spowodować, że komunikat o błędzie HTTP 500, które nie zawierają żadnych informacji do źródła problemu. Na przykład jeśli *Views/_ViewImports.cshtml* plik zawiera obszar nazw, który nie istnieje w projekcie, otrzymasz błąd HTTP 500. Domyślnie w aplikacji platformy ASP.NET Core `UseDeveloperExceptionPage` rozszerzenia jest dodawany do `IApplicationBuilder` i wykonywany, gdy konfiguracja jest *rozwoju*. Jest to szczegółowo w poniższym kodzie:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

Platforma ASP.NET Core konwertuje nieobsługiwanych wyjątków w aplikacji sieci web w odpowiedzi na błędy HTTP 500. Zwykle szczegóły błędu nie są uwzględniane w tych odpowiedzi, aby zapobiec ujawnieniu potencjalnie poufnych informacji o serwerze. Zobacz **za pomocą strony wyjątek dla deweloperów** w [obsługi błędów](../fundamentals/error-handling.md) Aby uzyskać więcej informacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
