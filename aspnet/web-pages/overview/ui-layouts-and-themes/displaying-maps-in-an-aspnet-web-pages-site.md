---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Wyświetlanie map we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób wyświetlania mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie związane z mapowaniem usług udostępnianych przez usługi Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368838"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie map w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób wyświetlania mapy interaktywne na stronach w witrynie internetowej ASP.NET Web Pages (Razor) na podstawie związane z mapowaniem usług udostępnianych przez usługi Bing, Google, MapQuest i Yahoo.
> 
> Zawartość:
> 
> - Jak można wygenerować mapy na podstawie adresu.
> - Jak można wygenerować mapy na podstawie współrzędnych szerokości i długości geograficznej.
> - Jak zarejestrować konto dla deweloperów map Bing i uzyskiwanie klucza do użycia z usługą mapy Bing.
> 
> Jest to ASP.NET wprowadzonej w artykule:
> 
> - `Maps` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> W tym samouczku współpracuje również z programu WebMatrix 3.


Na stronach sieci Web, można wyświetlić mapy na stronie, używając `Maps` pomocnika. Można wygenerować mapy albo na podstawie adresu lub zbiór współrzędne długości i szerokości geograficznej. `Maps` Klasy pozwala wywoływać aparatów popularnych mapy, takich jak Bing, Google, MapQuest i Yahoo.

Kroki, aby dodać mapowanie do strony są takie same niezależnie od tego, który aparatów mapy wywołania. Wystarczy dodać odwołanie do pliku JavaScript, która sprawia, że dostępne metody, aby wyświetlić mapę, a następnie wywołać metody `Maps` pomocnika.

Wybierz usługę mapy na podstawie którego `Maps` używanej metody pomocnika. Można użyć dowolnego z nich:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalowanie elementów, których potrzebujesz

Do wyświetlania mapy, potrzebne są następujące elementy:

- `Maps` Pomocnika. Tego pomocnika jest w wersji 2 bibliotekę pomocników platformy ASP.NET sieci Web. Jeśli nie został jeszcze dodany biblioteki, można zainstalować go w witrynie, jako pakiet NuGet. Aby uzyskać więcej informacji, zobacz [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (W galerii, wyszukaj `microsoft-web-helpers` pakietu.)
- Biblioteka jQuery. Niektóre szablony witryn programu WebMatrix już zawierają biblioteki jQuery w ich *skryptu* folderów. Jeśli nie masz tych bibliotek, możesz pobrać najnowsze biblioteki jQuery bezpośrednio z [jQuery.org](http://jQuery.org) lokacji. Lub możesz utworzyć nową witrynę za pomocą szablonu (na przykład **witryny początkowej** szablonu) a następnie skopiuj pliki jQuery z tej lokacji, do bieżącej lokacji.

Ponadto jeśli chcesz użyć usługi mapy Bing, należy najpierw utworzyć konto (wersja bezpłatna) i uzyskiwanie klucza. Aby uzyskać klucz, wykonaj następujące kroki:

1. Utwórz konto na [mapy Bing dla konta dewelopera](https://www.microsoft.com/maps/developers/web.aspx). Musi mieć konto Microsoft (Windows Live ID) również.

    Można określić chcesz użyć klucza dla **oceny i testowania**. Jeśli testujesz funkcji mapowania na swoim komputerze przy użyciu programu WebMatrix i usług IIS Express, przejdź **witryny** obszaru roboczego i zanotuj adres URL witryny (na przykład `http://localhost:50408`, mimo że numer portu, prawdopodobnie będą inne). Możesz użyć tej funkcji *localhost* adres witryny podczas rejestrowania.
2. Po zarejestrowaniu konta, przejdź do Centrum konta map Bing i kliknij **Utwórz lub Wyświetl klucze**:

    ![Mapowanie 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Zapisz klucz, który tworzy Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Tworzenie mapy na podstawie adresu (przy użyciu Google)

Poniższy przykład pokazuje, jak utworzyć stronę, która renderuje mapy na podstawie adresu. W tym przykładzie pokazano, jak używać mapy Google.

1. Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny. Na tej stronie spowoduje wygenerowanie mapy na podstawie adresu i przekażesz do niego.
2. Skopiuj następujący kod do pliku, zastępując istniejącą zawartość.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Zwróć uwagę, następujące funkcje strony:

    - `<script>` Element `<head>` elementu. W tym przykładzie `<script>` odwołania do elementu *jquery 1.6.4.min.js* pliku, który jest zminimalizowany (skompresowane) wersję biblioteki jQuery, wersja 1.6.4. Należy pamiętać, odwołanie założono, że *js* plik znajduje się w *skrypty* folder witryny. 

        > [!NOTE]
        > Jeśli używasz innej wersji biblioteki jQuery po prostu upewnij się, że wskazuje do tej wersji poprawnie.
    - Wywołanie `@Maps.GetGoogleHtml` w treści strony. Aby zamapować adresu, należy przekazać ciąg adresu. Metody silników mapy działają w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Uruchom stronę, a następnie wprowadź adres. Zostanie wyświetlona strona mapie, według map programu Google, pokazujący określonej lokalizacji.

     ![Mapowanie 1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Tworzenie mapy, w oparciu o długość i szerokość geograficzną służy do koordynowania (przy użyciu usługi Bing)

Ten przykład przedstawia sposób tworzenia na podstawie współrzędnych mapy. Ten przykład pokazuje, jak używać usługi mapy Bing i sposobu uwzględniania klucza usługi Bing. (Można utworzyć mapę na podstawie współrzędnych przy użyciu innych silników mapy także bez korzystania z kluczem usługi Bing).

1. Utwórz plik o nazwie *MapCoordinates.cshtml* w katalogu głównym witryny i Zastąp istniejącą zawartość z następującymi kodu i znaczników:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Zastąp `your-key-here` za pomocą klucza usługi mapy Bing, wcześniej wygenerowany.
3. Uruchom *MapCoordinates.cshtml* strony, wprowadzić współrzędne geograficzne, a następnie kliknij przycisk **mapy ją!** przycisk. (Jeśli nie znasz wszystkie współrzędne, spróbuj wykonać następujące czynności. Jest to lokalizacja na uczelniach Microsoft Redmond).

   - Szerokość geograficzna: 47.6781005859375
   - Longitude: -122.158317565918

     Ta strona jest wyświetlana przy użyciu określonych współrzędnych.

     ![Mapowanie 3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Dokumentacja interfejsu API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
