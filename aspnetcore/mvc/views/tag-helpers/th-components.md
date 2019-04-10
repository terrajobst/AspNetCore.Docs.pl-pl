---
title: Składniki Pomocnika tagów w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jakie są składniki pomocnika tagów i sposobu ich używania w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: fdad4ae367245cd3beabaf90587c1fe5e9162afe
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468598"
---
# <a name="tag-helper-components-in-aspnet-core"></a>Składniki Pomocnika tagów w programie ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Hasan Fiyaz pojemnika](https://github.com/fiyazbinhasan)

Składnik pomocnika tagów jest pomocnika tagów, która pozwala na warunkowo zmodyfikować lub dodać elementów HTML w kodzie po stronie serwera. Ta funkcja jest dostępna w programie ASP.NET Core 2.0 lub nowszej.

Platforma ASP.NET Core obejmuje dwie wbudowane składniki pomocnika tagów: `head` i `body`. Znajdują się one w <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> przestrzeni nazw i mogą być używane w MVC i stron Razor. Składniki Pomocnika tagów nie wymagają rejestracji za pomocą aplikacji w *_ViewImports.cshtml*.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>Przypadki zastosowań

Dwie typowe przypadki użycia składników pomocnika tagów obejmują:

1. [Wprowadzanie `<link>` do `<head>`.](#inject-into-html-head-element)
1. [Wprowadzanie `<script>` do `<body>`.](#inject-into-html-body-element)

W poniższych sekcjach opisano te przypadki użycia.

### <a name="inject-into-html-head-element"></a>Wstrzyknięcie do głównego elementu HTML

W kodzie HTML `<head>` elementu, pliki CSS są często zaimportowane z kodem HTML `<link>` elementu. Poniższy kod wprowadza `<link>` elementu do `<head>` elementu za pomocą `head` składnik pomocnika tagów:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

W poprzednim kodzie:

* `AddressStyleTagHelperComponent` Implementuje <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>. Abstrakcja:
  * Umożliwia inicjowanie klasy <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.
  * Umożliwia korzystanie z składniki pomocnika tagów do dodawania lub modyfikowania elementów HTML.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Właściwość definiuje kolejność renderowania składników. `Order` jest to konieczne w przypadku użycia wielu składników pomocnika tagów w aplikacji.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*> porównuje kontekstu wykonania <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> wartość właściwości `head`. Jeśli porównanie zwraca wartość true, zawartość `_style` pola są wstrzykiwane do pliku HTML `<head>` elementu.

### <a name="inject-into-html-body-element"></a>Wstrzyknięcie do elementu body HTML

`body` Składnik pomocnika tagów może wprowadzać `<script>` elementu do `<body>` elementu. Poniższy przykład demonstruje tej techniki:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

Oddzielny plik HTML jest używany do przechowywania `<script>` elementu. Plik HTML sprawia, że kod bardziej przejrzystym i będzie łatwiejszy w utrzymaniu. Powyższy kod odczytuje zawartość *TagHelpers/Templates/AddressToolTipScript.html* i dołącza je z danymi wyjściowymi Pomocnik tagu. *AddressToolTipScript.html* plik zawiera następujące znaczniki:

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

Powyższy kod wiąże [Bootstrap etykietka narzędzia elementu widget](https://getbootstrap.com/docs/3.3/javascript/#tooltips) do dowolnego `<address>` element, który zawiera `printable` atrybutu. Efekt jest widoczny, gdy wskaźnik myszy znajduje się nad elementem.

## <a name="register-a-component"></a>Zarejestruj składnik

Składnik pomocnika tagów, należy dodać do kolekcji składników pomocnika tagów aplikacji. Istnieją trzy sposoby dodania do kolekcji:

1. [Rejestracja za pośrednictwem usługi kontenera](#registration-via-services-container)
1. [Rejestracja za pośrednictwem pliku Razor](#registration-via-razor-file)
1. [Rejestracja za pośrednictwem modelu strony lub kontrolera](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>Rejestracja za pośrednictwem usługi kontenera

Jeśli klasa składnika pomocnika tagów nie jest zarządzane przy użyciu <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>, musi być zarejestrowana w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) systemu. Następujące `Startup.ConfigureServices` kodu rejestrów `AddressStyleTagHelperComponent` i `AddressScriptTagHelperComponent` klasy za pomocą [przejściowych okres istnienia](xref:fundamentals/dependency-injection#lifetime-and-registration-options):

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>Rejestracja za pośrednictwem pliku Razor

Jeśli DI nie jest zarejestrowany składnik pomocnika tagów, może być zarejestrowany ze stronami Razor strony lub widoku MVC. Ta technika jest używane do kontrolowania wprowadzonego kodu znaczników i kolejność wykonywania składnika z pliku Razor.

`ITagHelperComponentManager` Służy do dodawania składników pomocnika tagów lub usuń je z poziomu aplikacji. Poniższy przykład demonstruje ta technika, za pomocą `AddressTagHelperComponent`:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

W poprzednim kodzie:

* `@inject` Wystąpienie zapewnia dyrektywa `ITagHelperComponentManager`. Wystąpienie jest przypisany do zmiennej o nazwie `manager` dostępu podrzędnych w pliku Razor.
* Wystąpienie `AddressTagHelperComponent` zostanie dodany do kolekcji składników pomocnika tagów aplikacji.

`AddressTagHelperComponent` zostanie zmodyfikowany w celu uwzględnienia Konstruktor, który akceptuje `markup` i `order` parametry:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

Podany `markup` parametr jest używany w `ProcessAsync` w następujący sposób:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>Rejestracja za pośrednictwem modelu strony lub kontrolera

Jeśli DI nie jest zarejestrowany składnik pomocnika tagów, może być zarejestrowany z modelem strony stron Razor lub kontroler MVC. Ta technika jest przydatna do oddzielania logiki języka C# w plikach Razor.

Umożliwia dostęp do wystąpienia iniekcji konstruktora `ITagHelperComponentManager`. Składnik pomocnika tagów jest dodawany do kolekcji składników pomocnika tagów wystąpienia. Następujące modelu strony stron Razor pokazano tej techniki, za pomocą `AddressTagHelperComponent`:

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

W poprzednim kodzie:

* Umożliwia dostęp do wystąpienia iniekcji konstruktora `ITagHelperComponentManager`.
* Wystąpienie `AddressTagHelperComponent` zostanie dodany do kolekcji składników pomocnika tagów aplikacji.

## <a name="create-a-component"></a>Tworzenie składnika

Aby utworzyć niestandardowy składnik pomocnika tagów:

* Utwórz klasę publiczną pochodząca od <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.
* Zastosuj [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) do klasy atrybutu. Określ nazwę docelowego elementu HTML.
* *Opcjonalnie*: Zastosuj [[EditorBrowsable(EditorBrowsableState.Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) atrybutów do klasy, aby wyłączyć wyświetlanie typu w technologii IntelliSense.

Poniższy kod tworzy niestandardowe składnik pomocnika tagów przeznaczonego `<address>` elementu HTML:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

Użyj niestandardowego `address` składnik pomocnika tagów można wstrzyknąć kod znaczników HTML w następujący sposób:

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
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

Poprzedni `ProcessAsync` metoda wprowadza HTML udostępniane <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> do dopasowywania `<address>` elementu. Iniekcja występuje, gdy:

* Kontekst wykonywania `TagName` wartość właściwości jest równa `address`.
* Odpowiedni `<address>` element ma `printable` atrybutu.

Na przykład `if` instrukcja zwraca wartość true, podczas przetwarzania następujących `<address>` elementu:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
