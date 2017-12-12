---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: "Sprawdzanie poprawności z idataerrorinfo — interfejs (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Stephen Walther przedstawiono sposób wyświetlania niestandardowych komunikatów o błędach zaimplementowanie idataerrorinfo — interfejs klasy modelu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c04088c576481e4a91676d7e6962c03b56e7a8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Sprawdzanie poprawności z idataerrorinfo — interfejs (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther przedstawiono sposób wyświetlania niestandardowych komunikatów o błędach zaimplementowanie idataerrorinfo — interfejs klasy modelu.


Celem tego samouczka jest wyjaśniono jeden z nich do przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. Sposób zapobiec ktoś przesyłania formularza HTML bez podawania wartości wymaganych pól formularza. W tym samouczku Dowiedz się jak przeprowadzić weryfikacji za pomocą interfejsu IErrorDataInfo.

## <a name="assumptions"></a>Założenia

W tym samouczku I Użyj bazy danych MoviesDB i filmy tabeli bazy danych. Ta tabela zawiera następujące kolumny:

<a id="0.5_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Identyfikator | int | False |
| Tytuł | Nvarchar(100) | False |
| Dyrektor | Nvarchar(100) | False |
| DateReleased | DataGodzina | False |


W tym samouczku używam programu Entity Framework firmy Microsoft do generowania klasy modelu mojej bazy danych. Klasa filmu generowane przez program Entity Framework jest wyświetlany na rysunku 1.


[![Jednostka film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Rysunek 01**: jednostki Movie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Aby dowiedzieć się więcej o korzystaniu z programu Entity Framework do generowania klasy modelu z bazy danych, zobacz temat zatytułowany Moje samouczek: Tworzenie klasy modelu Entity Framework.


## <a name="the-controller-class"></a>Klasa kontrolera

Możemy użyć kontrolera głównej do listy filmów i Utwórz nowości. Kod dla tej klasy są zawarte w wyświetlania 1.

**Wyświetlanie listy 1 - Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Klasy kontrolera głównej w 1 Lista zawiera dwa działania Create(). Pierwszą akcją Wyświetla formularza HTML służący do tworzenia nowych filmu. Drugiej akcji Create() wykonuje rzeczywiste Wstaw nowe filmu do bazy danych. Drugiej akcji Create() jest wywoływana po przesłaniu formularza wyświetlany przez pierwszą akcją Create() do serwera.

Zwróć uwagę, że drugiej akcji Create() zawiera następujące wiersze kodu:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

Właściwość IsValid zwraca wartość false, gdy występuje błąd weryfikacji. W takim przypadku widok Utwórz zawierający formularz HTML do tworzenia filmu zostanie wyświetlony ponownie.

## <a name="creating-a-partial-class"></a>Tworzenie klasy częściowe

Klasa film jest generowany przez program Entity Framework. Można wyświetlić kod klasy filmu Rozwiń plik MoviesDBModel.edmx w oknie Eksploratora rozwiązań i Otwórz plik MoviesDBModel.Designer.cs w edytorze kodu (patrz rysunek 2).


[![Kod dla obiekt filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Rysunek 02**: kod dla obiekt filmu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


Klasa film jest klasy częściowej. Oznacza to, że możemy dodać innej klasy częściowe o takiej samej nazwie, aby rozszerzyć funkcjonalność klasy filmu. Dodamy logiki sprawdzania poprawności do nowej klasy częściowej.

Dodaj klasę wyświetlania 2 do folderu modeli.

**Wyświetlanie listy 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

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


**Wyświetlanie listy 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Na przykład, Jeśli spróbujesz przypisać pusty ciąg dla właściwości Title, następnie komunikat o błędzie jest przypisany do słownika o nazwie \_błędy.

W tym momencie faktycznie nie powoduje żadnych zmian pustego ciągu można przypisać do właściwości tytuł i błąd zostanie dodany do prywatnego \_pola błędy. Musimy zaimplementować interfejsu idataerrorinfo — Aby udostępnić te błędy weryfikacji do platformę ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementowanie idataerrorinfo — interfejs

Idataerrorinfo — interfejs został częścią programu .NET framework od pierwszej wersji. Ten interfejs jest bardzo prosty interfejs:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Jeśli klasa implementuje interfejs idataerrorinfo —, platforma ASP.NET MVC zostanie użyty ten interfejs, podczas tworzenia wystąpienia klasy. Na przykład kontroler głównej akcji Create() akceptuje wystąpienia klasy film:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Platforma ASP.NET MVC tworzy wystąpienie filmu przekazany do akcji Create() za pomocą integratora modelu (DefaultModelBinder). Integrator modelu jest odpowiedzialny za tworzenie wystąpienia obiektu filmu przez powiązanie pola formularza HTML do wystąpienia obiektu filmu.

DefaultModelBinder wykrywa, czy klasa implementuje interfejs idataerrorinfo —. Jeśli klasa implementuje ten interfejs integratora modelu wywołuje indeksatora IDataErrorInfo.this dla każdej właściwości klasy. Jeśli indeksatora zwraca komunikat o błędzie integratora modelu dodaje ten komunikat o błędzie do modelowania stanu automatycznie.

DefaultModelBinder sprawdza również właściwość IDataErrorInfo.Error. Ta właściwość jest przeznaczona do reprezentowania błędy sprawdzania poprawności określonego niż właściwość skojarzonego z klasą. Na przykład można wymuszać reguły sprawdzania poprawności, która zależy od wartości wielu właściwości klasy filmu. W takim przypadku zwróci błąd sprawdzania poprawności z właściwości błędu.

Zaktualizowano klasy filmu w listę 4 implementuje interfejs idataerrorinfo —.

**Wyświetlanie listy 4 - Models\Movie.cs (implementuje idataerrorinfo —)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

W przypadku wyświetlania 4 sprawdza właściwości indeksatora \_kolekcji błędów, aby zobaczyć, czy zawiera klucz, który odpowiada nazwie właściwości przekazany do indeksatora. Jeśli nie było weryfikacji błędu skojarzony z właściwością, zwracany jest pustym ciągiem.

Nie należy modyfikować kontrolera głównej w dowolny sposób, aby użyć zmodyfikowane klasy filmu. Strona wyświetlona na rysunku 3 przedstawiono, co się stanie, gdy została wprowadzona żadna wartość dla pola formularza tytuł lub dyrektor.


[![Automatyczne tworzenie metod akcji](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Rysunek 03**: formularza z brakujące wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Należy zauważyć, sprawdzania poprawności wartości DateReleased automatycznie. Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd sprawdzania poprawności dla tej właściwości automatycznie, gdy nie ma wartości. Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, a następnie należy utworzyć niestandardowego integratora modelu.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób idataerrorinfo — interfejs umożliwia generowanie komunikatów o błędach weryfikacji. Po pierwsze utworzyliśmy częściowej klasy film, rozszerzający funkcje klasy częściowej filmu generowane przez program Entity Framework. Następnie dodano logikę weryfikacji filmu klasy OnTitleChanging() OnDirectorChanging() częściowe metod i. Na koniec wprowadziliśmy idataerrorinfo — interfejs, aby udostępnić te komunikatów dotyczących sprawdzania poprawności na platformę ASP.NET MVC.

>[!div class="step-by-step"]
[Poprzednie](performing-simple-validation-cs.md)
[dalej](validating-with-a-service-layer-cs.md)
