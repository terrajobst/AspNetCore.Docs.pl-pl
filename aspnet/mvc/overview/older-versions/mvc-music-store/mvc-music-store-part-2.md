---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: "Część 2: Kontrolerów | Dokumentacja firmy Microsoft"
author: jongalloway
description: "Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 2 obejmuje kontrolerów."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a>Część 2: kontrolerów
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 2 obejmuje kontrolerów.


Web tradycyjnych struktur przychodzących adresów URL są zwykle mapowane do plików na dysku. Na przykład: żądanie dla danego adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".

Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób. Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapują adresy URL do metody klasy. Klasy te są nazywane "Kontrolerów" i są one odpowiedzialna za przetwarzanie przychodzących żądań HTTP w celu obsługi danych wejściowych użytkownika, pobieranie i zapisywanie danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).

## <a name="adding-a-homecontroller"></a>Dodawanie HomeController

Naszej aplikacji MVC utworów muzycznych magazynu rozpocznie się przez dodanie klasy kontrolera, który będzie obsługiwać adresy URL do strony głównej witryny firmy Microsoft. Firma Microsoft będzie wykonaj domyślnych konwencji nazewnictwa platformy ASP.NET MVC i wywołać go HomeController.

Kliknij prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierz opcję "Dodaj" i "Kontrolera..." polecenie:

![](mvc-music-store-part-2/_static/image1.jpg)

Zostanie wyświetlone okno dialogowe "Dodaj kontroler". Nazwa kontrolera "HomeController" i naciśnij przycisk Dodaj.

![](mvc-music-store-part-2/_static/image1.png)

Spowoduje to utworzenie nowego pliku HomeController.cs, z następującym kodem:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Aby uruchomić ułatwianiu, umożliwia Zamień metody indeksu prosta metoda, która po prostu zwraca ciąg. Wybierzemy dwie zmiany:

- Zmień metodę zwraca ciąg zamiast element ActionResult
- Zmień instrukcję return powrót do "Hello z domu"

Metoda powinna wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz załóżmy uruchamiania witryny. Możemy uruchomić naszych serwera sieci web i wypróbować lokacji przy użyciu następujących::

- Wybierz element menu Rozpocznij debugowanie ⇨ debugowania
- Kliknij przycisk zieloną strzałkę na pasku narzędzi![](mvc-music-store-part-2/_static/image2.jpg)
- Użyj skrótu klawiaturowego, F5.

Przy użyciu dowolnego z powyższych kroków będzie skompilować naszych projektu, a następnie spowodować ASP.NET Development Server jest wbudowane Visual Web Developer można uruchomić. Powiadomienia będą wyświetlane w dolnym rogu ekranu, aby wskazać, że ASP.NET Development Server rozpoczęła i będzie wyświetlany numer portu, czy działa w ramach.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer następnie zostanie automatycznie Otwórz okno przeglądarki, w których adres URL wskazuje naszych serwera sieci web. Pozwoli to nam możesz szybko wypróbować usługę naszej aplikacji sieci web:

![](mvc-music-store-part-2/_static/image3.png)

OK, który był dość szybki — utworzyliśmy nową witrynę sieci Web, dodać funkcję trzech linii i mamy tekstu w przeglądarce. Nie okazji nauki, ale jest rozpoczęcia.

*Uwaga: Visual Web Developer zawiera ASP.NET Development Server, co spowoduje uruchomienie witryny sieci Web na liczby losowe wolnego "port". Na zrzucie ekranu powyżej, witryna jest uruchomiona na `http://localhost:26641/`, dlatego używa portu 26641. Numer portu będzie różna. Przy omawianiu /Store/Browse podobnego adresy URL, w tym samouczku, które przechodzą po numer portu. Zakładając, że numer portu z 26641, przechodząc do sklepu/Przeglądaj oznacza przeglądania `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Dodawanie StoreController

Dodaliśmy HomeController proste, który implementuje strony głównej witryny. Teraz Dodajmy innego kontrolera, które będą używane do implementowania przeglądania naszych magazynu utworów muzycznych. Kontrolera magazynu obsługuje trzy scenariusze:

- Strony listę gatunkami muzyki utworów muzycznych w naszym magazynie utworów muzycznych
- Strona przeglądania, która zawiera listę wszystkich albumów muzycznych określonego rodzaju
- Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych

Zaczniemy przez dodanie nowych klas StoreController... Jeśli nie jest jeszcze zatrzymać uruchomienie aplikacji przez zamknięcie przeglądarki lub wybranie ⇨ debugowania Zatrzymaj debugowanie elementu menu.

Teraz Dodaj nowe StoreController. Tak samo, jak robiliśmy z HomeController, firma Microsoft będzie to zrobić przez kliknięcie prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierając Add -&gt;kontrolera elementu menu

![](mvc-music-store-part-2/_static/image4.png)

Naszej nowej StoreController już ma metodę "Index". Aby zaimplementować stronę dotyczącą listę zawierającą wszystkie genres w naszym magazynie utworów muzycznych użyjemy tej metody "Index". Dodamy również dwie dodatkowe metody służące do implementacji dwa inne scenariusze chcemy naszych StoreController do obsługi: przeglądania i szczegóły.

Te metody (indeks, przeglądania i szczegóły) w ramach kontrolera są nazywane "Akcji kontrolera" i jako już zapoznaniu się z metodą akcji HomeController.Index (), ich zadanie ma odpowiadać na żądania adresu URL i (ogólnie rzecz biorąc) określenie zawartości ma zostać odesłana do przeglądarki lub użytkownik, który wywołał adres URL.

Zaczniemy implementacja naszej StoreController zmieniając theIndex() metoda zwraca ciąg "Hello z Store.Index()" i Browse() i Details() dodamy podobnych metod:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Uruchom ponownie projekt, a następnie przejdź do następujących adresów URL:

- / Store
- / Magazynu/przeglądania
- / / Szczegóły magazynu

Uzyskiwanie dostępu do tych adresów URL wywołania metody akcji w obrębie kontrolera i zwraca ciąg odpowiedzi:

![](mvc-music-store-part-2/_static/image5.png)

To świetnie, ale są tylko stałe ciągów. Można wprowadzić je dynamiczny, aby pobrać informacje z adresu URL i wyświetl ją w danych wyjściowych strony.

Najpierw zmienimy metody akcji przeglądania można pobrać wartości querystring w adresie URL. Firma Microsoft można to zrobić przez dodanie parametru "rodzaju" do naszej metody akcji. W takim przypadku to ASP.NET MVC automatycznie przejdzie żadnych parametrów post formularza lub ciągu kwerendy o nazwie "rodzaju" do naszej metody akcji, gdy jest wywoływana.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Uwaga: Firma Microsoft korzysta z metody narzędzie HttpUtility.HtmlEncode do oczyszczenia danych wejściowych użytkownika. Zapobiega to wstrzykiwania Javascript do naszej widoku z łączem, takich jak /Store/Browse użytkowników? Genre =&lt;skryptu&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*

Teraz załóżmy przejdź do sklepu/Przeglądaj? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Zmieńmy obok akcji Details do odczytu i wyświetlanie parametru wejściowego o nazwie identyfikatora. W przeciwieństwie do naszej poprzedniej — metoda firma Microsoft nie będzie można osadzanie wartość Identyfikatora jako parametr querystring. Zamiast tego firma Microsoft będzie osadź go bezpośrednio w ramach samego adresu. Na przykład: /Store/Details/5.

ASP.NET MVC umożliwia nam to łatwo zrobić bez konieczności konfigurowania. ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie "ID". Jeśli stosowana metoda akcji ma parametr o nazwie identyfikator platformy ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Uruchom aplikację i przejdź do /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Załóżmy recap, co możemy wykonane wykonanej do tej pory:

- W programie Visual Web Developer utworzyliśmy nowy projekt ASP.NET MVC
- Zostały omówione struktury folderów podstawowe aplikacji platformy ASP.NET MVC
- Zostały dowiedzieliśmy się, jak uruchomić naszą witrynę sieci Web za pomocą programu ASP.NET Development Server
- Utworzyliśmy dwa klasy kontrolera: HomeController i StoreController
- Dodaliśmy metod akcji do naszej kontrolerów, które odpowiadają na żądania adresu URL i zwrócić tekst do przeglądarki


>[!div class="step-by-step"]
[Poprzednie](mvc-music-store-part-1.md)
[dalej](mvc-music-store-part-3.md)
