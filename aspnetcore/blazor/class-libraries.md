---
title: Biblioteki klas składników razor
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6c0b741de1e3b9ad2b226cc376f06ad8365542e8
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614872"
---
# <a name="razor-components-class-libraries"></a>Biblioteki klas składników razor

Przez [Simon Timms](https://github.com/stimms)

Składniki mogą być udostępniane w bibliotekach klas Razor w projektach. Składniki można uwzględnić od:

* Innego projektu w rozwiązaniu.
* Pakiet NuGet.
* Odwołania biblioteki .NET.

Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez biblioteki klas Razor są normalne zestawów platformy .NET.

Użyj `razorclasslib` szablonu (biblioteki klas Razor) za pomocą [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia:

```console
dotnet new razorclasslib -o MyComponentLib1
```

Dodaj pliki Razor składników (*.razor*) do biblioteki klas Razor.

Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet sln](/dotnet/core/tools/dotnet-sln) polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Biblioteki klas razor nie są zgodne z aplikacjami Blazor w ASP.NET Core w wersji zapoznawczej 3.
>
> Do tworzenia składników w bibliotece, które mogą być udostępniane innym Blazor po stronie klienta i Razor składniki po stronie serwera aplikacji, użyj utworzonych przez bibliotekę klas Blazor `blazorlib` szablonu.
>
> Biblioteki klas razor statycznych zasobów w ASP.NET Core w wersji zapoznawczej 3 nie są obsługiwane. Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów. W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego (*.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów. Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.

## <a name="consume-a-library-component"></a>Używanie składnik biblioteki

Aby można było korzystających ze składników zdefiniowane w bibliotece w innym projekcie [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) dyrektywa musi być używana. Poszczególne składniki mogą być dodawane według nazwy.

Ogólny format dyrektywy jest:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Na przykład następująca dyrektywa dodaje `Component1` z `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Jednak jest często obejmują wszystkie elementy z zestawu przy użyciu symboli wieloznacznych (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

`@addTagHelper` Mogą być dołączane dyrektywą *_ViewImport.cshtml* dokonać składników dostępne dla całego projektu lub zastosowane pojedynczej strony lub zbiór stron w folderze. Za pomocą `@addTagHelper` dyrektywy w miejscu, składniki biblioteki składników mogą być używane tak, jakby znajdowały się w tym samym zestawie co aplikacja.

## <a name="build-pack-and-ship-to-nuget"></a>Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet

Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet. Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia:

```console
dotnet pack
```

Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia:

```console
dotnet nuget publish
```

Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet. Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.
