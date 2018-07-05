---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: ulepszanie weryfikacji danych | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: a328aa8aec2c512d77ddabec31b3785b8742e018
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376717"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF bazy danych, najpierw z platformą ASP.NET MVC: ulepszanie weryfikacji danych
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na dodawanie adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania. Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.


## <a name="add-data-annotations"></a>Dodawanie adnotacji danych

Jak pokazano w poprzednim temacie niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika. Można na przykład tylko podać numer dla właściwości klasy korporacyjnej. Aby określić więcej reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu. Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości. Można także zastosować atrybutów formatowania, które zmieniają się, jak wyświetlić właściwości; takie jak zmiana wartości używanego do etykiet tekstu.

W tym samouczku zostaną dodane adnotacje danych, aby ograniczyć długość wartości podanych dla właściwości FirstName, LastName i MiddleName. W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków: obecnie nie są wymuszane. Jeśli użytkownik poda więcej niż 50 znaków, dla jednego z tych wartości, strony ulegnie awarii podczas próby zapisania wartości w bazie danych. Zostanie również ograniczyć klasy korporacyjnej, do wartości z zakresu od 0 do 4.

Otwórz **Student.cs** w pliku **modeli** folderu. Dodaj następujący wyróżniony kod do klasy.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

W Enrollment.cs Dodaj następujący wyróżniony kod.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Skompiluj rozwiązanie.

Przejdź do strony edytowania lub tworzenia studenta. Jeśli użytkownik podejmie próbę wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

Przejdź do strony do edycji rejestracji i podejmowana próba udostępnienia klasy powyżej 4.

![Błąd zakresu klasy korporacyjnej](enhancing-data-validation/_static/image2.png)

Aby uzyskać pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Dodawanie klasy metadanych

Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i chcesz ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu. Takie podejście może być bardzo mało wydajne i podatne na utratę reguł sprawdzania poprawności ważne.

Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty. Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu. W tym podejściu klasy modelu może być generowany ponownie bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.

W **modeli** folderu, Dodaj klasę o nazwie **Metadata.cs**.

![Dodaj klasę metadanych](enhancing-data-validation/_static/image3.png)

Zastąp kod w Metadata.cs następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które były zastosowane wcześniej klasy modelu. **Wyświetlania** atrybut jest używany, aby zmienić wartość etykiety tekstowe.

Teraz należy skojarzyć klas modelu za pomocą klasy metadanych.

W **modeli** folderu, Dodaj klasę o nazwie **PartialClasses.cs**.

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każda jest zgodna nazwa i nazw jako klasa, która jest generowana automatycznie. Stosowanie atrybutu metadanych do klasy częściowej, gwarantuje, że atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie. Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w klas częściowych, które nie są generowane.

Aby ponownie wygenerować automatycznie wygenerowane klasy, otwórz plik ContosoModel.edmx. Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Model aktualizacji z bazy danych**. Mimo że nie uległy zmianie bazy danych, ten proces spowoduje to ponowne wygenerowanie klasy. W **Odśwież** zaznacz **tabel** i **Zakończ**.

![Odświeżenie tabel](enhancing-data-validation/_static/image4.png)

Zapisz plik ContosoModel.edmx, aby zastosować zmiany.

Otwórz plik Student.cs lub plik Enrollment.cs i zwróć uwagę, że atrybuty sprawdzania poprawności danych, które zostały zastosowane wcześniej nie są już w pliku. Uruchom aplikację i zwróć uwagę, czy reguły sprawdzania poprawności nadal są stosowane podczas wprowadzania danych.

> [!div class="step-by-step"]
> [Poprzednie](customizing-a-view.md)
> [dalej](publish-to-azure.md)
