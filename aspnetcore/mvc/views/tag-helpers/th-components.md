---
title: Oznacz składniki pomocnika w ASP.NET Core
author: scottaddie
description: Informacje o składnikach pomocników tagów i sposobach ich użycia w ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 5e2eb2d4322068c5864fbe49acaa6d0859bd319a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660770"
---
# <a name="tag-helper-components-in-aspnet-core"></a>Oznacz składniki pomocnika w ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Fiyaz bin Hasan](https://github.com/fiyazbinhasan)

Składnik pomocnika tagów to pomocnik tagów, który umożliwia warunkowe modyfikowanie lub Dodawanie elementów HTML z kodu po stronie serwera. Ta funkcja jest dostępna w ASP.NET Core 2,0 lub nowszej.

ASP.NET Core obejmuje dwa wbudowane składniki pomocnika tagów: `head` i `body`. Znajdują się one w przestrzeni nazw <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> i mogą być używane zarówno w MVC, jak i Razor Pages. Składniki pomocnika tagów nie wymagają rejestracji w aplikacji w *_ViewImports. cshtml*.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>Przypadki zastosowań

Dwa typowe przypadki użycia składników pomocnika tagów obejmują:

1. [Wprowadzanie `<link>` do `<head>`.](#inject-into-html-head-element)
1. [Wprowadzanie `<script>` do `<body>`.](#inject-into-html-body-element)

W poniższych sekcjach opisano te przypadki użycia.

### <a name="inject-into-html-head-element"></a>Wsuń do elementu głównego HTML

Wewnątrz elementu `<head>` HTML pliki CSS są zwykle importowane z elementem HTML `<link>`. Poniższy kod wprowadza `<link>` elementu do elementu `<head>` przy użyciu składnika pomocnika tagów `head`:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

W powyższym kodzie:

* `AddressStyleTagHelperComponent` implementuje <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>. Streszczenie:
  * Umożliwia inicjowanie klasy za pomocą <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.
  * Umożliwia używanie składników pomocnika tagów do dodawania lub modyfikowania elementów HTML.
* Właściwość <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> definiuje kolejność, w jakiej składniki są renderowane. `Order` jest konieczne, gdy w aplikacji istnieje wiele użycia składników pomocnika tagów.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> porównuje wartość właściwości <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> kontekstu wykonania do `head`. Jeśli wynikiem porównania jest wartość true, zawartość pola `_style` jest wstrzykiwana do elementu `<head>` HTML.

### <a name="inject-into-html-body-element"></a>Wsuń do elementu treści HTML

Składnik pomocnika tagów `body` może wstrzyknąć element `<script>` do elementu `<body>`. Poniższy kod ilustruje tę technikę:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

Oddzielny plik HTML jest używany do przechowywania elementu `<script>`. Plik HTML sprawia, że kod jest bardziej przejrzysty i bardziej czytelny. Poprzedni kod odczytuje zawartość *TagHelpers/templates/AddressToolTipScript.html* i dołącza ją z danymi wyjściowymi pomocnika tagów. Plik *AddressToolTipScript. html* zawiera następujące znaczniki:

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

Poprzedni kod wiąże [widżet etykietki narzędzia Bootstrap](https://getbootstrap.com/docs/3.3/javascript/#tooltips) z dowolnym elementem `<address>`, który zawiera atrybut `printable`. Efekt jest widoczny, gdy wskaźnik myszy znajduje się nad elementem.

## <a name="register-a-component"></a>Rejestrowanie składnika

Składnik pomocnika tagów należy dodać do kolekcji składników pomocnika tagów aplikacji. Istnieją trzy sposoby dodawania do kolekcji:

* [Rejestracja za pośrednictwem kontenera usług](#registration-via-services-container)
* [Rejestracja za pośrednictwem pliku Razor](#registration-via-razor-file)
* [Rejestracja za pośrednictwem modelu lub kontrolera strony](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>Rejestracja za pośrednictwem kontenera usług

Jeśli Klasa składnika pomocnika tagów nie jest zarządzana z <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, musi być zarejestrowana w systemie [wtrysku zależności (di)](xref:fundamentals/dependency-injection) . Poniższy kod `Startup.ConfigureServices` rejestruje klasy `AddressStyleTagHelperComponent` i `AddressScriptTagHelperComponent` z [przejściowym okresem istnienia](xref:fundamentals/dependency-injection#lifetime-and-registration-options):

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>Rejestracja za pośrednictwem pliku Razor

Jeśli składnik pomocnika tagów nie jest zarejestrowany przy użyciu DI, może być zarejestrowany na stronie Razor Pages lub widoku MVC. Ta technika służy do kontrolowania wstrzykniętego znacznika i kolejności wykonywania składników z pliku Razor.

`ITagHelperComponentManager` służy do dodawania składników pomocnika tagów lub usuwania ich z aplikacji. Poniższy kod ilustruje tę technikę z `AddressTagHelperComponent`:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

W powyższym kodzie:

* Dyrektywa `@inject` zawiera wystąpienie `ITagHelperComponentManager`. Wystąpienie jest przypisane do zmiennej o nazwie `manager` w celu uzyskania dostępu do pliku Razor.
* Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.

`AddressTagHelperComponent` został zmodyfikowany w celu uwzględnienia konstruktora akceptującego parametry `markup` i `order`:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

Podany `markup` parametr jest używany w `ProcessAsync` w następujący sposób:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>Rejestracja za pośrednictwem modelu lub kontrolera strony

Jeśli składnik pomocnika tagów nie jest zarejestrowany w programie DI, może być zarejestrowany z poziomu modelu strony Razor Pages lub kontrolera MVC. Ta technika jest przydatna do oddzielania C# logiki od plików Razor.

Iniekcja konstruktora jest używana w celu uzyskania dostępu do wystąpienia `ITagHelperComponentManager`. Składnik pomocnika tagów jest dodawany do kolekcji składników pomocnika tagów wystąpienia. Poniższy Razor Pages model strony ilustruje tę technikę z `AddressTagHelperComponent`:

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

W powyższym kodzie:

* Iniekcja konstruktora jest używana w celu uzyskania dostępu do wystąpienia `ITagHelperComponentManager`.
* Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.

## <a name="create-a-component"></a>Tworzenie składnika

Aby utworzyć składnik pomocnika tagów niestandardowych:

* Utwórz klasę publiczną wynikającą z <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.
* Zastosuj atrybut [`[HtmlTargetElement]`](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) do klasy. Określ nazwę docelowego elementu HTML.
* *Opcjonalne*: zastosuj atrybut [`[EditorBrowsable(EditorBrowsableState.Never)]`](xref:System.ComponentModel.EditorBrowsableAttribute) do klasy, aby pominąć wyświetlanie typu w IntelliSense.

Poniższy kod tworzy składnik pomocnika tagów niestandardowych, który jest przeznaczony dla `<address>` elementu HTML:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

Użyj składnika pomocnika tagów niestandardowych `address`, aby wstrzyknąć znaczniki HTML w następujący sposób:

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open(" +
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

Poprzednia metoda `ProcessAsync` wprowadza kod HTML udostępniony do <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> do zgodnego elementu `<address>`. Iniekcja występuje, gdy:

* Wartość właściwości `TagName` kontekstu wykonywania jest równa `address`.
* Odpowiadający element `<address>` ma atrybut `printable`.

Na przykład, instrukcja `if` ma wartość true podczas przetwarzania następującego elementu `<address>`:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
