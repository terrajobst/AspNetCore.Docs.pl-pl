---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Praca z obrazami w witrynie ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale pokazano, jak dodać, wyświetlania i manipulowania obrazami (zmienić rozmiar, przerzucić i dodawać znaki wodne) w witrynie sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 32cfb9cc3c3eabcd6d724e05b85c5bf1160a39f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362930"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Praca z obrazami w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule przedstawiono sposób dodawania, wyświetlania i manipulowania obrazami (zmienić rozmiar, przerzucić i dodawać znaki wodne) w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak dynamicznie dodać obraz do strony.
> - Jak umożliwić użytkownikom przekazywanie obrazu.
> - Jak zmienić rozmiar obrazu.
> - Jak przerzucić lub obrócić obraz.
> - Jak dodać znak wodny do obrazu.
> - Jak używać obrazu jako znaku wodnego.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule programowania programu ASP.NET:
> 
> - `WebImage` Pomocnika.
> - `Path` Obiektu, który udostępnia metody, które pozwala manipulować ścieżkę i nazwy plików.
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


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Dynamiczne dodawanie obrazu do strony sieci Web

Można dodać obrazy do witryny sieci Web i do poszczególnych stron podczas tworzenia witryny sieci Web. Można także pozwolić użytkownikom przekazywanie obrazów, które mogą być przydatne dla zadań, takich jak umożliwienie im na dodawanie zdjęcie w profilu.

Jeśli obraz nie jest jeszcze dostępna w witrynie i chcesz tylko wyświetlić go na stronie, używasz języka HTML `<img>` elementu w następujący sposób:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Czasami jednak musisz mieć możliwość wyświetlania obrazów dynamicznie &#8212; oznacza to, nie wiem, jakie obraz do wyświetlania, dopóki strona nie zostanie uruchomiony.

Procedura w tej sekcji przedstawiono sposób wyświetlania obrazu na bieżąco, w którym użytkownicy określić nazwę pliku obrazu z listy nazw obrazów. Wybierają nazwę obrazu z listy rozwijanej, a gdy przesyła strony, obraz, który została wybrana opcja jest wyświetlana.

![[Obraz] ](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. W programie WebMatrix Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *DynamicImage.cshtml*.
3. W folderze głównym witryny sieci Web, należy dodać nowy folder i nadaj mu nazwę *obrazów*.
4. Dodaj cztery obrazy *obrazów* właśnie utworzony folder. (Wszystkie obrazy mają będzie przydatna, czy, ale powinien mieści się na stronie). Zmiana nazwy obrazów *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, i *Photo4.jpg*. (Nie będzie używać *Photo4.jpg* w tej procedurze, ale użyjemy go w dalszej części tego artykułu.)
5. Upewnij się, że cztery obrazy nie są oznaczone jako tylko do odczytu.
6. Zastąp istniejącą zawartość na stronie następujących czynności:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Ciała strony ma listy rozwijanej ( `<select>` elementu) o nazwie `photoChoice`. Lista zawiera trzy opcje, a `value` atrybut każdej z opcji listy ma nazwę jednego z obrazów, które należy umieścić w *obrazów* folderu. Zasadniczo lista umożliwia użytkownikowi wybranie przyjaznej nazwy, takie jak &quot;1 zdjęcie&quot;, a następnie przekazuje *.jpg* nazwy pliku po przesłaniu strony.

    W kodzie, można uzyskać wybranych przez użytkownika (innymi słowy, nazwa pliku obrazu) z listy, czytając `Request["photoChoice"]`. Należy najpierw sprawdzić, czy zaznaczenie na wszystkich. Jeśli, możesz zbudować ścieżki dla obrazu, który składa się z nazwy folderu obrazów i nazwa pliku obrazu użytkownika. (Jeśli próbowano zbudować ścieżki, ale żadne w `Request["photoChoice"]`, będzie wyświetlany komunikat o błędzie.) Skutkuje to ścieżka względna następująco:

    *obrazy/Photo1.jpg*

    Ścieżka jest przechowywany w zmiennej o nazwie `imagePath` będą potrzebne w dalszej części strony.

    W treści, dostępna jest również `<img>` element, który służy do wyświetlania obrazu, który użytkownik pobrane. `src` Atrybut nie jest równa nazwy pliku lub adresu URL, tak samo, jak można wyświetlić element statyczny. Zamiast tego jest równa `@imagePath`, co oznacza, że pobiera ona swoją wartość ze ścieżki ustawić w kodzie.

    Przy pierwszym uruchomieniu strony, jednak nie ma żadnego obrazu do wyświetlenia, ponieważ użytkownik nie wybrał niczego. To zwykle oznacza, że `src` atrybut może być pusty i obraz, który będzie wyświetlany jako czerwone &quot;x&quot; (lub niezależnie od przeglądarki renderowanie, gdy nie można odnaleźć obrazu). Aby tego uniknąć, możesz umieścić `<img>` element `if` blok, który umożliwia sprawdzenie, aby zobaczyć, czy `imagePath` zmienna ma niczego w nim. Jeśli użytkownik niczego, `imagePath` zawiera ścieżkę. Jeśli użytkownik nie wybierz obraz lub ta strona jest wyświetlana, jeśli jest to po raz pierwszy, `<img>` element nie jest jeszcze renderowane.
7. Zapisz plik i uruchomić strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.)
8. Wybierz obraz z listy rozwijanej, a następnie kliknij przycisk **przykładowy obraz**. Upewnij się, czy widzisz różnych obrazów dla różnych opcji.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Przekazywanie obrazu

W poprzednim przykładzie pokazano sposób wyświetlania obrazu dynamicznie, ale działał tylko w przypadku obrazów, które zostały już w witrynie sieci Web. Ta procedura pokazuje, jak umożliwić użytkownikom przekazywanie obrazu, który następnie jest wyświetlany na stronie. W programie ASP.NET można manipulować obrazów na bieżąco, za pomocą `WebImage` pomocnika, która posiada metody, które umożliwiają tworzenie, modyfikowania i zapisać obrazy. `WebImage` Pomocnika obsługuje wszystkie typowe web obraz typów plików, w tym *.jpg*, *.png*, i *.bmp*. W tym artykule użyjemy *.jpg* obrazów, ale Wy możecie użyć dowolnego typu obraz.

![[Obraz] ](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Dodaj nową stronę i nadaj mu nazwę *UploadImage.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Treść tekstu ma `<input type="file">` element, który umożliwia użytkownikom, wybierz plik do przekazania. Po kliknięciu **przesyłania**, przesyłania pliku, o których one pobrane z formularza.

    Aby uzyskać przekazanego obrazu, należy użyć `WebImage` pomocnika, która udostępnia szeroką gamę użyteczne metody do pracy z obrazami. W szczególności użyj `WebImage.GetImageFromRequest` uzyskać przekazany obraz (jeśli istnieje) i zapisz go w zmiennej o nazwie `photo`.

    Dużo pracy, w tym przykładzie obejmuje pobierania i ustawiania nazwy pliku i ścieżki. Problem polega na tym, czy chcesz uzyskać nazwy (i tylko nazwę) obraz, który użytkownik przekazał, a następnie utwórz nową ścieżkę dla Dokąd chcesz dojść do przechowywania obrazu. Ponieważ użytkownicy potencjalnie może przekazać wiele obrazów, które mają taką samą nazwę, umożliwia nieco dodatkowy kod tworzenia unikatowych nazw i upewnij się, że użytkownicy nie zastępuj istniejących obrazów.

    Jeśli obraz rzeczywiście został przekazany (test `if (photo != null)`), możesz uzyskać nazwę obrazu za pomocą obrazu na `FileName` właściwości. Gdy użytkownik przesyła obraz, `FileName` zawiera oryginalna nazwa użytkownika, który zawiera ścieżkę z komputera użytkownika. Jego może wyglądać następująco:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Te informacje ścieżki nie ma jednak &#8212; Ty chcesz po prostu rzeczywiste nazwy plików (*SamplePhoto1.jpg*). Po prostu pliku ze ścieżki mogą odłączenia przy użyciu `Path.GetFileName` metoda następująco:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Następnie utworzysz nowy unikatową nazwę pliku, dodając identyfikatora GUID na oryginalną nazwę. (Aby uzyskać więcej informacji o identyfikatorach GUID, zobacz [dotyczące identyfikatorów GUID](#SB_AboutGUIDs) w dalszej części tego artykułu.) Następnie możesz utworzyć pełną ścieżkę, która służy do zapisania obrazu. Zapisz ścieżkę składa się z nową nazwę pliku, folderu (obrazy) i bieżącej lokalizacji witryny sieci Web.

    > [!NOTE]
    > W kolejności, w kodzie zapisać plik w *obrazów* folderu, aplikacja musi odczytu i zapisu uprawnienia do tego folderu. Na komputerze deweloperskim to nie jest zazwyczaj problem. Jednak podczas publikowania witryny dostawcy hostingu serwera sieci web, konieczne może być jawnie ustawić te uprawnienia. Jeśli możesz uruchomić ten kod na serwerze dostawcy hostingu i występują błędy, skontaktuj się z dostawcy hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

    Na koniec Przekaż Zapisz ścieżkę do `Save` metody `WebImage` pomocnika. Spowoduje to zapisanie przekazanego obrazu pod nową nazwą. Zapisz metoda wygląda następująco: `photo.Save(@"~\" + imagePath)`. Pełna ścieżka jest dołączany do `@"~\"`, która jest bieżąca lokalizacja witryny sieci Web. (Aby uzyskać informacje o `~` operatora, zobacz [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Co w poprzednim przykładzie treść strony zawiera `<img>` element, aby wyświetlić obraz. Jeśli `imagePath` została ustawiona, `<img>` element jest renderowany i jego `src` ma ustawioną wartość atrybutu `imagePath` wartość.
3. Uruchom stronę w przeglądarce.
4. Przekazywanie obrazu i upewnij się, że jest wyświetlany na stronie.
5. W witrynie, otwórz *obrazów* folderu. Zobaczysz, że został dodany nowy plik których nazwa pliku wygląda następująco: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Jest to obraz, który został przekazany z identyfikatorem GUID poprzedza nazwę. (Własny plik będzie miał inny identyfikator GUID i prawdopodobnie nosi nazwę coś innego niż *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Identyfikatory GUID — informacje
> 
> Identyfikator GUID (globalnie unikatowy identyfikator) jest identyfikatorem, który zazwyczaj jest wyświetlana w formacie następująco: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Cyfry i litery (od A do F) różnią się dla każdego identyfikatora GUID, ale wszystkie one oparte na wzorcu przy użyciu grup 8-4-4-4-12 znaków. (Z technicznego punktu widzenia identyfikator GUID jest liczbą 16-bajtowy/128-bitowy). Gdy będziesz potrzebować identyfikatora GUID, można wywołać wyspecjalizowany kod, który generuje identyfikator GUID dla Ciebie. Idei identyfikatorów GUID jest fakt, że między inwestują rozmiar liczby (3.4 x 10<sup>38</sup>) i algorytm generującego go, wynikowa liczba praktycznie musi być jednym z rodzajem. Identyfikatory GUID są w związku z tym dobrym sposobem na potrzeby generowania nazw, w przypadku elementów, gdy musisz gwarantować, że nie używasz tej samej nazwie dwa razy. Wadą jest oczywiście, że identyfikatory GUID nie są szczególnie przyjazny dla użytkownika i tak często ma być używany, gdy nazwa jest używana tylko w przypadku kodu.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Zmiana rozmiaru obrazu

Witryny sieci Web akceptuje obrazów z użytkownikiem, można zmienić rozmiar obrazów, aby wyświetlić lub zapisać je. Możesz ponownie użyć `WebImage` pomocnika, w tym.

Ta procedura pokazuje, jak zmienić rozmiar przekazanego obrazu do utworzenia miniatury, a następnie Zapisz miniatury i oryginalny obraz w witrynie sieci Web. Wyświetlić miniaturę na stronie i użyj hiperlink, aby przekierować użytkowników do obrazu w pełnym rozmiarze.

![[Obraz] ](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Dodaj nową stronę o nazwie *Thumbnail.cshtml*.
2. W *obrazów* folderze utwórz podfolder o nazwie *thumbs*.
3. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Ten kod jest podobny do kodu z poprzedniego przykładu. Różnica polega na tym, ten kod zapisuje obraz, dwa razy, gdy normalnie i jeden raz po utworzeniu miniatury kopię obrazu. Najpierw pobierz przekazany obraz i zapisz go w *obrazów* folderu. Możesz następnie utworzyć nową ścieżkę dla obraz miniatury. Aby utworzyć miniatury, należy wywołać `WebImage` Pomocnika `Resize` metodę, aby utworzyć obraz 60 pikseli przez 60 pikseli. W przykładzie pokazano, jak zachować współczynnik proporcji i jak można zapobiec obraz jest powiększany (w przypadku, gdy nowy rozmiar będzie faktycznie powiększyć obraz). Obrazów o zmienionym rozmiarze są następnie zapisywane w *thumbs* podfolderu.

    Na końcu znaczników, możesz używać tego samego `<img>` element z dynamicznego `src` atrybut, który widzisz w poprzednich przykładach, aby warunkowo wyświetlany obraz. W takim przypadku możesz wyświetlić miniaturę. Możesz także użyć `<a>` elementu, aby utworzyć hiperłącze do dużych wersję obrazu. Podobnie jak w przypadku `src` atrybutu `<img>` elementu, ustaw `href` atrybutu `<a>` element dynamicznie, aby niezależnie od rodzaju znajduje się w `imagePath`. Aby upewnić się, że ścieżki może działać jako adres URL, możesz przekazać `imagePath` do `Html.AttributeEncode` metody, która konwertuje znaki, które są ok w adresie URL zastrzeżone znaki w ścieżce.
4. Uruchom stronę w przeglądarce.
5. Przekaż zdjęcie i sprawdź, czy jest wyświetlany miniatury.
6. Kliknij miniaturę, aby wyświetlić obraz w pełnym rozmiarze.
7. W *obrazów* i *obrazów/thumbs*, należy pamiętać, że nowe pliki zostały dodane.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Obracanie i przerzucanie obrazu

`WebImage` Pomocnika umożliwia także Przerzucanie i obracanie obrazów. Ta procedura pokazuje, jak pobrać obraz z serwera, przerzucić obraz nogami (w pionie), zapisz go, a następnie Wyświetl odwrócony obraz, na stronie. W tym przykładzie po prostu używasz plików, masz już na serwerze (*Photo2.jpg*). W rzeczywistej aplikacji będzie prawdopodobnie przerzucić obraz o nazwie otrzymasz dynamicznie, jak w poprzednich przykładach.

![[Obraz] ](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Dodaj nową stronę o nazwie *FlipImage.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Kod używa `WebImage` element pomocniczy służący do pobierania obrazu z serwera. Utwórz ścieżkę do obrazu przy użyciu tej samej techniki, które są używane w przykładach wcześniej do zapisywania obrazów i przekazać tę ścieżkę, podczas tworzenia obrazów przy użyciu `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Jeśli obraz zostanie znaleziony, konstruowania nową ścieżkę i nazwę, jak w przypadku wcześniejszych przykładów. Aby przerzucić obraz, należy wywołać `FlipVertical` metody, a następnie zapisać go ponownie.

    Obraz ponownie jest wyświetlany na stronie przy użyciu `<img>` element z `src` ustawioną wartość atrybutu `imagePath`.
3. Uruchom stronę w przeglądarce. Obraz dla *Photo2.jpg* przedstawiono odwrócony.
4. Odśwież stronę lub żądania strony ponownie, aby zobaczyć, że obraz jest odwrócony po prawej stronie się ponownie.

Aby obrócić obraz, należy użyć tego samego kodu, chyba że zamiast wywoływać metodę `FlipVertical` lub `FlipHorizontal`, należy wywołać `RotateLeft` lub `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Dodawanie znaku wodnego do obrazu

Po dodaniu obrazów do swojej witryny sieci Web, możesz chcieć dodać znak wodny do obrazu, zanim go zapisać lub wyświetlić go na stronie. Znaki wodne są często używają Dodaj informacje o prawach autorskich do obrazu lub anonsować ich nazwy firmy.

![[Obraz] ](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Dodaj nową stronę o nazwie *Watermark.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Ten kod jest podobny kod w *FlipImage.cshtml* strony wcześniej (jednak tym razem z zastosowaniem *Photo3.jpg* pliku). Aby dodać znak wodny, należy wywołać `WebImage` Pomocnika `AddTextWatermark` metody, aby zapisać obraz. W wywołaniu `AddTextWatermark`, należy przekazać tekst &quot;test&quot;, ustaw kolor czcionki na żółty i ustaw rodzinę czcionek Arial. (Mimo że nie jest w tym miejscu pokazano `WebImage` pomocnika pozwala także określić nieprzezroczystość rodzinę czcionek i rozmiar czcionki i położenie tekstu znaku wodnego.) Podczas zapisywania obrazu nie może być tylko do odczytu.

    Jak przedstawiono przed, obraz jest wyświetlany na stronie przy użyciu `<img>` element z atrybutem src ustawiony na wartość `@imagePath`.
3. Uruchom stronę w przeglądarce. Zwróć uwagę, tekst "Test" w prawym dolnym rogu obrazu.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Przy użyciu obrazu jako znaku wodnego

Zamiast przy użyciu tekstu dla limitu, możesz użyć innego obrazu. Osoby czasami używać obrazów, takich jak logo firmy jako znaku wodnego, lub używają obrazu znaku wodnego zamiast tekstu, aby uzyskać informacje o prawach autorskich.

![[Obraz] ](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Dodaj nową stronę o nazwie *ImageWatermark.cshtml*.
2. Dodawanie obrazu do *obrazów* folderu pełnić logo, a zmiana nazwy obrazu *MyCompanyLogo.jpg*. Ten obraz powinien być obrazu, który możesz zobaczyć wyraźnie po ustawieniu 80 pikseli szerokości i wysokości 20 pikseli.
3. Zastąp istniejącą zawartość na stronie następujących czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    To jest inna wersja nad kodem we wcześniejszych przykładach. W takim przypadku należy wywołać `AddImageWatermark` dodawania obrazu znaku wodnego do obrazu docelowego (*Photo3.jpg*) przed zapisaniem obrazu. Gdy wywołujesz `AddImageWatermark`, ustaw jego szerokość w pikselach 80 i na 20 pikseli wysokości. *MyCompanyLogo.jpg* obraz jest wyrównany w poziomie w Centrum i wyrównane w pionie w dolnej części obrazu docelowego. Przezroczystość jest ustawiony na 100% i uzupełnienie jest równa 10 pikseli. Jeśli obraz znaku wodnego jest większy niż obrazu docelowego, nic się nie stanie. Obraz znaku wodnego jest większy niż obrazu docelowego, a następnie ustaw dopełnienie obrazu znaku wodnego do zera, znaku wodnego jest ignorowana.

    Jak wcześniej, wyświetlanie obrazów przy użyciu `<img>` elementu i dynamiczny `src` atrybutu.
4. Uruchom stronę w przeglądarce. Należy zauważyć, że obraz znaku wodnego pojawia się w dolnej części głównego obrazu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Praca z plikami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202896)

[Wprowadzenie do programowania z użyciem składni Razor strony sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=251587)
