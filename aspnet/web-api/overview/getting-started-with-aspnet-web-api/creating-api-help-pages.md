---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Podczas tworzenia składnika web API często jest przydatne do tworzenia strony pomocy, aby inni deweloperzy będą wiedzieli, sposób wywołania interfejsu API. Można utworzyć cała dokumentacja ręcznie, ale zaleca się generowanie możliwie.

Aby ułatwić to zadanie, Web API platformy ASP.NET udostępnia bibliotekę do automatycznego generowania strony pomocy w czasie wykonywania.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Tworzenie stron pomocy interfejsu API

Zainstaluj [ASP.NET i sieć Web narzędzi aktualizacji 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja integruje strony pomocy szablonu projektu interfejsu API sieci Web.

Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web. Szablon projektu tworzy kontroler przykładowy interfejs API o nazwie `ValuesController`. Szablon tworzy także interfejsu API strony pomocy. Wszystkie pliki kodu na stronie pomocy są umieszczane w folderze obszarów projektu.

![](creating-api-help-pages/_static/image2.png)

Po uruchomieniu aplikacji, strony głównej zawiera łącze do strony pomocy interfejsu API. Na stronie głównej ścieżka względna jest/Help.

![](creating-api-help-pages/_static/image3.png)

To łącze powoduje przeniesienie na stronie Podsumowanie interfejsu API.

![](creating-api-help-pages/_static/image4.png)

Widok MVC dla tej strony jest zdefiniowany w Areas/HelpPage/Views/Help/Index.cshtml. Możesz edytować tę stronę, aby zmodyfikować układ, wprowadzenie, title, style i tak dalej.

Główna część strony znajduje się tabela interfejsów API, które są pogrupowane według kontrolera. Wpisy tabeli są generowane dynamicznie, przy użyciu **IApiExplorer** interfejsu. (I będzie komunikował więcej informacji na temat tego interfejsu później.) Po dodaniu nowego kontrolera interfejsu API tabeli jest automatycznie aktualizowany w czasie wykonywania.

Kolumna "Interfejsu API" zawiera metody HTTP oraz względnego identyfikatora URI. Kolumna "Opis" zawiera dokumentacja dla każdego interfejsu API. Dokumentacja jest początkowo tylko tekst zastępczy. W następnej sekcji I opisano sposób dodawania dokumentacji z komentarze XML.

Każdy interfejs API zawiera łącze do strony z bardziej szczegółowe informacje, w tym przykładzie treści żądania i odpowiedzi.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Dodawanie strony pomocy do istniejącego projektu

Można dodać strony pomocy do istniejącego projektu interfejsu API sieci Web za pomocą Menedżera pakietów NuGet. Ta opcja jest przydatna w przypadku uruchomienia szablonu projektu innego niż szablon "Interfejsu API sieci Web".

Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okna, wpisz jedno z następujących poleceń:

Aby uzyskać **C#** aplikacji:`Install-Package Microsoft.AspNet.WebApi.HelpPage`

Aby uzyskać **Visual Basic** aplikacji:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Istnieją dwa pakiety, dla C# i jeden dla języka Visual Basic. Upewnij się użyć zgodną z projektu.

To polecenie instaluje konieczne zestawy i dodaje widoków MVC dla strony pomocy (znajdujące się w folderze obszarów/HelpPage). Należy ręcznie dodać łącze do strony pomocy. Identyfikator URI jest/Help. Aby utworzyć łącze w widoku razor, Dodaj następujące informacje:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Upewnij się również zarejestrować obszarów. W pliku Global.asax, Dodaj następujący kod do **aplikacji\_Start** metody, jeśli nie jest już:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Dodawanie dokumentacji interfejsu API

Domyślnie pomocy strony mają ciągów symbolu zastępczego dla dokumentacji. Można użyć [komentarze dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) można utworzyć w dokumentacji. Aby włączyć tę funkcję, otwórz plik obszarów/HelpPage/aplikacji\_Start/HelpPageConfig.cs i Usuń komentarz następujący wiersz:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Teraz Włącz dokumentacji XML. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. Wybierz **kompilacji** strony.

![](creating-api-help-pages/_static/image6.png)

W obszarze **dane wyjściowe**, sprawdź **pliku dokumentacji XML**. W polu edycji wpisz "App\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Następnie otwórz folder kod `ValuesController` kontrolera interfejsu API, który jest zdefiniowany w /Controllers/ValuesControler.cs. Dodaj niektórych komentarzy do dokumentacji do metody kontrolera. Na przykład:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Porada: Jeśli pozycji karetki w wierszu powyżej metody i typu trzy ukośniki, Visual Studio automatycznie wstawia elementy XML. Można następnie Wypełnij puste wartości.


Teraz utworzyć i uruchomić ponownie aplikację i przejdź do strony pomocy. Ciągi dokumentacji powinny być wyświetlane w tabeli interfejsu API.

![](creating-api-help-pages/_static/image8.png)

Strona pomocy odczytuje ciągi z pliku XML w czasie wykonywania. (Podczas wdrażania aplikacji, upewnij się, że wdrażanie pliku XML.)

## <a name="under-the-hood"></a>Kulisy

Strony pomocy są wbudowane nad **ApiExplorer** klasy, która jest częścią struktury interfejsu API sieci Web. **ApiExplorer** klasy zawiera materiały raw do tworzenia strony pomocy. Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API. W tym celu "interfejsu API" jest zdefiniowany jako kombinacja metody HTTP i względnym identyfikatorem URI. Na przykład poniżej przedstawiono niektóre różne funkcje interfejsu API:

- Pobierz /api/Products
- Pobierz /api/produkty / {id}
- POST/api/produktów

Jeśli akcja kontroler obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdej metody jako odrębne interfejsu API.

Aby ukryć interfejsu API z **ApiExplorer**, Dodaj **ApiExplorerSettings** atrybutu akcji i zestawu *IgnoreApi* na wartość true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Można również dodać ten atrybut z kontrolerem, aby wykluczyć całego kontrolera.

Klasa ApiExplorer pobiera ciągi dokumentacji **IDocumentationProvider** interfejsu. Jak przedstawiono wcześniej, biblioteka strony pomocy zawiera **IDocumentationProvider** który pobiera dokumentację z ciągów dokumentacji XML. Kod znajduje się w /Areas/HelpPage/XmlDocumentationProvider.cs. Można uzyskać dokumentację z innego źródła pisanie własnych **IDocumentationProvider**. Aby połączenie go w górę, należy wywołać **SetDocumentationProvider** zdefiniowany w metodzie rozszerzenia **HelpPageConfigurationExtensions**

**ApiExplorer** automatycznie wywołuje **IDocumentationProvider** interfejsu można pobrać ciągów dokumentacji dla każdego interfejsu API. Przechowuje je w **dokumentacji** właściwość **ApiDescription** i **ApiParameterDescription** obiektów.

## <a name="next-steps"></a>Następne kroki

Nie są ograniczone do strony pomocy pokazano poniżej. W rzeczywistości **ApiExplorer** nie jest ograniczona do tworzenia strony pomocy. Łącz Huang Yao został zapisany się, że niektóre dużą blogu ogłoszenia ułatwiających myśląc fabrycznej:

- [Dodawanie prostego klienta testowego do strona pomocy interfejsu API sieci Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Tworzenie stronę sieci Web ASP.NET interfejsu API pomocy pracować nad własnym obsługiwane usługi](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generowanie czasu projektowania strony pomocy (lub klienta) interfejsu API sieci Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Zaawansowane dostosowania strona pomocy](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
