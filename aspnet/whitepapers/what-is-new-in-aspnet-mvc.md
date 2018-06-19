---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Co to jest nowe w programie ASP.NET MVC 2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w programie ASP.NET MVC 2. Ten dokument jest również dostępny do pobrania.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885235"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Co to jest nowe w programie ASP.NET MVC 2
====================
> Ten dokument zawiera opis nowych funkcji i ulepszeń wprowadzonych w programie ASP.NET MVC 2. Ten dokument jest również dostępny do [Pobierz](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Wprowadzenie](#_TOC1)   
[Uaktualnianie do platformy ASP.NET MVC 2 projektu programu ASP.NET MVC w wersji 1.0](#_TOC2)   
[Nowe funkcje](#_TOC3)   
[Pomocników szablonu](#_TOC3_1)   
[Obszary](#_TOC3_2)   
[Obsługa kontrolerów asynchroniczne](#_TOC3_3)   
[Obsługa DefaultValueAttribute parametrów metody akcji](#_TOC3_4)   
[Obsługa powiązania danych binarnych z integratorów modeli](#_TOC3_5)   
[Klasy ModelMetadataProvider i ModelMetadata](#_TOC3_6)   
[Obsługa DataAnnotations atrybutów](#_TOC3_7)   
[Dostawcy modułów weryfikacji modelu](#_TOC3_8)   
[Weryfikacja po stronie klienta](#_TOC3_9)   
[Nowe wstawki kodu programu Visual Studio 2010](#_TOC3_10)   
[Nowy filtr akcji RequireHttpsAttribute](#_TOC3_11)   
[Zastępowanie zlecenie HTTP — metoda](#_TOC3_12)   
[Nowa klasa HiddenInputAttribute dla pomocników szablonu](#_TOC3_13)   
[Metoda pomocnicza Html.ValidationSummary można wyświetlać błędy na poziomie modelu](#_TOC3_14)   
[Szablony T4 w Visual Studio Generuj kod który jest charakterystyczny dla wersji docelowej platformy .NET](#_TOC3_15)[ulepszenia interfejsu API](#_TOC4)  
[Fundamentalne zmiany](#_TOC5)  
[Zrzeczenie odpowiedzialności](#_TOC6)  

## <a id="_TOC1"></a>Wprowadzenie

ASP.NET MVC 2 oparty na programie ASP.NET MVC 1.0 i wprowadza dużego zestawu rozszerzeń i funkcji, które są ukierunkowane na zwiększenie produktywności. Tej wersji jest zgodny z platformy ASP.NET MVC w wersji 1.0, aby kontynuować stosowanie wiedzy, umiejętności, kodu i rozszerzenia dla platformy ASP.NET MVC w wersji 1.0.

Więcej informacji o platformie ASP.NET MVC można znaleźć w następujących zasobach:

- [ASP.NET MVC dokumentacji w witrynie MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Witryny sieci Web programu ASP.NET MVC](https://asp.net/mvc/)
- [Fora programu ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>Uaktualnianie do platformy ASP.NET MVC 2 projektu programu ASP.NET MVC w wersji 1.0

ASP.NET MVC 2 można zainstalować równolegle program ASP.NET MVC w wersji 1.0 na tym samym serwerze, zapewnia elastyczność deweloperzy aplikacji, wybierając podczas uaktualniania aplikacji ASP.NET MVC 2 platformy ASP.NET MVC w wersji 1.0. Aby uzyskać informacje na temat sposobu uaktualnienia, znajdziesz w dokumencie [uaktualniania aplikacji ASP.NET MVC 1.0 ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>Nowe funkcje

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji MVC 2.

### <a id="_TOC3_1"></a>Pomocników szablonu

Pomocników szablonu umożliwiają automatyczne kojarzenie elementów HTML do edycji i wyświetlić przy użyciu typów danych. Na przykład gdy dane typu System.DateTime są wyświetlane w widoku, element selektora daty interfejsu użytkownika można automatycznie renderowane. Jest to podobne do działania Szablony pól w danych dynamicznych platformy ASP.NET. Aby uzyskać więcej informacji, zobacz [przy użyciu pomocników szablonu w celu wyświetlania danych](https://go.microsoft.com/fwlink/?LinkId=159062) w witrynie MSDN.

### <a id="_TOC3_2"></a>Obszary

Obszary umożliwiają organizowanie dużego projektu na mniejsze części wiele, aby można było zarządzać złożoność dużych aplikacji sieci Web. Każda sekcja ("obszaru") zazwyczaj reprezentuje osobnej sekcji dużych witryn sieci Web i służy do grupowania powiązanych zestawów widoków i kontrolerów. Aby uzyskać więcej informacji, zobacz [wskazówki: organizowania aplikacji platformy ASP.NET MVC wg obszarów](https://go.microsoft.com/fwlink/?LinkId=158978) w witrynie MSDN.

Aby utworzyć nowy obszar, w Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy projekt, kliknij przycisk Dodaj, a następnie kliknij przycisk obszaru. Zostaną wyświetlone okno dialogowe zawierające monit o podanie nazwy obszaru. Po wprowadzeniu nazwy obszaru na Visual Studio dodaje nowy obszar do projektu.

Na poniższej ilustracji przedstawiono przykład układu dla projektu z dwoma obszarami, blogów i administratora.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Po utworzeniu obszaru Visual Studio Dodaje klasę, która jest pochodną AreaRegistration do każdego obszaru. Ta klasa jest wymagane do zarejestrowania obszaru i jego tras, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Domyślny szablon projektu programu ASP.NET MVC 2 zawiera wywołanie metody RegisterAllAreas kodu dla pliku Global.asax. Ta metoda rejestruje wszystkie obszary w projekcie wyszukiwania dla wszystkich typów wyprowadzonych z klasy AreaRegistration, instancji typu, a następnie wywołanie metody RegisterArea w wystąpieniu. W poniższym przykładzie pokazano, jak to zrobić.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Jeśli nie określisz przestrzeni nazw w metodzie RegisterArea przez wywołanie metody kontekstu. Metoda Namespaces.Add, nazw klasy rejestracji jest używany domyślnie.

### <a id="_TOC3_3"></a>Obsługa kontrolerów asynchroniczne

ASP.NET MVC 2 umożliwia teraz kontrolery do asynchronicznego przetwarzania żądań. Może to prowadzić do wzrost wydajności dzięki serwerów, które często wywołania operacji blokowania (takie jak żądania sieci) zamiast tego wywołać nieblokujące odpowiedniki. Aby uzyskać więcej informacji, zobacz [na platformie ASP.NET MVC przy użyciu kontrolera asynchronicznego](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) temacie w witrynie MSDN.

### <a id="_TOC3_4"></a>Obsługa DefaultValueAttribute parametrów metody akcji

Klasa System.ComponentModel.DefaultValueAttribute umożliwia wartość domyślna ma być podany w parametrze argument do metody akcji. Załóżmy na przykład, czy jest zdefiniowana Następująca trasa domyślna:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Założono także zdefiniowano następujące metody akcji i kontrolerów:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Jedną z poniższych adresów URL wywoła widoku metody akcji, która jest zdefiniowana w poprzednim przykładzie.

- / Artykuł/widok/123
- / Artykuł/widok/123? strony = 1 (skutecznie taką jak poprzednie żądanie)
- / Artykuł/widok/123? strony = 2

Bez atrybutu DefaultValueAttribute pierwszy adres URL z powyższej listy nie będzie działało, ponieważ argument strony jest typ niedopuszczający wartości null, którego wartość nie została dostarczona.

Jeśli kod został napisany w języku Visual Basic 2010 lub Visual C# 2010, służą następujące parametry opcjonalne zamiast atrybutu DefaultValueAttribute, jak pokazano w poniższym przykładzie:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>Obsługa powiązania danych binarnych z integratorów modeli

Istnieją dwa nowe przeciążenia metody pomocnika Html.Hidden, które kodowanie binarne wartości jako ciągi algorytmem base-64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Typowym zastosowaniem jest osadzanie sygnaturę czasową dla obiekt w widoku. Na przykład aplikacja może zawierać następujący obiekt produktu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Formularz edycji umożliwiający renderowanie właściwość sygnatury czasowej w formularzu, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Ten kod znaczników renderuje ukryty element wejściowy o wartości sygnatury czasowej jako ciąg algorytmem base-64 podobnego do następującego:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Ten formularz może być publikowane do metody akcji, która wymaga argumentu typu produkt, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

W metodzie akcji właściwość sygnatury czasowej jest wypełnić poprawnie, ponieważ oczekujących na opublikowanie algorytmem base-64 ciągu jest konwertowany na tablicę bajtów.

### <a id="_TOC3_6"></a>Klasy ModelMetadataProvider i ModelMetadata

Klasa ModelMetadataProvider udostępnia abstrakcję do uzyskania metadanych dla modelu w widoku. MVC 2 zawiera domyślnego dostawcę, który udostępnia udostępnianym przez atrybutów w przestrzeni nazw System.ComponentModel.DataAnnotations metadanych. Istnieje możliwość tworzenia dostawcy metadanych metadanych od innych magazynów danych, takich jak bazy danych lub pliki XML.

Klasa ViewDataDictionary udostępnia obiekt ModelMetadata zawierający metadane, który został wyodrębniony z modelu przez klasę ModelMetadataProvider. Dzięki temu pomocników szablonu w celu używania tych metadanych i odpowiednio zmienić ich dane wyjściowe.

Aby uzyskać więcej informacji, zobacz dokumentację [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) i [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) klasy.

### <a id="_TOC3_7"></a>Obsługa DataAnnotations atrybutów

Obsługuje platformy ASP.NET MVC 2 przy użyciu atrybutów sprawdzania poprawności RangeAttribute, RequiredAttribute StringLengthAttribute i RegexAttribute (zdefiniowany w przestrzeni nazw System.ComponentModel.DataAnnotations) po powiązaniu do modelu w celu podawanie danych wejściowych Sprawdzanie poprawności.

Aby uzyskać więcej informacji, zobacz [porady: Sprawdzanie poprawności atrybutów przy użyciu DataAnnotations programu modelu danych](https://go.microsoft.com/fwlink/?LinkId=159063) w witrynie MSDN. Przykładowy projekt, który przedstawia użycie tych atrybutów jest dostępny do pobrania na [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>Dostawcy modułów weryfikacji modelu

Klasa dostawcy weryfikacji modelu reprezentuje abstrakcji, która udostępnia logikę weryfikacji dla modelu. Platforma ASP.NET MVC zawiera domyślnego dostawcę na podstawie atrybutów sprawdzania poprawności, które znajdują się w przestrzeni nazw System.ComponentModel.DataAnnotations. Istnieje również możliwość utworzenia własnych dostawców weryfikacji, definiujące niestandardowych reguł walidacji i niestandardowe mapowanie reguł sprawdzania poprawności do modelu. Aby uzyskać więcej informacji, zobacz dokumentację [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) klasy.

### <a id="_TOC3_9"></a>Weryfikacja po stronie klienta

Klasa dostawcy modułów weryfikacji modelu udostępnia metadane sprawdzania poprawności do przeglądarki w formie danych serializacji JSON, które mogą być używane przez bibliotekę weryfikacji po stronie klienta. Platforma ASP.NET MVC 2 zawiera Biblioteka weryfikacji klienta i karty, która obsługuje wcześniej wymienione atrybuty weryfikacji DataAnnotations przestrzeni nazw. Klasa dostawcy umożliwia także korzystać z innych bibliotek weryfikacji klienta przez napisanie karty, który przetwarza dane JSON i wywołuje alternatywnej biblioteki.

### <a id="_TOC3_10"></a>Nowe wstawki kodu programu Visual Studio 2010

Zestaw fragmentów kodu HTML dla platformy ASP.NET MVC 2 jest instalowany z programu Visual Studio 2010. Aby wyświetlić listę tych fragmentów, w menu Narzędzia, wybierz Menedżerze fragmentów kodu. Dla języka wybierz HTML, a dla lokalizacji, wybierz ASP.NET MVC 2. Aby uzyskać więcej informacji o sposobie wstawki kodu za pomocą, zobacz dokumentację programu Visual Studio.

### <a id="_TOC3_11"></a>Nowy filtr akcji RequireHttpsAttribute

Platforma ASP.NET MVC 2 zawiera nową klasę RequireHttpsAttribute, który można zastosować do metody akcji i kontrolerów. Domyślnie filtr przekierowuje żądanie bez użycia protokołu SSL HTTP do jego odpowiednik włączony protokół SSL (HTTPS).

### <a id="_TOC3_12"></a>Zastępowanie zlecenie HTTP — metoda

Podczas tworzenia witryny sieci Web przy użyciu stylu architektury REST, zleceń HTTP są używane do określania akcję do wykonania dla zasobu. REST wymaga obsługujące aplikacje pełnego zakresu typowe zlecenia HTTP, w tym GET PUT, POST i usuwania.

Platforma ASP.NET MVC 2 zawiera nowe atrybuty, które można zastosować do metody akcji i zwięzłą składnią tej funkcji. Te atrybuty Włącz ASP.NET MVC na wybór metody akcji, na podstawie zleceń HTTP. W poniższym przykładzie żądania POST wywoła metodę akcji pierwszy i żądanie PUT wywoła drugiej metody akcji.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

We wcześniejszych wersjach programu ASP.NET MVC te metody akcji wymagane pełniejsze składni, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Ponieważ przeglądarki obsługują tylko zlecenia GET i POST protokołu HTTP, nie jest możliwe księgowanie akcji wymagającej różne zlecenia. W związku z tym nie jest możliwe natywnie obsługuje wszystkie żądania RESTful.

Jednak do obsługi RESTful żądania podczas operacji POST, ASP.NET MVC 2 wprowadzono nowe metody pomocnika HttpMethodOverride HTML. Ta metoda renderuje ukryty element input, który powoduje, że formularz, aby efektywnie emulować dowolnej metody HTTP. Na przykład przy użyciu metody pomocnika HttpMethodOverride HTML, może mieć przesyłania formularza, wyświetlane jest żądanie PUT i DELETE. Zachowanie HttpMethodOverride ma wpływ na następujące atrybuty:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Ukryty element input ma jego nazwę X-HTTP-Method-Override i wartością ustawioną na zlecenie HTTP do emulacji. Wartość przesłaniająca również można określić nagłówka HTTP lub w wartości ciągu zapytania jako pary nazwa/wartość.

Zastąpienie można użyć tylko w przypadku, gdy rzeczywista żądanie jest żądaniem POST. Wartość przesłaniająca będzie ignorowana podczas żądania używające innych zlecenie HTTP.

### <a id="_TOC3_13"></a>Nowa klasa HiddenInputAttribute dla pomocników szablonu

Nowy atrybut HiddenInputAttribute można stosować do właściwości modelu, aby wskazać, czy ukryty element input powinien być renderowany przy wyświetlaniu modelu w szablonie edytora. (Ten atrybut ustawia niejawne wartości UIHint HiddenInput). Wwłaściwość się pozycje DisplayValue pozwala określić, czy wartość jest wyświetlana w edytorze i tryby wyświetlania. Gdy się pozycje DisplayValue jest ustawiona na wartość false, nie będą wyświetlane żadne, nawet jeżeli nie zwykle wokół pola Kod znaczników HTML. Wartość domyślna dla się pozycje DisplayValue ma wartość true.

Atrybut HiddenInputAttribute można użyć w następujących scenariuszach:

- Gdy widok umożliwia użytkownikom edytowanie Identyfikatora obiektu i jest wyświetlić wartość również zapewniający ukryty element input, który zawiera identyfikator starego, dzięki czemu mogą być przekazywane do kontrolera.
- Gdy widok umożliwia użytkownikom Edytuj właściwość binarną, która nigdy nie powinna być wyświetlana takich jak właściwość sygnatury czasowej. W takim przypadku wartość i otaczającego kod znaczników HTML (na przykład etykiety i wartości) nie są wyświetlane.

Poniższy przykład przedstawia sposób użycia klasy HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Jeśli ten atrybut ma wartość true (lub parametr nie jest określony), wystąpią następujące zdarzenia:

- W szablonach wyświetlania etykiety jest renderowany i wartość jest wyświetlana użytkownikowi.
- W szablonach edytora jest renderowany etykiety i wartość jest wyświetlana w ukrytym elemencie wejściowym.

Gdy atrybut jest ustawiony na wartość false, są następujące operacje:

- W szablonach wyświetlania nic nie jest renderowany dla tego pola.
- W szablonach edytora jest renderowany bez etykiety, a wartość jest wyświetlana w ukrytym elemencie wejściowym.

### <a id="_TOC3_14"></a>Metoda pomocnicza Html.ValidationSummary można wyświetlać błędy na poziomie modelu

Zamiast zawsze wyświetlane wszystkie błędy weryfikacji, metoda pomocnika Html.ValidationSummary ma nową opcję, aby wyświetlić tylko błędy na poziomie modelu. Dzięki temu błędy na poziomie modelu ma być wyświetlany w podsumowaniu sprawdzania poprawności i błędy specyficzne dla pola mają być wyświetlane obok każdego pola.

### <a id="_TOC3_15"></a>Szablony T4 w Visual Studio Generuj kod który jest charakterystyczny dla wersji docelowej platformy .NET

Nowe właściwości jest dostępny do pliki T4 hosta platformy ASP.NET MVC T4, który określa wersję programu .NET Framework, która jest używana przez aplikację. Dzięki temu szablony T4 do generowania kodu i kod znaczników, który jest przeznaczony dla wersji platformy .NET. W programie Visual Studio 2008 wartość jest zawsze .NET 3.5. W programie Visual Studio 2010 wartość jest .NET 3.5 lub .NET 4.

## <a id="_TOC4"></a>Ulepszenia interfejsu API

W tej sekcji opisano zmiany w istniejących typów ASP.NET MVC i elementów członkowskich.

- Dodaje chronione metody wirtualnej CreateActionInvoker klasy kontrolera. Ta metoda jest wywoływana przez właściwość ActionInvoker kontrolera i umożliwia opóźnieniem podczas tworzenia wystąpienia elementu wywołującego, jeśli wywołujący nie jest już ustawiony.
- Dodaje chronione metody wirtualnej HandleUnauthorizedRequest w klasie klasy AuthorizeAttribute. Dzięki temu filtry, które pochodzi od klasy AuthorizeAttribute do kontrolowania zachowania, gdy autoryzacja nie powiedzie się.
- W klasie ValueProviderDictionary, należy dodać metody Add (ciąg klucza, wartość obiektu). Dzięki temu można użyć składni inicjatora słownika dla ValueProviderDictionary, jak w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Dodaje get\_obiekt metody w klasie Sys.Mvc.AjaxContext. Jest to metoda JavaScript, która jest podobna do pobierania\_metoda danych, ale jeśli typ zawartości odpowiedzi to application/json, Pobierz\_obiektu zwraca obiekt JSON.
- Dodaje właściwość ActionDescriptor w klasie elementu AuthorizationContext.
- Dodaje token UrlParameter.Optional, który może służyć do obejścia problemów podczas wiązania modelu, który zawiera właściwość ID, gdy właściwość jest nieobecny w post formularza. Aby uzyskać więcej szczegółów, zobacz wpis [ASP.NET MVC 2 adresu URL następujące parametry opcjonalne](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) na blogu Phila Haacka.

## <a id="_TOC5"></a>Fundamentalne zmiany

Następujące zmiany może spowodować błędy w istniejących aplikacji ASP.NET MVC w wersji 1.0.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Zmiana zachowania weryfikacji właściwości dla klas implementujących idataerrorinfo —

Model obiektów, które umożliwia idataerrorinfo — sprawdzania poprawności dla każdej właściwości jest weryfikowane, niezależnie od tego, czy nowa wartość została ustawiona. W programie ASP.NET MVC 1.0 została sprawdzona tylko właściwości, które były nowe wartości. W programie ASP.NET MVC 2 błąd właściwość idataerrorinfo — jest wywoływana tylko wtedy, gdy wszystkie moduły weryfikacji właściwości zostały pomyślnie.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skrypt mapowania skryptów usług IIS nie jest już dostępny w Instalatorze

Skrypt mapowania skryptów usług IIS jest skryptu wiersza polecenia, który jest używany do konfigurowania mapy skryptów dla usług IIS 6 i IIS 7 w trybie klasycznym. Skrypt mapowanie skryptu nie jest potrzebna, jeśli używasz serwera wdrożeniowego programu Visual Studio, albo jeśli używasz usług IIS 7 w trybie zintegrowanym. Skrypty są dostępne jako osobny plik do pobrania nieobsługiwany na [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Metoda pomocnika Html.Substitute prognoz MVC nie jest już dostępny

Z powodu zmian w sposób renderowania aparatów widoku MVC metoda pomocnika Html.Substitute nie działa i został usunięty.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Interfejs IValueProvider zastępuje wszelkie zastosowania IDictionary

Każdy argument właściwości lub metody obecnie akceptowane IDictionary w wersji 1.0 MVC akceptuje IValueProvider. Ta zmiana wpływa tylko aplikacje, które obejmują dostawców wartości niestandardowych lub integratorów modeli niestandardowych. Przykłady właściwości i metody, których dotyczy ta zmiana są następujące:

- Właściwość ValueProvider klasy ControllerBase i ModelBindingContext.
- Metody TryUpdateModel klasy kontrolera.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Dodano nowe klasy CSS w pliku Site.css

Pliku Site.css w szablonach projektu programu ASP.NET MVC została zaktualizowana w celu uwzględnienia nowych stylów używanych przez funkcję sprawdzania poprawności i pomocników szablonu.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Pomocnicy teraz zwracać obiekt MvcHtmlString

Aby można było korzystać z nowej składni wyrażenia kodowanie HTML w ASP.NET 4, typ zwracany przez pomocników HTML jest teraz MvcHtmlString zamiast ciągu. Jeśli używasz programu ASP.NET MVC 2 i nowych pomocników na programie ASP.NET 3.5, nie będzie wykorzystywać kodowanie HTML składni; nowej składni jest dostępna tylko po uruchomieniu programu ASP.NET MVC 2 platformy ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult teraz odpowiada tylko na żądania HTTP POST

Aby ograniczyć JSON przejmowanie, które mają potencjalnie ujawnienie informacji, domyślnie klasy JsonResult teraz odpowiada tylko na żądania HTTP POST. Pobierz AJAX wywołań do metod akcji, które zwracają obiekt JsonResult, należy zmienić zamiast tego użyć POST. W razie potrzeby można zmienić to zachowanie, ustawiając właściwość JsonRequestBehavior nowego elementu JsonResult. Aby uzyskać więcej informacji na temat potencjalnych wykorzystać Zobacz we wpisie blogu [przejmującą JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) na blogu Phila Haacka.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model i metody ustawiające właściwości ModelType na ModelBindingContext są nieaktualne

Nowe można ustawić właściwość ModelMetadata dodano do klasy ModelBindingContext. Nowe właściwości hermetyzuje zarówno właściwości ModelType, jak i modelu. Mimo że właściwości modelu i ModelType są nieaktualne, zgodności ze starszymi wersjami pobierających właściwości nadal działa; Właściwość ModelMetadata one przekazać do pobierania wartości.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Zmiany w klasie DefaultControllerFactory Podziel fabryki kontrolerów niestandardowych, pochodzących z niego

Klasa DefaultControllerFactory został rozwiązany przez usunięcie właściwości RequestContext. Zamiast tego właściwości wystąpienia kontekstu żądania są przekazywane do chronionego GetControllerInstance i GetControllerType metody wirtualne. Ta zmiana wpływa fabryki kontrolerów niestandardowych, które pochodzą z DefaultControllerFactory.

Fabryki kontrolerów niestandardowe są często używane do zapewnienia iniekcji zależności dla platformy ASP.NET MVC aplikacji. Aby zaktualizować fabryki kontrolera niestandardowego do obsługi platformy ASP.NET MVC 2, Zmień podpis metody lub podpisów odpowiadające nowe sygnatury, a parametr zamiast właściwości kontekstu żądania.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Obszaru" jest teraz klucz zarezerwowanej wartości trasy

Ciąg "obszaru" w wartości trasy teraz ma specjalne znaczenie w programie ASP.NET MVC w taki sam sposób jak "controller" i "Akcja". Jeden możliwa jest, że jeśli pomocników HTML są dostarczane z zawierający "obszaru" słownika wartości trasy, pomocników już dołączy "obszaru" w ciągu zapytania.

Jeśli używasz funkcji obszarów, upewnij się nie używać {obszaru} jako część adresu URL trasy.


## <a id="_TOC6"></a>Zrzeczenie odpowiedzialności

To jest dokument wstępny i może znacznie zmienione przed końcowej wersji oprogramowania opisanych tutaj.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok elementu firmy Microsoft Corporation na zagadnień omówionych w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft i firma Microsoft nie może zagwarantować dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH LUB USTAWOWYCH, ODNOŚNIE DO INFORMACJI W TYM DOKUMENCIE.

Zgodne z wszystkich stosownych praw autorskich jest odpowiedzialny za użytkownika. Bez ograniczenia praw autorskich, żadnej części tego dokumentu może być odtworzyć, przechowywane w wprowadzane do systemu pobierania lub przekazywać w jakimkolwiek formularzu za pomocą jakichkolwiek metod (elektronicznych, mechanicznych, postaci kopii, nagrań lub innych) w jakimkolwiek celu, bez pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może mieć patenty, patentowe, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej będących przedmiotem tego dokumentu. Z wyjątkiem wyraźnie określonych w umowach licencyjnych firmy Microsoft, otrzymanie tego dokumentu nie oznacza udzielenia licencji na te patenty, znaki towarowe, prawa autorskie lub inne prawa własności intelektualnej.

Jeśli nie podano inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia opisywane w przykładach są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, poczty e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami towarowymi ich prawnych właścicieli.
