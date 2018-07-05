---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Wyświetlanie obrazu wideo we wzorcu ASP.NET Web Pages lokacji (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale wyjaśniono, jak wyświetlać wideo w stron ASP.NET Web Pages ze stroną składni Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: d80fd8ab54bf1c049fa4ee6b7592925ebe0549f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369390"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie obrazu wideo w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak na potrzeby odtwarzacz wideo (w przypadku nośników) w witrynie internetowej ASP.NET Web Pages (Razor) umożliwiają użytkownikom wyświetlanie filmów wideo, które są przechowywane w witrynie. Stron ASP.NET Web Pages o składni Razor umożliwia Flash (*SWF*), usługa Media Player (*wmv*) i Silverlight (*.xap*) filmów wideo.
> 
> Zawartość:
> 
> - Jak wybrać odtwarzacza wideo.
> - Jak dodać wideo do strony sieci web.
> - Jak ustawić atrybuty odtwarzacza wideo.
> 
> Są one ASP.NET Razor strony funkcji wprowadzonych w artykule:
> 
> - `Video` Pomocnika.
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


## <a name="introduction"></a>Wprowadzenie

Możesz chcieć wyświetlić wideo w witrynie. Jednym sposobem wykonania tego zadania jest link do witryny, która ma już wideo, takich jak usługi YouTube. Jeśli chcesz osadzić wideo z tych lokacji bezpośrednio na własnych stronach, można zwykle uzyskać kod znaczników HTML z witryny i skopiuj go na swojej stronie. Na przykład poniższy przykład pokazuje, jak do usługi YouTube osadzania wideo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Jeśli chcesz odtworzyć film wideo, który jest we własnej witrynie internetowej (nie w publicznej witrynie udostępnianie wideo), nie można połączyć bezpośrednio do niego przy użyciu osadzonych znaczników następująco. Jednak odtwarzania wideo z witryny przy użyciu `Video` pomocnika, która renderuje bezpośrednio na stronie odtwarzacza multimedialnego.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Wybór odtwarzacza wideo

Dostępnych jest wiele formatów plików wideo, a każdy format zwykle wymaga innego i inny sposób, aby skonfigurować odtwarzacza. Na stronach ASP.NET Razor można odtworzyć wideo na stronie sieci web za pomocą `Video` pomocnika. `Video` Pomocnika upraszcza proces osadzania filmów wideo na stronie sieci web, ponieważ automatycznie generuje `object` i `embed` elementów HTML, które zwykle są używane do dodawania wideo do strony.

`Video` Pomocnika obsługuje następujące odtwarzacze multimedialne:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Odtwarzacza w środowisku Flash

`Flash` Odtwarzacza `Video` pomocnika pozwalają odtwarzania wideo Flash (*SWF* plików) na stronie sieci web. Jako minimum należy podać ścieżkę do pliku wideo. Jeśli określisz nic, ale ścieżka odtwarzacz używa wartości domyślnych, które są ustawiane przez bieżącą wersję programu Flash. Typowe domyślne ustawienia są następujące:

- Film wideo jest wyświetlana, przy użyciu jego domyślnej szerokości i wysokości i bez kolor tła.
- Odtwarzanie wideo odbywa się automatycznie po załadowaniu strony.
- Wideo w pętli stale, dopóki nie zostanie jawnie zatrzymana.
- Wideo jest skalowana, aby pokazać wszystkie wideo, a nie Przycinanie wideo do określonego rozmiaru.
- W oknie odtwarzanie wideo odbywa się.

### <a name="the-mediaplayer-player"></a>Odtwarzacz MediaPlayer

`MediaPlayer` Odtwarzacza `Video` pomocnika umożliwia odtwarzanie filmów wideo Windows Media (*wmv* plików), Windows Media audio (*.wma* plików) i MP3 (*.mp3* pliki) na stronie sieci web. Musi zawierać ścieżkę do pliku multimediów do odtwarzania; wszystkie inne parametry są opcjonalne. Jeśli określisz tylko ścieżki, gracz używa ustawienia domyślne ustawione przez bieżącą wersję MediaPlayer, takich jak:

- Film wideo jest wyświetlana, przy użyciu jego domyślnej szerokości i wysokości.
- Odtwarzanie wideo odbywa się automatycznie po załadowaniu strony.
- Odtwarzanie wideo odbywa się raz (go nie pętli).
- Odtwarzacz wyświetla pełnego zestawu kontrolek interfejsu użytkownika.
- W oknie odtwarzanie wideo odbywa się.

### <a name="the-silverlight-player"></a>Odtwarzacz Silverlight

`Silverlight` Odtwarzacza `Video` pomocnika umożliwia Windows Media Video (*wmv* plików), Windows Media Audio (*.wma* plików) i MP3 (*.mp3* pliki). Należy ustawić parametr ścieżki, aby wskazać pakiet aplikacji opartych na technologii Silverlight (*.xap* pliku). Można również ustawić parametry szerokość i wysokość. Wszystkie inne parametry są opcjonalne. Gdy używasz odtwarzacz Silverlight dla filmów wideo, jeśli ustawisz tylko wymagane parametry, odtwarzacz Silverlight Wyświetla wideo bez kolor tła.

> [!NOTE]
> W przypadku, gdy nie znasz jeszcze programu Silverlight: *.xap* plik jest skompresowany plik, który zawiera układ zgodnie z instrukcjami w *.xaml* plik kodu zarządzanego w zestawach i opcjonalnych zasobów. Możesz utworzyć *.xap* pliku w programie Visual Studio jako projekt aplikacji Silverlight.


`Silverlight` Odtwarzacza wideo używa zarówno zapewniające dla gracza, ustawienia i ustawienia, które znajdują się w *.xap* pliku.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Typy MIME
> 
> Gdy przeglądarka pobierze plik, przeglądarka sprawia, że się, że typ pliku odpowiada typ MIME, który jest określony dla dokumentu, który jest renderowany. Typ MIME jest typu lub nośnik typ zawartości pliku. `Video` Pomocnika używa następujących typów MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Odtwarzanie wideo Flash (SWF)

Ta procedura pokazuje, jak odtworzyć wideo Flash o nazwie *sample.swf*. W procedurze przyjęto założenie, że masz folder o nazwie *Media* na swojej witryny, która *SWF* plik znajduje się w tym folderze.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie został dodany.
2. W witrynie sieci Web, należy dodać stronę i nadaj mu nazwę *FlashVideo.cshtml*.
3. Na stronie, Dodaj następujący kod: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Uruchom stronę w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Ta strona jest wyświetlana, a odtwarzanie wideo odbywa się automatycznie. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Możesz ustawić `quality` parametr Flash film wideo, aby `low`, `autolow`, `autohigh`, `medium`, `high`, i `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Możesz zmienić Flash odtworzenie filmu wideo o określonym rozmiarze przy użyciu `scale` parametr, który można ustawić następujące czynności:

- `showall`. To sprawia, że całe wideo widoczne, zachowując jego oryginalny współczynnik proporcji. Jednak użytkownik może pozostać z obramowaniem na każdej stronie.
- `noorder`. To jest skalowana w wideo, zachowując jego oryginalny współczynnik proporcji, ale mogą być przycięte.
- `exactfit`. To sprawia, że całe wideo widoczna nie zachowując jego oryginalny współczynnik proporcji, ale mogą wystąpić zakłócenia.

Jeśli nie określisz `scale` parametru całe wideo będą widoczne, a jego oryginalny współczynnik proporcji zostaną zachowane bez żadnych przycinania. Poniższy przykład pokazuje, jak używać `scale` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player obsługuje tryb wideo, ustawienie o nazwie `windowMode`. Ustaw tę opcję na `window`, `opaque`, i `transparent`. Domyślnie `windowMode` ustawiono `window`, który zawiera wideo w osobnym oknie na stronie sieci web. `opaque` Ustawienie ukrywa wszystko, czego za wideo na stronie sieci web. `transparent` Ustawienie umożliwia tła strony sieci web są widoczne wideo, zakładając, że wszystkie części klipu wideo jest przezroczysty.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Odtwarzanie MediaPlayer (*wmv*) filmów wideo

Poniższa procedura pokazuje sposób odtwarzania wideo multimediów okna o nazwie *sample.wmv* w *Media* folderu.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
2. Utwórz nową stronę o nazwie *MediaPlayerVideo.cshtml*.
3. Na stronie, Dodaj następujący kod: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Uruchom stronę w przeglądarce. Film wideo ładuje i odtwarzany automatycznie. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Możesz ustawić `playCount` do liczby całkowitej, która wskazuje, ile razy do odtwarzania wideo automatycznie:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Parametr umożliwia określenie, które kontrolki pojawiają się w interfejsie użytkownika. Możesz ustawić `uiMode` do `invisible`, `none`, `mini`, lub `full`. Jeśli nie określisz `uiMode` parametru filmu wideo będzie wyświetlane w oknie stanu, wyszukiwanie pasku sterowania przyciski i formanty woluminie oprócz okna wideo. Te kontrolki będą również wyświetlane, jeśli używasz odtwarzacz do odtwarzania pliku audio. Oto przykład sposobu użycia `uiMode` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Domyślnie dźwięk jest odtwarzany film wideo. Możesz wyciszyć dźwięk, ustawiając `mute` parametru na wartość true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Można kontrolować poziom audio, wideo MediaPlayer przez ustawienie `volume` parametru na wartość z zakresu od 0 do 100. Wartością domyślną jest 50. Oto przykład:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Odtwarzanie wideo Silverlight

Ta procedura pokazuje, jak odtworzyć wideo znajdujących się w technologii Silverlight *.xap* strony w folderze o nazwie *Media*.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
2. Utwórz nową stronę o nazwie *SilverlightVideo.cshtml*.
3. Na stronie, Dodaj następujący kod: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Uruchom stronę w przeglądarce. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Omówienie technologii Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atrybuty znacznika obiektu i osadzania Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM tagów](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
