---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Informacje o lokalizacji kodu ASP.NET AJAX | Dokumentacja firmy Microsoft
author: scottcate
description: Lokalizacja to proces projektowania i integracji pomocy technicznej dla określonego języka i kultury aplikacji lub składnika aplikacji. Mic...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: ce6404ce4faa1018a4f8118f6167a4f93956abd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815014"
---
<a name="understanding-aspnet-ajax-localization"></a>Informacje o lokalizacji kodu ASP.NET AJAX
====================
przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalizacja to proces projektowania i integracji pomocy technicznej dla określonego języka i kultury aplikacji lub składnika aplikacji. Platformy Microsoft ASP.NET oferuje zaawansowaną obsługę dla lokalizacji dla standardowych aplikacji ASP.NET, integrując standardowy model lokalizacji .NET; Microsoft AJAX Framework wykorzystać zintegrowane modelu do obsługi różnych scenariuszy, w których mogą być wykonywane lokalizacji.


## <a name="introduction"></a>Wprowadzenie

Firmy Microsoft technologii ASP.NET oferuje zorientowane obiektowo i oparte na zdarzeniach model programowania i łączy w sobie go z korzyściami skompilowanego kodu. Jednak jego przetwarzania po stronie serwera modelu ma kilka wady związane z technologii, z których wiele może zostać zlikwidowane przez nowych funkcji dostępnych w przestrzeni nazw System.Web.Extensions, który hermetyzuje usług AJAX programu Microsoft .NET Framework 3.5. Te rozszerzenia umożliwiają wiele funkcji wzbogaconego klienta wcześniej dostępny jako część rozszerzenia AJAX programu ASP.NET 2.0, ale teraz część Biblioteka klasy podstawowej struktury. Formanty i funkcje w tej przestrzeni nazw obejmują częściowe renderowanie stron, bez konieczności odświeżenie całej strony, możliwość dostępu do usług sieci Web za pośrednictwem skrypt po stronie klienta (w tym ASP.NET, profilowania API) i rozbudowane API po stronie klienta, który został zaprojektowany wiele duplikatów systemy kontroli w zestaw formantów po stronie serwera ASP.NET.

Lokalizacja funkcji Microsoft AJAX Framework i biblioteki skryptów AJAX firmy Microsoft, w kontekście potrzebę dysponowania obsługi lokalizacji i przeglądając już zintegrowana obsługa lokalizacji w sieci web sprawdza, czy ten oficjalny dokument aplikacji dostarczanych przez .NET Framework. Biblioteki skryptów AJAX firmy Microsoft korzysta z formatu plików resx już używane przez aplikacje platformy .NET, co zapewnia zintegrowane Obsługa środowiska IDE i typu zasobu możliwego do udostępnienia.

Ten dokument jest oparty na wersji Beta 2 programu Microsoft Visual Studio 2008. Tym oficjalnym dokumencie przyjęto założenie, będzie on pracować przy użyciu programu Visual Studio 2008, nie Visual Web Developer Express, i zapewnia wskazówki, zgodnie z interfejsu użytkownika programu Visual Studio. Przykłady kodu będzie korzystać z szablonów projektów, które mogą być niedostępne w Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*Potrzebę lokalizacji*

Szczególnie w przypadku deweloperom aplikacji dla przedsiębiorstw i deweloperów składników możliwość tworzenia narzędzia, które mogą być świadome różnice między języków i kultur stał się koniecznością. Projektowanie składników z możliwością dostosowania ustawień regionalnych klienta zwiększa wydajność deweloperów i pozwala na zmniejszenie ilości pracy wymaganej dla dostosowania składnika działać globalnie.

Lokalizacja to proces projektowania i integracji pomocy technicznej dla określonego języka i kultury aplikacji lub składnika aplikacji. Platformy Microsoft ASP.NET oferuje zaawansowaną obsługę dla lokalizacji dla standardowych aplikacji ASP.NET, integrując standardowy model lokalizacji .NET; Microsoft AJAX Framework wykorzystać zintegrowane modelu do obsługi różnych scenariuszy, w których mogą być wykonywane lokalizacji. Za pomocą oprogramowania Microsoft AJAX Framework skryptów albo może być lokalizowana wdrażane w zestawy satelit lub przy użyciu struktury systemu plików statycznych.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Osadzanie skryptów za pomocą zestawów satelickich*

Zgodnie z standardowa strategii .NET Framework w lokalizacji, zasoby mogą być dołączane w zestawach satelickich. Zestawy satelickie mają kilka zalet za pośrednictwem tradycyjnych zasobów dopuszczenia plików binarnych - dowolnej podanej lokalizacji można zaktualizować bez aktualizowania większy obraz, można wdrożyć dodatkowe lokalizacje poprzez instalowanie zestawów satelickich na folder projektu, a zestawy satelickie można wdrożyć bez powodowania załadowanie zestawu głównego projektu. Szczególnie w projektach programu ASP.NET to jest korzystne, ponieważ jego mogą znacznie zmniejszyć ilość zasobów systemowych, które posługują się aktualizacje przyrostowe i co najmniej zakłóca użycia witryny sieci Web w środowisku produkcyjnym.

Skrypty są osadzone w zestawach, umieszczając je w zarządzanych resx (lub skompilowane .resources) plików, które są dołączone do zestawu w czasie kompilacji. Ich zasoby są następnie udostępniane aplikacja skryptu za pomocą technologii AJAX generowane przez środowisko uruchomieniowe kodu, za pośrednictwem atrybuty poziomu zestawu

*Konwencje nazewnictwa plików osadzonych skryptów*

Zarządzanie skryptów Microsoft AJAX Framework obsługuje różne opcje do użycia we wdrożeniu i testowania skryptów i wytyczne znajdują się w celu ułatwienia tych opcji.

*Aby ułatwić debugowanie:*

Skrypty wersji (produkcja) nie może zawierać `.debug` kwalifikatorem w nazwie pliku. Skrypty zaprojektowane w celu debugowania powinny zawierać `.debug` w nazwie pliku.

*Aby ułatwić lokalizacji:*

Kultura neutralna skrypty nie może zawierać żadnych identyfikator kultury, nazwę pliku. W przypadku skryptów, które zawierają zlokalizowane zasoby należy określić kod języka ISO w nazwie pliku. Na przykład `es-CO` oznacza hiszpańskim, Kolumbii.

Poniższa tabela zawiera podsumowanie Konwencji wraz z przykładami nazewnictwa plików:

| Nazwa pliku | Znaczenie |
| --- | --- |
| Script.js | Skrypt neutralnej wydanej wersji. |
| Script.debug.js | Wersja do debugowania skryptu kultury neutralnej. |
| Script.en-US.js | Wydania wersji angielskiej, Stany Zjednoczone skrypt. |
| Script.debug.es-CO.js | Wersja do debugowania hiszpańskim, Kolumbii skrypt. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Przewodnik: Tworzenie zlokalizowanego, Embedded skryptu

*Uwaga: w tym instruktażu wymagane jest użycie programu Visual Studio 2008 zgodnie z Visual Web Developer Express nie obejmuje szablon projektu służący do projekty bibliotek klas.*

1. Utwórz nowy projekt witryny sieci Web za pomocą zintegrowane rozszerzenia AJAX programu ASP.NET. Utwórz innego projektu, projekt biblioteki klas, w ramach rozwiązania o nazwie LocalizingResources.
2. Dodaj plik języka Jscript o nazwie VerifyDeletion.js do projektu LocalizingResources, a także o nazwie DeletionResources.resx i DeletionResources.es.resx plików zasobów resx. Pierwsza będzie zawierać zasobów kultury neutralnej. te ostatnie zawierają zasoby języka hiszpańskiego.
3. Dodaj następujący kod do VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Dla osób, które są nieznane przy użyciu składni wyrażeń regularnych języka JavaScript, tekst w obrębie jednej kreski ułamkowe (w poprzednim przykładzie /FILENAME/ jest przykładem) wskazuje na obiekt RegExp. Biblioteka MSDN zawiera obszerne dokumentacja języka JavaScript i natywnych obiektów języka JavaScript można znaleźć zasobów online.

1. Dodaj następujące ciągi zasobów DeletionResources.resx: 

    **VerifyDelete**: czy na pewno chcesz usunąć, nazwa_pliku?

    **Usunięto**: Nazwa pliku została usunięta.

1. Dodaj następujące ciągi zasobów DeletionResources.es.resx: 

    **VerifyDelete**: roczyste seguro Est desee quitar, nazwa_pliku?

    **Usunięto**: nazwa_pliku se ha quitado.
2. Dodaj następujące wiersze kodu do pliku AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Dodaj odwołania do System.Web i System.Web.Extensions projektu LocalizingResources.
2. Dodaj odwołanie do projektu LocalizingResources z projektu witryny sieci Web.
3. W default.aspx w ramach projektu witryny sieci Web zaktualizuj formantu ScriptManager następujące dodatkowe znaczniki:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. W default.aspx w dowolnym miejscu na stronie obejmują ten kod znaczników:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Naciśnij F5. Jeśli zostanie wyświetlony monit, Włącz debugowanie. Po załadowaniu strony, kliknij przycisk Usuń. Należy pamiętać, że monit w języku angielskim (chyba że komputer jest domyślnie preferowanie zasoby języka hiszpańskiego) o potwierdzenie.
2. Zamknij okno przeglądarki i wróć do default.aspx. W @Page dyrektywy nagłówka, automatycznego zamieniania Culture i UICulture z es-ES. Naciśnij klawisz F5, aby uruchomić ponownie aplikację sieci web w przeglądarce. Monit usunąć plik w języku hiszpańskim w tej chwili należy zwrócić uwagę:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image6.png))


Należy zauważyć, że istnieje kilka zmian w ramach tego przewodnika. Na przykład skrypty może być zarejestrowany za pomocą formantu ScriptManager programowo podczas ładowania strony.

## <a name="including-a-static-script-file-structure"></a>*W tym struktury plików statycznych skryptów*

Używając plików statycznych skryptów dla wdrożenia, utratę niektórych korzyści z używania nieprzerwaną pracę programu .NET w lokalizacji. Przede wszystkim widoczny jest utraty automatyczne typ wygenerowany na podstawie łącznie z plikami zasobów skryptu; w instruktażu powyżej na przykład zasoby zostały ujawnione przez typ generowane automatycznie, o nazwie wiadomości z formantu ScriptManager.

Istnieją jednak pewne korzyści wynikające z używania struktury plików statycznych skryptów. Aktualizacje mogą być wykonywane bez konieczności ponownego kompilowania i ponownego wdrożenia zestawy satelickie i Użyj struktury plików statycznych, można również wykonać do zastąpienia osadzonych skryptów, aby zintegrować pomocnicza część funkcji, które nie mogły zostać wysłane za pomocą składnika.

Firma Microsoft zaleca, unikając problem kontroli wersji przez automatyczne generowanie skryptu zasobów podczas kompilowania projektu. Podczas obsługi kod skryptu rozbudowane podstawowej, może stać się bardziej skomplikowane upewnić się, że zmian w kodzie są odzwierciedlane w każdy skrypt zlokalizowane. Alternatywnie można po prostu utrzymać jeden skrypt logiki i wiele lokalizacji skryptów, scalanie plików podczas kompilowania projektu.

Ponieważ nie są zasoby do uwzględnienia w sposób deklaratywny, statycznych skryptów, które pliki powinny być przywoływane przez dodanie `<asp:ScriptElement>` elementów jako element podrzędny elementu `<Scripts>` tagu formantu ScriptManager lub programowo dodając `ScriptReference` obiektów Aby `Scripts` właściwość `ScriptManager` kontrolki na stronie w czasie wykonywania.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Funkcja ScriptManager i jego rolę w lokalizacji*

Funkcja ScriptManager umożliwia automatyczne zachowaniami w przypadku zlokalizowanych aplikacji:

- Automatycznie lokalizuje pliki skryptów na podstawie ustawień i konwencje nazewnictwa; na przykład ładuje włączyć debugowanie skryptów w trybie debugowania i obciążeń zlokalizowane skryptów, w oparciu o wybór interfejsu użytkownika w przeglądarce.
- Dzięki temu definicji kultur, w tym niestandardowych kultur.
- Umożliwia kompresję plików skryptów za pośrednictwem protokołu HTTP.
- Buforuje ona skryptów w celu efektywnego zarządzania wiele żądań.
- Dodaje warstwę operatora pośredniego do skryptów przez przekazanie w potoku je za pomocą adresu URL zaszyfrowane.

Programowo lub za oznaczeniu deklaracyjnym, można dodać odwołania do skryptu do formantu ScriptManager. Oznaczeniu deklaracyjnym jest szczególnie przydatne podczas pracy ze skryptami osadzone w zespoły inny niż projekt witryny sieci web, jako nazwę skryptu prawdopodobnie nie spowoduje zmiany jako poprawki są przekazywane za pośrednictwem.

## <a name="summary"></a>Podsumowanie

Jak powiększać aplikacji sieci web, aby dotrzeć do większej liczby osób, trzeba mieć dostęp do szerszego kultur i społeczności staje się podstawowym model biznesowy; aplikacje sieci web handlu elektronicznego muszą być w stanie poradzić sobie ze walutowym, systemy zarządzania zawartością muszą być obecne można nie tylko ich zawartość, ale także wskazówki dotyczące nawigacji i pól formularza, w innych językach i firm trzeba wiedzieć, że jest to potrzebę dostępne.

Program .NET Framework obsługuje wewnętrznie framework sformatowanego lokalizacji, wykorzystując zestawy satelickie i pliki zasobów (.resx) XML, aby przedstawić w jednolity sposób, w celu wyszukania zasobów ciągów i obrazów. Rozszerzenia AJAX programu ASP.NET, w tym Microsoft AJAX Framework i biblioteki skryptów AJAX firmy Microsoft, zapewniają obsługę ten model programowania do kodu po stronie klienta, umożliwiając łatwe zasobu ciągu wyszukiwania. Zestawy satelickie obsługuje automatyczne włączenie skryptu zasobów (pliki .js rzeczywiste) za pośrednictwem ScriptResource.axd tak długo, jak nazwy plików postępuj zgodnie z danym schemat nazewnictwa. Dzięki temu rozszerzenia AJAX programu ASP.NET uprościć skrypty lokalizacji i globalizacji aplikacji.

## <a name="bio"></a>*Bio*

Scott Cate pracował nad przy użyciu technologii Microsoft Web od 1997 r i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacji koncentruje się na rozwiązania programowe wiedzy opartych na. Scott można się skontaktować za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blog znajduje się na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [dalej](understanding-asp-net-ajax-web-services.md)
