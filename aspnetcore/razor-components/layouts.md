---
title: Układy składniki razor
author: guardrex
description: Dowiedz się, jak tworzyć składniki wielokrotnego użytku układu dla aplikacji Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159470"
---
# <a name="razor-components-layouts"></a>Układy składniki razor

Przez [Rainer Stropek](https://www.timecockpit.com)

Aplikacje zwykle zawierają więcej niż jedną stronę. Układ elementów, takich jak menu, komunikaty o prawach autorskich i logo musi być obecny na wszystkich stronach. Skopiować kod z tych elementów układu do wszystkich stron aplikacji nie jest wydajne rozwiązanie. Takie duplikowanie jest trudne do utrzymania i prawdopodobnie prowadzi do niespójne zawartość wraz z upływem czasu. *Układy* rozwiązać ten problem.

Technicznie rzecz biorąc układ jest po prostu inny składnik. Układ jest zdefiniowany w szablonie Razor lub w C# kodu i może zawierać wiązania danych, wstrzykiwanie zależności i inne funkcje zwykłych składników. Włącz dwa dodatkowe aspekty *składnika* do *układ*:

* Składnik układu musi dziedziczyć `BlazorLayoutComponent`. `BlazorLayoutComponent` definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.
* Składnik układ używa `Body` właściwości w celu określenia, w którym treść powinna być renderowany przy użyciu składni Razor `@Body`. Podczas renderowania, `@Body` zastępuje zawartość układu.

Poniższy przykład kodu pokazuje szablon Razor składnika układu. Zwróć uwagę na użycie `BlazorLayoutComponent` i `@Body`:

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a>Używanie układu w składniku

Użyj dyrektywy Razor `@layout` można zastosować układu do składnika. Kompilator konwertuje tej dyrektywy do `LayoutAttribute`, który jest stosowany do klasy składnika.

W poniższym przykładzie kodu pokazano pojęcia. Zawartość tego składnika jest wstawiany do *MasterLayout* w położeniu `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Wybór układu scentralizowane

Każdy folder z aplikacji, można opcjonalnie zawiera plik szablonu o nazwie *_ViewImports.cshtml*. Kompilator zawiera dyrektywy określone w pliku importu widoku we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów. W związku z tym *_ViewImports.cshtml* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout* układu. Nie ma potrzeby można wielokrotnie dodać `@layout` do wszystkich  *\*.cshtml* plików.

Należy zauważyć, że korzysta z domyślnego szablonu *_ViewImports.cshtml* mechanizm wyboru układu. Zawiera nowo utworzoną aplikację *_ViewImports.cshtml* w pliku *stron* folderu.

## <a name="nested-layouts"></a>Układy zagnieżdżonych

Aplikacje może zawierać zagnieżdżone układów. Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu. Na przykład zagnieżdżenia układów może służyć do odzwierciedlenia struktury wielopoziomowe menu.

Poniższe przykłady kodu przedstawiają sposób używać zagnieżdżonych układów. *CustomersComponent.cshtml* plik jest składnikiem do wyświetlenia. Należy pamiętać, że składnik odwołuje się do układu `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

*MasterDataLayout.cshtml* plik zawiera `MasterDataLayout`. Układ odwołuje się do innego układu `MainLayout`, gdzie ma to być osadzone.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Na koniec `MainLayout` zawiera elementy układu najwyższego poziomu, takie jak nagłówek, stopka i menu głównego.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
