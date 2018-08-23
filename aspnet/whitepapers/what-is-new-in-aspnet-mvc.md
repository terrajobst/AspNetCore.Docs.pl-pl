---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Co nowego we wzorcu ASP.NET MVC 2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzone w programie ASP.NET MVC 2. Ten dokument jest również dostępny do pobrania.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 82a3fd4fe74202ed9a23298390322458cfc029f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752083"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Co nowego we wzorcu ASP.NET MVC 2
====================
> W tym dokumencie opisano nowe funkcje i ulepszenia wprowadzone w programie ASP.NET MVC 2. Ten dokument jest również dostępna dla [pobierania](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Wprowadzenie](#_TOC1)   
[Uaktualnianie projektu ASP.NET MVC 1.0 do ASP.NET MVC 2](#_TOC2)   
[Nowe funkcje](#_TOC3)   
[Pomocników szablonu](#_TOC3_1)   
[Obszary](#_TOC3_2)   
[Wsparcie dla asynchronicznego kontrolerów](#_TOC3_3)   
[Obsługa DefaultValueAttribute — parametry metody akcji](#_TOC3_4)   
[Obsługa powiązań danych binarnych z Integratorami modeli](#_TOC3_5)   
[Klasy ModelMetadataProvider i ModelMetadata](#_TOC3_6)   
[Obsługa atrybutów DataAnnotations](#_TOC3_7)   
[Dostawcy modułów weryfikacji modelu](#_TOC3_8)   
[Weryfikacja po stronie klienta](#_TOC3_9)   
[Nowe wstawki kodu programu Visual Studio 2010](#_TOC3_10)   
[Nowy filtr akcji RequireHttpsAttribute](#_TOC3_11)   
[Zastępowanie zlecenie HTTP, metoda](#_TOC3_12)   
[Nowa klasa HiddenInputAttribute dla pomocników szablonu](#_TOC3_13)   
[Metoda pomocnika Html.ValidationSummary może wyświetlać błędy na poziomie modelu](#_TOC3_14)   
[Szablony T4 w programie Visual Studio generować Code, który jest charakterystyczny dla wersji docelowej programu .NET Framework](#_TOC3_15)[ulepszeń interfejsu API](#_TOC4)  
[Fundamentalne zmiany](#_TOC5)  
[Zrzeczenie odpowiedzialności](#_TOC6)  

## <a id="_TOC1"></a>  Wprowadzenie

ASP.NET MVC 2 opiera się na programie ASP.NET MVC 1.0 i wprowadzono dużego zestawu funkcji, które koncentrują się na zwiększenie produktywności i rozszerzeń. Ta wersja jest zgodna z programu ASP.NET MVC 1.0, dzięki wiedzy, umiejętności, kodu i rozszerzenia dla programu ASP.NET MVC 1.0 będą dotyczyć.

Aby uzyskać więcej informacji na temat platformy ASP.NET MVC odwiedź następujące zasoby:

- [Dokumentacja platformy ASP.NET MVC w witrynie MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Witryny sieci Web programu ASP.NET MVC](https://asp.net/mvc/)
- [Fora ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  Uaktualnianie projektu ASP.NET MVC 1.0 do ASP.NET MVC 2

ASP.NET MVC 2 można zainstalować równolegle z programem ASP.NET MVC 1.0 na tym samym serwerze, który zapewnia elastyczność deweloperom aplikacji w wyborze kiedy aktualizować aplikację ASP.NET MVC 1.0 do ASP.NET MVC 2. Aby uzyskać informacje na temat uaktualniania, zobacz dokument [uaktualniania aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Nowe funkcje

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji 2 do MVC.

### <a id="_TOC3_1"></a>  Pomocników szablonu

Pomocników szablonu pozwalają na automatyczne kojarzenie elementów HTML do edycji i wyświetlania z typami danych. Na przykład gdy dane typu System.DateTime jest wyświetlana w widoku, element selektora daty interfejsu użytkownika mogą być automatycznie renderowane. Jest to podobne do działania szablonów pól danych dynamicznych platformy ASP.NET. Aby uzyskać więcej informacji, zobacz [przy użyciu pomocników szablonu w celu wyświetlania danych](https://go.microsoft.com/fwlink/?LinkId=159062) w witrynie MSDN w sieci Web.

### <a id="_TOC3_2"></a>  Obszary

Obszary umożliwiają organizowanie dużego projektu na wiele mniejszych sekcje w celu zarządzania złożonością dużych aplikacji sieci Web. Każda sekcja ("obszar") zazwyczaj reprezentuje osobnej sekcji dużych witryn sieci Web i służy do grupowania powiązane zestawy widoków i kontrolerów. Aby uzyskać więcej informacji, zobacz [Instruktaż: organizowanie aplikacji ASP.NET MVC wg obszarów](https://go.microsoft.com/fwlink/?LinkId=158978) w witrynie MSDN w sieci Web.

Aby utworzyć nowy obszar w Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy projekt, kliknij przycisk Dodaj, a następnie kliknij obszar. Zostaną wyświetlone okno dialogowe, które wyświetli monit o podanie nazwy obszaru. Po wprowadzeniu nazwy obszaru, Visual Studio dodaje nowy obszar do projektu.

Na poniższej ilustracji przedstawiono przykładowy układ strony dla projektu za pomocą dwóch obszarach: administratora i blogi sieci Web.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Po utworzeniu obszaru, Visual Studio Dodaje klasę, która jest pochodną AreaRegistration do każdego obszaru. Ta klasa jest wymagane do zarejestrowania obszaru i jego trasy, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Domyślny szablon projektu dla platformy ASP.NET MVC 2 zawiera wywołanie metody RegisterAllAreas w kodzie, w pliku Global.asax. Ta metoda rejestruje każdy obszar w projekcie wyszukiwania dla wszystkich typów wyprowadzonych z klasy AreaRegistration, instancji tego typu, a następnie wywołania metody RegisterArea na wystąpienie. Poniższy przykład pokazuje, jak to zrobić.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Jeśli nie określisz przestrzeni nazw w metodzie RegisterArea przez wywołanie metody kontekstu. Metoda Namespaces.Add, przestrzeń nazw, klasy rejestracji jest używany domyślnie.

### <a id="_TOC3_3"></a>  Wsparcie dla asynchronicznego kontrolerów

ASP.NET MVC 2 umożliwia teraz kontrolery do asynchronicznego przetwarzania żądań. Może to prowadzić do wzrost wydajności, umożliwiając serwerów, które często wymagają blokowania operacje (takie jak żądania sieci) zamiast tego wywołać nieblokującej na poziomie odpowiedniki. Aby uzyskać więcej informacji, zobacz [we wzorcu ASP.NET MVC przy użyciu kontrolera asynchronicznego](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) tematu w witrynie MSDN.

### <a id="_TOC3_4"></a>  Obsługa DefaultValueAttribute — parametry metody akcji

Klasa System.ComponentModel.DefaultValueAttribute umożliwia wartość domyślną, należy podać argument parametru do metody akcji. Na przykład załóżmy, że Następująca trasa domyślna jest zdefiniowany:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Również założono zdefiniowano następującą metodę kontrolerów i akcji:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Znajdujących się w poniższych dokumentach adresy URL wywoła Wyświetl metody akcji, która jest zdefiniowana w poprzednim przykładzie.

- / Artykuł/widok/123
- / Artykuł/widok/123? strony = 1 (skutecznie taka sama jak poprzednie żądanie)
- / Artykuł/widok/123? strony = 2

Bez atrybutu DefaultValueAttribute — pierwszy adres URL z powyższej listy nie będzie działać, ponieważ argumentu strony jest typem wartości niedopuszczającym wartości, którego wartość nie została podana.

Jeśli kod został napisany w języku Visual Basic 2010 lub Visual C# 2010, można użyć parametrów opcjonalnych zamiast atrybutu DefaultValueAttribute — jak pokazano w poniższym przykładzie:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Obsługa powiązań danych binarnych z Integratorami modeli

Istnieją dwa nowe przeciążenia metody pomocnika Html.Hidden, kodowanie binarne wartości jako ciągi algorytmem base-64, które:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Typowym zastosowaniem jest do osadzenia sygnaturę czasową dla obiektu w widoku. Na przykład aplikacja może zawierać następujący obiekt produktu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Formularz edycji można renderować właściwość sygnatury czasowej w postaci, jak pokazano w poniższym przykładzie:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Ten kod znaczników renderuje ukryty element input przy użyciu wartość znacznika czasu jako ciąg algorytmem base-64, który przypomina poniższy przykład:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Ten formularz może być opublikowane do metody akcji, która ma argument typu produktu, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

W metodzie akcji właściwość sygnatury czasowej jest wypełniony poprawnie, ponieważ przesłanych ciągu algorytmem base-64 jest konwertowana na tablicę bajtów.

### <a id="_TOC3_6"></a>  Klasy ModelMetadataProvider i ModelMetadata

Klasa ModelMetadataProvider udostępnia abstrakcję do uzyskania metadanych dla modelu w widoku. MVC 2 zawiera domyślnego dostawcę, który udostępnia metadane, które jest uwidaczniany przez atrybuty w przestrzeni nazw System.ComponentModel.DataAnnotations. Istnieje możliwość tworzenia dostawcy metadanych, zapewniających metadanych z innych magazynów danych, takich jak bazy danych lub pliki XML.

Klasa ViewDataDictionary udostępnia obiekt ModelMetadata, który zawiera metadane, który został wyodrębniony z modelu przez klasę ModelMetadataProvider. Dzięki temu pomocników szablonu w celu umożliwienia użycia tych metadanych i odpowiednio dostosować swoje dane wyjściowe.

Aby uzyskać więcej informacji, zobacz dokumentację dla [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) i [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) klasy.

### <a id="_TOC3_7"></a>  Obsługa atrybutów DataAnnotations

ASP.NET MVC 2 obsługuje przy użyciu atrybutów sprawdzania poprawności RangeAttribute, RequiredAttribute, StringLengthAttribute i RegexAttribute (zdefiniowany w przestrzeni nazw System.ComponentModel.DataAnnotations) po powiązaniu do modelu w celu zapewnienia danych wejściowych Sprawdzanie poprawności.

Aby uzyskać więcej informacji, zobacz [porady: Sprawdzanie poprawności modelu danych Using DataAnnotations Attributes](https://go.microsoft.com/fwlink/?LinkId=159063) w witrynie MSDN w sieci Web. Przykładowy projekt, który ilustruje sposób używania tych atrybutów jest dostępna do pobrania na [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Dostawcy modułów weryfikacji modelu

Klasa dostawcy weryfikacji modelu reprezentuje klasą abstrakcyjną, która udostępnia logikę weryfikacji dla modelu. Platforma ASP.NET MVC zawiera domyślnego dostawcę, w oparciu o atrybuty weryfikacji, które znajdują się w przestrzeni nazw System.ComponentModel.DataAnnotations. Można również utworzyć własne dostawców weryfikacji, definiujące niestandardowe reguły sprawdzania poprawności i niestandardowe mapowania reguł sprawdzania poprawności do modelu. Aby uzyskać więcej informacji, zobacz dokumentację dla [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) klasy.

### <a id="_TOC3_9"></a>  Weryfikacja po stronie klienta

Klasa dostawcy modułów weryfikacji modelu udostępnia metadane sprawdzania poprawności w przeglądarce w postaci danych serializacji JSON, które mogą być używane przez bibliotekę sprawdzania poprawności po stronie klienta. ASP.NET MVC 2 zawiera bibliotekę klienta usługi weryfikacji oraz karta, która obsługuje atrybutów sprawdzania poprawności nazw DataAnnotations zanotowanej wcześniej. Klasa dostawcy umożliwia także korzystać z innych bibliotek weryfikacji klienta przez napisanie karty, która przetwarza dane JSON i wywołuje alternatywnej biblioteki.

### <a id="_TOC3_10"></a>  Nowe wstawki kodu programu Visual Studio 2010

Zbiór fragmentów kodu HTML dla platformy ASP.NET MVC 2 zainstalowano za pomocą programu Visual Studio 2010. Aby wyświetlić listę tych fragmentów, w menu Narzędzia, wybierz Menedżera wstawek kodu. Dla języka wybierz HTML i lokalizację, ASP.NET MVC 2. Aby uzyskać więcej informacji o sposobie używania fragmenty kodu zobacz dokumentację programu Visual Studio.

### <a id="_TOC3_11"></a>  Nowy filtr akcji RequireHttpsAttribute

Platforma ASP.NET MVC 2 zawiera nową klasę RequireHttpsAttribute, które mogą być stosowane do metody akcji i kontrolerów. Domyślnie filtr przekierowuje żądania bez użycia protokołu SSL HTTP do równowartości włączony protokół SSL (HTTPS).

### <a id="_TOC3_12"></a>  Zastępowanie zlecenie HTTP, metoda

Gdy tworzysz witrynę sieci Web przy użyciu stylu architektury REST zleceń HTTP są używane do określenia akcję do wykonania dla zasobu. REST wymaga, które obsługują aplikacje pełną gamę typowe zlecenia HTTP, w tym GET PUT, POST i usuwania.

Platforma ASP.NET MVC 2 zawiera nowe atrybuty, które można zastosować do metody akcji i zwarta Składnia tej funkcji. Te atrybuty Włącz platformy ASP.NET MVC na wybór metody akcji, oparte na zlecenie HTTP. W poniższym przykładzie żądanie POST wywoła pierwszej metody akcji i żądanie PUT wywoła drugiej metody akcji.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

We wcześniejszych wersjach programu ASP.NET MVC te metody akcji wymagany bardziej szczegółowy składni, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Ponieważ przeglądarki obsługują tylko zlecenia GET i POST protokołu HTTP, nie jest możliwe do wysłania do akcji, które wymagają różnych zlecenie. Dlatego nie jest możliwe, pod kątem natywnej obsługi wszystkich żądań RESTful.

Jednak do obsługi zgodne ze specyfikacją REST żądania POST operacji, ASP.NET MVC 2 wprowadzono nowe metody pomocnika HttpMethodOverride HTML. Ta metoda renderuje ukryty element input, który powoduje, że formularz, aby skutecznie emulować dowolną metodę HTTP. Na przykład przy użyciu metody pomocnika HttpMethodOverride HTML, może mieć przesyłania formularza, pojawiają się być żądania PUT lub DELETE. Zachowanie HttpMethodOverride ma wpływ na następujące atrybuty:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Ukryty element input ma nazwy X-HTTP-Method-Override i jego wartość ustawioną czasownik HTTP w celu emulacji. Jako wartość zastąpienia również można określić w nagłówku HTTP lub w wartości ciągu zapytania jako pary nazwa/wartość.

Zastąpienia należy używać tylko w przypadku rzeczywistych żądanie jest żądaniem POST. Jako wartość zastąpienia zostaną zignorowane dla żądania, które używają innych zlecenie HTTP.

### <a id="_TOC3_13"></a>  Nowa klasa HiddenInputAttribute dla pomocników szablonu

Nowy atrybut HiddenInputAttribute można zastosować do właściwości modelu, aby wskazać, czy ukryty element input powinien być renderowany przy wyświetlaniu modelu w szablonie edytora. (Ten atrybut ustawia niejawne wartości UIHint HiddenInput). Ten atrybut się pozycje DisplayValue właściwości pozwala określić, czy wartość jest wyświetlana w edytorze i tryby wyświetlania. Gdy się pozycje DisplayValue jest ustawiony na wartość false, nic nie jest wyświetlane, nie nawet kod znaczników HTML, zwykle wokół pola. Wartością domyślną dla się pozycje DisplayValue jest true.

Można użyć atrybutu HiddenInputAttribute w następujących scenariuszach:

- Gdy widok umożliwia użytkownikom edytowanie Identyfikatora obiektu i jest wyświetlić wartość, jak również zapewniający ukryty element input, który zawiera starą nazwę, dlatego mogą być przekazywane do kontrolera.
- Gdy widok umożliwia użytkownikom Edytuj właściwość binarną, która nigdy nie powinna być wyświetlana takie jak właściwości sygnatury czasowej. W takim przypadku wartość i otaczającego znacznik HTML (takie jak etykieta i wartość) nie są wyświetlane.

Poniższy przykład przedstawia sposób użycia klasy HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Gdy atrybut jest ustawiony na wartość true (lub parametr nie jest określony), zostaną wykonane następujące zadania:

- W szablonach wyświetlania etykiety jest renderowany, a wartość jest wyświetlana użytkownikowi.
- W szablonach edytora jest renderowany etykiety i wartość jest wyświetlana w ukrytym elemencie wejściowym.

Jeśli ten atrybut jest równa false, zostaną wykonane następujące zadania:

- W szablonach wyświetlania nic nie jest renderowany dla tego pola.
- W szablonach edytora jest renderowany bez etykiety i wartość jest wyświetlana w ukrytym elemencie wejściowym.

### <a id="_TOC3_14"></a>  Metoda pomocnika Html.ValidationSummary może wyświetlać błędy na poziomie modelu

Zamiast zawsze wyświetla wszystkie błędy sprawdzania poprawności, metoda pomocnika Html.ValidationSummary ma nową opcję, aby wyświetlić tylko błędy na poziomie modelu. Umożliwia to błędy na poziomie modelu mają być wyświetlane w podsumowania weryfikacji i błędach pola mają być wyświetlane obok każdego pola.

### <a id="_TOC3_15"></a>  Szablony T4 w programie Visual Studio generować Code, który jest charakterystyczny dla wersji docelowej programu .NET Framework

Nowa właściwość jest dostępna w plikach T4 hosta ASP.NET MVC T4, który określa wersję programu .NET Framework, która jest używana przez aplikację. Dzięki temu szablony T4 do generowania kodu i znaczników, które są specyficzne dla wersji programu .NET Framework. W programie Visual Studio 2008 wartość jest zawsze .NET 3.5. W programie Visual Studio 2010 wartość jest .NET 3.5 lub .NET 4.

## <a id="_TOC4"></a>  Ulepszenia interfejsu API

W tej sekcji opisano zmiany w istniejących typów platformy ASP.NET MVC i elementów członkowskich.

- Dodano chroniony metodę wirtualną CreateActionInvoker w klasy kontrolera. Ta metoda jest wywoływana przez właściwość ActionInvoker kontrolera i umożliwia z opóźnieniem podczas tworzenia wystąpienia elementu wywołującego, jeśli wywołujący nie jest już ustawiona.
- Dodano chroniony metodę wirtualną HandleUnauthorizedRequest w klasie klasy AuthorizeAttribute. Dzięki temu filtry, które pochodzą z klasy AuthorizeAttribute do sterowania zachowaniem, gdy autoryzacja nie powiedzie się.
- W klasie ValueProviderDictionary, należy dodać metodę Add (wartość obiektu ciągu klucza). Dzięki temu przy użyciu składni inicjatora słownika dla ValueProviderDictionary, jak w poniższym przykładzie:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Dodano get\_metody w klasie Sys.Mvc.AjaxContext obiektu. Jest to metoda JavaScript, która jest podobna do pobierania\_metodę danych, ale jeśli typ zawartości odpowiedzi to application/json, Pobierz\_zwraca obiekt JSON.
- Dodano właściwość ActionDescriptor w klasie autoryzacji.
- Dodaje token UrlParameter.Optional, który może służyć do obejścia problemów podczas tworzenia wiązania do modelu, który zawiera właściwość ID, gdy właściwość jest nieobecny w post formularza. Aby uzyskać więcej szczegółów, zobacz wpis [platformy ASP.NET MVC 2 opcjonalne parametry adresu URL](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) na phila haacka.

## <a id="_TOC5"></a>  Fundamentalne zmiany

Następujące zmiany mogą spowodować błędy w istniejących aplikacji programu ASP.NET MVC 1.0.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Zmiana zachowania poprawności właściwości dla klasy, które implementują IDataErrorInfo

Dla modelu obiektów, które IDataErrorInfo do wykonywania sprawdzania poprawności dla każdej właściwości jest weryfikowana, niezależnie od tego, czy ustawiono nową wartość. W programie ASP.NET MVC 1.0 zostały zatwierdzone, tylko te właściwości, które ma nowej wartości. W programie ASP.NET MVC 2 właściwości błąd IDataErrorInfo jest wywoływana tylko wtedy, gdy wszystkie moduły weryfikacji właściwości zakończyły się pomyślnie.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skrypt mapowania skryptów usług IIS nie jest już dostępny w Instalatorze

Skrypt mapowania skryptów usług IIS to skrypt wiersza polecenia, który jest używany do konfigurowania mapy skryptów dla usług IIS 6 i IIS 7 w trybie klasycznym. Skrypt mapowanie skryptu nie jest wymagany, jeśli używasz serwera wdrożeniowego programu Visual Studio, lub jeśli używasz usług IIS 7 w trybie zintegrowanym. Skrypty są dostępne do pobrania osobno nieobsługiwany na [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Metoda pomocnika Html.Substitute prognoz MVC nie jest już dostępny

Ze względu na zmiany w zachowaniu renderowania aparatów widoków MVC metoda pomocnika Html.Substitute nie działa i został usunięty.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Interfejs IValueProvider zastępuje wszystkie przypadki użycia IDictionary

Każdy argument właściwości lub metody teraz zaakceptowana IDictionary w MVC 1.0 akceptuje IValueProvider. Ta zmiana dotyczy tylko aplikacji, które zawierają dostawców wartości niestandardowe lub niestandardowe integratorów modeli. Następujące przykłady właściwości i metod, których dotyczy ta zmiana:

- Właściwość ValueProvider klasy ControllerBase i ModelBindingContext.
- Metody TryUpdateModel klasy kontrolera.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Dodano nowe klasy CSS w pliku Site.css

Pliku Site.css w szablonach projektu ASP.NET MVC zawiera zaktualizowany i zawiera nowe style używane przez funkcje walidacji i przez pomocników szablonu.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Pomocnicy teraz zwracają obiekt MvcHtmlString

Aby móc korzystać z nowej składni wyrażenia kodowanie HTML w ASP.NET 4, typ zwracany dla pomocników HTML jest teraz MvcHtmlString zamiast ciągu. Jeśli używasz platformy ASP.NET MVC 2 i nowych wątków na programie ASP.NET 3.5, nie będzie wykorzystywać kodowanie HTML składni; Nowa składnia jest dostępna tylko po uruchomieniu programu ASP.NET MVC 2 na platformie ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult teraz odpowiada tylko na żądania HTTP POST

Aby można było rozwiązać JSON, które mają ryzyko ujawnienia informacji domyślnie przejmowanie klasy JsonResult teraz odpowiada tylko na żądania HTTP POST. Pobierz AJAX wywołania do metod akcji, które zwracają obiekt JsonResult, należy je zmienić Użyj zamiast tego wpisu. W razie potrzeby można zmienić to zachowanie przez ustawienie nowej właściwości JsonRequestBehavior JsonResult. Aby uzyskać więcej informacji na temat potencjalne luki, zobacz wpis w blogu [przejmującą adresy JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) na phila haacka.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model i metod ustawiających właściwości ModelType na ModelBindingContext są nieaktualne

Nowe, można ustawić właściwość ModelMetadata została dodana do klasy ModelBindingContext. Nowa właściwość hermetyzuje zarówno modelu, jak i właściwości ModelType. Mimo że właściwości modelu i ModelType są nieaktualne, zgodności z poprzednimi wersjami metody pobierające właściwości nadal działa; delegują one właściwości ModelMetadata do pobrania wartości.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Zmiany w klasie DefaultControllerFactory Przerwij fabryki kontrolerów niestandardowych, które wynikają z niego

Klasa DefaultControllerFactory został rozwiązany przez usunięcie właściwości RequestContext. Zamiast tego właściwość wystąpienie kontekstu żądania jest przekazywany do chronionego GetControllerInstance i GetControllerType metody wirtualne. Ta zmiana ma wpływ na fabryki kontrolerów niestandardowych, które wynikają z DefaultControllerFactory.

Fabryki kontrolerów niestandardowe są często używane aplikacje zapewnienie wstrzykiwanie zależności dla platformy ASP.NET MVC. Aby zaktualizować fabryki kontrolerów niestandardowych, do obsługi platformy ASP.NET MVC 2, Zmień podpis metody lub podpisy do dopasowania, nowe sygnatury, a zamiast właściwości należy użyć parametru kontekstu żądania.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Obszar" jest teraz klucza zastrzeżonej wartości trasy

Ciąg "obszar" w wartości trasy teraz ma specjalne znaczenie we wzorcu ASP.NET MVC w taki sam sposób, jak zrobić "controller" i "action". Jeden możliwa jest, że jeśli pomocników HTML są dostarczane za pomocą słownika wartości trasy, zawierające "obszar", pomocników nie jest już dołączy "obszar" w ciągu zapytania.

Jeśli używasz funkcji obszarów, upewnij się nie używać {obszaru} jako część adresu URL trasy.


## <a id="_TOC6"></a>  Zrzeczenie odpowiedzialności

To jest dokument wstępny i może ulec znacznym zmianom przed ostatecznym wydaniem komercyjnym oprogramowania opisanych tutaj materiałach.

Informacje zawarte w tym dokumencie reprezentuje bieżący widok firmy Microsoft Corporation na problemy omówione w dniu publikacji. Ponieważ firma Microsoft musi reagować na zmieniające się warunki na rynku, nie powinny być rozumiane jako zobowiązanie ze strony firmy Microsoft, a firma Microsoft nie gwarantuje dokładności informacji po opublikowaniu.

Ten dokument jest tylko do celów informacyjnych. MICROSOFT NIE UDZIELA ŻADNYCH GWARANCJI, WYRAŹNYCH, DOROZUMIANYCH ANI USTAWOWYCH, W ODNIESIENIU DO INFORMACJI W TYM DOKUMENCIE.

Zgodnie z obowiązującymi przepisami prawa autorskiego jest odpowiedzialny za użytkownika. Bez ograniczenia, praw autorskich, żadna część tego dokumentu może być odtworzyć, przechowywane w lub wprowadzone do systemu wyszukiwania lub przekazywanych w jakiejkolwiek formie lub za pomocą jakichkolwiek środków (elektronicznych, mechanicznych, fotokopiowania, nagrywania lub w przeciwnym razie) lub w jakimkolwiek celu, bez wyraźnej pisemnej zgody firmy Microsoft Corporation.

Firma Microsoft może być patentów, wniosków patentowych, znaków towarowych, praw autorskich lub innych praw własności intelektualnej dotyczące elementów zawartych w tym dokumencie. Wyraźnie określone w umowach licencyjnych firmy Microsoft, udostępnienie tego dokumentu nie mu żadnych licencji dotyczących tych patentów, znaków towarowych, praw autorskich i innej własności intelektualnej.

Jeśli nie określono inaczej, przykładowe firmy, organizacje, produkty, nazwy domen, adresy e-mail, logo, osoby, miejsca i zdarzenia wymienione w tym dokumencie są fikcyjne i żadne skojarzenia z rzeczywistymi firmami, organizacji, produktu, nazwa domeny, adres e-mail adres, logo, osoby, miejsca lub zdarzeń jest przeznaczone lub powinny być zakładane.

© 2010 Microsoft Corporation. Wszelkie prawa zastrzeżone.

Firma Microsoft i Windows są zastrzeżonymi znakami towarowymi lub znakami towarowymi firmy Microsoft Corporation w Stanach Zjednoczonych i/lub innych krajach.

Nazwy firm i produktów wymienione w niniejszym dokumencie mogą być znakami przez ich właścicieli.
