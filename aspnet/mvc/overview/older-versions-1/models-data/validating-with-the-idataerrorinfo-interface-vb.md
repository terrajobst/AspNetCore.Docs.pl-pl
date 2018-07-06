---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Weryfikowanie z użyciem interfejsu IDataErrorInfo (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 9faaab92b7baf0af5f0d8718e1d80e26c29bcca0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399477"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Weryfikowanie z użyciem interfejsu IDataErrorInfo (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Autor: Stephen Walther dowiesz się, jak wyświetlać komunikaty o błędach niestandardowego sprawdzania poprawności poprzez implementację interfejsu IDataErrorInfo w klasie modelu.


Celem tego samouczka jest wyjaśnić jedno z podejść do przeprowadzania weryfikacji w aplikacji ASP.NET MVC. Dowiesz się, jak zapobiec ktoś przesyłania formularza HTML bez podawania wartości wymaganych pól formularza. W tym samouczku dowiesz się, jak przeprowadzić weryfikacji za pomocą interfejsu IErrorDataInfo.

## <a name="assumptions"></a>Założenia

W tym samouczku czy mogę użyć MoviesDB bazy danych i tabeli bazy danych filmów. Ta tabela zawiera następujące kolumny:

<a id="0.6_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(100) | False |
| Dyrektor ds. | Nvarchar(100) | False |
| DateReleased | DataGodzina | False |


W tym samouczku używam Microsoft Entity Framework do generowania klasy modelu mojej bazy danych. Klasa filmu generowane przez program Entity Framework jest wyświetlany na rysunku 1.


[![Jednostki filmu](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Rysunek 01**: jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Aby dowiedzieć się więcej o korzystaniu z programu Entity Framework do generowania klasy modelu z bazy danych, zobacz temat zatytułowany Moje samouczek: Tworzenie klas modelu za pomocą programu Entity Framework.


## <a name="the-controller-class"></a>Klasa kontrolera

Używane kontrolera głównego do filmów z listy i tworzenia nowych filmów. Kod dla tej klasy są zawarte w ofercie 1.

**Wyświetlanie listy 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Klasa kontrolera głównej w ofercie 1 zawiera dwie akcje Create(). Pierwszą akcją Wyświetla formularza HTML do tworzenia nowych filmów. Drugiej akcji Create() przeprowadza rzeczywiste Wstaw nowy film do bazy danych. Drugiej akcji Create() jest wywoływana po przesłaniu formularza wyświetlany przez pierwszą akcją Create() do serwera.

Zwróć uwagę, że druga Akcja Create() zawiera następujące wiersze kodu:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Właściwość IsValid zwraca wartość false, gdy występuje błąd weryfikacji. W takim przypadku widok Utwórz, który zawiera formularza HTML do tworzenia film zostanie wyświetlony ponownie.

## <a name="creating-a-partial-class"></a>Tworzenie klasy częściowe

Klasa film jest generowany przez program Entity Framework. Widać kod klasy filmu, jeśli Rozwiń plik MoviesDBModel.edmx w oknie Eksploratora rozwiązań i Otwórz plik MoviesDBModel.Designer.vb w edytorze kodu (patrz rysunek 2).


[![Kod dla jednostki filmu](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Rysunek 02**: kod dla jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Klasa film jest klasy częściowej. Oznacza to, że możemy dodać innej klasy częściowej o takiej samej nazwie, aby rozszerzyć funkcjonalność klasy filmu. Dodamy naszych logikę walidacji do nowej klasy częściowej.

Dodaj klasę w ofercie 2 do folderu modeli.

**Wyświetlanie listy 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Należy zauważyć, że klasa w ofercie 2 obejmuje *częściowe* modyfikator. Wszelkie metody lub właściwości, które możesz dodać do tej klasy stają się częścią klasy filmu generowane przez program Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Dodawanie OnChanging i metod OnChanged częściowego

Gdy platforma Entity Framework generuje klasę jednostki, platformy Entity Framework metod częściowych do klasy automatycznie dodaje. Entity Framework generuje OnChanging i OnChanged metod częściowych, które odpowiadają każdej właściwości klasy.

W przypadku klasy filmu programu Entity Framework tworzy następujące metody:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Metoda OnChanging jest wywoływana prawo, przed zmianą odpowiadającą właściwość. Metoda OnChanged jest wywoływana prawej, po zmianie właściwości.

Możesz korzystać z zalet tych metod częściowych, aby dodać logikę walidacji do klasy filmu. Aktualizacja klasy filmu w ofercie 3 sprawdza, czy tytuł i dyrektor ds. właściwości są przypisywane niepustych wartości.

> [!NOTE] 
> 
> Metoda częściowa jest metoda zdefiniowana w klasie, które nie są wymagane do wdrożenia. Jeśli nie implementować metodę częściową kompilator usuwa podpis metody i wszystkie wywołania do metody, więc są bez kosztów czasu wykonywania skojarzony z metody częściowej. W edytorze programu Visual Studio Code można dodać metody częściowej, wpisując słowa kluczowego *częściowe* następuje spacja, aby wyświetlić listę częściowych do zaimplementowania.


**Wyświetlanie listy 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Na przykład, jeśli użytkownik podejmie próbę przypisania pustego ciągu do właściwości tytułu, komunikat o błędzie jest przypisywany do słownika o nazwie \_błędy.

W tym momencie faktycznie nie powoduje żadnych zmian przypisania pustego ciągu do właściwości tytułu, a błąd jest dodawany do prywatnego \_pola błędy. Musimy zaimplementować interfejsu IDataErrorInfo, aby udostępnić te błędy sprawdzania poprawności, platforma ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementowanie interfejsu IDataErrorInfo

Interfejsu IDataErrorInfo było częścią programu .NET framework od pierwszej wersji. Ten interfejs jest bardzo prosty interfejs:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Jeśli klasa implementuje interfejsu IDataErrorInfo, platforma ASP.NET MVC użyje tego interfejsu, podczas tworzenia wystąpienia klasy. Na przykład kontrolera głównego działania Create() akceptuje wystąpienia klasy film:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Platforma ASP.NET MVC tworzy wystąpienie filmu przekazywane do działania Create() za pomocą integratora modelu (DefaultModelBinder). Integrator modelu jest odpowiedzialny za utworzenie wystąpienia obiektu filmu przez powiązanie pola formularza HTML do wystąpienia obiektu filmu.

DefaultModelBinder wykrywa, czy klasa implementuje interfejsu IDataErrorInfo. Jeśli klasa implementuje ten interfejs integratora modelu wywołuje indeksatora IDataErrorInfo.this dla każdej właściwości klasy. Jeśli indeksatora zwróci komunikat o błędzie integratora modelu dodaje ten komunikat o błędzie do modelowania stanu automatycznie.

DefaultModelBinder sprawdza również właściwość IDataErrorInfo.Error. Ta właściwość jest przeznaczona do reprezentowania błędy sprawdzania poprawności określonego inną niż właściwość skojarzony z klasą. Na przykład możesz chcieć wymusić regułę poprawności, który zależy od wartości wielu właściwości klasy filmu. W takiej sytuacji zwróci błąd sprawdzania poprawności z właściwości błędu.

Zaktualizowano klasy filmu w ofercie 4 implementuje interfejsu IDataErrorInfo.

**Wyświetlanie listy 4 - Models\Movie.vb (implementuje IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

W ofercie 4, sprawdza właściwości indeksatora \_kolekcji błędów, aby zobaczyć, czy zawiera klucz, który odpowiada nazwie właściwości przekazany do indeksatora. W przypadku braku błędów weryfikacji skojarzony z właściwością, zwracany jest pusty ciąg.

Nie potrzebujesz zmodyfikować kontrolera głównego w jakikolwiek sposób, aby użyć zmodyfikowane klasy filmu. Strona wyświetlona na rysunku 3 przedstawiono, co się stanie, gdy została wprowadzona żadna wartość dla pola formularza tytuł lub dyrektor ds.


[![Automatyczne tworzenie metod akcji](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Rysunek 03**: formularz z brakującymi wartościami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Zwróć uwagę, że wartość DateReleased jest automatycznie wykonane sprawdzanie poprawności. Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd sprawdzania poprawności dla tej właściwości automatycznie, gdy nie ma wartości. Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, a następnie należy utworzyć niestandardowego integratora modelu.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób korzystania z użyciem interfejsu IDataErrorInfo do generowania komunikatów o błędach weryfikacji. Po pierwsze utworzyliśmy częściową klasą filmów, która rozszerza funkcjonalność klasy częściowej filmu generowane przez program Entity Framework. Następnie dodaliśmy logikę walidacji do OnTitleChanging() i OnDirectorChanging() metod częściowych dla klasy filmu. Ponadto wprowadziliśmy interfejsu IDataErrorInfo, aby udostępnić te komunikaty weryfikacji platformę ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](performing-simple-validation-vb.md)
> [dalej](validating-with-a-service-layer-vb.md)
