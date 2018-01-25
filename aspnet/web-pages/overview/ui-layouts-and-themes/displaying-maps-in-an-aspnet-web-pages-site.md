---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: "Wyświetlanie mapy w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule opisano sposób wyświetlania interaktywnego mapy na stronach witryny sieci Web ASP.NET Web Pages (Razor) oparte na mapowania usług świadczonych przez Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie mapy witryny (Razor) stron sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób wyświetlania interaktywnego mapy na stronach witryny sieci Web ASP.NET Web Pages (Razor) oparte na mapowania usług świadczonych przez Bing, MapQuest, Google i Yahoo.
> 
> Zawartość:
> 
> - Jak wygenerować mapy na podstawie adresu.
> - Jak wygenerować mapę oparte na współrzędne geograficzne.
> - Jak zarejestrować konto deweloperów map Bing i uzyskiwanie klucza do użycia z usługą mapy Bing.
> 
> Jest to funkcja ASP.NET, wprowadzona w artykule:
> 
> - `Maps` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> W tym samouczku współdziała również z 3 programu WebMatrix.


Strony sieci Web, map można wyświetlić na stronie przy użyciu `Maps` pomocnika. Można wygenerować na podstawie adresu lub zestaw długości i szerokości geograficznej współrzędnych mapy. `Maps` Klasy umożliwia wywołują aparaty popularnych mapy, w tym Bing, MapQuest, Google i Yahoo.

Procedura dodawania mapowania do strony są takie same niezależnie od tego, który aparatów mapy wywołania. Po prostu Dodaj odwołanie pliku JavaScript, która sprawia, że dostępne metody, aby wyświetlić mapę, a następnie wywołać metody `Maps` pomocnika.

Wybierz usługę mapy na podstawie którego `Maps` używanej metody pomocnika. Można użyć dowolnego z nich:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalowanie elementy, które są potrzebne

Do wyświetlenia mapy, potrzebne są następujące elementy:

- `Maps` Pomocnika. Tego pomocnika jest w wersji 2 bibliotekę pomocników platformy ASP.NET sieci Web. Jeśli nie został jeszcze dodany biblioteki, można zainstalować go w witrynie jako pakietu NuGet. Aby uzyskać więcej informacji, zobacz [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). (W galerii, wyszukaj `microsoft-web-helpers` pakietu.)
- Biblioteki jQuery. Kilka szablonów witryn program WebMatrix zawiera biblioteki jQuery w ich *skryptu* folderów. Jeśli nie masz tych bibliotek, możesz pobrać najnowsze biblioteki jQuery bezpośrednio z [jQuery.org](http://jQuery.org) lokacji. Lub utworzyć nową lokację przy użyciu szablonu (na przykład **witryny początkowej** szablonu), a następnie skopiuj pliki jQuery z tej lokacji do bieżącej witryny.

Ponadto jeśli chcesz użyć mapy Bing, najpierw musisz utworzyć konto (bezpłatnie) i uzyskiwanie klucza. Aby uzyskać klucz, wykonaj następujące kroki:

1. Tworzenie konta na [konta dewelopera mapy Bing](https://www.microsoft.com/maps/developers/web.aspx). Musi mieć konto Microsoft (Windows Live ID) również.

    Można określić, czy chcesz użyć klucza dla **oceny i testowania**. Jeśli testujesz funkcja mapowania na własnym komputerze za pomocą programu WebMatrix i usług IIS Express, przejdź **lokacji** obszaru roboczego i zanotuj adres URL witryny sieci (na przykład `http://localhost:50408`, mimo że numer portu, prawdopodobnie będą inne). Możesz użyć tej funkcji *localhost* adres witryny podczas rejestrowania.
2. Po zarejestrowaniu dla konta, przejdź do Centrum konta map Bing i kliknij przycisk **Create lub widok kluczy**:

    ![Mapowanie-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Zarejestruj klucz, który tworzy Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Tworzenie mapy na podstawie adresu (przy użyciu usługi Google)

Poniższy przykład przedstawia sposób tworzenia strony, który renderuje mapy na podstawie adresu. Ten przykład przedstawia sposób użycia map programu Google.

1. Utwórz plik o nazwie *MapAddress.cshtml* w katalogu głównym witryny. Ta strona wygeneruje mapy na podstawie adresu, który przekazywania do niej.
2. Skopiuj następujący kod do pliku, zastępując istniejącej zawartości.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Zwróć uwagę, następujące funkcje strony:

    - `<script>` Element `<head>` elementu. W tym przykładzie `<script>` odwołania do elementu *jquery 1.6.4.min.js* pliku, który jest zminimalizowany (skompresowane) wersji biblioteki jQuery, wersja 1.6.4. Należy pamiętać, że odwołanie zakłada się, że *js* plik znajduje się w *skryptów* folder witryny. 

        > [!NOTE]
        > Jeśli używasz innej wersji biblioteki jQuery po prostu upewnij się, że wskazuje tej wersji poprawnie.
    - Wywołanie `@Maps.GetGoogleHtml` w treści strony. Aby mapować adres, należy podać ciąg adresu. Metody silników mapy działa w podobny sposób (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
- Uruchom strony, a następnie wprowadź adres. Na stronie są wyświetlane mapy, oparte na map programu Google, pokazujący określonej lokalizacji.

    ![Mapowanie-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Tworzenie mapy oparte na współrzędne geograficzne współrzędne (przy użyciu usługi Bing)

W tym przykładzie pokazano, jak utworzyć na podstawie współrzędnych mapy. Ten przykład przedstawia sposób użycia mapy Bing i sposobu uwzględniania klucza usługi Bing. (Można utworzyć mapę na podstawie współrzędnych przy użyciu innych aparatów mapy także bez korzystania z usługi Bing klucza).

1. Utwórz plik o nazwie *MapCoordinates.cshtml* w folderze głównym lokacji i Zastąp istniejącą zawartość z następującymi kodu i znaczników:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Zastąp `your-key-here` z kluczem usługi mapy Bing, wcześniej wygenerowany.
3. Uruchom *MapCoordinates.cshtml* wprowadź współrzędne geograficzne, a następnie kliknij pozycję **mapy go!** button. (Jeśli nie znasz wszystkie współrzędne, wykonaj następujące czynności. Jest to lokalizacja w firmy Microsoft Redmond).

    - Szerokość: 47.6781005859375
    - Longitude: -122.158317565918

    Ta strona jest wyświetlana, przy użyciu współrzędnych, określone przez użytkownika.

    ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Dokumentacja interfejsu API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
