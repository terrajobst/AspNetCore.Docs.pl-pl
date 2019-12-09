---
title: ASP.NET Core biblioteki klas składników Razor
author: guardrex
description: Odkryj, jak składniki mogą być dołączane do aplikacji Blazor z zewnętrznej biblioteki składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/class-libraries
ms.openlocfilehash: 6bac007e3e1d046d6b16a3a0be6dc5976b99b766
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943878"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core biblioteki klas składników Razor

Autor [Simon Timms](https://github.com/stimms)

Składniki mogą być współużytkowane w [bibliotece klas Razor (RCL)](xref:razor-pages/ui-class) w różnych projektach. *Biblioteka klas składników Razor* może być dołączona z:

* Inny projekt w rozwiązaniu.
* Pakiet NuGet.
* Przywoływana Biblioteka platformy .NET.

Podobnie jak składniki są zwykłymi typami .NET, składniki udostępniane przez RCL są normalnymi zestawami platformy .NET.

## <a name="create-an-rcl"></a>Utwórz RCL

Postępuj zgodnie ze wskazówkami w artykule <xref:blazor/get-started>, aby skonfigurować środowisko dla Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **bibliotekę klas Razor**. Wybierz opcję **Dalej**.
1. W oknie dialogowym **Tworzenie nowej biblioteki klas Razor** wybierz pozycję **Utwórz**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. W przykładach w tym temacie użyto nazwy projektu `MyComponentLib1`. Wybierz **tworzenie**.
1. Dodaj RCL do rozwiązania:
   1. Kliknij prawym przyciskiem myszy rozwiązanie. Wybierz pozycję **dodaj** > **istniejący projekt**.
   1. Przejdź do pliku projektu RCL.
   1. Wybierz plik projektu RCL ( *. csproj*).
1. Dodaj odwołanie RCL z aplikacji:
   1. Kliknij prawym przyciskiem myszy projekt aplikacji. Wybierz pozycję dodaj **odwołanie** > .
   1. Wybierz projekt RCL. Wybierz **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Użyj szablonu **biblioteki klas Razor** (`razorclasslib`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) w powłoce poleceń. W poniższym przykładzie jest tworzony RCL o nazwie `MyComponentLib1`. Folder, który zawiera `MyComponentLib1` jest tworzony automatycznie podczas wykonywania polecenia:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Aby dodać bibliotekę do istniejącego projektu, użyj polecenia [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) w powłoce poleceń. W poniższym przykładzie RCL jest dodawany do aplikacji. Wykonaj następujące polecenie z folderu projektu aplikacji z ścieżką do biblioteki:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Korzystanie ze składnika biblioteki

Aby można było korzystać ze składników zdefiniowanych w bibliotece w innym projekcie, należy użyć jednej z następujących metod:

* Użyj pełnej nazwy typu z przestrzeni nazw.
* Użyj\@składnicy [za pomocą](xref:mvc/views/razor#using) dyrektywy. Poszczególne składniki można dodawać według nazwy.

W poniższych przykładach `MyComponentLib1` jest biblioteka składników zawierająca składnik `SalesReport`.

Do składnika `SalesReport` można odwoływać się za pomocą jego pełnej nazwy typu z przestrzenią nazw:

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Składnik może być również przywoływany, jeśli biblioteka została wprowadzona do zakresu za pomocą dyrektywy `@using`:

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Uwzględnij dyrektywę `@using MyComponentLib1` w pliku *_Import. Razor* najwyższego poziomu, aby udostępnić składniki biblioteki dla całego projektu. Dodaj dyrektywę do pliku *_Import. Razor* na dowolnym poziomie, aby zastosować przestrzeń nazw do pojedynczej strony lub zestawu stron w folderze.

## <a name="build-pack-and-ship-to-nuget"></a>Kompilowanie, pakowanie i dostarczanie do narzędzia NuGet

Ponieważ biblioteki składników są standardowymi bibliotekami .NET, pakowanie i dostarczanie ich do narzędzia NuGet nie różni się od pakowania i wysyłania żadnej biblioteki do narzędzia NuGet. Pakowanie jest wykonywane przy użyciu polecenia [pakietu dotnet](/dotnet/core/tools/dotnet-pack) w powłoce poleceń:

```dotnetcli
dotnet pack
```

Przekaż pakiet do narzędzia NuGet przy użyciu polecenia [push NuGet w trybie wypychania](/dotnet/core/tools/dotnet-nuget-push) w powłoce poleceń.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Tworzenie biblioteki klas składników Razor ze statycznymi zasobami

RCL może zawierać statyczne zasoby. Zasoby statyczne są dostępne dla każdej aplikacji, która korzysta z biblioteki. Aby uzyskać więcej informacji, zobacz temat <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-pages/ui-class>
