---
title: Biblioteki klas składników razor
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Razor składników z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159232"
---
# <a name="razor-components-class-libraries"></a>Biblioteki klas składników razor

Przez [Simon Timms](https://github.com/stimms)

> [!NOTE]
> Zestaw SDK platformy .NET Core 3.0 w wersji zapoznawczej 2 nie zawiera szablon projektu dla bibliotek klas składników Razor, ale oczekuje się dodać szablon w przyszłej wersji zapoznawczej. W międzyczasie można użyć szablonu biblioteki klas składników Blazor szczegółowo opisane w tym temacie.

Składniki mogą być udostępniane w bibliotekach składnika w projektach. Składniki można uwzględnić od:

* Innego projektu w rozwiązaniu.
* Pakiet NuGet.
* Odwołania biblioteki .NET.

Tak, jak składniki są regularnie typów .NET, składnik biblioteki są normalne zestawów platformy .NET.

Aby utworzyć nową bibliotekę składników, należy użyć `blazorlib` szablon [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia. Szablon jest częścią szablonów zainstalowanych podczas [Konfigurowanie składników Razor](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet sln](/dotnet/core/tools/dotnet-sln) polecenia:

```console
dotnet sln add .\MyComponentLib1
```

Biblioteki składników może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów. W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego (*.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów. Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego. 

## <a name="consume-a-library-component"></a>Używanie składnik biblioteki

Aby można było korzystających ze składników zdefiniowane w bibliotece w innym projekcie [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) dyrektywa musi być używana. Poszczególne składniki mogą być dodawane według nazwy. Na przykład następująca dyrektywa dodaje `Component1` z `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Ogólny format dyrektywy jest:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Jednak jest wspólne dla wszystkich składników z zestawu przy użyciu symboli wieloznacznych zawierają:

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

Wszystkie dołączone zasoby statyczne znajdują się w pakiecie NuGet. Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.
