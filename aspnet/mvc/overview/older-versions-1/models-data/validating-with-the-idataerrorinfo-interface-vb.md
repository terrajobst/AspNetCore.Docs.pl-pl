---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: "Sprawdzanie poprawności z idataerrorinfo — interfejs (VB) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Stephen Walther przedstawiono sposób wyświetlania niestandardowych komunikatów o błędach zaimplementowanie idataerrorinfo — interfejs klasy modelu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 1439d470a7fa3cb1171dbdd0b7eec6a6aa52912d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Sprawdzanie poprawności z idataerrorinfo — interfejs (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther przedstawiono sposób wyświetlania niestandardowych komunikatów o błędach zaimplementowanie idataerrorinfo — interfejs klasy modelu.


Celem tego samouczka jest wyjaśniono jeden z nich do przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. Sposób zapobiec ktoś przesyłania formularza HTML bez podawania wartości wymaganych pól formularza. W tym samouczku Dowiedz się jak przeprowadzić weryfikacji za pomocą interfejsu IErrorDataInfo.

## <a name="assumptions"></a>Założenia

W tym samouczku I Użyj bazy danych MoviesDB i filmy tabeli bazy danych. Ta tabela zawiera następujące kolumny:

<a id="0.6_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Identyfikator | int | False |
| Tytuł | Nvarchar(100) | False |
| Dyrektor | Nvarchar(100) | False |
| DateReleased | DataGodzina | False |


W tym samouczku używam programu Entity Framework firmy Microsoft do generowania klasy modelu mojej bazy danych. Klasa filmu generowane przez program Entity Framework jest wyświetlany na rysunku 1.


[![Jednostka film](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Rysunek 01**: jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Aby dowiedzieć się więcej o korzystaniu z programu Entity Framework do generowania klasy modelu z bazy danych, zobacz temat zatytułowany Moje samouczek: Tworzenie klasy modelu Entity Framework.


## <a name="the-controller-class"></a>Klasa kontrolera

Możemy użyć kontrolera głównej do listy filmów i Utwórz nowości. Kod dla tej klasy są zawarte w wyświetlania 1.

**Wyświetlanie listy 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Klasy kontrolera głównej w 1 Lista zawiera dwa działania Create(). Pierwszą akcją Wyświetla formularza HTML służący do tworzenia nowych filmu. Drugiej akcji Create() wykonuje rzeczywiste Wstaw nowe filmu do bazy danych. Drugiej akcji Create() jest wywoływana po przesłaniu formularza wyświetlany przez pierwszą akcją Create() do serwera.

Zwróć uwagę, że drugiej akcji Create() zawiera następujące wiersze kodu:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

Właściwość IsValid zwraca wartość false, gdy występuje błąd weryfikacji. W takim przypadku widok Utwórz zawierający formularz HTML do tworzenia filmu zostanie wyświetlony ponownie.

## <a name="creating-a-partial-class"></a>Tworzenie klasy częściowe

Klasa film jest generowany przez program Entity Framework. Można wyświetlić kod klasy filmu Rozwiń plik MoviesDBModel.edmx w oknie Eksploratora rozwiązań i Otwórz plik MoviesDBModel.Designer.vb w edytorze kodu (patrz rysunek 2).


[![Kod dla obiekt filmu](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Rysunek 02**: kod dla obiekt filmu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Klasa film jest klasy częściowej. Oznacza to, że możemy dodać innej klasy częściowe o takiej samej nazwie, aby rozszerzyć funkcjonalność klasy filmu. Dodamy logiki sprawdzania poprawności do nowej klasy częściowej.

Dodaj klasę wyświetlania 2 do folderu modeli.

**Wyświetlanie listy 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Należy zauważyć, że klasa wyświetlania 2 zawiera *częściowe* modyfikator. Wszystkie metody lub właściwości, które można dodać do tej klasy staną się częścią klasy filmu generowane przez program Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Dodawanie OnChanging i metody częściowe OnChanged

Entity Framework, generując klasę jednostki, Entity Framework dodaje metody częściowe do klasy automatycznie. Entity Framework generuje OnChanging i OnChanged metody częściowe, które odpowiadają każdej właściwości klasy.

W przypadku klasy filmu programu Entity Framework utworzy następujących metod:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Metoda OnChanging jest wywoływana prawo, zanim odpowiadających im właściwości zostanie zmieniona. Metoda OnChanged jest wywoływana prawej, po zmianie właściwości.

Można korzystać z tych metod częściowych Dodaj logikę sprawdzania poprawności do klasy filmu. Aktualizacja klasy filmu w 3 wyświetlania sprawdza, czy tytuł i dyrektora przypisano niepusty wartości.

> [!NOTE] 
> 
> Metoda częściowa to metoda zdefiniowana w klasie, która nie jest wymagane do wdrożenia. Jeśli nie implementować metodę częściową kompilator usuwa podpis metody i wszystkie wywołania metody, więc są Brak kosztów czasu wykonywania skojarzony z metody częściowej. W Visual Studio edytorze kodu, można dodać metody częściowej wpisując słowo kluczowe *częściowe* następuje spacja, aby wyświetlić listę częściowe do zaimplementowania.


**Wyświetlanie listy 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Na przykład, Jeśli spróbujesz przypisać pusty ciąg dla właściwości Title, następnie komunikat o błędzie jest przypisany do słownika o nazwie \_błędy.

W tym momencie faktycznie nie powoduje żadnych zmian pustego ciągu można przypisać do właściwości tytuł i błąd zostanie dodany do prywatnego \_pola błędy. Musimy zaimplementować interfejsu idataerrorinfo — Aby udostępnić te błędy weryfikacji do platformę ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementowanie idataerrorinfo — interfejs

Idataerrorinfo — interfejs został częścią programu .NET framework od pierwszej wersji. Ten interfejs jest bardzo prosty interfejs:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Jeśli klasa implementuje interfejs idataerrorinfo —, platforma ASP.NET MVC zostanie użyty ten interfejs, podczas tworzenia wystąpienia klasy. Na przykład kontroler głównej akcji Create() akceptuje wystąpienia klasy film:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

Platforma ASP.NET MVC tworzy wystąpienie filmu przekazany do akcji Create() za pomocą integratora modelu (DefaultModelBinder). Integrator modelu jest odpowiedzialny za tworzenie wystąpienia obiektu filmu przez powiązanie pola formularza HTML do wystąpienia obiektu filmu.

DefaultModelBinder wykrywa, czy klasa implementuje interfejs idataerrorinfo —. Jeśli klasa implementuje ten interfejs integratora modelu wywołuje indeksatora IDataErrorInfo.this dla każdej właściwości klasy. Jeśli indeksatora zwraca komunikat o błędzie integratora modelu dodaje ten komunikat o błędzie do modelowania stanu automatycznie.

DefaultModelBinder sprawdza również właściwość IDataErrorInfo.Error. Ta właściwość jest przeznaczona do reprezentowania błędy sprawdzania poprawności określonego niż właściwość skojarzonego z klasą. Na przykład można wymuszać reguły sprawdzania poprawności, która zależy od wartości wielu właściwości klasy filmu. W takim przypadku zwróci błąd sprawdzania poprawności z właściwości błędu.

Zaktualizowano klasy filmu w listę 4 implementuje interfejs idataerrorinfo —.

**Wyświetlanie listy 4 - Models\Movie.vb (implementuje idataerrorinfo —)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

W przypadku wyświetlania 4 sprawdza właściwości indeksatora \_kolekcji błędów, aby zobaczyć, czy zawiera klucz, który odpowiada nazwie właściwości przekazany do indeksatora. Jeśli nie było weryfikacji błędu skojarzony z właściwością, zwracany jest pustym ciągiem.

Nie należy modyfikować kontrolera głównej w dowolny sposób, aby użyć zmodyfikowane klasy filmu. Strona wyświetlona na rysunku 3 przedstawiono, co się stanie, gdy została wprowadzona żadna wartość dla pola formularza tytuł lub dyrektor.


[![Automatyczne tworzenie metod akcji](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Rysunek 03**: formularza z brakujące wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Należy zauważyć, sprawdzania poprawności wartości DateReleased automatycznie. Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd sprawdzania poprawności dla tej właściwości automatycznie, gdy nie ma wartości. Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, a następnie należy utworzyć niestandardowego integratora modelu.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób idataerrorinfo — interfejs umożliwia generowanie komunikatów o błędach weryfikacji. Po pierwsze utworzyliśmy częściowej klasy film, rozszerzający funkcje klasy częściowej filmu generowane przez program Entity Framework. Następnie dodano logikę weryfikacji filmu klasy OnTitleChanging() OnDirectorChanging() częściowe metod i. Na koniec wprowadziliśmy idataerrorinfo — interfejs, aby udostępnić te komunikatów dotyczących sprawdzania poprawności na platformę ASP.NET MVC.

>[!div class="step-by-step"]
[Poprzednie](performing-simple-validation-vb.md)
[dalej](validating-with-a-service-layer-vb.md)
