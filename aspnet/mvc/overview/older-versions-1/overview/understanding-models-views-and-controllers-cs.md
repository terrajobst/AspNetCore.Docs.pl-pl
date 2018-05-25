---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Opis modele, widoki i kontrolery (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Masz wątpliwości dotyczące modele, widoki i kontrolery? W tym samouczku Stephen Walther przedstawiono różne części aplikacji platformy ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c9a0cbbf6f786944d7892fbb14859939f21bdd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-c"></a>Opis modele, widoki i kontrolery (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Masz wątpliwości dotyczące modele, widoki i kontrolery? W tym samouczku Stephen Walther przedstawiono różne części aplikacji platformy ASP.NET MVC.


Ten samouczek umożliwia Omówienie programu ASP.NET MVC modele, widoki i kontrolery. Innymi słowy, wyjaśniono M ", V" i C "na platformie ASP.NET MVC.

Po przeczytaniu tego samouczka, należy zrozumieć, jak różne części aplikacji platformy ASP.NET MVC współdziałać ze sobą. Należy również poznać sposób architektury aplikacji platformy ASP.NET MVC różni się od aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Przykładowa aplikacja platformy ASP.NET MVC

Domyślny szablon programu Visual Studio do tworzenia aplikacji sieci Web platformy ASP.NET MVC zawiera bardzo proste przykładowej aplikacji, która może służyć do zrozumieć różne części aplikacji platformy ASP.NET MVC. Firma Microsoft może korzystać z tej prostej aplikacji, w tym samouczku.

Tworzenie nowej aplikacji ASP.NET MVC przy użyciu szablonu MVC, uruchamiając program Visual Studio 2008 i wybraniu opcji menu pliku nowego projektu (zobacz rysunek 1). W oknie dialogowym Nowy projekt, wybierz swój ulubiony język programowania w obszarze Typy projektu (Visual Basic lub C#) i wybierz **aplikacji sieci Web platformy ASP.NET MVC** w szablonach. Kliknij przycisk OK.


[![Okno dialogowe nowego projektu](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Rysunek 01**: okno dialogowe nowego projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image2.png))


Podczas tworzenia nowej aplikacji ASP.NET MVC, **Utwórz jednostkowy projekt testowy** zostanie wyświetlone okno dialogowe (patrz rysunek 2). To okno dialogowe umożliwia utworzenie oddzielny projekt na potrzeby testowania aplikacji ASP.NET MVC rozwiązania. Wybierz opcję **nie twórz jednostkowy projekt testowy** i kliknij przycisk **OK** przycisku.


[![Tworzenie okna dialogowego testów jednostkowych](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Rysunek 02**: Tworzenie jednostki okno dialogowe testu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image4.png))


Po nowe platformy ASP.NET MVC jest tworzona aplikacja. Pojawi się wiele folderów i plików w oknie Eksploratora rozwiązań. W szczególności zobaczysz folderów trzy modele, widoki i kontrolery. Jak może przypuszczalnie z nazw folderów, te foldery zawierają pliki wykonywania modele, widoki i kontrolery.

Po rozwinięciu folder kontrolery, powinny być widoczne w pliku o nazwie AccountController.cs i plik o nazwie HomeController.cs. Po rozwinięciu folderu widoki powinna zostać wyświetlona trzy podfoldery o nazwach konta, strona główna i udostępnione. Jeśli rozwiń folder macierzysty, zostanie wyświetlone dwa dodatkowe pliki o nazwie About.aspx i Index.aspx (patrz rysunek 3). Pliki te tworzą przykładowej aplikacji uwzględnione przy użyciu domyślnego szablonu platformy ASP.NET MVC.


[![Okno Eksploratora rozwiązań](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Rysunek 03**: okno Eksploratora rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image6.png))


Można uruchomić przykładowej aplikacji przez wybranie opcji menu **debugowania i Rozpocznij debugowanie**. Alternatywnie można naciśnij klawisz F5.

Przy pierwszym uruchomieniu aplikacji ASP.NET, wyświetli się okno dialogowe na rysunku 4 zalecane, aby włączyć tryb debugowania. Kliknij przycisk OK, a aplikacja będzie działać.


[![Debugowanie nie jest włączone okna dialogowego](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Rysunek 04**: debugowanie nie włączone okna dialogowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image8.png))


Po uruchomieniu aplikacji platformy ASP.NET MVC programu Visual Studio uruchamia aplikację w przeglądarce sieci web. Przykładowa aplikacja zawiera tylko dwie strony: strona indeksu i na stronie informacje. Po pierwszym uruchomieniu aplikacji, zostanie wyświetlona strona indeksu, (patrz rysunek 5). Można przejść do strony informacje, klikając łącze menu u góry po prawej z aplikacji.


[![Strona indeksu](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Rysunek 05**: na stronie indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image11.png))


Zwróć uwagę, adresy URL na pasku adresu przeglądarki. Na przykład po kliknięciu łącza menu informacje zmiany adresu URL na pasku adresu przeglądarki do **domowej/o**.

Po zamknięciu okna przeglądarki i wróć do programu Visual Studio, nie można odnaleźć pliku głównej ścieżki/około. Pliki nie istnieją. Jak jest to możliwe?

## <a name="a-url-does-not-equal-a-page"></a>Adres URL nie jest równa strony

Podczas konstruowania tradycyjnych aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages, Brak odpowiednika między adres URL i strony. Jeśli żądania stronę o nazwie SomePage.aspx z serwera ma lepiej będzie strony na dysku o nazwie SomePage.aspx. Jeśli plik SomePage.aspx nie istnieje, możesz uzyskać fałszywych **404 — Nie znaleziono strony** błędu.

Tworzenie aplikacji platformy ASP.NET MVC, natomiast nie ma zgodności między wpisane na pasku adresu w przeglądarce adres URL i pliki, które możesz znaleźć w aplikacji. Adres URL w aplikacji platformy ASP.NET MVC odnosi się do akcji kontrolera, zamiast strony na dysku.

W tradycyjnym aplikacji ASP.NET i ASP przeglądarki żądanie jest mapowane do stron. W aplikacji ASP.NET MVC, natomiast w przeglądarce żądanie jest mapowane na akcji kontrolera. Aplikacja formularzy sieci Web ASP.NET jest zorientowany zawartości. Aplikacji platformy ASP.NET MVC jest z kolei na logiki aplikacji.

## <a name="understanding-aspnet-routing"></a>Opis routingu platformy ASP.NET

Żądanie przeglądarki pobiera zamapowane do akcji kontrolera za pośrednictwem funkcji platformy ASP.NET o nazwie *routingu platformy ASP.NET*. ASP.NET Routing jest używany przez platformę ASP.NET MVC w celu *trasy* żądania przychodzące do akcji kontrolera.

ASP.NET Routing używa tabelę tras do obsługi żądań przychodzących. Ta tabela tras jest tworzony po pierwszym uruchomieniu aplikacji sieci web. Tabela tras jest skonfigurowana w pliku Global.asax. Domyślny plik MVC Global.asax znajduje się w 1 wyświetlania.

**Wyświetlanie listy 1 - pliku Global.asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Gdy aplikacja ASP.NET uruchomieniu aplikacji\_Start() metoda jest wywoływana. Wyświetlanie 1 Ta metoda wywołuje metodę RegisterRoutes() i metoda RegisterRoutes() tworzy domyślną tabelę routingu.

Tabela tras domyślnych składa się z jedną trasę. Tej trasy domyślnej dzieli wszystkie żądania przychodzące na trzy segmenty (segment adresu URL jest wszystko między ukośnikami). Pierwszy segment jest mapowany na nazwę kontrolera, jak drugi segment jest mapowany na nazwę akcji i końcowe segmentu jest mapowany na parametr przekazany do akcji o nazwie identyfikatora.

Rozważmy na przykład następujący adres URL:

/ Produkt/szczegóły/3

Ten adres URL jest analizowana na trzy parametry następująco:

Kontroler = produktu

Akcja szczegóły =

Identyfikator = 3

Trasa domyślna zdefiniowana w pliku Global.asax zawiera wartości domyślne dla wszystkich trzech parametrów. Domyślnie kontrolera jest Narzędzia główne, domyślna akcja to indeks i domyślnego identyfikatora jest pustym ciągiem. Z tych wartości domyślnych, pamiętając należy rozważyć sposób analizowania następującego adresu URL:

/ Pracownika

Ten adres URL jest analizowana na trzy parametry następująco:

Kontroler = pracownika

Akcja = indeks

Identyfikator =

Ponadto jeśli otworzysz bez podawania dowolny adres URL aplikacji platformy ASP.NET MVC (na przykład `http://localhost`), a następnie adres URL jest analizowana następująco:

Kontroler = Home

Akcja = indeks

Identyfikator =

Żądanie jest kierowane do akcji indeks() w klasie HomeController.

## <a name="understanding-controllers"></a>Opis kontrolerów

Kontroler jest odpowiedzialny za kontrolowanie sposób, w jaki użytkownik korzysta z aplikacji MVC. Kontroler zawiera logikę kontroli przepływu dla aplikacji platformy ASP.NET MVC. Kontroler Określa, jakie odpowiedzi do odesłania do użytkownika, gdy użytkownik przesyła żądanie przeglądarki.

Kontroler to po prostu klasy (na przykład klasy Visual Basic lub C#). Przykładowej aplikacji ASP.NET MVC zawiera kontrolerze o nazwie HomeController.cs znajdujące się w folderze kontrolerów. Zawartość pliku HomeController.cs jest przedstawiony w wyświetlania 2.

**Wyświetlanie listy 2 - HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Zwróć uwagę, że HomeController ma dwie metody o nazwie indeks() i About(). Te dwie metody odpowiadają dwie akcje udostępnianych przez kontroler. Adres URL /Home/indeksu wywołuje metodę HomeController.Index() i /Home adresu URL/o wywołuje metodę HomeController.About().

Wszystkie metody publicznej w kontrolerze jest ujawniona jako akcji kontrolera. Należy zachować ostrożność to. Oznacza to, wszystkie metody publicznej zawartych w kontrolerze może być wywoływany wszystkim osobom z dostępem do Internetu, wprowadzając adres URL prawego do przeglądarki.

## <a name="understanding-views"></a>Opis widoków

Akcje dwóch kontrolera udostępniane przez klasę HomeController indeks() i About(), zwrócą widoku. Widok zawiera kod znaczników HTML i zawartości, który jest wysyłany do przeglądarki. Widok jest odpowiednikiem stronę podczas pracy z aplikacji platformy ASP.NET MVC.

Należy utworzyć widoków w odpowiedniej lokalizacji. Akcja HomeController.Index() zwraca widok znajdujący się w następującej ścieżce:

\Views\Home\Index.aspx

Akcja HomeController.About() zwraca widok znajdujący się w następującej ścieżce:

\Views\Home\About.aspx

Ogólnie rzecz biorąc Jeśli chcesz powrócić do widoku dla akcji kontrolera, następnie należy utworzyć podfolderu w folderze widoków z taką samą nazwę jak kontroler. W podfolderze należy utworzyć plik .aspx z taką samą nazwę jak akcji kontrolera.

Plik w 3 lista zawiera widok About.aspx.

**Wyświetlanie listy 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Jeśli zignorujesz w pierwszym wierszu wyświetlania 3 większości pozostałej części widoku składa się z standardowego kodu HTML. Wprowadź kod HTML, w tym miejscu ma można modyfikować zawartość widoku.

Widok jest bardzo podobna do strony Active Server Pages lub formularzy sieci Web ASP.NET. Widok może zawierać zawartość HTML i skryptów. Twoje ulubione .NET można napisać skrypty programowania w języku (na przykład C# i Visual Basic .NET). Skrypty służy do wyświetlania dynamicznej zawartości takiej jak dane z bazy danych.

## <a name="understanding-models"></a>Opis modeli

Omówiliśmy kontrolerów i Omówiliśmy widoków. Ostatni temat, który musimy omówienia jest modeli. Co to jest model MVC?

MVC model zawiera wszystkie logiki aplikacji, który nie jest zawarty w widoku lub kontrolera. Model powinien zawierać wszystkie aplikacji logiki biznesowej, logikę weryfikacji i logika dostępu do bazy danych. Na przykład jeśli używasz programu Entity Framework firmy Microsoft dostęp do bazy danych, następnie należy utworzyć klas Entity Framework (pliku edmx) w folderze modeli.

Widok powinien zawierać tylko logiki związane z generowaniem interfejsu użytkownika. Kontroler powinien zawierać tylko podstawowe czynności logiki musiał zwrócić widok prawym lub przekierować użytkownika do innej akcji (Sterowanie przepływem). Wszystkie inne powinny być zawarte w modelu.

Ogólnie rzecz biorąc należy dążyć dla modeli fat i szczupły kontrolerów. Metody kontrolera powinna zawierać tylko kilka wierszy kodu. Jeśli zbyt fat pobiera akcji kontrolera, następnie należy rozważyć przeniesienie logiki na nową klasę w folderze modeli.

## <a name="summary"></a>Podsumowanie

W tym samouczku udostępniony wysokiego poziomu przegląd różnych części programu ASP.NET MVC aplikacji sieci web. Przedstawiono sposób routingu platformy ASP.NET mapowania przychodzącego żądania przeglądarki kontroler akcji. Przedstawiono sposób organizowania kontrolerach jak widoki są zwracane do przeglądarki. Ponadto przedstawiono sposób modele zawierają aplikacji biznesowych, weryfikacji i logika dostępu do bazy danych.
