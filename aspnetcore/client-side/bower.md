---
title: Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower w programie ASP.NET Core
author: rick-anderson
description: Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 08e6daa537c6c6f92a1cf80d70745e8ef606f580
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665616"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Zarządzanie pakietami po stronie klienta za pomocą narzędzi Bower w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [ryżu Noel](https://twitter.com/noelrice1), i [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Jednocześnie jest Bower, zaleca się jego maintainers przy użyciu innego rozwiązania. [Menedżer biblioteki](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan w skrócie) to narzędzie programu Visual Studio do pozyskiwania nowych biblioteki po stronie klienta (Visual Studio, należy zachować 15,8 lub nowszej). Aby uzyskać więcej informacji, zobacz <xref:client-side/libman/index>. Bower jest świadczona w programie Visual Studio w wersji 15.5.
>
> Yarn z Webpack jest jeden popularną alternatywę dla którego [instrukcjach migracji](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) są dostępne.

[Program bower](https://bower.io/) wywołuje sam siebie "Menedżer pakietów dla sieci web". W ramach ekosystemu .NET umieszcza void pozostawiony przez brakiem NuGet, aby dostarczać plików zawartości statycznej. Dla projektów ASP.NET Core, te pliki statyczne są wbudowane w bibliotek po stronie klienta, takich jak [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/). Dla bibliotek .NET, możesz nadal używać [NuGet](https://www.nuget.org/) Menedżera pakietów.

Proces kompilacji nowe projekty utworzone za pomocą szablonów projektu ASP.NET Core, skonfiguruj po stronie klienta. [jQuery](http://jquery.com/) i [Bootstrap](http://getbootstrap.com/) są zainstalowane, i Bower jest obsługiwany.

Pakiety po stronie klienta są wymienione w *bower.json* pliku. Szablony projektów programu ASP.NET Core konfiguruje *bower.json* przy użyciu jQuery i dotyczącą weryfikacji jQuery, Bootstrap.

W tym samouczku dodamy obsługę [Font Awesome](http://fontawesome.io). Można zainstalować za pomocą pakietów bower **Zarządzanie pakietami programu Bower** interfejsu użytkownika lub ręcznie w *bower.json* pliku.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalację za pomocą Zarządzanie pakietami programu Bower interfejsu użytkownika

* Utwórz nową aplikację sieci Web platformy ASP.NET Core za pomocą **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu. Wybierz **aplikacji sieci Web** i **bez uwierzytelniania**.

* Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie pakietami programu Bower** (również w menu głównym **projektu** > **Zarządzanie pakietami programu Bower**).

* W **Bower: \<Nazwa projektu\>**  , kliknij kartę "Przeglądaj", a następnie Przefiltruj listę pakietów, wprowadzając `font-awesome` w polu wyszukiwania:

  ![Zarządzaj pakietami programu bower](bower/_static/manage-bower-packages.png)

* Upewnij się, że "zapisać zmiany w *bower.json*" pole wyboru jest zaznaczone. Wybierz wersję z listy rozwijanej, a następnie kliknij przycisk **zainstalować** przycisku. **Dane wyjściowe** okno zawiera szczegółowe informacje dotyczące instalacji.

### <a name="manual-installation-in-bowerjson"></a>Instalacja ręczna, w pliku bower.json

Otwórz *bower.json* pliku i Dodaj "font-awesome" do zależności. Funkcja IntelliSense wyświetla dostępne pakiety. Po wybraniu pakietu dostępnych wersji są wyświetlane. Poniższe obrazy są starsze i nie będzie zgodne, zostanie wyświetlony.

![IntelliSense Eksplorator pakietów bower](bower/_static/add-package.png)

![wersja programu bower IntelliSense](bower/_static/version-intelliSense.png)

Bower używa [wersji semantycznej](http://semver.org/) do organizowania zależności. Semantyczne przechowywania wersji, znany także jako SemVer identyfikuje pakiety ze schematu numerowania \<główna >.\< pomocnicza >. \<poprawki >. IntelliSense ułatwia semantycznego versioning przedstawiający kilka typowe opcje. Pierwszy element na liście funkcji IntelliSense (4.6.3 w powyższym przykładzie), jest uznawana za stabilną najnowszą wersję pakietu. Symbolu daszka (^) odpowiada najbardziej aktualną wersję główną i tyldy (~) dopasowuje najbardziej aktualną wersję pomocniczą.

Zapisz *bower.json* pliku. Program Visual Studio obserwuje *bower.json* zmiany w pliku. Po zapisaniu, *Zainstaluj program bower* polecenie jest wykonywane. Zobacz okno dane wyjściowe **Bower/npm** widok pełne polecenie wykonane.

Otwórz *.bowerrc* plik *bower.json*. `directory` Właściwość jest ustawiona na *wwwroot/lib* która wskazuje lokalizację Bower zainstaluje zasobów pakietu.

```json
{
 "directory": "wwwroot/lib"
}
```

Aby znaleźć i wyświetlić pakiet font awesome, można użyć pola wyszukiwania w Eksploratorze rozwiązań.

Otwórz *Views\Shared\_Layout.cshtml* pliku i Dodaj font awesome pliku CSS w środowisku [Pomocnik tagu](xref:mvc/views/tag-helpers/intro) dla `Development`. Za pomocą Eksploratora rozwiązań przeciągnij i upuść *awesome.css czcionki* wewnątrz `<environment names="Development">` elementu.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

W aplikacji produkcyjnych należy dodać *awesome.min.css czcionki* do Pomocnik tagu środowiska dla `Staging,Production`.

Zastąp zawartość *Views\Home\About.cshtml* Razor pliku następującym kodem:

[!code-html[](bower/sample/About.cshtml)]

Uruchom aplikację i przejdź do widoku informacje, aby Sprawdź, czy działa font awesome pakietu.

## <a name="exploring-the-client-side-build-process"></a>Eksplorowanie procesu kompilacji po stronie klienta

Większość szablonów projektów ASP.NET Core są już skonfigurowane do użycia rozwiązania Bower. Ten przewodnik dalej rozpoczyna się od pustego projektu platformy ASP.NET Core i dodaje każdy element ręcznie, dzięki czemu można uzyskać pewne pojęcie dotyczące sposobu używania rozwiązania Bower w projekcie. Możesz zobaczyć, co się dzieje z struktury projektu i środowiska uruchomieniowego, zgodnie z każdej zmiany konfiguracji.

Ogólne kroki procesu kompilacji po stronie klienta za pomocą rozwiązania Bower są:

* Definiowanie pakietów używanych w projekcie. <!-- once defined, you don't need to download them, VS does -->
* Pakiety odwołania ze stron sieci web.

### <a name="define-packages"></a>Definiowanie pakietów

Po wyświetleniu listy pakietów w *bower.json* plików, programu Visual Studio pobierze je. W poniższym przykładzie użyto narzędzia Bower do załadowania, jQuery i Bootstrap do *wwwroot* folderu.

* Utwórz nową aplikację sieci Web platformy ASP.NET Core za pomocą **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu. Wybierz **pusty** szablonu projektu i kliknij przycisk **OK**.

* W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy Projekt > **Dodaj nowy element** i wybierz **plik konfiguracji programu Bower**. Uwaga: A *.bowerrc* również zostanie dodany plik.

* Otwórz *bower.json*, Dodaj jquery i uruchamiania na `dependencies` sekcji. Wartość wynikowa *bower.json* plik będzie wyglądać podobnie jak w poniższym przykładzie. Wersje zmieni się wraz z upływem czasu i może nie odpowiadać na poniższej ilustracji.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Zapisz *bower.json* pliku.

  Sprawdź projekt obejmuje *bootstrap* i *jQuery* katalogi *wwwroot/lib*. Zastosowań bower *.bowerrc* plik, aby zainstalować zasoby w *wwwroot/lib*.

  Uwaga: Interfejs użytkownika "Zarządzaj pakietami Bower" stanowi alternatywę dla pliku ręcznej edycji.

### <a name="enable-static-files"></a>Włącz pliki statyczne

* Dodaj `Microsoft.AspNetCore.StaticFiles` pakiet NuGet do projektu.
* Włącz pliki statyczne z [oprogramowanie pośredniczące plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Dodaj wywołanie do [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) do `Configure` metody `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Odwołanie do pakietów

W tej sekcji utworzysz stronę HTML, aby sprawdzić, czy będzie miał dostęp do wdrożonych pakietów.

* Dodaj nową stronę HTML o nazwie *Index.html* do *wwwroot* folderu. Uwaga: Należy dodać plik HTML *wwwroot* folderu. Domyślnie nie może zostać wyświetlona zawartość statyczną, poza *wwwroot*. Zobacz [pliki statyczne](xref:fundamentals/static-files) Aby uzyskać więcej informacji.

  Zastąp zawartość *Index.html* następującym kodem:

[!code-html[](bower/sample/Index.html)]

* Uruchom aplikację i przejdź do `http://localhost:<port>/Index.html`. Alternatywnie za pomocą *Index.html* otwarte, naciśnij klawisz `Ctrl+Shift+W`. Sprawdź stosowanie stylów jumbotron, kodu jQuery odpowiada po kliknięciu przycisku i ładowania przycisku zmienia się stan.

  ![Styl jumbotron stosowany](bower/_static/jumbotron.png)
