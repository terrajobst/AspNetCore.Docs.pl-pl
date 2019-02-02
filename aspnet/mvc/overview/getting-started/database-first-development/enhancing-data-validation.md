---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Samouczek: Zwiększ sprawdzania poprawności danych dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na temat dodawania adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667625"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Zwiększ sprawdzania poprawności danych dla platformy EF Database First w aplikacji ASP.NET MVC

Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.

Ten samouczek koncentruje się na temat dodawania adnotacji danych do modelu danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania. Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodawanie adnotacji danych
> * Dodawanie klasy metadanych

## <a name="prerequisites"></a>Wymagania wstępne

* [Dostosowywanie widoku](customizing-a-view.md)

## <a name="add-data-annotations"></a>Dodawanie adnotacji danych

Jak pokazano w poprzednim temacie niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika. Można na przykład tylko podać numer dla właściwości klasy korporacyjnej. Aby określić więcej reguł sprawdzania poprawności danych, można dodać adnotacje danych na klasę modelu. Adnotacje są stosowane w całej aplikacji sieci web dla określonej właściwości. Można także zastosować atrybutów formatowania, które zmieniają się, jak wyświetlić właściwości; takie jak zmiana wartości używanego do etykiet tekstu.

W tym samouczku zostaną dodane adnotacje danych, aby ograniczyć długość wartości podanych dla właściwości FirstName, LastName i MiddleName. W bazie danych te wartości są ograniczone do 50 znaków. Jednak w aplikacji sieci web ten limit znaków: obecnie nie są wymuszane. Jeśli użytkownik poda więcej niż 50 znaków, dla jednego z tych wartości, strony ulegnie awarii podczas próby zapisania wartości w bazie danych. Zostanie również ograniczyć klasy korporacyjnej, do wartości z zakresu od 0 do 4.

Wybierz **modeli** > **ContosoModel.edmx** > **ContosoModel.tt** , a następnie otwórz *Student.cs* pliku. Dodaj następujący wyróżniony kod do klasy.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Otwórz *Enrollment.cs* i Dodaj następujący wyróżniony kod.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Skompiluj rozwiązanie.

Kliknij przycisk **listę uczniów** i wybierz **Edytuj**. Jeśli użytkownik podejmie próbę wprowadzić więcej niż 50 znaków, jest wyświetlany komunikat o błędzie.

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

Wróć do strony głównej. Kliknij przycisk **listę rejestracje** i wybierz **Edytuj**. Podjęto próbę zapewnienie jakości powyżej 4. Zostanie wyświetlony ten błąd: *Pola, które klasy korporacyjnej musi należeć do zakresu od 0 do 4.*

## <a name="add-metadata-classes"></a>Dodawanie klasy metadanych

Dodawanie atrybutów sprawdzania poprawności bezpośrednio do klasy modelu działa, gdy nie będzie bazy danych, aby zmienić; Jednak jeśli zmian w bazie danych i chcesz ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty, które były stosowane do klasy modelu. Takie podejście może być bardzo mało wydajne i podatne na utratę reguł sprawdzania poprawności ważne.

Aby uniknąć tego problemu, można dodać klasę metadanych, który zawiera atrybuty. Po skojarzeniu klasy modelu do klasy metadanych te atrybuty są stosowane do modelu. W tym podejściu klasy modelu może być generowany ponownie bez utraty wszystkie atrybuty, które zostały zastosowane do klasy metadanych.

W **modeli** folderu, Dodaj klasę o nazwie *Metadata.cs*.

Zastąp kod w *Metadata.cs* następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Te klasy metadanych zawiera wszystkie atrybuty weryfikacji, które były zastosowane wcześniej klasy modelu. **Wyświetlania** atrybut jest używany, aby zmienić wartość etykiety tekstowe.

Teraz należy skojarzyć klas modelu za pomocą klasy metadanych.

W **modeli** folderu, Dodaj klasę o nazwie *PartialClasses.cs*.

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Należy zauważyć, że każda klasa jest oznaczona jako `partial` klasy, a każda jest zgodna nazwa i nazw jako klasa, która jest generowana automatycznie. Stosowanie atrybutu metadanych do klasy częściowej, gwarantuje, że atrybutów sprawdzania poprawności danych zostaną zastosowane do klasy generowane automatycznie. Te atrybuty nie zostaną utracone podczas ponownego generowania klasy modelu, ponieważ atrybut metadanych jest stosowany w klas częściowych, które nie są generowane.

Aby ponownie wygenerować automatycznie wygenerowane klasy, otwórz *ContosoModel.edmx* pliku. Ponownie, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Model aktualizacji z bazy danych**. Mimo że nie uległy zmianie bazy danych, ten proces spowoduje to ponowne wygenerowanie klasy. W **Odśwież** zaznacz **tabel** i **Zakończ**.

Zapisz *ContosoModel.edmx* plik, aby zastosować zmiany.

Otwórz *Student.cs* pliku lub *Enrollment.cs* plików i zwróć uwagę, że atrybutów sprawdzania poprawności danych, które są stosowane w starszych nie są już w pliku. Uruchom aplikację i zwróć uwagę, czy reguły sprawdzania poprawności nadal są stosowane podczas wprowadzania danych.

## <a name="conclusion"></a>Wniosek

Ta seria podać prosty przykład sposobu generowania kodu z istniejącej bazy danych, która umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych. ASP.NET MVC 5, platformy Entity Framework i ASP.NET tworzenie szkieletów on używany do tworzenia projektu. 

Wprowadzający przykład rozwiązania deweloperskiego Code First, zobacz [wprowadzenie do ASP.NET MVC 5](../introduction/getting-started.md). 

Na przykład bardziej zaawansowanych, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji platformy ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Pamiętaj, że interfejs API typu DbContext, służące do pracy z danymi w pierwszej bazy danych jest taki sam jak interfejs API, można użyć do pracy z danymi w Code First. Nawet wtedy, gdy użytkownik zamierza użyć pierwszej bazy danych, możesz dowiedzieć się, jak do obsługi bardziej złożonych scenariuszy, takich jak odczytywanie i aktualizowanie powiązanych danych, obsługa konfliktów współbieżności, i innych elementów z samouczka Code First. Jedyną różnicą jest w sposób tworzenia bazy danych, klasy kontekstu i klas jednostek.

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać pełną listę można zastosować do klasy i właściwości adnotacji sprawdzania poprawności danych, zobacz [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dane dodane adnotacje
> * Dodano metadanych klas

Aby dowiedzieć się, jak wdrożyć aplikację sieci web i bazy danych SQL w usłudze Azure App Service, zobacz w tym samouczku:
> [!div class="nextstepaction"]
> [Wdrażanie aplikacji .NET w usłudze Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
