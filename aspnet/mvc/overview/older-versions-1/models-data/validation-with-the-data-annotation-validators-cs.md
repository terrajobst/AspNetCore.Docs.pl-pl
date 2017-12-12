---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: "Weryfikowanie przy użyciu adnotacji danych modułów sprawdzania poprawności (C#) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Skorzystaj z integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów modułu sprawdzania poprawności..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 306dcb0197dfc9317ea9665dd2b1c058ba8bd712
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-c"></a>Weryfikowanie przy użyciu adnotacji danych modułów sprawdzania poprawności (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Skorzystaj z integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów atrybutów modułu sprawdzania poprawności i pracować z nimi w ramach jednostki firmy Microsoft.


Z tego samouczka dowiesz sposobu używania adnotacji danych modułów sprawdzania poprawności do przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC. Zaletą używania adnotacji danych modułów sprawdzania poprawności jest, czy umożliwiają one sprawdzania poprawności, po prostu dodając jeden lub więcej atrybutów — na przykład wymagane lub atrybut StringLength — do właściwości klasy.

Przed użyciem moduły weryfikacji adnotacji danych, należy pobrać integratora modelu adnotacji danych. Możesz pobrać próbki integratora modelu adnotacji danych w witrynie CodePlex, klikając [tutaj](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Należy zrozumieć, że integratora modelu adnotacji danych nie jest oficjalną częścią struktury Microsoft ASP.NET MVC. Mimo że integratora modelu adnotacji danych została utworzona przez zespół Microsoft ASP.NET MVC, Microsoft nie oferuje oficjalną pomoc techniczna dla integratora modelu adnotacji danych opisane i używane w tym samouczku.


## <a name="using-the-data-annotation-model-binder"></a>Za pomocą integratora modelu adnotacji danych

Aby można było używać integratora modelu adnotacji danych w aplikacji platformy ASP.NET MVC, należy najpierw dodać odwołanie do zestawu Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll. Wybierz opcję menu **projekt, Dodaj odwołanie**. Następnie kliknij przycisk **Przeglądaj** karcie i przejdź do lokalizacji, do którego pobrano (i rozpakowane) próbki integratora modelu adnotacji danych (zobacz **rysunek 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Rysunek 1**: Dodawanie odwołania do integratora modelu adnotacji danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Zaznacz zestawu Microsoft.Web.Mvc.DataAnnotations.dll i zestawu System.ComponentModel.DataAnnotations.dll, a następnie kliknij przycisk **OK** przycisku.


Nie można użyć zestawu System.ComponentModel.DataAnnotations.dll dołączone do programu .NET Framework Service Pack 1 z integratora modelu adnotacji danych. Należy użyć wersji zestawu System.ComponentModel.DataAnnotations.dll dołączone do pobierania próbki integratora modelu adnotacji danych.


Ponadto należy zarejestrować DataAnnotations integratora modelu w pliku Global.asax. Dodaj następujący wiersz kodu do aplikacji\_Start() obsługi zdarzeń, aby aplikacja\_metodę Start() wygląda następująco:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Ten wiersz kodu rejestruje ataAnnotationsModelBinder jako domyślnego integratora modelu dla całej aplikacji ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Przy użyciu modułu sprawdzania poprawności atrybutów adnotacji danych

Gdy używasz integratora modelu adnotacji danych służy do sprawdzania poprawności atrybutów modułu walidacji. Przestrzeń nazw System.ComponentModel.DataAnnotations zawiera następujące atrybuty modułu sprawdzania poprawności:

- Zakres — umożliwia sprawdzanie, czy wartość właściwości należące do określonego zakresu wartości.
- Wyrażenia regularnego — umożliwia sprawdzanie, czy wartość właściwości odpowiada wzorcowi określonemu wyrażeniu regularnemu.
- Wymagane — można oznaczać właściwość jako wymaganą.
- StringLength — umożliwia określenie maksymalnej długości dla właściwości ciągu.
- Sprawdzanie poprawności — klasa podstawowa dla wszystkich atrybutów modułu walidacji.

> [!NOTE] 
> 
> Jeśli Twoje potrzeby sprawdzania poprawności nie są spełnione przez standardowe moduły weryfikacji należy zawsze wybrać opcję tworzenia atrybutu niestandardowego modułu weryfikacji przez dziedziczenie nowy atrybut modułu sprawdzania poprawności z atrybutu podstawowego sprawdzania poprawności.


Klasa produktu w **wyświetlania 1** ilustruje sposób używania tych atrybutów modułu walidacji. Właściwości nazwę, opis i UnitPrice są oznaczone jako wymagane. Właściwość Name musi mieć długość ciągu, która jest mniejsza niż 10 znaków. Na koniec właściwość UnitPrice musi być zgodna wzorzec wyrażenia regularnego, który reprezentuje kwotę waluty.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Wyświetlanie listy 1**: Models\Product.cs

Klasa produktu ilustruje sposób używania jeden atrybut dodatkowe: atrybut Nazwa wyświetlana. Atrybut DisplayName umożliwia zmianę nazwy właściwości, gdy właściwość jest wyświetlany komunikat o błędzie. Zamiast wyświetlania komunikatu o błędzie "UnitPrice pole jest wymagane" można wyświetlić komunikat o błędzie "cena pole jest wymagane".

> [!NOTE] 
> 
> Aby dostosować komunikat o błędzie wyświetlany przez moduł weryfikacji można przypisać niestandardowy komunikat o błędzie do modułu sprawdzania poprawności właściwości komunikat o błędzie podobny do tego:`<Required(ErrorMessage:="This field needs a value!")>`


Można użyć klasy produktu w **wyświetlania 1** z akcji kontrolera Create() w **wyświetlania 2**. Ta akcja kontrolera zostanie ponownie Utwórz widok, gdy stanu modelu zawiera błędy.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Wyświetlanie listy 2**: Controllers\ProductController.vb

Na koniec można utworzyć widoku w **wyświetlania 3** prawym przyciskiem myszy działanie Create() i wybierając opcję menu **Dodaj widok**. Utwórz widok jednoznacznie przy użyciu klasy produktu jako klasa modelu. Wybierz **Utwórz** z listy rozwijanej zawartości widoku (zobacz **na rysunku 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Rysunek 2**: Dodawanie widoku Create

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Wyświetlanie listy 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Usuń pola Identyfikator w formularzu Utwórz generowane przez **Dodaj widok** opcji menu. Ponieważ w polu identyfikatora odnosi się do kolumny tożsamości, nie chcesz zezwolić użytkownikom na wprowadzanie wartości dla tego pola.


Jeśli Prześlij formularz służący do tworzenia produktu i nie należy wprowadzać wartości dla pól wymaganych, a następnie w wiadomości błąd sprawdzania poprawności **rysunek 3** są wyświetlane.

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Rysunek 3**: Brak pola wymaganego

Jeśli wprowadzisz nieprawidłowy waluty, a następnie komunikat o błędzie w **4 rysunek** jest wyświetlany.

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Rysunek 4**: nieprawidłowy waluty.

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Używanie modułów weryfikacji adnotacji danych z programu Entity Framework

Jeśli używasz programu Entity Framework firmy Microsoft można wygenerować klas modelu danych nie można zastosować atrybutów modułu walidacji bezpośrednio do klas. Ponieważ Entity Framework Designer generuje klasy modeli, wszystkie zmiany wprowadzone w klasach modelu zostaną zastąpione podczas następnego wprowadzenia zmian w projektancie.

Jeśli chcesz użyć moduły weryfikacji z klasy generowane przez program Entity Framework następnie musisz utworzyć meta klasy danych. Moduły weryfikacji dotyczą meta klasy danych zamiast stosować moduły weryfikacji, do rzeczywistych klasy.

Na przykład, załóżmy, że utworzono klasy filmu przy użyciu programu Entity Framework (zobacz **rysunek 5**). Załóżmy, ponadto, czy mają być tytuł filmu i dyrektora wymagane właściwości. W takim przypadku można utworzyć klasy częściowe i meta klasy danych w **listę 4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Rysunek 5**: klasa filmu generowane przez program Entity Framework

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Wyświetlanie listy 4**: Models\Movie.cs

Plik w **listę 4** zawiera dwie klasy o nazwie filmów oraz MovieMetaData. Klasa film jest klasy częściowej. Odnosi się do klasy częściowej generowane przez program Entity Framework, który jest zawarty w pliku DataModel.Designer.vb.

.NET framework nie obsługuje obecnie, właściwości częściowej. W związku z tym nie istnieje sposób się atrybutów modułu sprawdzania poprawności do właściwości klasy filmu zdefiniowane w pliku DataModel.Designer.vb przez zastosowanie atrybutów modułu sprawdzania poprawności do właściwości klasy filmu zdefiniowane w pliku w **listę 4**.

Zwróć uwagę, że klasy częściowej filmu zostanie nadany atrybut MetadataType, który wskazuje klasę MovieMetaData. Klasa MovieMetaData zawiera właściwości serwera proxy dla właściwości klasy filmu.

Moduł sprawdzania poprawności atrybutów są stosowane do właściwości klasy MovieMetaData. Tytuł, dyrektor i DateReleased zostały oznaczone jako wymagane właściwości. Właściwość dyrektor musi być przypisana ciąg, który zawiera mniej niż 5 znaków. Na koniec atrybut DisplayName jest stosowane do właściwości DateReleased, aby wyświetlić komunikat o błędzie, takie jak "Data wydania pole jest wymagane." zamiast błędu "DateReleased pole jest wymagane."

> [!NOTE] 
> 
> Należy zauważyć, że właściwości serwera proxy w klasie MovieMetaData nie ma potrzeby reprezentują ten sam typ jak odpowiednie właściwości w klasie filmu. Na przykład właściwość Dyrektor jest właściwością ciągu w klasie filmu i właściwości obiektu w klasie MovieMetaData.


Strona w **rysunek 6** przedstawiono zwrócony podczas wprowadzania nieprawidłowe wartości dla właściwości filmu komunikaty o błędach.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Rysunek 6**: Używanie modułów weryfikacji z programu Entity Framework ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób wykorzystać integratora modelu adnotacji danych do sprawdzania poprawności w aplikacji ASP.NET MVC. Przedstawiono sposób użycia różnych typów atrybutów modułu sprawdzania poprawności, takich jak wymaganych i atrybuty StringLength. Przedstawiono również sposób używania tych atrybutów, podczas pracy z programu Entity Framework firmy Microsoft.

>[!div class="step-by-step"]
[Poprzednie](validating-with-a-service-layer-cs.md)
[dalej](creating-model-classes-with-the-entity-framework-vb.md)
