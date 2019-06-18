---
title: Platforma ASP.NET Core Razor składniki klasy biblioteki
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/class-libraries
ms.openlocfilehash: 40dc029b35e0997e526fdfc02c275da5a84450b9
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152882"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Platforma ASP.NET Core Razor składniki klasy biblioteki

Przez [Simon Timms](https://github.com/stimms)

Składniki mogą być udostępniane w bibliotekach klas Razor w projektach. Składniki można uwzględnić od:

* Innego projektu w rozwiązaniu.
* Pakiet NuGet.
* Odwołania biblioteki .NET.

Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez biblioteki klas Razor są normalne zestawów platformy .NET.

## <a name="create-a-razor-class-library"></a>Tworzenie biblioteki klas Razor

Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby skonfigurować środowisko dla Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu. Przykłady w tym temacie użyje nazwy projektu `MyComponentLib1`. Wybierz pozycję **Utwórz**.
1. W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.
1. Wybierz **biblioteki klas Razor** szablonu. Wybierz pozycję **Utwórz**.
1. Dodawanie biblioteki klas Razor do rozwiązania:
   1. Kliknij prawym przyciskiem myszy rozwiązanie. Wybierz **Dodaj** > **istniejący projekt**.
   1. Przejdź do pliku projektu biblioteki klas Razor.
   1. Wybierz plik projektu biblioteki klas Razor ( *.csproj*).
1. Dodaj odwołanie do biblioteki klas Razor z poziomu aplikacji:
   1. Kliknij prawym przyciskiem myszy projekt aplikacji. Wybierz **Dodaj** > **odwołania**.
   1. Wybierz projekt biblioteki klas Razor. Kliknij przycisk **OK**.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Program Visual Studio Code / .NET Core interfejsu wiersza polecenia](#tab/visual-studio-code+netcore-cli)

1. Użyj biblioteki klas Razor (`razorclasslib`) szablon [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia powłoki poleceń. W poniższym przykładzie jest tworzony o nazwie biblioteki klas Razor `MyComponentLib1`. Folder, który przechowuje `MyComponentLib1` jest tworzona automatycznie podczas wykonywania polecenia.

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference) polecenia powłoki poleceń. W poniższym przykładzie biblioteki klas Razor jest dodawany do aplikacji. Wykonaj następujące polecenie z folderu projektu aplikacji ze ścieżką do biblioteki:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

Dodaj pliki składników Razor ( *.razor*) do biblioteki klas Razor.

## <a name="razor-class-libraries-not-supported-for-client-side-apps"></a>Biblioteki klas razor nie jest obsługiwane dla aplikacji po stronie klienta

W programie ASP.NET Core 3.0 (wersja zapoznawcza) biblioteki klas Razor nie są zgodne z aplikacji po stronie klienta Blazor.

W przypadku aplikacji po stronie klienta Blazor używać biblioteki składników Blazor utworzone przez `blazorlib` szablonu z powłoki poleceń:

```console
dotnet new blazorlib -o MyComponentLib1
```

Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów. W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego ( *.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów. Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.

## <a name="static-assets-not-supported-for-server-side-apps"></a>Statyczne elementy zawartości, które nie są obsługiwane w przypadku aplikacji po stronie serwera

W programie ASP.NET Core 3.0 (wersja zapoznawcza), Blazor po stronie serwera aplikacji nie może zużywać zasoby statyczne z obu biblioteki klas Razor (`razorclasslib`) lub bibliotekę Blazor (`blazorlib`).

Jako rozwiązanie tymczasowe, możesz spróbować [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).

> [!NOTE]
> [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.

## <a name="consume-a-library-component"></a>Używanie składnik biblioteki

Aby można było używać składników zdefiniowane w bibliotece w innym projekcie, użyj jednej z następujących metod:

* Pełna nazwa typu za pomocą przestrzeni nazw.
* Użyj firmy Razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy. Poszczególne składniki można dodać według nazwy.

W poniższych przykładach `MyComponentLib1` to biblioteka składnika zawierająca raport sprzedaży (`SalesReport`) składnika.

Składnik raport sprzedaży mogą być przywoływane z przestrzenią nazw przy użyciu jego pełna nazwa typu:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Składnik również mogą być przywoływane, jeśli biblioteka zostanie przełączony w tryb do zakresu za pomocą `@using` dyrektywy:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Obejmują `@using MyComponentLib1` dyrektywy w najwyższego poziomu *_Import.razor* pliku udostępniania biblioteki składników dla całego projektu. Dodaj dyrektywę do *_Import.razor* plik na dowolnym poziomie, aby zastosować przestrzeń nazw pojedynczej strony lub zbiór stron w folderze.

## <a name="build-pack-and-ship-to-nuget"></a>Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet

Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet. Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia powłoki poleceń:

```console
dotnet pack
```

Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia powłoki poleceń:

```console
dotnet nuget publish
```

Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet. Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów. Należy pamiętać, że [statycznych zasobów nie są obsługiwane w przypadku aplikacji po stronie serwera](#static-assets-not-supported-for-server-side-apps), w tym przypadku biblioteki Blazor (`blazorlib`) odwołuje się do aplikacji po stronie serwera.

## <a name="create-a-razor-class-library-with-static-assets"></a>Tworzenie biblioteki klas Razor przy użyciu statycznych zasobów

Biblioteki klas razor (RCL) często wymagają pomocnika statycznych zasobów, które mogą być przywoływane przez aplikację odbierającą RCL. Platforma ASP.NET Core umożliwia tworzenie RCLs, które obejmują zasoby statyczne, które są dostępne dla aplikacji.

Aby uwzględnić zasoby pomocnika jako część biblioteki klas Razor, utworzyć *wwwroot* folder w ułatwieniach i zawierać wszystkie wymagane pliki w tym folderze.

Podczas pakowania, biblioteki klas Razor, wszystkie pomocnika zasobów w *wwwroot* folderów są automatycznie uwzględnione w pakiecie i są dostępne dla aplikacji, które odwołuje się do pakietu.

### <a name="consume-content-from-a-referenced-razor-class-library"></a>Korzystanie z zawartości z przywoływanych biblioteki klas Razor

Pliki zawarte w *wwwroot* folder biblioteki klas Razor są widoczne dla aplikacji w ramach prefiksu `_content/{LIBRARY NAME}/`. Aplikacja odbierająca komunikaty odwołuje się do tych zasobów za pomocą `<script>`, `<style>`, `<img>`, a inne tagi HTML.

### <a name="multi-project-development-flow"></a>Przepływ rozwoju wielu projektów

Po uruchomieniu aplikacji:

* Zasoby pozostają w ich oryginalnych folderów.
* Wszelkie zmiany w bibliotece klas *wwwroot* folder jest widoczny w aplikacji bez ponownie skompilować.

W czasie kompilacji manifest jest generowany ze wszystkich lokalizacji zasobów statyczną sieci web. Manifest jest do odczytu w czasie wykonywania i umożliwia aplikacji korzystanie z zasobów w projektach odwołania i pakietów.

### <a name="publish"></a>Publikowanie

Po opublikowaniu aplikacji zasoby pomocnika z wszystkie przywoływane projekty i pakiety są kopiowane do *wwwroot* folderu opublikowanej aplikacji, w obszarze `_content/{LIBRARY NAME}/`.
