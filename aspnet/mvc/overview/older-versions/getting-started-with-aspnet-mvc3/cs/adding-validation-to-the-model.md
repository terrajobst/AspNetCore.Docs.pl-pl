---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Dodawanie walidacji do modelu (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Tworzenie kontrolera
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: a1d6a6468a39f31c3af8779abbbced093288773c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model-c"></a>Dodawanie walidacji do modelu (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.


W tej sekcji dodasz logikę weryfikacji `Movie` modelu, a będzie wymusić reguł sprawdzania poprawności w dowolnym momencie, użytkownik próbuje do tworzenia lub edytowania filmu przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie suchej rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest suchej ("nie powtarzaj samodzielnie"). ASP.NET MVC zachęca do określone funkcje lub działanie tylko raz, a następnie go wszędzie odzwierciedlone w aplikacji. To zmniejsza ilość kodu, który należy napisać i sprawia, że kod napisany znacznie łatwiejsze do zachowania.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First jest doskonałym przykładem suchej zasady w akcji. Deklaratywnie można określić reguły sprawdzania poprawności w jednym miejscu (w klasie modelu), a następnie te zasady są wymuszane wszędzie w aplikacji.

Oto jak możliwość korzystania z tej obsługi sprawdzania poprawności w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji modelu film

Będzie rozpocząć, dodając logikę sprawdzania poprawności do `Movie` klasy.

Otwórz *Movie.cs* pliku. Dodaj `using` instrukcji w górnej części pliku, który odwołuje się do [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Przestrzeń nazw jest częścią programu .NET Framework. Zapewnia wbudowany zestaw atrybutów sprawdzania poprawności, które można zastosować deklaratywnie do klasy lub właściwości.

Teraz zaktualizować `Movie` klasy, aby móc korzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności . Użyć poniższego kodu, na przykład gdzie stosować atrybutów.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Atrybuty weryfikacji Określ zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane do. `Required` Atrybut wskazuje, że właściwość musi mieć wartość; w tym przykładzie filmu musi mieć wartości `Title`, `ReleaseDate`, `Genre`, i `Price` właściwości, aby był prawidłowy. `Range` Atrybut ogranicza wartość do określonego zakresu. `StringLength` Atrybut pozwala określić maksymalną długość ciągu właściwości oraz opcjonalnie długości minimalnej.

Kod najpierw gwarantuje, że reguły sprawdzania poprawności, wybrane na klasę modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład poniższy kod spowoduje zgłoszenie wyjątku podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości i cena wynosi zero (która jest poza prawidłowym zakresem).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

O reguł sprawdzania poprawności, automatycznie są wymuszane przez program .NET Framework pomaga zwiększyć bezpieczeństwo aplikacji bardziej niezawodne. Gwarantuje również, że nie zapomnisz do sprawdzania poprawności coś i przypadkowo let złe dane do bazy danych.

W tym miejscu jest kompletny kod dla zaktualizowanego *Movie.cs* pliku:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności interfejsu użytkownika na platformie ASP.NET MVC

Ponownie uruchom aplikację i przejdź do */Movies* adresu URL.

Kliknij przycisk **utworzyć film** łącze, aby dodać nowy filmu. Wypełnij formularz z niektóre nieprawidłowe wartości, a następnie kliknij przycisk **Utwórz** przycisku.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Zwróć uwagę, jak formularz automatycznie został użyty kolor tła aby zaznaczyć wszystkie pola, które zawierają nieprawidłowe dane i ma wysyłanego odpowiedni komunikat o błędzie weryfikacji obok każdego z nich. Komunikaty o błędach Dopasuj ciągi błąd, określić, kiedy adnotacji `Movie` klasy. Błędy są wymuszane zarówno po stronie klienta (przy użyciu języka JavaScript) i po stronie serwera (w przypadku, gdy użytkownik ma JavaScript wyłączone).

Rzeczywiste korzyści jest nie potrzebuję zmienić pojedynczy wiersz kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu umożliwienia tej weryfikacji interfejsu użytkownika. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierane sprawdzania poprawności reguły, określona przy użyciu atrybutów w górę `Movie` klasa modelu.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób sprawdzanie poprawności jest wykonywane w tworzenia, wyświetlania i tworzenia metody akcji

Może zastanawiasz się, jak weryfikacji interfejsu użytkownika został wygenerowany bez żadnych aktualizacji do kodu w kontrolerze lub widoków. Zawiera listę dalej, co `Create` metod w `MovieController` wygląd klasy. Są one takie same jak w sposób tworzenia we wcześniejszej części tego samouczka.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Pierwsza metoda akcji Wyświetla formularza tworzenia początkowego. Drugi obsługuje post formularza. Drugi `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy film ma jakieś błędy sprawdzania poprawności. Wywołanie tej metody ocenia wszystkie atrybuty weryfikacji, które zostały zastosowane do tego obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` metody ładowaniu formularza. Jeśli nie ma żadnych błędów, metoda zapisuje nowe filmu w bazie danych.

Poniżej znajduje się *Create.cshtml* Wyświetl szablon, który szkieletu wcześniej w samouczku. Jest on używany przez metody akcji pokazanym powyżej zarówno do wyświetlania formularza początkowego i wyświetl ją ponownie w przypadku wystąpienia błędu.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika do wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołanie `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować z obiektu modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie sprawdzania poprawności atrybutów określonych dla modelu i wyświetlanie komunikatów o błędach zależnie od potrzeb.

Naprawdę nieuprzywilejowany o tej metody jest to, że kontroler ani Utwórz szablon widoku zna niczego dotyczące reguł rzeczywista weryfikacja wymuszany lub określone komunikaty o błędach wyświetlane. Reguły sprawdzania poprawności i ciągi błąd są określane tylko w `Movie` klasy.

Aby później zmienić logikę weryfikacji, możesz to zrobić w dokładnie w jednym miejscu. Nie musisz martwić się o różnych częściach aplikacji jest niespójna z jak zasady są wymuszane — całą logikę sprawdzania poprawności zostanie zdefiniowana w jednym miejscu i używany wszędzie. Przechowuje kod bardzo czystą i ułatwia utrzymanie i rozwijać. I oznacza, że będziesz w pełni ramach suchej zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodanie formatowania do modelu film

Otwórz *Movie.cs* pliku. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanych zestaw atrybutów weryfikacji. Będzie stosowane [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu i [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) wartość wyliczenia Data wydania i pola cen. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Alternatywnie można jawnie ustawić [ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) wartości. Poniższy kod przedstawia właściwość Data wydania zawierające ciąg formatu daty (to znaczy, "d"). Spowoduje to użyć do określenia, że nie chcesz czas jako część Data wydania.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Poniższy kod formatów `Price` właściwość jako walutę.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Pełną `Movie` klasy są wyświetlane poniżej.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Uruchom aplikację i przejdź do `Movies` kontrolera.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

W następnej części serii, firma Microsoft będzie Przejrzyj aplikacji i poprawiają do automatycznie generowanego `Details` i `Delete` metody.

>[!div class="step-by-step"]
[Poprzednie](adding-a-new-field.md)
[dalej](improving-the-details-and-delete-methods.md)
