---
title: "Za pomocą rozwiązania Bower w platformy ASP.NET Core"
author: rick-anderson
description: "Zarządzanie pakietami po stronie klienta z Bower."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 67695843846cfaf1619db11a7bffcc65802e0f69
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Zarządzaj pakietami po stronie klienta z Bower w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), i [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Jednocześnie jest Bower, jego maintainers zaleca się użycie innego rozwiązania. Yarn z Webpack jest jeden popularną alternatywę dla którego [instrukcje migracji](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) są dostępne.

[Bower](https://bower.io/) wywołuje sam siebie "Menedżer pakietów dla sieci web". W ekosystemie .NET umieszcza void pozostawionego przez brakiem NuGet do dostarczania zawartości plików statycznych. Dla projektów platformy ASP.NET Core, te pliki statyczne są wbudowane w bibliotekach po stronie klienta, takich jak [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/). W przypadku bibliotek .NET, możesz nadal używać [NuGet](https://www.nuget.org/) Menedżera pakietów.

Proces kompilacji nowych projektów utworzonych za pomocą szablonów projektu platformy ASP.NET Core — konfiguracja po stronie klienta. [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/) są zainstalowane i Bower jest obsługiwany.

Pakiety po stronie klienta są wyświetlane w *bower.json* pliku. Szablony projektów platformy ASP.NET Core konfiguruje *bower.json* jQuery, weryfikacji jQuery i ładowania początkowego.

W tym samouczku dodamy obsługę [czcionki świetny](http://fontawesome.io). Można je zainstalować pakiety bower **Zarządzaj pakietami Bower** interfejsu użytkownika lub ręcznie w *bower.json* pliku.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalacja za pomocą pakietów Bower Zarządzaj interfejsu użytkownika

* Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu. Wybierz **aplikacji sieci Web** i **bez uwierzytelniania**.

* Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzaj pakietami Bower** (również w menu głównym **projektu** > **Zarządzaj pakietami Bower**).

* W **Bower: \<Nazwa projektu\>**  , kliknij kartę "Przeglądaj", a następnie przeprowadź filtrowanie listy pakietów, wprowadzając `font-awesome` w polu wyszukiwania:

 ![Zarządzaj pakietami bower](bower/_static/manage-bower-packages.png)

* Upewnij się, że "zapisać zmiany w *bower.json*" jest zaznaczone pole wyboru. Wybierz z listy rozwijanej wersję, a następnie kliknij przycisk **zainstalować** przycisku. **Dane wyjściowe** okno zawiera szczegółowe informacje dotyczące instalacji.

### <a name="manual-installation-in-bowerjson"></a>Ręczna instalacja w pliku bower.json

Otwórz *bower.json* pliku, a następnie dodaj "font świetny" do zależności. IntelliSense zawiera dostępnych pakietów. Po wybraniu pakietu dostępne wersje są wyświetlane. Poniżej obrazy są starsze i nie będzie zgodne, zostanie wyświetlony.

![IntelliSense bower Eksploratora pakietów](bower/_static/add-package.png)

![Wersja bower IntelliSense](bower/_static/version-intelliSense.png)

Bower używa [wersjonowania semantycznego](http://semver.org/) do organizowania zależności. Wersjonowania semantycznego, znanej także jako programu SemVer identyfikuje pakiety ze schematu numerowania \<głównych >.\< drobne >. \<poprawki >. Przedstawiający kilka typowe opcje IntelliSense upraszcza wersjonowania semantycznego. Pierwszy element na liście IntelliSense (4.6.3 w powyższym przykładzie) jest uznawany za najnowsza stabilna wersja pakietu. Symbol daszek (^) najnowszą wersją główną i tyldy (~) najnowszą wersją pomocniczą.

Zapisz *bower.json* pliku. Visual Studio Obserwujący *bower.json* zmiany w pliku. Przy zapisywaniu *instalacji bower* polecenie jest wykonywane. Zobacz okno dane wyjściowe **Bower/npm** widok pełne polecenie wykonane.

Otwórz *.bowerrc* plików w obszarze *bower.json*. `directory` Właściwość jest ustawiona na *wwwroot/lib* który wskazuje lokalizację Bower zainstaluje zasoby pakietu.

```json
{
 "directory": "wwwroot/lib"
}
```

Pole wyszukiwania w Eksploratorze rozwiązań umożliwia znaleźć i wyświetlić świetny czcionki pakietu.

Otwórz *Views\Shared\_Layout.cshtml* plik i dodać świetny czcionki pliku CSS do środowiska [pomocnika tagów](xref:mvc/views/tag-helpers/intro) dla `Development`. W Eksploratorze rozwiązań, przeciągnij i upuść *awesome.css czcionki* wewnątrz `<environment names="Development">` elementu.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

W aplikacji produkcyjnej należy dodać *awesome.min.css czcionki* do pomocniczego znacznika środowiska dla `Staging,Production`.

Zastąp zawartość *Views\Home\About.cshtml* pliku Razor z następujący kod:

[!code-html[](bower/sample/About.cshtml)]

Uruchom aplikację i przejdź do widoku informacje weryfikowanie działania pakietu świetny czcionki.

## <a name="exploring-the-client-side-build-process"></a>Eksploracja proces kompilacji po stronie klienta

Większość szablonów projektu platformy ASP.NET Core są już skonfigurowane do używania Bower. Ten przewodnik dalej rozpoczyna się od pustego projektu platformy ASP.NET Core i dodaje każdego z nich ręcznie, dzięki czemu możesz uzyskać pewne pojęcie dotyczące sposobu używania Bower w projekcie. Widać, co się dzieje z struktury projektu i środowiska uruchomieniowego output, ponieważ każda zmiana konfiguracji.

Ogólne kroki procesu kompilacji po stronie klienta za pomocą rozwiązania Bower są:

* Definiowanie pakietów używany w projekcie. <!-- once defined, you don't need to download them, VS does -->
* Odwołanie pakiety ze stron sieci web.

### <a name="define-packages"></a>Definiowanie pakietów

Po listy pakietów *bower.json* pliku, Visual Studio będzie je pobrać. W poniższym przykładzie użyto Bower załadować jQuery i ładowania początkowego do *wwwroot* folderu.

* Utwórz nową aplikację sieci Web platformy ASP.NET Core z **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu. Wybierz **pusty** szablonu projektu i kliknij przycisk **OK**.

* W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy Projekt > **Dodaj nowy element** i wybierz **pliku konfiguracyjnego Bower**. Uwaga: A *.bowerrc* również zostanie dodany plik.

* Otwórz *bower.json*, Dodaj jquery i bootstrap do `dependencies` sekcji. Powstałe w ten sposób *bower.json* pliku będzie wyglądać jak w następującym przykładzie. Wersje zmieni się wraz z upływem czasu i może nie odpowiadać na poniższym obrazie.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Zapisz *bower.json* pliku.

 Sprawdź projekt zawiera *bootstrap* i *jQuery* katalogów w *wwwroot/lib*. Bower używa *.bowerrc* plik, aby zainstalować zasoby w *wwwroot/lib*.

 Uwaga: Interfejsu użytkownika "Zarządzaj pakietami Bower" stanowi alternatywę do edycji plik ręcznie.

### <a name="enable-static-files"></a>Włącz pliki statyczne

* Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu.
* Włącz pliki statyczne z [oprogramowanie pośredniczące plików statycznych](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Dodaj wywołanie do [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) do `Configure` metody `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Pakietów odniesienia

W tej sekcji utworzysz stronę HTML, aby sprawdzić, czy można uzyskać dostępu do wdrożone pakiety.

* Dodaj nową stronę HTML o nazwie *Index.html* do *wwwroot* folderu. Uwaga: Należy dodać do pliku w formacie HTML *wwwroot* folderu. Domyślnie funkcja zawartość statyczna nie może zostać wyświetlona poza *wwwroot*. Zobacz [Praca z pliki statyczne](xref:fundamentals/static-files) Aby uzyskać więcej informacji.

 Zastąp zawartość *Index.html* z następujący kod:

[!code-html[](bower/sample/Index.html)]

* Uruchom aplikację i przejdź do `http://localhost:<port>/Index.html`. Alternatywnie z *Index.html* otwarty, naciśnij klawisz `Ctrl+Shift+W`. Sprawdź stosowanie stylów jumbotron, kodu jQuery odpowiada po kliknięciu przycisku i że Bootstrap przycisku zmienia stan.

 ![Styl jumbotron](bower/_static/jumbotron.png)
