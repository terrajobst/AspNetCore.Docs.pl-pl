---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Wyświetlanie wideo w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale opisano sposób wyświetlania wideo w stron ASP.NET Web Pages ze stroną składni Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Wyświetlanie wideo w sieci Web platformy ASP.NET (Razor) stron witryny
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób użycia odtwarzacza wideo (nośników) w witrynie sieci Web platformy ASP.NET Web Pages (Razor), aby zezwolić użytkownikom na wyświetlanie filmów wideo, które są przechowywane w witrynie. Strony ASP.NET Web Pages o składni Razor umożliwia Flash (*SWF*), Media Player (*wmv*) i Silverlight (*XAP*) wideo.
> 
> Zawartość:
> 
> - Jak wybrać odtwarzacza wideo.
> - Jak dodać wideo do strony sieci web.
> - Jak ustawić atrybuty odtwarzacza wideo.
> 
> Są to ASP.NET Razor strony funkcje dodane w artykule:
> 
> - `Video` Pomocnika.
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


## <a name="introduction"></a>Wprowadzenie

Możesz wyświetlić wideo w witrynie. Jednym ze sposobów w tym celu jest łącze do witryny, która ma już wideo, takich jak YouTube. Jeśli chcesz osadzenie klipu wideo w tych witrynach bezpośrednio w własne strony można zwykle uzyskać kod znaczników HTML z witryny i skopiuj go na stronie. Na przykład poniższy przykład przedstawia sposób osadzenia YouTube wideo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Chcąc odtwarzania wideo w serwisie (a nie w publicznej witryny udostępniania wideo), nie można bezpośrednio połączyć za pomocą znacznika osadzonego następująco. Jednak odtwarzania wideo z witryny przy użyciu `Video` pomocnika, która renderuje Windows media player bezpośrednio na stronie.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Wybieranie odtwarzacza wideo

Istnieje wiele formatów plików wideo, a każdy format wymaga zwykle od innego i inny sposób, aby skonfigurować odtwarzacza. Strony ASP.NET Razor można odtwarzanie wideo na stronie sieci web za pomocą `Video` pomocnika. `Video` Pomocnika upraszcza proces osadzanie klipów wideo na stronie sieci web, ponieważ automatycznie generuje `object` i `embed` elementów HTML, które zwykle są używane do dodawania wideo do strony.

`Video` Pomocnika obsługuje następujące odtwarzacze multimedialne:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash` Odtwarzacza `Video` pomocnika umożliwiają odtwarzania wideo Flash (*SWF* pliki) na stronie sieci web. Co najmniej należy podać ścieżkę do pliku wideo. Jeśli określisz nic, ale ścieżka odtwarzacz korzysta z wartości domyślnych, które są ustawiane przez bieżącą wersję Flash. Typowy domyślne ustawienia są:

- Przy użyciu domyślnej szerokości i wysokości wyświetlania wideo i bez kolor tła.
- Odtwarzanie odbywa się automatycznie podczas ładowania strony.
- Wideo wykonuje pętlę w sposób ciągły, dopóki nie zostanie jawnie zatrzymana.
- Aby wyświetlić wszystkie wideo, a nie Przycinanie wideo do określonego rozmiaru skalowania wideo.
- Odtwarzania wideo w oknie.

### <a name="the-mediaplayer-player"></a>Odtwarzacz MediaPlayer

`MediaPlayer` Odtwarzacza `Video` pomocnika umożliwia odtwarzania wideo Windows Media (*wmv* plików), Windows Media audio (*.wma* plików) i MP3 (*MP3* pliki) na stronie sieci web. Musi zawierać ścieżkę do pliku multimediów do odtwarzania; wszystkie inne parametry są opcjonalne. Jeśli określisz tylko ścieżka odtwarzacz korzysta z domyślnych ustawień ustawić przez bieżącą wersję MediaPlayer, takich jak:

- Zawartość wideo jest wyświetlana, przy użyciu domyślnej szerokości i wysokości.
- Odtwarzanie odbywa się automatycznie podczas ładowania strony.
- Wideo jest odtwarzany raz (go nie pętla).
- Odtwarzacz przedstawia pełny zestaw kontrolek interfejsu użytkownika.
- Odtwarzania wideo w oknie.

### <a name="the-silverlight-player"></a>Odtwarzacz Silverlight

`Silverlight` Odtwarzacza `Video` pomocnika umożliwia odtwarzanie wideo Windows Media (*wmv* plików), Windows Media Audio (*.wma* plików) i MP3 (*MP3* pliki). Należy ustawić parametr path, aby wskazać pakiet aplikacji oparte na technologii Silverlight (*XAP* pliku). Należy również ustawić parametrów szerokości i wysokości. Wszystkie inne parametry są opcjonalne. Korzystając z Silverlight odtwarzacza wideo, jeśli ustawisz wymaganych parametrów, Silverlight wyświetlane wideo bez kolor tła.

> [!NOTE]
> Jeśli nie znasz już program Silverlight: *XAP* pliku to skompresowany plik, który zawiera instrukcje układu w *.xaml* pliku kodu zarządzanego w zestawy i opcjonalne zasoby. Można utworzyć *XAP* pliku w programie Visual Studio jako projekt aplikacji Silverlight.


`Silverlight` Odtwarzacza wideo używa zarówno ustawienia, które zapewniają odtwarzacza i ustawienia, które zostały opublikowane w *XAP* pliku.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Typy MIME
> 
> Gdy przeglądarka pobierze plik, przeglądarka sprawdza, czy typ pliku zgodny typ MIME, który jest określony dla dokumentu, który jest renderowany. Typ MIME jest typu lub nośnik typ zawartości pliku. `Video` Pomocnika korzysta z następujących typów MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Odtwarzanie wideo Flash (SWF)

Ta procedura pokazuje, jak odtwarzanie wideo Flash o nazwie *sample.swf*. W procedurze założono, że masz folder o nazwie *nośnika* w witrynie i *SWF* plik znajduje się w tym folderze.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie zostało dodane.
2. W witrynie sieci Web, należy dodać stronę i nadaj mu nazwę *FlashVideo.cshtml*.
3. Dodaj następujący kod do strony: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Uruchom strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Ta strona jest wyświetlana i odtwarzanie odbywa się automatycznie. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Można ustawić `quality` parametr Flash wideo, aby `low`, `autolow`, `autohigh`, `medium`, `high`, i `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Możesz zmienić Flash wideo, aby odtworzyć za pomocą określonego rozmiaru `scale` parametr, który można ustawić dla następujących:

- `showall`. Spowoduje to uruchomienie całego wideo widoczne, zachowując jego oryginalny współczynnik proporcji. Jednak może być na końcu obramowań na każdej stronie.
- `noorder`. To skaluje wideo, zachowując jego oryginalny współczynnik proporcji, ale może zostać obcięty.
- `exactfit`. Spowoduje to uruchomienie całego wideo widoczne nie zachowując jego oryginalny współczynnik proporcji, ale mogą wystąpić zakłócenia.

Jeśli nie określisz `scale` parametru całe wideo będzie widoczny i jego oryginalny współczynnik proporcji będzie przechowywany bez żadnych przycinania. Poniższy przykład przedstawia użycie `scale` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player obsługuje tryb wideo, ustawienie o nazwie `windowMode`. Możesz ustawić `window`, `opaque`, i `transparent`. Domyślnie `windowMode` ma ustawioną wartość `window`, które powoduje wyświetlenie wideo w osobnym oknie na stronie sieci web. `opaque` Ustawienie ukrywa wszystko za wideo na stronie sieci web. `transparent` Umożliwia ustawienie tła strony sieci web są widoczne wideo, zakładając, że wszystkie części klipu wideo jest niewidoczny.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Odtwarzanie MediaPlayer (*wmv*) wideo

Poniższej procedurze wyjaśniono, jak odtwarzanie wideo multimediów okna o nazwie *sample.wmv* w *nośnika* folderu.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
2. Utwórz nową stronę o nazwie *MediaPlayerVideo.cshtml*.
3. Dodaj następujący kod do strony: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Uruchom strony w przeglądarce. Wideo ładuje i odtwarzany automatycznie. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Można ustawić `playCount` na liczbę całkowitą, która wskazuje, ile razy, aby automatycznie odtwarzania wideo:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Parametr umożliwia określenie, które kontrolki widoczne w interfejsie użytkownika. Można ustawić `uiMode` do `invisible`, `none`, `mini`, lub `full`. Jeśli nie określisz `uiMode` parametru wideo będą wyświetlane w oknie stanu, wyszukiwanie, kontrolować przycisków i głośności oprócz okna wideo. Formanty te będą również wyświetlane użycie odtwarzacza do odtwarzania pliku audio. Poniżej przedstawiono przykład sposobu użycia `uiMode` parametru:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Domyślnie audio znajduje się na podczas odtwarzania wideo. Dźwięk można wyciszanie przez ustawienie `mute` parametru na wartość true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Można kontrolować poziom audio, wideo MediaPlayer przez ustawienie `volume` parametru na wartość z zakresu od 0 do 100. Wartością domyślną jest 50. Oto przykład:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Odtwarzanie wideo Silverlight

Ta procedura pokazuje, jak odtwarzanie wideo zawarte w programie Silverlight *.xap* strony w folderze o nazwie *nośnika*.

1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
2. Utwórz nową stronę o nazwie *SilverlightVideo.cshtml*.
3. Dodaj następujący kod do strony: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Uruchom strony w przeglądarce. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Omówienie technologii Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atrybuty tagu Flash obiekt i osadzania](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM tagów](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
