---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Tworzenie spójnego układu we wzorcu ASP.NET Web Pages (Razor) witryn | Dokumentacja firmy Microsoft
author: tfitzmac
description: Aby umożliwić bardziej efektywne tworzenie stron sieci web, można utworzyć wielokrotnego użytku bloki zawartości (na przykład nagłówków i stopek) dla witryny sieci Web, a użytkownik c...
ms.author: aspnetcontent
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: d27cdc70417f380d596f4d07384a615586427643
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821217"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Tworzenie spójnego układu w witrynach ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak skorzystać układu stron w witrynie internetowej ASP.NET Web Pages (Razor) do tworzenia wielokrotnego użytku bloki zawartości (na przykład nagłówków i stopek) i tworzenie spójnego wyglądu dla wszystkich stron w witrynie.
> 
> **Zawartość:** 
> 
> - Jak utworzyć wielokrotnego użytku bloki zawartości, takich jak nagłówki i stopki.
> - Jak utworzyć spójnego wyglądu dla wszystkich stron w witrynie przy użyciu układu.
> - Jak przekazać dane w czasie wykonywania do strony układu.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
> 
> - Bloki zawartości, które są pliki zawierające zawartości w formacie HTML do wstawienia na wielu stronach.
> - Układ stron, które są strony zawierające zawartości w formacie HTML, który może być współużytkowany przez strony w witrynie internetowej.
> - `RenderPage`, `RenderBody`, I `RenderSection` metod, które informują miejsca do wstawienia elementów strony ASP.NET.
> - `PageData` Słownika, która umożliwia udostępnianie danych między bloki zawartości i układu strony.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Układ strony — informacje

Wiele witryn sieci Web ma zawartość, która jest wyświetlana na każdej stronie, takie jak nagłówek i stopka lub informacje dla użytkowników, że są one rejestrowane w pole. ASP.NET umożliwia utworzenie oddzielnego pliku z bloku zawartości, która może zawierać tekstu, znaczników i kodu, tak jak zwykły strony sieci web. Następnie można wstawić blok zawartości w innych stron w witrynie, w której chcesz umieścić informacje. Dzięki temu nie trzeba skopiować i wkleić tę samą zawartość do każdej strony. Tworzenie typowych zawartości następująco również sprawia, że łatwiej zaktualizować lokację. Jeśli zachodzi potrzeba zmiany zawartości, można aktualizować tylko pojedynczy plik, a zmiany są stosowane wszędzie, gdzie został wstawiony zawartości.

Na poniższym diagramie przedstawiono, jak zawartości blokuje pracy. Gdy przeglądarka zgłasza żądanie strony z serwera sieci web, ASP.NET wstawia bloki zawartości w punkcie gdzie `RenderPage` metoda jest wywoływana strony głównej. Zakończono strony (scalonych) jest następnie wysyłany do przeglądarki.

![Diagram koncepcyjny przedstawiający, jak metoda RenderPage wstawia odwołania strony do bieżącej strony.](3-creating-a-consistent-look/_static/image1.jpg)

W tej procedurze utworzysz stronę, która odwołuje się dwa bloki zawartości (nagłówek i stopka), które znajdują się w oddzielnych plikach. Można użyć tych samych bloków zawartości w dowolnej stronie w witrynie. Gdy wszystko będzie gotowe, zostanie wyświetlona strona z informacją następująco:

![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchomienie strony, która zawiera wywołania metody RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. W folderze głównym witryny sieci Web, Utwórz plik o nazwie *Index.cshtml*.
2. Zastąp istniejący kod znaczników następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. W folderze głównym Utwórz folder o nazwie *Shared*.

    > [!NOTE]
    > Jest to powszechną praktyką do przechowywania plików, które są współużytkowane przez strony sieci web w folderze o nazwie *Shared*.
4. W *Shared* folderze utwórz plik o nazwie  *\_Header.cshtml*.
5. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Należy zauważyć, że nazwa pliku jest  *\_Header.cshtml*, od znaku podkreślenia (\_) jako prefiksu. Program ASP.NET nie wyśle strony do przeglądarki, jeśli jej nazwa rozpoczyna się od znaku podkreślenia. Zapobiega to żądanie (przypadkowo lub w inny sposób) tych stron bezpośrednio osób. To dobry pomysł, aby użyć podkreślenia do stron nazwy, które ma bloki zawartości, ponieważ nie chcesz tak naprawdę użytkownicy, aby móc żądać tych stron &#8212; istnieje wyłącznie do wstawienia do innych stron.
6. W *Shared* folderze utwórz plik o nazwie  *\_Footer.cshtml* i Zastąp zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. W *Index.cshtml* strony, należy dodać dwa wywołania `RenderPage` metody, jak pokazano poniżej:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    To pokazuje, jak wstawić blok zawartości do strony sieci web. Należy wywołać `RenderPage` metody i przekaż mu nazwę pliku, którego zawartość ma zostać wstawiony w tym momencie. W tym miejscu wstawiasz zawartość  *\_Header.cshtml* i  *\_Footer.cshtml* pliki do *Index.cshtml* pliku.
8. Uruchom *Index.cshtml* strony w przeglądarce. (W programie WebMatrix w **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy plik, a następnie wybierz pozycję **Uruchom w przeglądarce**.)
9. W przeglądarce Wyświetl źródło strony. (Na przykład w programie Internet Explorer, kliknij prawym przyciskiem myszy strony, a następnie kliknij przycisk **Wyświetl źródło**.)

    Dzięki temu możesz zobaczyć znaczników strony sieci web, które są wysyłane do przeglądarki, która łączy znaczników strony indeksu z bloki zawartości. W poniższym przykładzie przedstawiono źródła strony, który jest renderowany *Index.cshtml*. Wywołania `RenderPage` wstawiony do *Index.cshtml* zostały zastąpione rzeczywistej zawartości pliki nagłówków i stopek.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Tworzenie spójnego wyglądu za pomocą strony układu

Do tej pory wiesz, że jest łatwy do uwzględnienia tej samej zawartości na wielu stronach. Jest bardziej ustrukturyzowane podejście do tworzenia spójnego wyglądu witryny do użycia strony układu. Strona układu definiuje strukturę strony sieci web, ale nie zawiera żadnych rzeczywistej zawartości. Po utworzeniu strony układu można utworzyć strony sieci web z zawartością, a następnie połączyć je do strony układu. Te strony będą wyświetlane, będzie można sformatować zgodnie ze strony układu. (W tym sensie, strony układu działa jako to rodzaj szablonu dla zawartości, która jest zdefiniowana w innych stron.)

Strony układu jest podobne do dowolnej strony HTML, z tą różnicą, że zawiera on wywołanie `RenderBody` metody. Pozycja `RenderBody` metody w stronę układu określa, gdzie informacji z poziomu strony zawartości zostaną dołączone.

Na poniższym diagramie przedstawiono sposób zawartości strony i układu strony są łączone w czasie wykonywania, aby utworzyć stronę sieci web zakończona. Przeglądarka żąda zawartości strony. Strona zawartości zawiera kod określający stronę układu dla strony struktury. Na stronie układu, zawartość jest wstawiany w punkcie, w którym `RenderBody` metoda jest wywoływana. Zawartość, blokuje również mogą być wstawiane do strony układu, wywołując `RenderPage` metody w taki sposób, jak w poprzedniej sekcji. Po zakończeniu strony sieci web jest wysyłana do przeglądarki.

![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchomienie strony, która zawiera wywołania metody RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

Poniższa procedura pokazuje, jak utworzyć układ stron zawartości strony i link do niego.

1. W *Shared* folderu witryny sieci Web, Utwórz plik o nazwie  *\_Layout1.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Możesz użyć `RenderPage` metody na stronie układu, aby wstawić bloki zawartości. Strona układu może zawierać tylko jedno wywołanie `RenderBody` metody.
3. W *Shared* folderze utwórz plik o nazwie  *\_Header2.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. W folderze głównym, Utwórz nowy folder i nadaj mu nazwę *style*.
5. W *style* folderze utwórz plik o nazwie *Site.css* i dodaj następujące definicje styl:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Te definicje styl są tu tylko po to, aby pokazać, jak arkusze stylów można używać ze strony układu. Jeśli chcesz, można zdefiniować własne style dla tych elementów.
6. W folderze głównym, Utwórz plik o nazwie *Content1.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Jest to strona, która będzie używać strony układu. Blok kodu, w górnej części strony wskazuje stronę układu, które będą formatowane tej zawartości.
7. Uruchom *Content1.cshtml* w przeglądarce. Renderowanej strony używa formatu i arkusz stylów zdefiniowany w  *\_Layout1.cshtml* i tekst (zawartość) zdefiniowany w *Content1.cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    Możesz powtórzyć krok 6, aby utworzyć dodatkowe strony zawartości, które będzie mogło współużytkować tę samą stronę układu.

    > [!NOTE]
    > Można skonfigurować witryny, dzięki czemu można automatycznie używać tej samej stronie układ dla stron zawartości w folderze. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowania dla całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Projektowanie stron układu, które mają wiele sekcji zawartości

Zawartość strony może mieć wiele sekcji, co jest przydatne, jeśli chcesz używać układów, które mają wiele obszarów z zawartością wymienne. Na stronie zawartości Nadaj każdej sekcji unikatową nazwę. (Domyślnej sekcji zostanie pozostawiony bez nazwy.) Na stronie układu, możesz dodać `RenderBody` metodę, aby określić, gdzie powinna zostać wyświetlona sekcja bez nazwy (wartość domyślna). Następnie należy dodać oddzielny `RenderSection` metody w celu pojedynczo render nazwanej sekcji.

Na poniższym diagramie przedstawiono, jak ASP.NET obsługuje zawartość, która jest podzielona na sekcje. Każda sekcja o nazwie znajduje się w bloku sekcji na stronie zawartości. (Są one nazwane `Header` i `List` w przykładzie.) Struktura wstawia sekcji zawartość do strony układu w punkcie, w którym `RenderSection` metoda jest wywoływana. Sekcja bez nazwy (wartość domyślna) jest wstawiana w punkcie, w którym `RenderBody` metoda jest wywoływana, zgodnie z wcześniej.

![Diagram koncepcyjny przedstawiający, jak metoda RenderSection wstawia sekcje odwołania do bieżącej strony.](3-creating-a-consistent-look/_static/image5.jpg)

Ta procedura pokazuje, jak utworzyć strony zawartości, która ma wiele sekcji zawartości i sposób renderowania za pomocą strony układu, która obsługuje wiele sekcji zawartości.

1. W *Shared* folderze utwórz plik o nazwie  *\_Layout2.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Możesz użyć `RenderSection` metody do renderowania sekcje nagłówka i listy.
3. W folderze głównym, Utwórz plik o nazwie *Content2.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Ta strona zawartości zawiera blok kodu, w górnej części strony. Każda sekcja o nazwie znajduje się w bloku sekcji. Pozostałej części strony zawiera sekcja zawartość domyślna (bez nazwy).
4. Uruchom *Content2.cshtml* w przeglądarce.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchomienie strony, która zawiera wywołania metody RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Tworzenie opcjonalne sekcje zawartości

Zwykle sekcje, które tworzysz na stronie zawartości musi odpowiadać sekcje, które są zdefiniowane w stronę układu. Mogą wystąpić błędy, ponieważ wystąpienia któregokolwiek z następujących czynności:

- Strona zawartości zawiera sekcja, która nie ma odpowiedniej sekcji na stronie układu.
- Strona układu zawiera sekcja, dla których nie ma żadnej zawartości.
- Strona układu zawiera wywołania metody, w których podejmowana jest próba renderowania tej samej sekcji w więcej niż jeden raz.

Jednak można zastąpić to zachowanie dla nazwanej sekcji przez zadeklarowanie sekcja ma być opcjonalne w stronę układu. Dzięki temu można zdefiniować wiele stron zawartości mogą dzielić stronę układu, ale którzy mogą lub nie może mieć zawartości dla określonego obszaru.

1. Otwórz *Content2.cshtml* i usuń następującą sekcję:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Zapisz stronę, a następnie uruchom go w przeglądarce. Wyświetlany jest komunikat o błędzie, ponieważ strony zawartości nie udostępnia zawartość sekcji zdefiniowana na stronie układu, a mianowicie sekcji nagłówka.

    ![Zrzut ekranu pokazujący błąd występujący po uruchomieniu strony, która wywołuje metodę RenderSection, ale nie podano odpowiedniej sekcji.](3-creating-a-consistent-look/_static/image7.jpg)
3. W *Shared* folder, otwórz  *\_Layout2.cshtml* strony, a następnie Zastąp ten wiersz:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    następującym kodem:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Alternatywnie możesz Zastąp poprzedniego wiersza kodu, które poniższy blok kodu, który generuje te same wyniki:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Uruchom *Content2.cshtml* strony w przeglądarce ponownie. (Jeśli nadal masz tę stronę w przeglądarce, możesz po prostu odświeżyć go). Tym razem zostanie wyświetlona strona z żaden błąd nawet, jeśli go nie ma nagłówka.

## <a name="passing-data-to-layout-pages"></a>Przekazywanie danych do strony układu

Konieczne może być danymi zdefiniowanymi w strony zawartości, który należy do odwoływania się do strony układu. Jeśli tak, należy przekazać dane ze strony zawartość do strony układu. Na przykład możesz chcieć wyświetlić stan logowania użytkownika lub możesz chcieć pokazać lub ukryć obszarów zawartości na podstawie danych wejściowych użytkownika.

Aby przekazać dane ze strony zawartość do strony układ, można umieścić wartości `PageData` właściwości strony zawartość. `PageData` Właściwości to zbiór par nazwa/wartość, w których przechowywane są dane, które mają być przekazywane między stronami. Na stronie układu, można następnie odczytać wartości z `PageData` właściwości.

Oto inny diagram. To pokazuje jak używać platformy ASP.NET `PageData` właściwość, aby przekazać wartości ze strony zawartość do strony układu. Po rozpoczęciu tworzenia stron sieci web platformy ASP.NET tworzy `PageData` kolekcji. Na stronie zawartości pisanie kodu, aby umieścić dane `PageData` kolekcji. Wartości w `PageData` kolekcji można także uzyskać dostęp przez innych sekcjach strony zawartości lub dodatkowe bloki zawartości.

![Diagram koncepcyjny, który pokazuje, jak wypełnić słownika PageData i przekazać te informacje do strony układu strony zawartości.](3-creating-a-consistent-look/_static/image8.jpg)

Poniższa procedura pokazuje, jak przekazywanie danych od strony zawartości do strony układu. Po uruchomieniu strony wyświetli przycisk, który umożliwia użytkownikowi ukryć lub pokazać listy, która jest zdefiniowana w stronę układu. Gdy użytkownik kliknie przycisk, ustawia wartość PRAWDA/FAŁSZ (wartość logiczna) w `PageData` właściwości. Strona układu odczytuje tę wartość i jeśli ma wartość false, ukrywa listę. Wartość służy również na stronie zawartości do określenia, czy mają być wyświetlane **Ukryj listę** przycisk lub **Pokaż listę** przycisku.

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. W folderze głównym, Utwórz plik o nazwie *Content3.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Kod przechowuje dwóch fragmentów danych w `PageData` właściwość &#8212; tytuł strony sieci web i wartość PRAWDA lub FAŁSZ, aby określić, czy w celu wyświetlenia listy.

    Należy zauważyć, że program ASP.NET pozwala umieść kod znaczników HTML strony warunkowo przy użyciu bloku kodu. Na przykład `if/else` bloku w treści strony określa formularz do wyświetlania w zależności od tego, czy `PageData["ShowList"]` jest ustawiona na wartość true.
2. W *Shared* folderze utwórz plik o nazwie  *\_Layout3.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Strona układu zawiera wyrażenie w `<title>` element, który pobiera wartość tytuł `PageData` właściwości. Korzysta również `ShowList` wartość `PageData` właściwości w celu określenia, czy ma być wyświetlany blok zawartości listy.
3. W *Shared* folderze utwórz plik o nazwie  *\_List.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Uruchom *Content3.cshtml* strony w przeglądarce. Ta strona jest wyświetlana przy użyciu listy widocznych w lewej części strony i **Ukryj listę** znajdujący się u dołu.

    ![Zrzut ekranu przedstawiający stronę zawierającą listy i przycisk, który jest wyświetlany komunikat "Ukryj List".](3-creating-a-consistent-look/_static/image10.jpg)
5. Kliknij przycisk **Ukryj listę**. Lista znika i przycisku zmienia się na **Pokaż listę**.

    ![Zrzut ekranu przedstawiający stronę, która nie zawiera listy i przycisk, który jest wyświetlany komunikat "Pokaż listę".](3-creating-a-consistent-look/_static/image11.jpg)
6. Kliknij przycisk **Pokaż listę** przycisku i listy zostaną wyświetlone ponownie.

## <a name="additional-resources"></a>Dodatkowe zasoby


[Dostosowywanie zachowania dla całej witryny dla stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
