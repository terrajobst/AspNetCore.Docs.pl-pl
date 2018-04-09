---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Opis lokalizacji AJAX ASP.NET | Dokumentacja firmy Microsoft
author: scottcate
description: Lokalizacja jest proces projektowania i włączenie obsługi języka i kultury do aplikacji lub składnika aplikacji. Mikrofon...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-localization"></a>Opis lokalizacji AJAX ASP.NET
====================
przez [Scott kategorii](https://github.com/scottcate)

[Pobierz plik PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalizacja jest proces projektowania i włączenie obsługi języka i kultury do aplikacji lub składnika aplikacji. Platformy Microsoft ASP.NET zapewnia zaawansowaną obsługę lokalizacji dla standardowych aplikacji ASP.NET przez integrację standardowy model lokalizacji .NET; Microsoft AJAX Framework używają zintegrowanego modelu do obsługi różnych scenariuszy, w których można wykonać lokalizacji.


## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowane obiektowo i sterowane zdarzeniami i unites go z zalet skompilowanego kodu. Jednak jego model przetwarzania po stronie serwera ma kilka wady związane z technologii, z których wiele może zostać zlikwidowane przez nowe funkcje uwzględnione w obszarze nazw System.Web.Extensions, który hermetyzuje usług Microsoft AJAX w programie .NET Framework 3.5. Te rozszerzenia włączyć wiele funkcji wzbogaconego klienta wcześniej dostępne jako część rozszerzeń AJAX programu ASP.NET 2.0, ale teraz częścią Framework biblioteki klasy podstawowej. Formanty i funkcje w tej przestrzeni nazw obejmują częściowe renderowanie stron, bez konieczności odświeżenia całą stronę, możliwość dostępu do usług sieci Web za pośrednictwem skrypt po stronie klienta (w tym profilowania API ASP.NET), i rozbudowany interfejs API klienta przeznaczony do wiele duplikatów systemy formantu w zestawie formantu po stronie serwera ASP.NET.

Ten dokument sprawdza się w Microsoft AJAX Framework, biblioteki skryptów Microsoft AJAX, w kontekście potrzeba biznesowa lokalizacja pomocy technicznej i przeglądania już zintegrowana obsługę lokalizacji w sieci web funkcji lokalizacji aplikacje dostarczane przez program .NET Framework. Biblioteka skryptów Microsoft AJAX wykorzystuje .resx format pliku już używane przez aplikacje platformy .NET, która zapewnia zintegrowane Obsługa środowiska IDE i typu zasobu możliwe do udostępnienia.

Ten dokument jest oparty na wersji Beta 2 programu Microsoft Visual Studio 2008. Ten dokument również założono, że będzie działać z programu Visual Studio 2008, nie Visual Web Developer Express i zapewnia wskazówki zgodnie z interfejsu użytkownika programu Visual Studio. Przykładowy kod będzie wykorzystywać szablony projektów, które mogą być niedostępne w przypadku programu Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*Potrzebę lokalizacji*

Szczególnie dla deweloperów w przedsiębiorstwach aplikacji i składników deweloperów Tworzenie narzędzia, które mogą zwrócić uwagę różnice między kultur i języków stał się koniecznością. Projektowanie składniki z możliwością dostosowania ustawień regionalnych klienta zwiększa wydajność deweloperów i zmniejsza ilość pracy wymaganej do dostosowania składnika do funkcji globalnie.

Lokalizacja jest proces projektowania i włączenie obsługi języka i kultury do aplikacji lub składnika aplikacji. Platformy Microsoft ASP.NET zapewnia zaawansowaną obsługę lokalizacji dla standardowych aplikacji ASP.NET przez integrację standardowy model lokalizacji .NET; Microsoft AJAX Framework używają zintegrowanego modelu do obsługi różnych scenariuszy, w których można wykonać lokalizacji. Z programu Microsoft AJAX Framework albo można lokalizować skryptów wdrażania do zestawów satelickich lub korzystanie z struktury systemu plików statycznych.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Osadzanie skryptów z zestawy satelickie*

Zgodnie z standardowej strategii lokalizacji .NET Framework, zasoby mogą zostać włączone w zestawy satelickie. Zestawy satelickie mają kilka zalet za pośrednictwem tradycyjnych zasobów włączenia plików binarnych - żadnych danej lokalizacji mogą być aktualizowane bez aktualizowania większy obraz, po prostu, instalując zestawy satelickie do można wdrożyć dodatkowe lokalizacje folder projektu i zestawy satelickie można wdrożyć bez spowodowania, że ponowne załadowanie zestawu głównego projektu. Szczególnie w przypadku projektów ASP.NET to jest korzystne, ponieważ mogą znacznie zmniejszyć ilość zasobów systemowych używanych przez aktualizacji przyrostowych i co najmniej zakłóca produkcji użycia witryny sieci Web.

Skrypty są osadzone w zestawy, umieszczając je w zarządzanych .resx (lub skompilowany .resources) pliki, które znajdują się w zestawie w czasie kompilacji. Ich zasobów są następnie udostępniane do aplikacji skryptu za pomocą technologii AJAX środowiska uruchomieniowego wygenerowany kod, za pośrednictwem atrybuty poziomu zestawu

*Konwencje nazewnictwa plików osadzonych skryptów*

Zarządzanie skryptów Microsoft AJAX Framework obsługuje różne opcje wdrażania i testowania skryptów, a znajdują się wytyczne ułatwiające tych opcji.

*Ułatwianie debugowania:*

Skrypty zlecenia (środowisko produkcyjne) nie może zawierać `.debug` kwalifikator w nazwie pliku. Skrypty przeznaczony dla debugowania powinny zawierać `.debug` w nazwie pliku.

*Aby ułatwić lokalizacji:*

Kultura neutralna skrypty nie może zawierać żadnych identyfikator kultury pliku. Skryptów, które zawierają zlokalizowanych zasobów należy określić kod języka ISO w nazwie pliku. Na przykład `es-CO` oznacza hiszpańskim, Kolumbii.

Poniższa tabela zawiera podsumowanie Konwencji wraz z przykładami nazewnictwa plików:

| Nazwa pliku | Znaczenie |
| --- | --- |
| Script.js | Wersja wydana skrypt niezależny od kultury. |
| Script.debug.js | Wersja do debugowania skryptu niezależny od kultury. |
| Script.en-US.js | Wersja wersji angielskiej Stanów Zjednoczonych skrypt. |
| Script.debug.es-CO.js | Wersja do debugowania hiszpańskim, Kolumbii skrypt. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Wskazówki: Tworzenie zlokalizowanego, osadzone skryptu

*Uwaga: w tym przewodniku wymaga użycia programu Visual Studio 2008, zgodnie z programu Visual Web Developer Express nie ma szablonu projektu dla projektów biblioteki klas.*

1. Utwórz nowy projekt witryny sieci Web ASP.NET AJAX rozszerzenia zintegrowany. Utwórz innego projektu, projektu biblioteki klas, w ramach rozwiązania o nazwie LocalizingResources.
2. Dodaj plik języka Jscript w projekcie LocalizingResources o nazwie VerifyDeletion.js, a także .resx pliki zasobów o nazwie DeletionResources.resx i DeletionResources.es.resx. Pierwsza będzie zawierać niezależny od kultury zasobów. drugie będzie zawierać zasobów na język hiszpański.
3. Dodaj następujący kod do VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Dla tych, które znają składni wyrażeń regularnych języka JavaScript, tekst wewnątrz jednej ukośniki (w poprzednim przykładzie /FILENAME/ jest przykładem) oznacza obiekt RegExp. Biblioteki MSDN zawiera szeroką gamę odwołanie JavaScript i zasobów na obiektów macierzystych JavaScript można znaleźć.

1. Dodaj następujące ciągi zasobów DeletionResources.resx: 

    **VerifyDelete**: czy na pewno chcesz usunąć FILENAME?

    **Usunięte**: Nazwa pliku została usunięta.

1. Dodaj następujące ciągi zasobów DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar nazwy pliku?

    **Usunięte**: Nazwa pliku se ha quitado.
2. Dodaj następujące wiersze kodu do pliku AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Dodaj odwołania do System.Web i System.Web.Extensions do projektu LocalizingResources.
2. Dodaj odwołanie do projektu LocalizingResources z projektu witryny sieci Web.
3. W default.aspx w obszarze projekt witryny sieci Web zaktualizuj formantu ScriptManager o następujące dodatkowe znaczników:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. W plik default.aspx w dowolnym miejscu na stronie obejmują ten kod znaczników:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Naciśnij F5. Jeśli zostanie wyświetlony monit, Włącz debugowanie. Podczas ładowania strony, kliknij przycisk Usuń. Należy pamiętać, że monit w języku angielskim (chyba że komputer jest domyślnie preferować zasobów na język hiszpański) o potwierdzenie.
2. Zamknij okno przeglądarki i wróć do default.aspx. W @Page dyrektywy nagłówka, Zastąp automatycznie dla kultury i UICulture z es-ES. Naciśnij klawisz F5, aby ponownie uruchomić ponownie aplikację sieci web w przeglądarce. Teraz, należy pamiętać, że zostanie wyświetlony monit o usunięcie pliku w języku hiszpańskim:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image6.png))


Należy pamiętać, że ma kilka zmian w ramach tego przewodnika. Na przykład skrypty może być zarejestrowany za pomocą formantu ScriptManager programowo podczas ładowania strony.

## <a name="including-a-static-script-file-structure"></a>*W tym struktury plików statycznych skryptu*

Korzystając z plików statycznych skryptów dla wdrożenia, utracisz niektóre korzyści wynikające ze stosowania związanego z używaniem programu .NET lokalizacji. Przede wszystkim widoczny jest że utracić automatyczne typ wygenerowane z tym skryptu zasobu plików; w powyższym wskazówki na przykład zasoby były udostępniane przez generowane automatycznie typ o nazwie wiadomości z formantu ScriptManager.

Istnieją jednak pewne korzyści wynikające z używania struktury plików statycznych skryptu. Aktualizacje mogą być wykonywane bez ponownego kompilowania ani ponownego wdrażania zestawy satelickie i używanie struktury plików statycznych, można również wykonać do przesłonięcia osadzony skrypt do integracji ze składnikiem pomocnicza część funkcje, które nie mogły zostać wysłane.

Firma Microsoft zaleca unikanie problem kontroli wersji przez automatyczne generowanie zasobów skryptu podczas kompilowania projektu. Podczas obsługi kodu skryptu szeroką gamę podstawowego, może stać się coraz trudniejsze upewnić się, czy kod zmiany zostaną odzwierciedlone w każdej zlokalizowanych skryptu. Alternatywnie można po prostu utrzymać skrypt logiki i wielu skryptów lokalizacji, scalanie plików podczas kompilowania projektu.

Ponieważ nie ma zasobów, aby uwzględnić deklaratywnie, skrypt statyczne pliki powinny być odwołuje się do przez dodanie `<asp:ScriptElement>` elementów jako element podrzędny `<Scripts>` tagu formantu ScriptManager lub programowo dodając `ScriptReference` obiektów Aby `Scripts` właściwość `ScriptManager` kontrolki na stronie w czasie wykonywania.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Element ScriptManager i jego rola w lokalizacji*

Element ScriptManager umożliwia automatyczne zachowaniami zlokalizowanych aplikacji:

- Automatycznie lokalizuje pliki skryptów na podstawie ustawień i konwencje nazewnictwa; na przykład ładuje włączyć debugowanie skryptów w trybie debugowania i obciążeń zlokalizowane skryptów w oparciu o wybór interfejsu użytkownika w przeglądarce.
- Umożliwia on definicji kultur, w tym niestandardowe kultury.
- Umożliwia kompresję plików skryptów za pośrednictwem protokołu HTTP.
- Buforuje ją na efektywne zarządzanie dużą liczbą żądań skryptów.
- Dodaje warstwę pośredni do skryptów przez przekazanie w potoku je za pomocą adresu URL zaszyfrowane.

Odwołania do skryptu można dodać do formantu ScriptManager programowo lub przez deklaratywne znaczników. Deklaratywne znaczników jest szczególnie przydatne podczas pracy ze skryptami zestawy osadzony w innym niż projekt witryny sieci web, jako nazwę skryptu prawdopodobnie nie spowoduje zmiany jako poprawki przesyłanych za pośrednictwem.

## <a name="summary"></a>Podsumowanie

Wzrostem aplikacji sieci web do szerszego grona odbiorców, trzeba mieć dostęp do szerszego grona kultur i społeczności staje się core do modelu firm; aplikacji sieci web handlu elektronicznego muszą być w stanie rozwiązać walutowym, muszą być obecnie można nie tylko ich zawartość, ale także wskazówki dotyczące nawigacji i pól formularza w innych języków i firm trzeba wiedzieć, że jest to konieczność systemów zarządzania zawartością dostępny.

.NET Framework obsługuje bardzo ramy sformatowanego lokalizacji przy użyciu zestawów satelickich i pliki zasobów (resx) XML do prezentowania ujednolicone do wyszukiwania ciągów zasobów i obrazów. Rozszerzenia ASP.NET AJAX, w tym programu Microsoft AJAX Framework i biblioteki skryptów Microsoft AJAX, zapewniają obsługę dla tego modelu programowania kodu po stronie klienta umożliwiające łatwe zasobów ciągu wyszukiwania. Zestawy satelickie obsługuje automatyczne włączenie skryptu zasobów (pliki rzeczywiste js) za pośrednictwem ScriptResource.axd tak długo, jak nazwy plików wykonaj dany schemat nazewnictwa. Z tej obsługi rozszerzeń programu ASP.NET AJAX uprościć lokalizacja skryptów i globalizacji aplikacji.

## <a name="bio"></a>*Bio*

Scott IE pracuje z technologii Microsoft Web od 1997 i jest Prezes myKB.com ([www.myKB.com](http://www.myKB.com)) gdzie specjalizuje się on w pisaniu ASP.NET aplikacje oparte na systemie koncentruje się na rozwiązania w zakresie oprogramowania bazy wiedzy Knowledge Base. Scott można nawiązać połączenie za pośrednictwem poczty e-mail na [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) lub jego blogu w [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [dalej](understanding-asp-net-ajax-web-services.md)
