---
title: ASP.NET Core biblioteki klas składników Razor
author: guardrex
description: Odkryj, jak składniki mogą być dołączane do aplikacji Blazor z zewnętrznej biblioteki składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: b5857f2cf22bde801deeeaf227817fdf99862f4a
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545773"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core biblioteki klas składników Razor

Autor [Simon Timms](https://github.com/stimms)

Składniki mogą być współużytkowane w [bibliotece klas Razor (RCL)](xref:razor-pages/ui-class) w różnych projektach. *Biblioteka klas składników Razor* może być dołączona z:

* Inny projekt w rozwiązaniu.
* Pakiet NuGet.
* Przywoływana Biblioteka platformy .NET.

Podobnie jak składniki są zwykłymi typami .NET, składniki udostępniane przez RCL są normalnymi zestawami platformy .NET.

## <a name="create-an-rcl"></a>Utwórz RCL

Postępuj zgodnie ze wskazówkami zawartymi w <xref:blazor/get-started> artykule, aby skonfigurować środowisko dla programu Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **bibliotekę klas Razor**. Wybierz opcję **Dalej**.
1. W oknie dialogowym **Tworzenie nowej biblioteki klas Razor** wybierz pozycję **Utwórz**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. W przykładach w tym temacie użyto nazwy `MyComponentLib1`projektu. Wybierz pozycję **Utwórz**.
1. Dodaj RCL do rozwiązania:
   1. Kliknij prawym przyciskiem myszy rozwiązanie. Wybierz pozycję **Dodaj** > **istniejący projekt**.
   1. Przejdź do pliku projektu RCL.
   1. Wybierz plik projektu RCL ( *. csproj*).
1. Dodaj odwołanie RCL z aplikacji:
   1. Kliknij prawym przyciskiem myszy projekt aplikacji. Wybierz pozycję **Dodaj** > **odwołanie**.
   1. Wybierz projekt RCL. Kliknij przycisk **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Użyj szablonu **biblioteki klas Razor** (`razorclasslib`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) w powłoce poleceń. W poniższym przykładzie jest tworzony RCL o nazwie `MyComponentLib1`. Folder, który `MyComponentLib1` ma zostać utworzony, jest tworzony automatycznie podczas wykonywania polecenia:

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Aby dodać bibliotekę do istniejącego projektu, użyj polecenia [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) w powłoce poleceń. W poniższym przykładzie RCL jest dodawany do aplikacji. Wykonaj następujące polecenie z folderu projektu aplikacji z ścieżką do biblioteki:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Korzystanie ze składnika biblioteki

Aby można było korzystać ze składników zdefiniowanych w bibliotece w innym projekcie, należy użyć jednej z następujących metod:

* Użyj pełnej nazwy typu z przestrzeni nazw.
* Użyj dyrektywy [ \@using przy użyciu](xref:mvc/views/razor#using) składni Razor. Poszczególne składniki można dodawać według nazwy.

W poniższych przykładach `MyComponentLib1` jest biblioteka składników `SalesReport` zawierająca składnik.

Do `SalesReport` składnika można odwoływać się za pomocą jego pełnej nazwy typu z przestrzenią nazw:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Składnik może być również przywoływany, jeśli biblioteka została wprowadzona do zakresu przy `@using` użyciu dyrektywy:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Uwzględnij dyrektywę w pliku *_Import. Razor* najwyższego poziomu, aby udostępnić składniki biblioteki dla całego projektu. `@using MyComponentLib1` Dodaj dyrektywę do pliku *_Import. Razor* na dowolnym poziomie, aby zastosować przestrzeń nazw do pojedynczej strony lub zestawu stron w folderze.

## <a name="build-pack-and-ship-to-nuget"></a>Kompilowanie, pakowanie i dostarczanie do narzędzia NuGet

Ponieważ biblioteki składników są standardowymi bibliotekami .NET, pakowanie i dostarczanie ich do narzędzia NuGet nie różni się od pakowania i wysyłania żadnej biblioteki do narzędzia NuGet. Pakowanie jest wykonywane przy użyciu polecenia [pakietu dotnet](/dotnet/core/tools/dotnet-pack) w powłoce poleceń:

```console
dotnet pack
```

Przekaż pakiet do narzędzia NuGet przy użyciu polecenia [publikowania NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-push) w powłoce poleceń:

```console
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Tworzenie biblioteki klas składników Razor ze statycznymi zasobami

RCL może zawierać statyczne zasoby. Zasoby statyczne są dostępne dla każdej aplikacji, która korzysta z biblioteki. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/ui-class>
