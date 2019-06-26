---
title: Platforma ASP.NET Core Razor składniki klasy biblioteki
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/24/2019
uid: blazor/class-libraries
ms.openlocfilehash: 8676e0fd660b7d281c80d06d24d5593c2df6348b
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394620"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Platforma ASP.NET Core Razor składniki klasy biblioteki

Przez [Simon Timms](https://github.com/stimms)

Składniki mogą być współużytkowane w [biblioteki klas Razor (RCL)](xref:razor-pages/ui-class) w projektach. A *Biblioteka klas składników Razor* można uwzględnić od:

* Innego projektu w rozwiązaniu.
* Pakiet NuGet.
* Odwołania biblioteki .NET.

Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez RCL są normalne zestawów platformy .NET.

## <a name="create-an-rcl"></a>Utwórz RCL

Postępuj zgodnie ze wskazówkami w <xref:blazor/get-started> artykuł, aby skonfigurować środowisko dla Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu. Przykłady w tym temacie użyje nazwy projektu `MyComponentLib1`. Wybierz pozycję **Utwórz**.
1. W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.
1. Wybierz **biblioteki klas Razor** szablonu. Wybierz pozycję **Utwórz**.
1. Dodawanie RCL do rozwiązania:
   1. Kliknij prawym przyciskiem myszy rozwiązanie. Wybierz **Dodaj** > **istniejący projekt**.
   1. Przejdź do pliku projektu RCL.
   1. Wybierz plik projektu RCL ( *.csproj*).
1. Dodaj odwołanie RCL z poziomu aplikacji:
   1. Kliknij prawym przyciskiem myszy projekt aplikacji. Wybierz **Dodaj** > **odwołania**.
   1. Wybierz projekt RCL. Kliknij przycisk **OK**.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Program Visual Studio Code / .NET Core interfejsu wiersza polecenia](#tab/visual-studio-code+netcore-cli)

1. Szablon biblioteki klas Razor (`razorclasslib`) za pomocą [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia powłoki poleceń. W poniższym przykładzie jest tworzony o nazwie RCL `MyComponentLib1`. Folder, który przechowuje `MyComponentLib1` jest tworzona automatycznie podczas wykonywania polecenia.

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference) polecenia powłoki poleceń. W poniższym przykładzie RCL zostanie dodany do aplikacji. Wykonaj następujące polecenie z folderu projektu aplikacji ze ścieżką do biblioteki:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

Dodaj pliki składników Razor ( *.razor*) do RCL.

## <a name="rcls-not-supported-for-client-side-apps"></a>RCLs nie jest obsługiwane dla aplikacji po stronie klienta

W programie ASP.NET Core 3.0 (wersja zapoznawcza) biblioteki klas Razor nie są zgodne z aplikacji po stronie klienta Blazor.

W przypadku aplikacji po stronie klienta Blazor używać biblioteki składników Blazor utworzone przez `blazorlib` szablonu z powłoki poleceń:

```console
dotnet new blazorlib -o MyComponentLib1
```

Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów. W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego ( *.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów. Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.

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

Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet. Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Tworzenie biblioteki klas Razor składników za pomocą statycznych zasobów

RCL mogą obejmować zasoby statyczne. Statyczne elementy zawartości są dostępne dla żadnej aplikacji, która korzysta z biblioteki. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/ui-class>
