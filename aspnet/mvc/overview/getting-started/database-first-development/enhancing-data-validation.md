---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: "Baza danych EF najpierw o platformie ASP.NET MVC: udoskonalanie sprawdzanie poprawności danych | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 465be4b51ab280d0b3e2f4b33362fa771480d5e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Baza danych EF najpierw o platformie ASP.NET MVC: udoskonalanie sprawdzanie poprawności danych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na dodawanie adnotacji danych do modelu danych, aby określić wymagania weryfikacji i wyświetlenie formatowania. Zostało ulepszone oparte na opinii użytkowników w sekcji uwag.


## <a name="add-data-annotations"></a>Dodawanie adnotacji danych

Jak przedstawiono w temacie wcześniejszych niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika. Można na przykład tylko podać numer dla właściwości klasy. Aby określić dalszych reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu. Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości. Można także zastosować atrybuty formatowania, które spowodują zmianę sposobu wyświetlania właściwości; takie jak zmiana wartości używane do etykiet tekstowych.

W tym samouczku spowoduje dodanie adnotacji danych, aby ograniczyć długość wartości określone dla właściwości FirstName, LastName i MiddleName. W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków aktualnie nie jest wymuszana. Jeśli użytkownik udostępnia więcej niż 50 znaków dla jednej z tych wartości, strony ulegnie awarii podczas próby zapisania wartość w bazie danych. Klasa będzie również ograniczyć do wartości od 0 do 4.

Otwórz **Student.cs** w pliku **modele** folderu. Dodaj następujący wyróżniony kod do klasy.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

W Enrollment.cs Dodaj następujący wyróżniony kod.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Skompiluj rozwiązanie.

Przejdź do strony do edytowania lub tworzenia studenta. Jeśli spróbujesz wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

Przejdź do strony do edycji rejestracji, a podejmowana próba udostępnienia klasy powyżej 4.

![Błąd zakresu klasy](enhancing-data-validation/_static/image2.png)

Pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Dodawanie klasy metadanych

Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i potrzebujesz można ponownie wygenerować klasy modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu. Ta metoda może być bardzo mało wydajne i podatne na utraty reguł sprawdzania poprawności ważne.

Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty. Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu. W tej metody klasy modelu można ponownie wygenerować bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.

W **modele** folderu, Dodaj klasę o nazwie **Metadata.cs**.

![Dodaj klasę metadanych](enhancing-data-validation/_static/image3.png)

Zastąp kod w Metadata.cs następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które zastosowano wcześniej dla klasy modelu. **Wyświetlania** atrybut służy do zmiany wartość używaną do tekstu etykiety.

Teraz należy skojarzyć klasy modelu z klasy metadanych.

W **modele** folderu, Dodaj klasę o nazwie **PartialClasses.cs**.

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każdy odpowiada nazwę i przestrzeń nazw jako klasa, która jest generowana automatycznie. Zastosowanie atrybutu metadanych do klasy częściowej, sprawdź, czy atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie. Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w częściowej klasy, które nie są generowane.

Aby ponownie wygenerować klasy generowane automatycznie, otwórz plik ContosoModel.edmx. Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz pozycję **modelu aktualizacji z bazy danych**. Mimo że nie zmieniono bazy danych, ten proces spowoduje ponowne wygenerowanie klasy. W **Odśwież** wybierz opcję **tabel** i **Zakończ**.

![Odśwież tabele](enhancing-data-validation/_static/image4.png)

Zapisz plik ContosoModel.edmx w celu zastosowania zmian.

Otwórz plik Student.cs lub plik Enrollment.cs i nie są zastosowane wcześniej atrybuty weryfikacji danych w pliku. Uruchom aplikację i zwróć uwagę, reguły walidacji nadal są stosowane podczas wprowadzania danych.

>[!div class="step-by-step"]
[Poprzednie](customizing-a-view.md)
[dalej](publish-to-azure.md)
