---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8803c406660398ad3314306a1bcdf99af418082d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393141"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Tworzenie stron pomocy dla interfejsu API sieci Web platformy ASP.NET
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Podczas tworzenia interfejsu API sieci web, często jest przydatne utworzyć stronę pomocy, aby inni deweloperzy powinni wiedzieć, jak wywołać interfejs API. Można utworzyć całą dokumentację ręcznie, ale zaleca się automatyczne generowanie możliwie.

Aby ułatwić to zadanie, Web API platformy ASP.NET udostępnia bibliotekę do automatycznego wygenerowania stron pomocy w czasie wykonywania.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Tworzenie stron pomocy interfejsu API

Zainstaluj [ASP.NET and Web Tools 2012.2 aktualizacji](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja integruje stron pomocy szablonu projektu interfejsu API sieci Web.

Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web. Szablon projektu umożliwia utworzenie kontrolera przykładowy interfejs API o nazwie `ValuesController`. Ten szablon tworzy również stron pomocy interfejsu API. Wszystkie pliki kodu strony pomocy są umieszczane w folderze obszarów projektu.

![](creating-api-help-pages/_static/image2.png)

Po uruchomieniu aplikacji, na stronie głównej zawiera łącze do strony pomocy interfejsu API. Na stronie głównej ścieżka względna jest/Help.

![](creating-api-help-pages/_static/image3.png)

Ten link łączy do strony Podsumowanie interfejsu API.

![](creating-api-help-pages/_static/image4.png)

Widok MVC ta strona jest zdefiniowany w Areas/HelpPage/Views/Help/Index.cshtml. Możesz edytować tę stronę, aby modyfikować układ, wprowadzenie, tytuł, style i tak dalej.

Główna część strony znajduje się tabela interfejsów API, pogrupowane według kontrolera. Wpisy tabeli są generowane dynamicznie przy użyciu **IApiExplorer** interfejsu. (Czy powiemy więcej informacji na temat tego interfejsu później.) Jeśli dodasz nowy kontroler interfejsu API, tabeli jest automatycznie aktualizowana w czasie wykonywania.

Kolumna "Interfejs API" wymieniono metody HTTP i względnym identyfikatorem URI. Kolumna "Opis" zawiera dokumentacja dla każdego interfejsu API. Dokumentacja jest początkowo tylko tekst symbolu zastępczego. W następnej sekcji I opisano sposób dodawania dokumentacji z komentarzy XML.

Każdy interfejs API zawiera link do strony zawierającej więcej szczegółowych informacji, w tym przykładzie treści żądania i odpowiedzi.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Dodawanie stron pomocy do istniejącego projektu

Strony pomocy można dodać do istniejącego projektu interfejsu API sieci Web przy użyciu Menedżera pakietów NuGet. Ta opcja jest przydatna, Rozpocznij od szablonu projektu innego niż szablon "Interfejs API sieci Web".

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okna, wpisz jedno z następujących poleceń:

Aby uzyskać **C#** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Aby uzyskać **języka Visual Basic** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Istnieją dwa pakiety, jeden dla języka C# i jeden dla języka Visual Basic. Upewnij się, że używasz zgodną z projektu.

To polecenie instaluje konieczne zestawy, a także dodaje widoków MVC dla stron pomocy (znajdujący się w folderze obszarów/HelpPage). Należy ręcznie dodać link do strony pomocy. Identyfikator URI jest/Help. Aby utworzyć link w widoku razor, Dodaj następujący kod:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Upewnij się również zarejestrować obszarów. W pliku Global.asax, Dodaj następujący kod do **aplikacji\_Start** metody, jeśli go nie zostało to zrobione:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Dodawanie dokumentacji interfejsu API

Domyślnie pomocy strony zawierają symbol zastępczy parametrów dla dokumentacji. Możesz użyć [komentarze dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) do utworzenia w dokumentacji. Aby włączyć tę funkcję, otwórz plik obszarów/HelpPage/aplikacji\_Start/HelpPageConfig.cs i usuń znaczniki komentarza następujący wiersz:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Teraz Włącz dokumentacji XML. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. Wybierz **kompilacji** strony.

![](creating-api-help-pages/_static/image6.png)

W obszarze **dane wyjściowe**, sprawdź **pliku dokumentacji XML**. W polu edycji, wpisz "aplikacji\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Następnie otwórz kod `ValuesController` Kontroler interfejsu API, która jest zdefiniowana w /Controllers/ValuesControler.cs. Dodaj niektóre komentarze dokumentacji do metody kontrolera. Na przykład:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Porada: Jeśli położenie karetki na wiersz powyżej metody i typu trzy ukośniki, program Visual Studio automatycznie wstawia elementy XML. Następnie można wypełnić puste wartości.


Teraz tworzenie i uruchomić ponownie aplikację i przejdź do strony pomocy. Ciągi dokumentacji powinna pojawić się w tabeli interfejsu API.

![](creating-api-help-pages/_static/image8.png)

Strona pomocy odczytuje ciągi z pliku XML w czasie wykonywania. (Podczas wdrażania aplikacji, upewnij się wdrożyć plik XML.)

## <a name="under-the-hood"></a>Kulisy

Strony pomocy są wbudowane w górnej części **ApiExplorer** klasy, która jest częścią struktury Web API. **ApiExplorer** klasy zawiera surowce do tworzenia strony pomocy. Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API. W tym celu "API" jest zdefiniowany jako kombinacja metody HTTP i względnym identyfikatorem URI. Na przykład poniżej przedstawiono niektóre różne interfejsy API:

- Pobierz /api/Products
- Pobierz/interfejs API/produkty / {id}
- OPUBLIKUJ/api/produktów

Jeśli akcja kontrolera obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdą metodę jako odrębne interfejsu API.

Spowoduje ukrycie interfejsu API z poziomu **ApiExplorer**, Dodaj **ApiExplorerSettings** atrybutu akcji i ustaw *IgnoreApi* na wartość true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Ten atrybut można również dodać do kontrolera, które mają zostać wykluczone całego kontrolera.

Klasa ApiExplorer otrzymuje ciągów dokumentacji z **IDocumentationProvider** interfejsu. Jak wcześniej, biblioteka strony pomocy zawiera **IDocumentationProvider** , pobiera z ciągów dokumentacji XML dokumentacji. Kod znajduje się w /Areas/HelpPage/XmlDocumentationProvider.cs. Możesz uzyskać dokumentację z innego źródła, pisania własnych **IDocumentationProvider**. Aby powiązać go, należy wywołać **SetDocumentationProvider** zdefiniowany w metodzie rozszerzenia **HelpPageConfigurationExtensions**

**ApiExplorer** automatycznie wywołuje **IDocumentationProvider** interfejsu można pobrać ciągów dokumentacji dla każdego interfejsu API. Przechowuje je w **dokumentacji** właściwość **ApiDescription** i **ApiParameterDescription** obiektów.

## <a name="next-steps"></a>Następne kroki

Nie są ograniczone do stron pomocy, pokazano poniżej. W rzeczywistości **ApiExplorer** nie jest ograniczony do tworzenia stron pomocy. Połącz Huang Yao zapisane wpisy na kilka wspaniałych blogu pomagające w myśl gotowych:

- [Dodawanie prostego klienta testowego na stronie pomocy interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Tworzenie ASP.NET Web stronie pomocy interfejsu API pracy nad własnym hostowanych usług](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generowanie czasu projektowania strony pomocy (lub klienta) interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Zaawansowane dostosowania strona pomocy](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
