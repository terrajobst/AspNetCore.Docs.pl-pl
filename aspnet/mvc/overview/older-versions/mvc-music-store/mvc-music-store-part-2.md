---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Część 2: Kontrolery | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 2 obejmuje kontrolerów.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841098"
---
<a name="part-2-controllers"></a>Część 2: kontrolery
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 2 obejmuje kontrolerów.


Za pomocą platform sieci web tradycyjnych przychodzących adresów URL są zwykle mapowane na pliki na dysku. Na przykład: żądania dla adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".

Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób. Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapowania ich adresy URL do metod w klasach. Te klasy są nazywane "Kontrolerów" i są one odpowiedzialne za przetwarzanie przychodzących żądań HTTP, Obsługa danych wejściowych użytkownika, pobierania i zapisywania danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).

## <a name="adding-a-homecontroller"></a>Dodawanie HomeController

Nasza aplikacja MVC Music Store rozpocznie się przez dodawanie klasy kontrolera, który będzie obsługiwać adresy URL do strony głównej witryny firmy Microsoft. Firma Microsoft będzie zgodne z konwencjami nazewnictwa domyślnego platformy ASP.NET MVC i wywołać go HomeController.

Kliknij prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierz pozycję "Dodaj", a następnie polecenie "Kontrolera...":

![](mvc-music-store-part-2/_static/image1.jpg)

Zostanie wyświetlone okno dialogowe "Dodaj kontroler". Nazwa kontrolera "HomeController", a następnie naciśnij przycisk Dodaj.

![](mvc-music-store-part-2/_static/image1.png)

Spowoduje to utworzenie nowego pliku HomeController.cs, używając następującego kodu:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Aby rozpocząć ułatwianiu, Przyjrzyjmy zastąpić Index — metoda prostą metodę, która po prostu zwraca ciąg. My sprawiamy, żeby dwie zmiany:

- Zmiana metody w celu zwrócenia ciągu zamiast element ActionResult
- Zmień instrukcję return powrót do "Hello z domu"

Metoda powinna teraz wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz uruchommy lokacji. Możemy start Nasz serwer sieci web i wypróbować lokacji przy użyciu dowolnej z następujących czynności:

- Wybierz element menu Start Debugging ⇨ debugowania
- Kliknij przycisk zieloną strzałkę na pasku narzędzi ![](mvc-music-store-part-2/_static/image2.jpg)
- Użyj skrótu klawiaturowego, F5.

Przy użyciu dowolnej z powyższych kroków spowoduje kompilacji w projekcie i spowodować, że ASP.NET Development Server, która jest wbudowana w Visual Web Developer można uruchomić. Powiadomienie pojawi się w dolnym rogu ekranu, aby wskazać, że ASP.NET Development Server została uruchomiona i wyświetli numer portu, że jest on uruchomiony pod.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer następnie zostanie automatycznie otwarte okno przeglądarki, w którego adres URL wskazuje na naszym serwerze sieci web. Pozwoli to nam możesz szybko wypróbować usługę naszej aplikacji sieci web:

![](mvc-music-store-part-2/_static/image3.png)

Porządku, to było całkiem szybkiego — utworzyliśmy nową witrynę sieci Web, dodane trzy śródwierszową i mamy tekstu w przeglądarce. Nie okazji do analizy, ale jest rozpoczęcia.

*Uwaga: Visual Web Developer obejmuje ASP.NET Development Server, co spowoduje uruchomienie witryny sieci Web na wielu bezpłatnych losowy "port". Na powyższym zrzucie ekranu, witryna działa na `http://localhost:26641/`, więc używa portu 26641. Numer portu będzie różna. Odnosimy /Store/Browse like adresy URL w tym samouczku, które zaczną po numer portu. Zakładając, że numer portu w danych 26641, przechodząc do/Store/przeglądania oznacza przejście do `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Dodawanie StoreController

Dodaliśmy HomeController prostego, który implementuje strony głównej witryny firmy Microsoft. Teraz Dodajmy inny kontroler, który użyjemy do implementacji funkcji przeglądania naszego sklepu music. Kontrolera magazynu obsługuje trzy scenariusze:

- Strona listy gatunki muzyki w naszym Sklepie utworów muzycznych
- Strony przeglądania, który wymienia wszystkie albumów muzycznych określonego rodzaju
- Strona zawierająca szczegóły informacji na temat albumu określonych utworów muzycznych

Rozpoczniemy pracę, dodając nową klasę StoreController... Nie jest jeszcze można zatrzymać aplikacji przez zamknięcie przeglądarki lub wybierając element menu Zatrzymaj debugowanie ⇨ debugowania.

Teraz można dodać nowe StoreController. Tak samo, jak postępowanie z HomeController, możemy to zrobić, klikając prawym przyciskiem myszy w folderze "Kontrolerów" w Eksploratorze rozwiązań i wybierając Add -&gt;kontrolera elementu menu.

![](mvc-music-store-part-2/_static/image4.png)

Nasz nowy StoreController ma już metodę "Index". Użyjemy tej metody "Index", aby zaimplementować naszą stronę listy, który zawiera listę wszystkich gatunki w naszym music store —. Dodamy również dwie dodatkowe metody do zaimplementowania dwa inne scenariusze chcemy, aby nasz StoreController do obsługi: Przeglądaj i szczegółowe informacje.

Te metody (indeks i przeglądania szczegółów) w ramach kontrolera są nazywane "Akcji kontrolera", a jako już przeczytane z metodą akcji HomeController.Index (), swojej pracy ma odpowiadać na żądania adres URL i (ogólnie rzecz biorąc) określić zawartość powinny być wysyłane z powrotem do przeglądarki lub użytkownik, który wywołał adresu URL.

Zaczniemy naszej implementacji StoreController, zmieniając theIndex() metoda zwraca ciąg "Hello z Store.Index()" i dodamy podobne metody Browse() i Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Uruchom projekt ponownie i Przeglądaj następujące adresy URL:

- / Store
- / Store/przeglądania
- / Store/Details

Uzyskiwanie dostępu do tych adresów URL wywołania metody akcji w ramach kontrolera i zwracania odpowiedzi ciąg:

![](mvc-music-store-part-2/_static/image5.png)

To doskonałe rozwiązanie, ale są tylko stałe ciągi. Upewnijmy się, ich dynamiczny, dzięki czemu pobierają informacje z adresu URL i wyświetlania ich w danych wyjściowych strony.

Pierwsze zmienimy metody akcji przeglądania można pobrać wartości querystring w adresie URL. Możemy to zrobić, dodając parametr "gatunku" do naszych metody akcji. W takim przypadku to ASP.NET MVC automatycznie przekazują żadnych parametrów post formularza lub ciągu kwerendy o nazwie "gatunku" do naszych metody akcji, gdy jest wywoływana.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Uwaga: Firma Microsoft za pomocą metody narzędzie HttpUtility.HtmlEncode oczyszczają danych wejściowych użytkownika. Uniemożliwia to użytkownikom wprowadzanie Javascript do naszych widoku z linkiem, takich jak /Store/Browse? Gatunku =&lt;skryptu&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*

Teraz możemy przejść do/Store/przeglądania? Gatunku = Najdywania

![](mvc-music-store-part-2/_static/image6.png)

Następnie zmienimy akcji Details do odczytywania i wyświetlić parametr wejściowy identyfikator. W odróżnieniu od naszych Poprzednia metoda firma Microsoft nie będzie można osadzanie wartość Identyfikatora jako parametr ciągu kwerendy. Zamiast tego osadzimy, go bezpośrednio z poziomu samego adresu. Na przykład: /Store/Details/5.

ASP.NET MVC pozwala nam to łatwo zrobić bez konieczności nic konfigurować. Konwencji routingu domyślnego platformy ASP.NET MVC jest segment adresu URL po nazwie metody akcji jako parametr o nazwie "ID". Jeśli metoda akcji zawiera parametr o nazwie identyfikator następnie platformy ASP.NET MVC zostanie automatycznie przekazać segment adresu URL dla Ciebie jako parametr.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Uruchom aplikację, a następnie przejdź do /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Przypomnijmy przedstawiające wprowadzone aktualizacje do tej pory:

- Utworzyliśmy nowy projekt ASP.NET MVC w Visual Web Developer
- Wiemy już struktury folderów podstawowych aplikacji ASP.NET MVC
- Nauczyliśmy się sposobu uruchamiania naszej witryny sieci Web za pomocą programu ASP.NET Development Server
- Utworzyliśmy dwie klasy kontrolera: HomeController i StoreController
- Dodaliśmy metod akcji do naszych kontrolery, które odpowiadają na żądania adres URL i zwracają tekst w przeglądarce


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-1.md)
> [dalej](mvc-music-store-part-3.md)
