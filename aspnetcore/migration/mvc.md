---
title: Migrowanie z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC
author: ardalis
description: "Dowiedz się, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do platformy ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: 447b13eccf523cab81590405740bb194112b0dad
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrowanie z programu ASP.NET MVC do podstawowej platformy ASP.NET MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), i [Scott Addie](https://scottaddie.com)

W tym artykule pokazano, jak rozpocząć Migrowanie projektu programu ASP.NET MVC do [platformy ASP.NET Core MVC](../mvc/overview.md). W procesie zawiera opis wiele czynności, które zostały zmienione z platformy ASP.NET MVC. Migrowanie z programu ASP.NET MVC jest wiele procesów kroku i w tym artykule omówiono początkowej konfiguracji, podstawowe kontrolery i widoki, zawartość statyczna i zależności po stronie klienta. Dodatkowe artykuły obejmuje Migrowanie konfiguracji i kod tożsamość w wielu projektów platformy ASP.NET MVC.

> [!NOTE]
> Numery wersji w przykładach, mogą być nieaktualne. Konieczne może być odpowiednio zaktualizować swoje projekty.

## <a name="create-the-starter-aspnet-mvc-project"></a>Utwórz początkowy projektu programu ASP.NET MVC

Aby zademonstrować uaktualnienia, Zaczniemy przez tworzenie aplikacji ASP.NET MVC. Utwórz go o nazwie *WebApp1* , przestrzeń nazw będzie odpowiadała projektu platformy ASP.NET Core, utworzymy w następnym kroku.

![Visual Studio okno dialogowe Nowy projekt](mvc/_static/new-project.png)

![Okno dialogowe nowego aplikacji sieci Web: szablonu projektu MVC wybranego w panelu szablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcjonalnie:* zmienić nazwy rozwiązania z *WebApp1* do *Mvc5*. Visual Studio spowoduje wyświetlenie Nowa nazwa rozwiązania (*Mvc5*), który ułatwi mówić tego projektu z projektu dalej.

## <a name="create-the-aspnet-core-project"></a>Tworzenie projektu platformy ASP.NET Core

Utwórz nową *pusty* aplikacji sieci web platformy ASP.NET Core z taką samą nazwę jak poprzednie projektu (*WebApp1*), odpowiada przestrzeni nazw w dwóch projektów. O tej samej przestrzeni nazw ułatwia skopiować kod między dwa projekty. Musisz utworzyć ten projekt w katalogu innego niż poprzednie projektu do tej samej nazwie.

![Okno dialogowe nowego projektu](mvc/_static/new_core.png)

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET: pusty szablon projektu wybrany w panelu platformy ASP.NET Core szablonów](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcjonalnie:* Tworzenie nowej aplikacji platformy ASP.NET Core, za pomocą *aplikacji sieci Web* szablonu projektu. Nazwij projekt *WebApp1*i wybierz opcję uwierzytelniania programu **indywidualnych kont użytkowników**. Zmień nazwę tej aplikacji były *FullAspNetCore*. Tworzenie projektu będzie zaoszczędzić czas podczas konwersji. Można przyjrzeć się szablon wygenerowany kod, aby zobaczyć wynik końcowy lub skopiuj kod do konwersji projektu. Jest również przydatne, gdy zostać zablokowane w kroku konwersji do porównania z wygenerowane z szablonu projektu.

## <a name="configure-the-site-to-use-mvc"></a>Konfigurowanie lokacji do używania MVC

* Zainstaluj `Microsoft.AspNetCore.Mvc` i `Microsoft.AspNetCore.StaticFiles` pakietów NuGet.

  `Microsoft.AspNetCore.Mvc`to platforma ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles`Umożliwia to obsługę plików statycznych. Środowiska uruchomieniowego ASP.NET jest moduły i musi jawnie zgłosić się do obsługi plików statycznych (zobacz [Praca z pliki statyczne](../fundamentals/static-files.md)).

* Otwórz *.csproj* pliku (kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **Edytuj WebApp1.csproj**) i Dodaj `PrepareForPublish` docelowych:

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish` Docelowy jest wymagany dla pobierania biblioteki po stronie klienta za pomocą rozwiązania Bower. Będzie omawianiu który później.

* Otwórz *Startup.cs* plików i zmień kod zgodnie z poniższym:

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles` — Metoda rozszerzenia dodaje obsługę plików statycznych. Jak wspomniano wcześniej, środowiska uruchomieniowego ASP.NET jest moduły i musi jawnie zgłosić się do obsługi plików statycznych. `UseMvc` — Metoda rozszerzenia dodaje routingu. Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](../fundamentals/startup.md) i [Routing](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Dodawanie kontrolera i widoku

W tej sekcji możesz dodać minimalny kontrolera i widoku jako symbole zastępcze dla kontrolera ASP.NET MVC i widoki, będzie migracji w następnej sekcji.

* Dodaj *kontrolerów* folderu.

* Dodaj **klasy kontrolera MVC** o nazwie *HomeController.cs* do *kontrolerów* folderu.

![Dodaj nowy element okna dialogowego](mvc/_static/add_mvc_ctl.png)

* Dodaj *widoków* folderu.

* Dodaj *widoków domowych* folderu.

* Dodaj *Index.cshtml* MVC — Wyświetl stronę do *widoków domowych* folderu.

![Dodaj nowy element okna dialogowego](mvc/_static/view.png)

Poniżej przedstawiono struktury projektu:

![Eksploratora rozwiązań przedstawiający pliki i foldery WebApp1](mvc/_static/project-structure-controller-view.png)

Zastąp zawartość *Views/Home/Index.cshtml* pliku następującym kodem:

```html
<h1>Hello world!</h1>
```

Uruchom aplikację.

![Otwórz w programie Microsoft Edge aplikacji sieci Web](mvc/_static/hello-world.png)

Zobacz [kontrolerów](xref:mvc/controllers/actions) i [widoków](xref:mvc/views/overview) Aby uzyskać więcej informacji.

Teraz, gdy mamy minimalnego projektu platformy ASP.NET Core pracy, możemy rozpocząć Migrowanie funkcja z projektu programu ASP.NET MVC. Będzie trzeba przenieść następujące czynności:

* zawartość po stronie klienta (CSS, czcionki i skrypty)

* kontrolery

* widoki

* modele

* Tworzenie pakietów

* filtry

* Zaloguj się we/wy tożsamości (będzie to w następnym samouczku.)

## <a name="controllers-and-views"></a>Kontrolery i widoki

* Skopiuj każdej z metod z platformy ASP.NET MVC `HomeController` do nowego `HomeController`. Należy pamiętać, że w programie ASP.NET MVC, typ zwracany metody akcji kontrolera wbudowanych szablonów [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); na platformie ASP.NET MVC Core, zwracany metody akcji `IActionResult` zamiast tego. `ActionResult`implementuje `IActionResult`, więc nie trzeba zmienić zwracany typ metody akcji.

* Kopiuj *About.cshtml*, *Contact.cshtml*, i *Index.cshtml* pliki widoku Razor z projektu programu ASP.NET MVC do projektu platformy ASP.NET Core.

* Uruchamianie aplikacji platformy ASP.NET Core i testowanie każdej metody. Firma Microsoft nie migracji pliku układu i stylów jeszcze, więc renderowanych widoków będzie zawierać tylko zawartości w plikach widoku. Nie będziesz mieć łącza plik wygenerowany układu `About` i `Contact` widoków, więc musisz wywołać je za pomocą przeglądarki (Zastąp **4492** numer portu używany w projekcie).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Skontaktuj się z pomocą strony](mvc/_static/contact-page.png)

Zwróć uwagę na Brak elementów stylami i menu. Firma Microsoft będzie rozwiązać ten problem w następnej sekcji.

## <a name="static-content"></a>Zawartość statyczna

W poprzednich wersjach programu ASP.NET MVC zawartość statyczną hostowanej z katalogu głównego projektu sieci web i został zmieszać z plikami po stronie serwera. W przypadku platformy ASP.NET Core zawartość statyczną znajduje się w *wwwroot* folderu. Należy skopiować zawartość statyczną z aplikacji ASP.NET MVC starego *wwwroot* folder w projekcie platformy ASP.NET Core. W tej konwersji próbkowania:

* Kopiuj *favicon.ico* pliku starego projektu MVC *wwwroot* folderu w projekcie platformy ASP.NET Core.

ASP.NET MVC stary projekt używa [Bootstrap](http://getbootstrap.com/) stylów i magazyny plików ładowania początkowego programu *zawartości* i *skryptów* folderów. Szablon, który wygenerować stary projekt platformy ASP.NET MVC, odwołuje się do ładowania początkowego w pliku układu (*Views/Shared/_Layout.cshtml*). Można skopiować *bootstrap.js* i *bootstrap.css* pliki z platformy ASP.NET MVC projektu do *wwwroot* nie korzysta z folderu w nowym projekcie, ale tego podejścia Ulepszone mechanizm zarządzania zależności po stronie klienta w ASP.NET Core.

W nowym projekcie dodamy obsługę ładowania początkowego (i innych bibliotek po stronie klienta), za pomocą [Bower](https://bower.io/):

* Dodaj [Bower](https://bower.io/) pliku konfiguracji o nazwie *bower.json* do katalogu głównego projektu (kliknij prawym przyciskiem myszy na projekt, a następnie **Dodaj > Nowy element > pliku konfiguracji Bower**). Dodaj [Bootstrap](http://getbootstrap.com/) i [jQuery](https://jquery.com/) do pliku (zobacz wyróżnione wiersze poniżej).

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

Podczas zapisywania pliku, Bower będzie automatycznie pobierał zależności, aby *wwwroot/lib* folderu. Można użyć **Eksploratora rozwiązań wyszukiwania** pole można znaleźć ścieżki zasoby:

![zasoby jquery wyświetlane w wynikach wyszukiwania w Eksploratorze rozwiązań](mvc/_static/search.png)

Zobacz [Zarządzaj pakietami po stronie klienta z Bower](../client-side/bower.md) Aby uzyskać więcej informacji.

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>Migracja pliku układu

* Kopiuj *_ViewStart.cshtml* pliku starego projektu programu ASP.NET MVC *widoków* folderu do projektu platformy ASP.NET Core *widoków* folderu. *_ViewStart.cshtml* plik nie został zmieniony na platformie ASP.NET Core MVC.

* Utwórz *widoków/Shared* folderu.

* *Opcjonalnie:* kopiowania *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *widoków* folderu do projektu platformy ASP.NET Core *Widoków* folderu. Usuń wszelkie deklaracji przestrzeni nazw w *_ViewImports.cshtml* pliku. *_ViewImports.cshtml* plików zawiera przestrzenie nazw dla wszystkich plików widoku i w przypadku [pomocników tagów](xref:mvc/views/tag-helpers/intro). Pomocników tagów są używane w nowym pliku układu. *_ViewImports.cshtml* pliku jest nowa dla platformy ASP.NET Core.

* Kopia *_Layout.cshtml* pliku starego projektu programu ASP.NET MVC *widoków/Shared* folderu do projektu platformy ASP.NET Core *widoków/Shared* folderu.

Otwórz *_Layout.cshtml* plik, a następnie dokonaj następujących zmian (kompletny kod przedstawiony poniżej):

   * Zastąp `@Styles.Render("~/Content/css")` z `<link>` elementu, aby załadować *bootstrap.css* (patrz poniżej).

   * Usuń `@Scripts.Render("~/bundles/modernizr")`.

   * Komentarz `@Html.Partial("_LoginPartial")` wiersza (przestrzenny wiersz z `@*...*@`). Wrócimy do niego w przyszłości samouczka.

   * Zastąp `@Scripts.Render("~/bundles/jquery")` z `<script>` elementu (patrz poniżej).

   * Zastąp `@Scripts.Render("~/bundles/bootstrap")` z `<script>` elementu (patrz poniżej).

Zastąpienie łączy CSS:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

Zastąpienie tagów skryptu:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

Zaktualizowany interfejs *_Layout.cshtml* pliku przedstawiono poniżej:

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Wyświetlać witrynę w przeglądarce. Teraz powinien on załadowany poprawnie, z oczekiwanym style w miejscu.

* *Opcjonalnie:* możesz chcieć użyć nowego pliku układu. Dla tego projektu można skopiować pliku układu z *FullAspNetCore* projektu. Używa nowego pliku układu [pomocników tagów](xref:mvc/views/tag-helpers/intro) i ma inne ulepszenia.

## <a name="configure-bundling-and-minification"></a>Konfigurowanie, tworzenie pakietów i minimalizowanie

Aby uzyskać informacje o sposobie konfigurowania tworzenie pakietów i minimalizowanie, zobacz [tworzenie pakietów i minimalizowanie](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>Rozwiązywanie błędów HTTP 500

Istnieje wiele problemów, które mogą spowodować, że komunikat o błędzie HTTP 500, które nie zawierają żadnych informacji o źródło problemu. Na przykład jeśli *Views/_ViewImports.cshtml* plik zawiera przestrzeni nazw, która nie istnieje w projekcie, zostanie wyświetlony komunikat o błędzie HTTP 500. Aby uzyskać szczegółowy komunikat o błędzie, Dodaj następujący kod:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Zobacz **za pomocą strony wyjątek Developer** w [obsługi błędu](../fundamentals/error-handling.md) Aby uzyskać więcej informacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Programowanie po stronie klienta](xref:client-side/index)
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
