---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: "Tworzenie spójnego układu w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Aby umożliwić bardziej wydajne, można utworzyć strony sieci web dla witryny, można utworzyć wielokrotnego użytku bloki zawartości (na przykład nagłówków i stopek) dla witryny sieci Web, a użytkownik c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Tworzenie witryny sieci Web ASP.NET (Razor) stron spójnego układu
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób korzystania układ strony w witrynie sieci Web platformy ASP.NET Web Pages (Razor) do tworzenia wielokrotnych bloki zawartości (takich jak nagłówki i stopki) i tworzy spójny wygląd dla wszystkich stron w witrynie.
> 
> **Zawartość:** 
> 
> - Jak utworzyć wielokrotnego użytku bloki zawartości, takich jak nagłówki i stopki.
> - Jak utworzyć spójny wygląd dla wszystkich stron w witrynie przy użyciu układu.
> - Jak przekazywanie danych w czasie wykonywania do strony układu.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - Bloki zawartości, które są pliki, które zawierają zawartości do wstawienia na wielu stronach w formacie HTML.
> - Układ strony, strony, które zawierają zawartości w formacie HTML, który może być współużytkowany przez strony w witrynie sieci Web.
> - `RenderPage`, `RenderBody`, I `RenderSection` metod, które informują ASP.NET miejsca do wstawienia elementy na stronie.
> - `PageData` Słownik, który pozwala udostępniać dane między bloki zawartości i układ strony.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Układ strony — informacje

Wiele witryn sieci Web ma zawartość, która jest wyświetlana na każdej stronie, takie jak nagłówek i stopki lub pole informujący użytkowników, że są one rejestrowane w. ASP.NET umożliwia utworzenie osobnego pliku z bloku zawartości, zawierających tekst, znaczników i kodu, podobnie jak regularne strony sieci web. Następnie można wstawić zawartości bloku w innych stron w witrynie, w którym ma się pojawić informacje. Dzięki temu nie trzeba skopiować i wkleić tę samą zawartość do każdej strony. Tworzenie wspólnej zawartości, takiej jak to również ułatwia zaktualizować lokację. Jeśli musisz zmienić zawartość, można aktualizować tylko jednego pliku, a zmiany są następnie widoczne wszędzie włożono zawartości.

Na poniższym diagramie przedstawiono, jak zawartość zablokuje prac. Gdy przeglądarką zażąda strony z serwera sieci web, ASP.NET wstawia bloki zawartości w punkcie gdzie `RenderPage` metoda jest wywoływana na stronie głównej. Zakończono strony (scalonych) są następnie wysyłane do przeglądarki.

![Diagram koncepcyjny przedstawiający sposób metody RenderPage Wstawia strony do którego istnieje odwołanie do bieżącej strony.](3-creating-a-consistent-look/_static/image1.jpg)

W tej procedurze utworzysz strony, który odwołuje się do dwa bloki zawartości (nagłówek i stopkę), które znajdują się w oddzielnych plikach. Te bloki zawartości tej samej można użyć w dowolnej strony w witrynie. Gdy wszystko będzie gotowe, zostanie wyświetlony strony następująco:

![Zrzut ekranu przedstawiający stronę w przeglądarce, która powoduje uruchomienie strony, która zawiera wywołania metody RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. W folderze głównym witryny sieci Web, Utwórz plik o nazwie *Index.cshtml*.
2. Zastąp istniejący kod znaczników następujące czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. W folderze głównym, Utwórz folder o nazwie *Shared*.

    > [!NOTE]
    > Jest typowym rozwiązaniem do przechowywania plików, które są udostępniane między stronami sieci web w folderze o nazwie *Shared*.
4. W *Shared* folderu, Utwórz plik o nazwie  *\_Header.cshtml*.
5. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Należy zauważyć, że nazwa pliku jest  *\_Header.cshtml*, od znaku podkreślenia (\_) jako prefiksu. ASP.NET nie będzie wysyłać strony do przeglądarki, jeśli jej nazwa rozpoczyna się od znaku podkreślenia. Zapobiega to żądanie (przypadkowego lub w inny sposób) tych stron bezpośrednio osób. Zaleca się użyć znaku podkreślenia do nazwy stron, które zostały w nich bloki zawartości, ponieważ naprawdę uniemożliwić użytkownikom możliwość żądania te strony &#8212; istnieją bezwzględnie ma zostać wstawiony do innych stron.
6. W *Shared* folderu, Utwórz plik o nazwie  *\_Footer.cshtml* i Zastąp zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. W *Index.cshtml* Dodaj dwa wywołań `RenderPage` metody, jak pokazano poniżej:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    To pokazuje, jak można wstawić zawartości bloku do strony sieci web. Należy wywołać `RenderPage` — metoda i przekaż go nazwę pliku, którego zawartość ma zostać wstawiony w tym momencie. W tym miejscu wstawiania zawartość  *\_Header.cshtml* i  *\_Footer.cshtml* pliki *Index.cshtml* pliku.
8. Uruchom *Index.cshtml* strony w przeglądarce. (W programie WebMatrix, w **pliki** obszaru roboczego, kliknij prawym przyciskiem myszy plik, a następnie wybierz **Uruchom w przeglądarce**.)
9. W przeglądarce Wyświetl źródło strony. (Na przykład w przeglądarce Internet Explorer, kliknij prawym przyciskiem myszy strony, a następnie kliknij przycisk **Wyświetl źródło**.)

    Dzięki temu można zobaczyć znaczników strony sieci web, który jest wysyłany do przeglądarki, która łączy znaczników indeksu z bloki zawartości. W poniższym przykładzie przedstawiono źródło strony, który jest renderowany *Index.cshtml*. Wywołania `RenderPage` wstawioną *Index.cshtml* zostały zamienione na pliki nagłówków i stopek rzeczywistej zawartości.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Tworzenie spójny wygląd za pomocą strony układu

Do tej pory przedstawiono jest łatwy do uwzględnienia w tej samej zawartości na wielu stronach. Bardziej ustrukturyzowanymi sposobem tworzenia spójny wygląd witryny jest użyć strony układu. Strona układu definiuje strukturę strony sieci web, ale nie zawiera żadnych rzeczywistej zawartości. Po utworzeniu strony układu, można utworzyć strony sieci web z zawartością, a następnie połącz je do strony układu. Te strony są wyświetlane, będzie można sformatować zgodnie ze strony układu. (W tym sensie, strony układu działa jako rodzaj szablonu dla zawartości, która jest zdefiniowana w innych stron.)

Strona układu jest tak samo jak dowolnej strony HTML, z tą różnicą, że zawiera on wywołanie `RenderBody` metody. Pozycja `RenderBody` metody na stronie układu określa, gdzie będą się znajdować informacje ze strony zawartość.

Na poniższym diagramie przedstawiono sposób zawartości strony i układ strony są łączone w czasie wykonywania, aby wygenerować Zakończono strony sieci web. Przeglądarka żąda strony zawartość. Strona zawartości zawiera kod określający strony układu dla strony struktury. Na stronie układu dodaje się zawartość w punkcie gdzie `RenderBody` metoda jest wywoływana. Bloki również mogą być wstawiane do strony układu, wywołując zawartości `RenderPage` metody, sposób jak w poprzedniej sekcji. Po zakończeniu strony sieci web jest wysyłany do przeglądarki.

![Zrzut ekranu przedstawiający stronę w przeglądarce, która powoduje uruchomienie strony, która zawiera wywołania metody RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

Poniższa procedura przedstawia sposób utworzyć układ strony zawartości strony oraz łącza do niego.

1. W *Shared* folderu witryny sieci Web, Utwórz plik o nazwie  *\_Layout1.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Możesz użyć `RenderPage` metody na stronie układu, aby wstawić bloki zawartości. Strona układu może zawierać tylko jedno wywołanie `RenderBody` metody.
3. W *Shared* folderu, Utwórz plik o nazwie  *\_Header2.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. W folderze głównym, Utwórz nowy folder i nadaj mu nazwę *style*.
5. W *style* folderu, Utwórz plik o nazwie *Site.css* i dodaj następujące definicje stylu:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Te definicje stylu w tym miejscu są jedynie pokazanie, jak arkusze stylów, można użyć z strony układu. Można również zdefiniować style dla tych elementów.
6. W folderze głównym, Utwórz plik o nazwie *Content1.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Jest to strona, który będzie używany przez strony układu. Blok kodu w górnej części strony wskazuje stronę układu, która ma być będą formatowane tej zawartości.
7. Uruchom *Content1.cshtml* w przeglądarce. Renderowanej strony używa formatu i arkusza stylów określonego w  *\_Layout1.cshtml* i tekst (zawartość) zdefiniowana w *Content1.cshtml*.

    ![[Obraz]](3-creating-a-consistent-look/_static/image4.jpg)

    Można powtórzyć krok 6, aby utworzyć dodatkowe strony zawartości, które można następnie udostępnić strony układu.

    > [!NOTE]
    > Można skonfigurować witryny, dzięki czemu może automatycznie używać tej samej strony układu dla wszystkich stron zawartości w folderze. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowanie całej witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Projektowanie układu stron, które mają wiele sekcji zawartości

Strony zawartości, może mieć wiele sekcji, co jest przydatne, jeśli chcesz używać układów mających wiele obszarów z zawartością wymienne. Na stronie zawartości Nadaj każdej sekcji unikatową nazwę. (Domyślnej sekcji zostanie pozostawiony bez nazwy.) Na stronie układu, możesz dodać `RenderBody` metodę, aby określić, gdzie powinna zostać wyświetlona w sekcji nienazwane (ustawienie domyślne). Następnie dodasz oddzielnych `RenderSection` metod w celu renderowania sekcji o nazwie pojedynczo.

Na poniższym diagramie przedstawiono, jak ASP.NET obsługuje zawartość, która jest podzielony na wiele sekcji. Każdej nazwanej sekcji znajduje się w sekcji bloku na stronie zawartości. (W przypadku o nazwie `Header` i `List` w przykładzie.) Platformę wstawia sekcji zawartości do strony układu w punkcie gdzie `RenderSection` metoda jest wywoływana. Dodaje się w sekcji nienazwane (ustawienie domyślne) w punkcie gdzie `RenderBody` metoda jest wywoływana, jak przedstawiono wcześniej.

![Diagram koncepcyjny przedstawiający sposób metody RenderSection wstawia sekcje odwołania do bieżącej strony.](3-creating-a-consistent-look/_static/image5.jpg)

W tej procedurze pokazano, jak utworzyć strony zawartości, który ma wiele sekcji zawartości i sposób renderowania za pomocą strony układu, która obsługuje wiele sekcji zawartości.

1. W *Shared* folderu, Utwórz plik o nazwie  *\_Layout2.cshtml*.
2. Zastąp istniejącą zawartość następujących czynności:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Możesz użyć `RenderSection` metody do renderowania sekcje nagłówek i listy.
3. W folderze głównym, Utwórz plik o nazwie *Content2.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Ta strona zawartości zawiera blok kodu w górnej części strony. Każdej nazwanej sekcji znajduje się w bloku sekcji. Pozostała część strony zawiera sekcja zawartości domyślnego (bez nazwy).
4. Uruchom *Content2.cshtml* w przeglądarce.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce, która powoduje uruchomienie strony, która zawiera wywołania metody RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Tworzenie opcjonalne sekcje zawartości

Zwykle sekcje, które można utworzyć na stronie zawartości musi odpowiadać sekcje, które są zdefiniowane w strony układu. Mogą wystąpić błędy, ponieważ wystąpienia któregokolwiek z następujących czynności:

- Strona zawartości zawiera sekcja, która nie ma odpowiedniej sekcji w strony układu.
- Strona układu zawiera sekcji, dla której nie ma żadnej zawartości.
- Strona układu zawiera wywołania metody, które próbują renderowania więcej niż raz w tej samej sekcji.

Może jednak zmienić to zachowanie dla nazwanej sekcji przez zadeklarowanie sekcja ma być opcjonalne w strony układu. Dzięki temu można zdefiniować wiele stron zawartości, które współużytkują strony układu, ale który może lub nie może mieć zawartości dla określonej sekcji.

1. Otwórz *Content2.cshtml* i usunąć następujących sekcji:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Zapisz stronę, a następnie uruchom go w przeglądarce. Wyświetlany jest komunikat o błędzie, ponieważ zawartość strony nie zapewnia zawartości sekcji zdefiniowana na stronie układu, czyli sekcji nagłówka.

    ![Zrzut ekranu pokazujący błąd występujący po uruchomieniu strony, która wywołuje metodę RenderSection, ale nie podano odpowiedniej sekcji.](3-creating-a-consistent-look/_static/image7.jpg)
3. W *Shared* folder, otwórz  *\_Layout2.cshtml* strony i Zastąp ten wiersz:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    następującym kodem:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Alternatywnie można zastąpić poprzedniego wiersza kodu z następujący blok kodu, który jest taki sam:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Uruchom *Content2.cshtml* strony w przeglądarce ponownie. (Jeśli nadal masz tę stronę, Otwórz w przeglądarce, możesz po prostu odświeżyć go). Teraz zostanie wyświetlona strona nie błąd, nawet jeśli go nie ma nagłówka.

## <a name="passing-data-to-layout-pages"></a>Przekazywanie danych do strony układu

Konieczne może być danych zdefiniowana na stronie zawartości, który należy do odwoływania się do strony układu. Jeśli tak, należy przekazać dane ze strony zawartość do strony układu. Na przykład możesz wyświetlić stan logowania użytkownika lub może być pokazać lub ukryć zawartości obszary, w oparciu o dane wejściowe użytkownika.

Do przekazywania danych ze strony zawartości do strony układu, które można wprowadzić wartości do `PageData` właściwości strony zawartość. `PageData` Właściwości to zbiór par nazw i wartości, które zawierają dane, które mają być przekazywane między stronami. Na stronie układu może następnie odczytać wartości z `PageData` właściwości.

Oto innego diagramu. Ta pokazuje, jak używać ASP.NET `PageData` właściwości do przekazania wartości ze strony zawartości do strony układu. Po rozpoczęciu tworzenia strony sieci web ASP.NET tworzy `PageData` kolekcji. Na stronie zawartości, należy napisać kod, aby umieszczać dane `PageData` kolekcji. Wartości w `PageData` kolekcji są dostępne dodatkowe bloki zawartości lub innych części strony zawartość.

![Diagram koncepcyjny przedstawiający sposób strony zawartości można wypełnić słownika PageData i przekazać te informacje do strony układu.](3-creating-a-consistent-look/_static/image8.jpg)

Poniższa procedura przedstawia sposób przekazywania danych ze strony zawartości do strony układu. Po uruchomieniu na stronie, wyświetla przycisku, który umożliwia użytkownikowi ukrycie lub pokazanie listy, która jest zdefiniowana na stronie układu. Gdy użytkownik kliknie przycisk, ustawienie wartości PRAWDA/FAŁSZ (wartość logiczna) w `PageData` właściwości. Strona układu odczytuje tę wartość i jeśli ma wartość false, ukrywa listę. Wartość jest używany również na stronie zawartości do określenia, czy mają być wyświetlane **Ukryj listę** przycisk lub **Wyświetl listę** przycisku.

![[Obraz]](3-creating-a-consistent-look/_static/image9.jpg)

1. W folderze głównym, Utwórz plik o nazwie *Content3.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Kod przechowuje dwie części danych w `PageData` właściwości &#8212; tytuł strony sieci web i wartość PRAWDA lub FAŁSZ, aby określić, czy do wyświetlania listy.

    Zwróć uwagę, że program ASP.NET pozwala poddane kod znaczników HTML strony warunkowo przy użyciu bloków kodu. Na przykład `if/else` bloku w treści strony określa, które formularz służący do wyświetlania w zależności od tego, czy `PageData["ShowList"]` jest ustawiona na true.
2. W *Shared* folderu, Utwórz plik o nazwie  *\_Layout3.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Strona układu zawiera wyrażenie w `<title>` element, który pobiera wartość tytuł z `PageData` właściwości. Ponadto użyto `ShowList` wartość `PageData` właściwości w celu określenia, czy do wyświetlenia listy zawartości bloku.
3. W *Shared* folderu, Utwórz plik o nazwie  *\_List.cshtml* i Zastąp istniejącą zawartość następującym kodem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Uruchom *Content3.cshtml* strony w przeglądarce. Ta strona jest wyświetlana z listą widoczne w lewej części strony i **Ukryj listę** znajdujący się u dołu.

    ![Zrzut ekranu przedstawiający stronę zawierającą listy i przycisku "Lista Ukryj".](3-creating-a-consistent-look/_static/image10.jpg)
5. Kliknij przycisk **Ukryj listę**. Listy zniknie i przycisku zmienia się na **Wyświetl listę**.

    ![Zrzut ekranu przedstawiający stronę, która nie zawiera listy i przycisku Wyświetl listę.](3-creating-a-consistent-look/_static/image11.jpg)
6. Kliknij przycisk **Wyświetl listę** przycisk, a lista zostanie wyświetlony ponownie.

## <a name="additional-resources"></a>Dodatkowe zasoby


[Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
