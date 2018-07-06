---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Objaśnienie modeli, widoków i kontrolerów (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Masz wątpliwości dotyczące modeli, widoków i kontrolerów? W tym samouczku Walther Autor: Stephen poznasz różne części aplikacji ASP.NET MVC.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: c651201a34b9ab6b459d0f2ecf491b49feb64434
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399902"
---
<a name="understanding-models-views-and-controllers-vb"></a>Objaśnienie modeli, widoków i kontrolerów (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Masz wątpliwości dotyczące modeli, widoków i kontrolerów? W tym samouczku Walther Autor: Stephen poznasz różne części aplikacji ASP.NET MVC.


Ten samouczek umożliwia ogólne omówienie platformy ASP.NET MVC modeli, widoków i kontrolerów. Innymi słowy, wyjaśniono M ", V", a C "we wzorcu ASP.NET MVC.

Po przeczytaniu tego samouczka, należy zrozumieć, jak różne części aplikacji ASP.NET MVC współpracują ze sobą. Należy również zrozumieć, jak Architektura aplikacji ASP.NET MVC różni się od aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Przykładowa aplikacja platformy ASP.NET MVC

Domyślny szablon programu Visual Studio do tworzenia aplikacji sieci Web programu ASP.NET MVC zawiera bardzo prostą przykładowej aplikacji, która może służyć do zrozumienia różne części aplikacji ASP.NET MVC. Firma Microsoft może korzystać z tej prostej aplikacji, w tym samouczku.

Tworzenie nowej aplikacji platformy ASP.NET MVC za pomocą szablonu MVC, uruchamiając program Visual Studio 2008 i wybranie opcji menu Plik, nowy projekt (patrz rysunek 1). W oknie dialogowym Nowy projekt, wybierz ulubionym języku programowania w ramach typów projektu (Visual Basic lub C#) i ustaw **aplikacji sieci Web programu ASP.NET MVC** w obszarze Szablony. Kliknij przycisk OK.


[![Okno dialogowe nowego projektu](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Rysunek 01**: okno dialogowe nowego projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-vb/_static/image2.png))


Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, **Tworzenie projektu testu jednostkowego** zostanie wyświetlone okno dialogowe (patrz rysunek 2). To okno dialogowe umożliwia utworzenie oddzielnego projektu w rozwiązaniu do testowania aplikacji ASP.NET MVC. Wybierz opcję **nie twórz projektu testu jednostkowego** i kliknij przycisk **OK** przycisku.


[![Tworzenie testów jednostkowych w oknie dialogowym](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Rysunek 02**: Tworzenie jednostki Test dialogowy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-vb/_static/image4.png))


Aplikacja zostanie utworzona po nowe platformy ASP.NET MVC. Pojawi się kilka folderów i plików w oknie Eksploratora rozwiązań. W szczególności zostaną wyświetlone trzy foldery o nazwie modeli, widoków i kontrolerów. Jak może odgadnięcia nazw folderów te foldery zawierają pliki dotyczące wdrażania modeli, widoków i kontrolerów.

Po rozwinięciu folderze kontrolery powinien zostać wyświetlony plik o nazwie AccountController.vb i plik o nazwie HomeController.vb. Po rozwinięciu folderze Widoki powinny być widoczne trzy podfoldery o nazwie konto Home i udostępnione. Po rozwinięciu folderu macierzystego zobaczysz dwa dodatkowe pliki o nazwie About.aspx i Index.aspx (zobacz rysunek 3). Te pliki składają się przykładowa aplikacja uwzględniony przy użyciu domyślnego szablonu platformy ASP.NET MVC.


[![Okno Eksploratora rozwiązań](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Rysunek 03**: okno Eksploratora rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-vb/_static/image6.png))


Można uruchomić przykładową aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie**. Alternatywnie można naciśnij klawisz F5.

Przy pierwszym uruchomieniu aplikacji ASP.NET, zostanie wyświetlone okno dialogowe na rysunku 4 zaleca, aby włączyć tryb debugowania. Kliknij przycisk OK, a aplikacja zostanie uruchomiona.


[![Debugowanie nie jest włączone okna dialogowego](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Rysunek 04**: debugowanie nie włączone okna dialogowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-vb/_static/image8.png))


Podczas uruchamiania aplikacji ASP.NET MVC programu Visual Studio uruchamia aplikację w przeglądarce sieci web. Przykładowa aplikacja składa się z tylko dwoma stronami: strony indeksu i na stronie informacje. Po pierwszym uruchomieniu aplikacji zostanie wyświetlona strona indeksu, (zobacz rysunek 5). Można przejść do strony informacje, klikając link menu w prawym górnym rogu aplikacji.


[![Strony indeksu](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Rysunek 05**: strony indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-vb/_static/image10.png))


Zwróć uwagę, adresy URL w pasku adresu przeglądarki. Na przykład po kliknięciu łącza menu informacje, adres URL w pasku adresu przeglądarki zmienia się **/Home/About**.

Jeśli Zamknij okno przeglądarki i wróć do programu Visual Studio, nie można odnaleźć pliku przy użyciu główna ścieżka/informacje. Pliki nie istnieją. Jak to możliwe?

## <a name="a-url-does-not-equal-a-page"></a>Adres URL strony nie równa się

W przypadku tworzenia tradycyjnych aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages, ma relację między adresu URL i strony. Jeśli zażądasz stronę o nazwie SomePage.aspx z serwera, następnie miał lepiej istnieć strony na dysku o nazwie SomePage.aspx. Jeśli plik SomePage.aspx nie istnieje, otrzymasz fałszywych **404 — Nie można odnaleźć strony** błędu.

Podczas tworzenia aplikacji ASP.NET MVC, z kolei brak zgodności między adres URL, który można wpisać w pasku adresu przeglądarki i pliki, które możesz znaleźć w aplikacji. Adres URL w aplikacji ASP.NET MVC, odnosi się do akcji kontrolera, zamiast strony na dysku.

W tradycyjnych aplikacji ASP.NET i ASP żądania przeglądarki są mapowane na stronach. W aplikacji ASP.NET MVC, natomiast w przeglądarce żądania są mapowane do akcji kontrolera. Aplikacja ASP.NET Web Forms jest skoncentrowane na zawartość. Aplikacji ASP.NET MVC, z kolei jest skoncentrowane na logice aplikacji.

## <a name="understanding-aspnet-routing"></a>Opis routingu platformy ASP.NET

Żądanie przeglądarki na pobiera mapowany do akcji kontrolera za pośrednictwem funkcji struktury programu ASP.NET o nazwie *routingu platformy ASP.NET*. Routingu platformy ASP.NET jest używany przez platformę ASP.NET MVC w celu *trasy* żądania przychodzące do akcji kontrolera.

Routingu platformy ASP.NET używa tabeli tras do obsługi żądań przychodzących. Ta tabela tras jest tworzony podczas pierwszego uruchomienia aplikacji sieci web. Tabela tras jest skonfigurowana w pliku Global.asax. Domyślny plik Global.asax platformy MVC znajduje się w ofercie 1.

**Wyświetlanie listy 1 — w pliku Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Gdy aplikacja ASP.NET pierwszym uruchomieniu, aplikacja\_wywołanie metody Start(). W ofercie 1 Ta metoda wywołuje metodę RegisterRoutes() i metoda RegisterRoutes() tworzy tabela routingu domyślnego.

Tabela routingu domyślnego obejmuje jedną trasę. Ta trasa domyślna przerywa wszystkie żądania przychodzące na trzy segmenty (segment adresu URL to wszystko między ukośniki). Pierwszy segment jest mapowany na nazwę kontrolera, drugi segment jest mapowany na nazwę akcji i końcowego segmentu jest mapowany na parametr przekazywany do akcji o nazwie identyfikatora.

Na przykład rozważmy następujący adres URL:

/ Produkt/szczegóły/3

Ten adres URL jest przekształcany do trzech parametrów następująco:

Kontroler = produkt

Akcja = szczegóły

Identyfikator = 3

Trasa domyślna zdefiniowane w pliku Global.asax zawiera wartości domyślne dla wszystkich trzech parametrów. Domyślnie kontrolera jest strona główna, domyślna akcja jest indeksem i domyślny identyfikator jest ciągiem pustym. W tych wartości domyślnych, pamiętając weź pod uwagę sposób następujący adres URL jest analizowany:

/ Pracownika

Ten adres URL jest przekształcany do trzech parametrów następująco:

Kontroler = pracownik

Akcja = indeks

Identyfikator =

Na koniec możesz otworzyć aplikację ASP.NET MVC bez podawania dowolnego adresu URL (na przykład `http://localhost`), a następnie adres URL jest analizowany w następujący sposób:

Kontroler = strona główna

Akcja = indeks

Identyfikator =

Żądanie jest kierowane do akcji indeks() klasy HomeController.

## <a name="understanding-controllers"></a>Opis kontrolerów

Kontroler jest odpowiedzialny za kontrolowanie sposobu, w jaki użytkownik wchodzi w interakcję z aplikacją MVC. Kontroler zawiera logikę kontroli przepływu dla aplikacji ASP.NET MVC. Kontroler Określa, jakie odpowiedzi do odesłania do użytkownika, gdy użytkownik wysyła żądanie przeglądarki.

Kontroler to po prostu klasy (na przykład klasy Visual Basic lub C#). Przykład aplikacji platformy ASP.NET MVC obejmuje kontroler o nazwie HomeController.vb znajduje się w folderze kontrolerów. Zawartość pliku HomeController.vb jest przedstawiony w ofercie 2.

**Wyświetlanie listy 2 - HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Należy zauważyć, że HomeController ma dwie metody o nazwie indeks() i About(). Te dwie metody odpowiadają dwie akcje udostępnianych przez kontrolera. Adres URL /Home/Index wywołuje metodę HomeController.Index() i /Home adresu URL/o wywołuje metodę HomeController.About().

Wszystkie metody publicznej w kontrolerze jest ujawniona jako akcji kontrolera. Należy zachować ostrożność to. Oznacza to, że dowolnej metody publiczne, zawarte w kontrolerze może być wywoływany przez każdy z dostępem do Internetu, wprowadzając właściwego adresu URL do przeglądarki.

## <a name="understanding-views"></a>Objaśnienie widoków

Akcje kontrolera dwóch udostępnianych przez klasę HomeController indeks() i About(), oba zwracają widoku. Widok zawiera kod znaczników HTML i zawartości, który jest wysyłany do przeglądarki. Widok jest odpowiednikiem stroną, podczas pracy z aplikacją ASP.NET MVC.

Należy utworzyć widoków we właściwym miejscu. Akcja HomeController.Index() zwraca Widok znajduje się w następującej ścieżce:

\Views\Home\Index.aspx

Akcja HomeController.About() zwraca Widok znajduje się w następującej ścieżce:

\Views\Home\About.aspx

Ogólnie rzecz biorąc Jeśli chcesz powrócić do widoku dla akcji kontrolera, następnie należy utworzyć podfolder w folderze widoków z taką samą nazwę jak kontroler. W ramach tego podfolderu należy utworzyć plik .aspx z taką samą nazwę jak akcji kontrolera.

Plik w ofercie 3 zawiera widok About.aspx.

**Wyświetlanie listy 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Jeśli zignorujesz pierwszy wiersz w ofercie 3, większość pozostałych Widok składa się z standardowego kodu HTML. Możesz zmodyfikować zawartość widoku, wprowadzając kod HTML, chcesz, aby w tym miejscu.

Widok jest bardzo podobne do strony Active Server Pages lub formularzy sieci Web ASP.NET. Widok może zawierać zawartość HTML i skryptów. Skrypty można pisać w ulubionej platformy .NET, Programowanie w języku (na przykład C# lub Visual Basic .NET). Skrypty służy do wyświetlania zawartości dynamicznej, takich jak dane z bazy danych.

## <a name="understanding-models"></a>Objaśnienie modeli

Omówiliśmy kontrolery i Omówiliśmy widoków. Ostatni temat, którego potrzebujemy do omówienia jest modeli. Co to jest model MVC?

Modelu MVC zawiera wszystkie logika aplikacji, który nie jest zawarty w widoku lub kontrolera. Model powinien zawierać wszystkie aplikacji logiki biznesowej, logikę weryfikacji i logiką dostępu do bazy danych. Na przykład jeśli używasz programu Entity Framework Microsoft dostęp do bazy danych, następnie w należy utworzyć Twoich zajęciach Entity Framework (pliku edmx) folderu modeli.

Widok może zawierać tylko logiki związane z generowaniem interfejsu użytkownika. Kontroler powinien zawierać tylko podstawowe czynności logikę wymaganą do zwrotu prawego widoku lub przekierować użytkownika do innej akcji (Sterowanie przepływem). Wszystkie inne elementy powinny być zawarte w modelu.

Ogólnie rzecz biorąc należy dążyć dla modeli fat i szczupły kontrolerów. Metody kontrolera może zawierać tylko kilka wierszy kodu. Jeśli akcja kontrolera pobiera zbyt fat, następnie należy rozważyć przenoszenia logikę do nowej klasy, w tym folderze modeli.

## <a name="summary"></a>Podsumowanie

W tym samouczku został udostępniony Przegląd wysokiego poziomu różnych części platformy ASP.NET MVC aplikacji sieci web. Przedstawiono sposób routingu platformy ASP.NET mapowania przychodzących żądań przeglądarki określony kontroler akcji. Pokazaliśmy ci, jak kontrolery organizowanie, jak widoki są zwracane do przeglądarki. Na koniec pokazaliśmy ci, jak modele zawierają aplikacji biznesowych, weryfikacji i logiką dostępu do bazy danych.
