---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Praca z obrazami w witryny ASP.NET Web Pages (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: "W tym rozdziale przedstawiono sposób dodawania, wyświetlania i modyfikowania obrazów (zmienianie rozmiaru, przerzucania i dodać znaki wodne) w witrynie sieci Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Praca z obrazami w lokacji (Razor) stron sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule przedstawiono sposób dodawania, wyświetlania i modyfikowania obrazów (zmienianie rozmiaru, przerzucania i dodać znaki wodne) w witrynie sieci Web platformy ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak dodać obraz do strony dynamicznie.
> - Jak umożliwić użytkownikom przekazywanie obrazu.
> - Jak zmienić rozmiar obrazu.
> - Jak przerzucanie i obracanie obrazów.
> - Jak dodać do obrazu znaku wodnego.
> - Jak używać obrazu jako znaku wodnego.
> 
> Są to programowania funkcje dodane w artykule programu ASP.NET:
> 
> - `WebImage` Pomocnika.
> - `Path` Obiektu, który udostępnia metody, które umożliwiają manipulowania nazwę pliku i ścieżkę.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Program WebMatrix 2
>   
> 
> W tym samouczku współdziała również z 3 programu WebMatrix.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Dodawanie obrazu do strony sieci Web dynamicznie

Można dodać obrazy do witryny sieci Web i poszczególnych stron, podczas projektujesz witryny sieci Web. Można także pozwolić użytkownikom przekazywanie obrazów, które mogą być przydatne dla zadania, takie jak pozwalając im na dodawanie zdjęcia profilu.

Jeśli obraz nie jest jeszcze dostępna w witrynie i chcesz go wyświetlić na stronie, użyj HTML `<img>` element następująco:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Czasami jednak musisz mieć możliwość wyświetlania obrazów dynamicznie &#8212; oznacza to nie wiadomo, jakie obraz do wyświetlania aż do strony jest uruchomiona.

Procedura w tej sekcji przedstawiono sposób wyświetlania obrazu na bieżąco, w którym użytkownicy określić nazwę pliku obrazu z listy nazw obrazów. Nazwa obrazu one wybierz z listy rozwijanej i po przesyłają strony one wybranego obrazu.

![[Obraz] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. W programie WebMatrix Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *DynamicImage.cshtml*.
3. W folderze głównym witryny sieci Web, należy dodać nowy folder i nadaj mu nazwę *obrazów*.
4. Dodaj cztery obrazy do *obrazów* właśnie utworzony folder. (Wszystkie obrazy masz będą przydatne czy, ale powinny mieścić się na stronie). Zmień nazwę obrazy *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, i *Photo4.jpg*. (Nie będzie używać *Photo4.jpg* w tym procedura, ale będzie używać go w dalszej części tego artykułu.)
5. Upewnij się, że cztery obrazy nie są oznaczone jako tylko do odczytu.
6. Zastąp istniejącą zawartość na stronie następujące czynności:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Ciała strony ma listy rozwijanej ( `<select>` element) o nazwie `photoChoice`. Lista zawiera trzy opcje i `value` atrybut każdej opcji listy ma nazwę jednego z obrazów, które należy umieścić w *obrazów* folderu. Zasadniczo listy umożliwia użytkownikowi wybieranie przyjaznej nazwy, takie jak &quot;1 fotografii&quot;, a następnie przekazuje *.jpg* nazwę pliku po przesłaniu strony.

    W kodzie, można uzyskać określonego przez użytkownika (innymi słowy, nazwa pliku obrazu) z listy, odczytując `Request["photoChoice"]`. Należy najpierw sprawdzić, czy zaznaczenie w ogóle. Jeśli istnieje, można zbudować ścieżki dla obrazu, który składa się z nazwy folderu obrazów i nazwę pliku obrazu użytkownika. (Jeśli podjęto próbę utworzenia ścieżki, ale nie było postanowienia `Request["photoChoice"]`, będzie wyświetlany komunikat o błędzie.) Powoduje to ścieżka względna następująco:

    *obrazy/Photo1.jpg*

    Ścieżka jest przechowywany w zmiennej o nazwie `imagePath` będą potrzebne później na stronie.

    W treści, dostępna jest również `<img>` element, który służy do wyświetlania obrazu, który zostanie pobrana. `src` Atrybut nie jest ustawiony na nazwę pliku lub adres URL, tak, aby wyświetlić element statyczny, należy wykonać. Zamiast tego jest równa `@imagePath`, co oznacza, że pobiera ona swoją wartość ze ścieżki ustawić w kodzie.

    Po raz pierwszy uruchamia strony, jednak nie ma żadnego obrazu do wyświetlenia, ponieważ użytkownik nie wybrał żadnych czynności. To będzie zwykle oznaczają, że `src` atrybut może być pusty i obrazu będzie wyświetlany jako czerwony &quot;x&quot; (lub niezależnie od przeglądarki renderuje, gdy nie można odnaleźć obrazu). Aby tego uniknąć, możesz zaznaczyć `<img>` element `if` bloku, który umożliwia sprawdzenie, aby sprawdzić, czy `imagePath` zmienna ma niczego w nim. Jeśli użytkownik niczego, `imagePath` zawiera ścieżkę. Jeśli użytkownik nie wybierz obraz lub ta strona jest wyświetlana, jeśli po raz pierwszy, `<img>` elementu nawet nie jest renderowany.
7. Zapisz plik i uruchomić strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)
8. Wybierz obraz z listy rozwijanej, a następnie kliknij przycisk **obraz przykładowy**. Upewnij się, że widoczny różnych obrazów dla różnych opcji.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Przekazywanie obrazu

W poprzednim przykładzie pokazano sposób wyświetlania obrazu dynamicznie, ale działał tylko z obrazami, które zostały już w witrynie sieci Web. Za pomocą poniższej procedury zezwolić użytkownikom na przekazywanie obrazu, który następnie jest wyświetlany na stronie. W programie ASP.NET, można manipulować obrazów na bieżąco przy użyciu `WebImage` pomocnika, mającej metody, które umożliwiają tworzenia, modyfikowania i zapisywania obrazów. `WebImage` Pomocnika obsługuje wszystkie wspólne web obrazu typy plików, łącznie z *.jpg*, *.png*, i *.bmp*. W tym artykule użyjesz *.jpg* obrazów, ale można użyć dowolnego typu obrazu.

![[Obraz] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Dodaj nową stronę i nadaj mu nazwę *UploadImage.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Jednostka tekstu `<input type="file">` element, który umożliwia użytkownikom, wybierz plik do przekazania. Gdy użytkownik klika polecenie **przesyłania**, przesyłania pliku, są pobierane z formularza.

    Aby uzyskać załadowanego obrazu, należy użyć `WebImage` Pomocnik, którego szerokiej gamy przydatne metody do pracy z obrazami. W szczególności należy używać `WebImage.GetImageFromRequest` uzyskać załadowanego obrazu (jeśli istnieje) i zapisz go w zmiennej o nazwie `photo`.

    Dużo pracy w tym przykładzie obejmuje pobieranie i ustawianie nazwy pliku i ścieżkę. Problem polega na tym, czy chcesz pobrać nazwę (i tylko nazwę) obraz, który zostanie przekazany, a następnie utwórz nową ścieżkę, do których użytkownik chce przechowywania obrazu. Ponieważ użytkownicy potencjalnie może przekazać wiele obrazów, które mają taką samą nazwę, służy do tworzenia unikatowych nazw i upewnij się, że użytkownicy nie zastępuj istniejący obraz bit dodatkowy kod.

    Jeśli obraz faktycznie został przekazany (test `if (photo != null)`), uzyskać nazwę obrazu z obrazu `FileName` właściwości. Gdy użytkownik przesyła obraz, `FileName` zawiera oryginalna nazwa użytkownika, który zawiera ścieżki z komputera użytkownika. Go może wyglądać następująco:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Nie ma jednak te informacje Ścieżka &#8212; chcesz rzeczywiste nazwy plików (*SamplePhoto1.jpg*). Tylko plik ze ścieżki można usuwają przy użyciu `Path.GetFileName` metody następująco:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Następnie można utworzyć nowego unikatową nazwę pliku, dodając identyfikator GUID oryginalną nazwę. (Aby uzyskać więcej informacji na temat identyfikatorów GUID, zobacz [o identyfikatorów GUID](#SB_AboutGUIDs) dalszej części tego artykułu.) Następnie należy utworzyć pełną ścieżkę, która służy do zapisania obrazu. Zapisz ścieżkę składa się z nową nazwę pliku, folderu (obrazy) i bieżącej lokalizacji witryny sieci Web.

    > [!NOTE]
    > Aby swój kod, aby zapisać pliki w *obrazów* folderu, aplikacja musi odczytu i zapisu uprawnienia dla tego folderu. Na komputerze deweloperskim to nie jest zazwyczaj problem. Jednak opublikowanie witryny dostawcy hostingu serwera sieci web, należy jawnie ustawić te uprawnienia. Jeśli uruchomienie tego kodu na serwerze dostawcy hostingu i występują błędy, skontaktuj się z dostawcy hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

    Na koniec należy przekazać Zapisz ścieżkę do `Save` metody `WebImage` pomocnika. Spowoduje to zapisanie załadowanego obrazu pod nową nazwą. Zapisz metody wygląda następująco: `photo.Save(@"~\" + imagePath)`. Pełna ścieżka jest dołączany do `@"~\"`, która jest bieżącą lokalizację witryny sieci Web. (Aby uzyskać informacje o `~` operatora, zobacz [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Tak jak w poprzednim przykładzie ciała strony zawiera `<img>` element, aby wyświetlić obraz. Jeśli `imagePath` została ustawiona, `<img>` element jest renderowany i jego `src` atrybut ma ustawioną `imagePath` wartość.
3. Uruchom strony w przeglądarce.
4. Przekaż obraz i upewnij się, że jest wyświetlany na stronie.
5. W witrynie, otwórz *obrazów* folderu. Zobacz, czy plik został dodany których nazwa pliku wygląda następująco: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Jest to obraz, który został przekazany za pomocą identyfikatora GUID prefiksem nazwy. (Własny plik ma inny identyfikator GUID i prawdopodobnie nosi nazwę inną niż *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Identyfikatory GUID — informacje
> 
> Identyfikator GUID (globalnie unikatowy identyfikator) jest identyfikatorem, który zazwyczaj jest renderowany w formacie następująco: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Cyfry i litery (od A do F) są różne dla każdego identyfikatora GUID, ale wszystkie one oparte na wzorcu przy użyciu grup 8-4-4-4-12 znaków. (Technicznie, identyfikator GUID jest liczbą 16-bajtowych/128-bitowe). Jeśli potrzebujesz identyfikatora GUID należy wywołać wyspecjalizowany kod, który generuje identyfikator GUID dla Ciebie. Ideą identyfikatorów GUID jest to, że między znaczne rozmiary numer (3,4 x 10<sup>38</sup>) i algorytm generowania go wynikowa liczba jest praktycznie gwarancji jednego rodzaju. Identyfikatory GUID w związku z tym to dobry sposób na potrzeby generowania nazw elementów, gdy użytkownik gwarantuje, że nie używasz takiej samej nazwie dwa razy. Wadą interfejsu, jest, że identyfikatory GUID nie są szczególnie przyjazny dla użytkownika, więc mają zwykle można użyć, gdy nazwa jest używana tylko w kodzie.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Zmiana rozmiaru obrazu

Witryny sieci Web akceptuje obrazów z użytkownikiem, można zmienić rozmiar obrazów, aby wyświetlić lub zapisać je. Można ponownie użyć `WebImage` pomocnika dla tego.

W tej procedurze pokazano, jak zmienić rozmiar załadowanego obrazu, aby utworzyć miniaturę, a następnie zapisać miniatur i oryginalnego obrazu w witrynie internetowej. Wyświetlania miniatury na stronie i używać hiperłącza do przekierowywania użytkowników do obrazu w pełnym rozmiarze.

![[Obraz] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Dodaj nową stronę o nazwie *Thumbnail.cshtml*.
2. W *obrazów* folderu, utwórz podfolder o nazwie *Kciuki*.
3. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Ten kod jest podobny do kodu z poprzedniego przykładu. Różnica polega na ten kod zapisuje obraz, dwa razy, raz normalnie i jeden raz po utworzeniu miniatur kopię obrazu. Najpierw uzyskać załadowanego obrazu i zapisać ją w *obrazów* folderu. Następnie utworzymy nową ścieżkę dla obraz miniatury. Aby utworzyć faktycznie miniaturę, należy wywołać `WebImage` przez pomocnika `Resize` metodę, aby utworzyć obraz 60 pikseli przez 60 pikseli. W przykładzie pokazano sposób zachowania proporcji i jak można zapobiec obrazu trwa powiększenia (w przypadku nowego rozmiaru czy rzeczywiście powiększenia obrazu). Obraz o zmienionym rozmiarze jest następnie zapisywana w *Kciuki* podfolderu.

    Na koniec kod znaczników, używać tego samego `<img>` element z dynamicznej `src` atrybut, który przedstawiono w poprzednich przykładach warunkowo pokazanie obrazu. W takim przypadku możesz wyświetlić miniatury. Można również użyć `<a>` elementu, aby utworzyć hiperłącza do big wersję obrazu. Jak `src` atrybutu `<img>` element, należy ustawić `href` atrybutu `<a>` element dynamicznie, tak aby niezależnie od znajduje się w `imagePath`. Aby upewnić się, że ścieżka może działać jako adres URL, Przekaż `imagePath` do `Html.AttributeEncode` metodę, która konwertuje znaki, które są ok w adresie URL zarezerwowanych znaków w ścieżce.
4. Uruchom strony w przeglądarce.
5. Przekaż zdjęcie i sprawdź, czy jest wyświetlany miniatury.
6. Kliknij miniaturę, aby wyświetlić obraz w pełnym rozmiarze.
7. W *obrazów* i *obrazów/Kciuki*, należy pamiętać, że zostały dodane nowe pliki.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Obracanie i przerzucanie obrazu

`WebImage` Pomocnika umożliwia także Przerzucanie i obracanie obrazów. W tej procedurze pokazano, jak pobrać obrazu z serwera, Przerzucanie obrazu odwrócona (pionowo), zapisz go, a następnie Wyświetl odwrócony obraz na stronie. W tym przykładzie tylko używasz plik istnieje już na serwerze (*Photo2.jpg*). W rzeczywistej aplikacji będzie prawdopodobnie Przerzucanie obrazu o nazwie otrzymasz dynamicznie, tak jak w poprzednich przykładach.

![[Obraz] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Dodaj nową stronę o nazwie *FlipImage.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    W kodzie użyto `WebImage` pomocnika można pobrać obrazu z serwera. Utwórz ścieżkę do obrazu przy użyciu tę samą metodę używany w przykładach wcześniejszych zapisywania obrazów i przekazania tej ścieżki, podczas tworzenia obrazu przy użyciu `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Jeśli obraz zostanie znaleziony, utworzymy nową ścieżkę i nazwę, jak w starszych przykłady. Aby Przerzucanie obrazu, należy wywołać `FlipVertical` metody, a następnie zapisać go ponownie.

    Obraz jest ponownie wyświetlana na stronie przy użyciu `<img>` element z `src` ustawić atrybutu `imagePath`.
3. Uruchom strony w przeglądarce. Obraz dla *Photo2.jpg* przedstawiono odwrócony.
4. Odśwież stronę lub żądania strony ponownie, aby zobaczyć, że obraz jest odwrócony po prawej stronie się ponownie.

Obracanie obrazów, możesz użyć tego samego kodu, z wyjątkiem zamiast wywoływać metodę `FlipVertical` lub `FlipHorizontal`, należy wywołać `RotateLeft` lub `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Dodawanie do obrazu znaku wodnego

Po dodaniu obrazów do witryny sieci Web, można dodać do obrazu znaku wodnego, zanim zostanie zapisany lub wyświetl ją na stronie. Aby dodać informacje o prawach autorskich do obrazu lub anonsowanie ich firma ludzie często używają znaków wodnych.

![[Obraz] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Dodaj nową stronę o nazwie *Watermark.cshtml*.
2. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Ten kod jest podobny kod w *FlipImage.cshtml* strony z wcześniej (ale tym razem używa *Photo3.jpg* pliku). Aby dodać znaku wodnego, należy wywołać `WebImage` przez pomocnika `AddTextWatermark` metody, aby zapisać obraz. W wywołaniu `AddTextWatermark`, Przekaż tekst &quot;test&quot;Ustaw kolor czcionki żółty, a wartość Arial rodziny czcionek. (Mimo że nie jest w tym miejscu pokazano `WebImage` Pomocnik pozwala także określić nieprzezroczystość rodziny czcionek i rozmiar czcionki i położenie tekstu znaku wodnego.) Podczas zapisywania obrazu nie może być tylko do odczytu.

    Jak przedstawiono przed, obraz jest wyświetlany na stronie przy użyciu `<img>` element z atrybutem src ustawiony na wartość `@imagePath`.
3. Uruchom strony w przeglądarce. Zwróć uwagę tekst "Test" w prawym dolnym rogu obrazu.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Jako znaku wodnego przy użyciu obrazu

Zamiast przy użyciu tekstu znaku wodnego, możesz użyć innego obrazu. Osoby czasami użyć obrazów, takich jak logo firmy jako znaku wodnego lub ich obraz znaku wodnego zamiast tekstu informacjami o prawach autorskich.

![[Obraz] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Dodaj nową stronę o nazwie *ImageWatermark.cshtml*.
2. Dodawanie obrazu do *obrazów* folderu używany jako logo i Zmień nazwę obrazu *MyCompanyLogo.jpg*. Ten obraz powinien być obrazu, który widać wyraźnie po ustawieniu 80 pikseli szerokości i wysokości 20 pikseli.
3. Zastąp istniejącą zawartość na stronie następujące czynności: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Jest to inny wariant kod w przykładach wcześniejszych. W takim przypadku należy wywołać `AddImageWatermark` można dodać obrazu znaku wodnego do obrazu docelowego (*Photo3.jpg*) przed zapisaniem obrazu. Podczas wywoływania `AddImageWatermark`, ustaw jej szerokość 80 pikseli i wysokości do 20 pikseli. *MyCompanyLogo.jpg* obraz jest wyrównania w poziomie w Centrum i wyrównane w pionie w dolnej części obrazu docelowego. Przezroczystość ma ustawioną wartość 100% i uzupełnienie jest równa 10 pikseli. Jeśli obraz znaku wodnego jest większy niż obrazu docelowego, nic się nie stanie. Jeśli obraz znaku wodnego jest większy niż obrazu docelowego i ustawić dopełnienie obrazu znaku wodnego do zera, znak wodny zostanie zignorowany.

    Jak wcześniej, wyświetlić przy użyciu obrazu `<img>` elementu i dynamicznym `src` atrybutu.
4. Uruchom strony w przeglądarce. Należy zauważyć, że obrazu znaku wodnego, który znajduje się w dolnej części głównego obrazu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Praca z plikami w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Wprowadzenie do programowania przy użyciu składni Razor strony sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=251587)
