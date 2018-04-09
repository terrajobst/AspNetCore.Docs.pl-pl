---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Iteracja #2 — zastosować Szukaj nieuprzywilejowany (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: cad28fb6ff02625674e59674d1ec08d52373c269
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iteracja #2 — zastosować Szukaj nieuprzywilejowany (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji platformy ASP.NET MVC kontaktu administracyjnego (C#)
  

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.

## <a name="this-iteration"></a>Tej iteracji

Celem tej iteracji jest poprawienie wyglądu aplikacji menedżera kontaktu. Obecnie Contact Manager używa domyślnej strony wzorcowej widoku programu ASP.NET MVC i kaskadowy arkusz stylów (zobacz rysunek 1). Te ADAM t wygląd zły, ale nie chcę chcesz t Contact Manager do wyglądać podobnie jak każdy witryny ASP.NET MVC. Chcę zastąpić te pliki niestandardowe pliki.


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Rysunek 01**: wygląd domyślny aplikacji platformy ASP.NET MVC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


W tym iteracji I omówiono w nim dwa podejścia do poprawy projekt visual naszej aplikacji. Po pierwsze I opisano, jak korzystać z Galerii projektów MVC ASP.NET można pobrać szablonu projektu programu ASP.NET MVC wolnego. Galeria projektów MVC ASP.NET umożliwia tworzenie aplikacji sieci professional web bez działał.

I zdecydowała się bez użycia szablonu z Galerii projektów MVC ASP.NET dla aplikacji Menedżer kontaktu. Zamiast tego użytkownik miał niestandardowe projektu utworzonego przez firmę profesjonalnego projektu. W drugiej części w tym samouczku opisano sposób pracy z firmą profesjonalnego projektu do tworzenia ostatecznego projektu programu ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>Galeria projektów programu ASP.NET MVC

Galeria projektów programu ASP.NET MVC jest bezpłatnym zasobem obsługiwane przez firmę Microsoft. ASP.NET MVC galerii znajduje się pod następującym adresem:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Galeria projektów programu ASP.NET MVC obsługuje kolekcji projektów bezpłatne witryny sieci Web, które zostały utworzone specjalnie z myślą o użyciu w projekcie platformy ASP.NET MVC. Projekty są wysyłane przez członków społeczności. Osoby odwiedzające galerii głosować ich ulubionych wzorów (patrz rysunek 2).


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Rysunek 02**: Galerii projektów MVC ASP.NET ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Pisania tego samouczka, najpopularniejsze projektu w galerii to projekt o nazwie października przez Dominika Hauser. W tym projekcie można użyć w projekcie platformy ASP.NET MVC, wykonując następujące czynności:

1. Kliknij przycisk **Pobierz** przycisk, aby pobrać plik October.zip na komputerze.
2. Kliknij prawym przyciskiem myszy pobrany plik October.zip, a następnie kliknij przycisk **Odblokuj** przycisk (patrz rysunek 3).
3. Rozpakuj plik do folderu o nazwie października.
4. Zaznacz wszystkie pliki w folderze DesignTemplate znajdujące się w folderze października, kliknij prawym przyciskiem myszy plik i wybierz opcję menu **kopiowania**.
5. Kliknij prawym przyciskiem myszy węzeł projektu ContactManager w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz opcję menu **Wklej** (patrz rysunek 4).
6. Wybierz opcję menu programu Visual Studio **edytować, Znajdź i Zamień, Zastąp szybkie** i Zastąp *[MyProjectName]* z *ContactManager* (patrz rysunek 5).


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Rysunek 03**: odblokowywania pliku pobrane z sieci web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Rysunek 04**: zastąpienie plików w Eksploratorze rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Rysunek 05**: zastępowanie [ProjectName] ContactManager ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Po wykonaniu tych kroków, aplikacji sieci web użyje nowy projekt. Strony na rysunku 6 przedstawiono wygląd aplikacji menedżera kontaktu z projektu października.


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Rysunek 06**: ContactManager przy użyciu szablonu października ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Tworzenie projektu niestandardowych platformy ASP.NET MVC

Galeria projektów programu ASP.NET MVC ma dobry wybór style innego projektu. Galerii zapewnia bezproblemowego sposób, aby dostosować wygląd aplikacji ASP.NET MVC. I oczywiście galerii ma dużą zaletą jest całkowicie za darmo.

Jednak może być konieczne utworzenie całkowicie unikatowy projekt witryny sieci Web. W takim przypadku warto do pracy z firmowymi projekt witryny sieci Web. I decyzję o zastosowaniu takiego podejścia do projektowania dla aplikacji, skontaktuj się z Menedżera.

I Konfigurowanie Menedżera skontaktuj się z od 1 # iteracji i przesłania projektu do firmie projektowej. Ich nie jest właścicielem Visual Studio (shame na nich!), ale ten t powodują problemu. Udało bezpłatnie pobrać z Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net) witryny sieci Web i otwórz Menedżera skontaktuj się z aplikacji w Visual Web Developer. Kilka dni ich miał utworzonego projektu na rysunku 7.


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Rysunek 07**: projekt programu ASP.NET MVC Contact Manager ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


Nowy projekt składa się z dwóch głównych plików: nowy plik arkusza stylów kaskadowych i nowego pliku strony wzorcowej widoku. Widok strony głównej zawiera układu i zawartości udostępnionej w widokach w aplikacji platformy ASP.NET MVC. Na przykład strony wzorcowej widoku zawiera nagłówek, kartach nawigacji i stopce strony, który pojawia się na rysunku 7. Istniejące strony wzorcowej widoku w Site.Master w folderze Views\Shared I zastąpiła z nowym plikiem Site.Master w firmie projektowej

Firmie projektowej również utworzyć nowy kaskadowy arkusz stylów i zestawu obrazów. I umieścić te nowe pliki w folderze zawartości i zastąpiła istniejącego pliku Site.css. Wszystkie zawartość statyczną należy umieścić w folderze zawartości.

Zwróć uwagę, że nowy projekt dla menedżera kontaktu zawiera obrazy, edytowanie i usuwanie kontaktów. Edytowanie i usuwanie obrazów są wyświetlane obok każdego kontakt w tabeli HTML kontaktów.

Pierwotnie poniższych linków, które jest renderowany kod HTML. Pomocnik ActionLink() następująco:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Metoda Html.ActionLink() nie obsługuje obrazów (metoda HTML koduje tekst łącza ze względów bezpieczeństwa). W związku z tym I zastąpione wywołań Html.ActionLink() wywołania Url.Action() następująco:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Metoda Html.ActionLink() renderuje całego hiperłącza HTML. Metoda Url.Action() z drugiej strony, renderuje tylko adres URL bez &lt;&gt; tagu.

Zwróć uwagę, ponadto, czy nowy projekt zawiera karty zaznaczone i niezaznaczone. Na przykład na rysunku 8 **Utwórz nowy kontakt** wybrana karta i **Moje kontakty** karta nie jest zaznaczone.


[![Okno dialogowe nowego projektu](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Rysunek 08**: zaznaczony i niezaznaczony karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Aby zapewnić obsługę renderowania zaznaczone i niezaznaczone karty, utworzony niestandardowy pomocnika kodu HTML o nazwie MenuItemHelper. Ta metoda pomocnika renderuje albo &lt;li&gt; tag lub &lt;klasy li = "zaznaczone"&gt; tag w zależności od tego, czy bieżący kontroler i akcja odpowiada nazwie kontroler i akcja przekazany do elementu pomocniczego. Kod MenuItemHelper znajduje się lista 1.

**Wyświetlanie listy 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper korzysta klasa TagBuilder wewnętrznie w celu skompilowania &lt;li&gt; tagu HTML. Klasa TagBuilder jest klasą przydatne narzędzie pomocne, gdy zajdzie taka potrzeba zbudowania nowego tagu HTML. Zawiera metody do dodawania atrybutów, dodawanie klas CSS generowania identyfikatorów i modyfikowanie tag s wewnętrznego kodu HTML.

## <a name="summary"></a>Podsumowanie

W tej iteracji możemy udoskonalony projekt visual naszej aplikacji ASP.NET MVC. Po pierwsze zostały wprowadzone do Galerii projektów programu ASP.NET MVC. Przedstawiono sposób pobierania szablonów projektu wolnego z Galerii projektów programu ASP.NET MVC, używanego w aplikacjach ASP.NET MVC.

Następnie omówiono sposób tworzenia niestandardowych, modyfikując domyślnego pliku w kaskadowego arkusza stylów i widoku głównego pliku stronicowania. Aby zapewnić obsługę nowego projektu, było dokonanie niewielkimi zmianami w naszej aplikacji menedżera kontaktu. Na przykład dodać nowy Pomocnik kodu HTML o nazwie MenuItemHelper, która wyświetla karty zaznaczone i niezaznaczone.

W następnej iteracji możemy rozwiązania bardzo ważne przedmiotem weryfikacji. Dodamy kodu walidacji do naszej aplikacji, dzięki czemu użytkownik nie można utworzyć nowego kontaktu bez podawania wymagane wartości, takie jak osoby s najpierw i nazwiska.

> [!div class="step-by-step"]
> [Poprzednie](iteration-1-create-the-application-cs.md)
> [dalej](iteration-3-add-form-validation-cs.md)
