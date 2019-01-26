---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Samouczek: Generowanie widoków dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten artykuł koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889785"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Generowanie widoków dla platformy EF Database First w aplikacji ASP.NET MVC

Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.

Ten artykuł koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodaj szkielet
> * Dodawanie łączy do nowych widoków
> * Wyświetlanie widoków dla uczniów
> * Wyświetlanie widoków rejestracji

## <a name="prerequisite"></a>Wymaganie wstępne

* [Tworzenie modeli danych i aplikacji sieci web](creating-the-web-application.md)

## <a name="add-scaffold"></a>Dodaj szkielet

Jesteś gotowy do generowania kodu, który zapewni operacji danych w warstwie standardowa dla klasy modelu. Możesz dodać kod przez dodawanie elementu szkieletu. Istnieje wiele opcji dla typu tworzenia szkieletów, które można dodać; w tym samouczku szkieletu obejmuje kontrolera i widoki, które odnoszą się do modeli dla uczniów i rejestracji, utworzony w poprzedniej sekcji.

Aby zachować spójność w projekcie, dodasz nowy kontroler do istniejącej **kontrolerów** folderu. Kliknij prawym przyciskiem myszy **kontrolerów** folder, a następnie wybierz **Dodaj** > **nowy element szkieletu**.

Wybierz **kontroler MVC 5 z widokami używający narzędzia Entity Framework** opcji. Ta opcja spowoduje wygenerowanie kontrolera i widoki dla aktualizowanie, usuwanie, tworzenie i wyświetlanie danych w modelu.

![Dodaj kontroler mvc](generating-views/_static/image2.png)

Wybierz **uczniów (ContosoSite.Models)** klasy modelu i wybierz **ContosoUniversityDataEntities (ContosoSite.Models)** dla klasy kontekstu. Zachowaj nazwę kontrolera jako **StudentsController**.

Kliknij przycisk **Dodaj**.

Jeśli otrzymasz komunikat o błędzie, może to być, ponieważ nie skompilowano projekt w poprzedniej sekcji. Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.

Po zakończeniu procedury generowania kodu zobaczysz nowy kontroler i widoków w swoim projekcie **kontrolerów** i **widoków** > **studentów** folderów .


Ponownie wykonaj te same czynności, ale Dodaj szkielet dla **rejestracji** klasy. Po zakończeniu będziesz mieć **EnrollmentsController.cs** plików i folderów w obszarze **widoków** o nazwie **rejestracje** z widokami Create, Delete, szczegółowe informacje, edycji i indeksu.

## <a name="add-links-to-new-views"></a>Dodawanie łączy do nowych widoków

Aby ułatwić przejście do nowych widoków, można dodać kilka hiperłącza do widoków indeksu dla uczniów i rejestracji. Otwórz plik w rozmiarze **widoków** > **macierzystego** > *Index.cshtml*, który jest stroną główną witryny. Dodaj następujący kod poniżej jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

W przypadku metody ActionLink pierwszy parametr jest tekst do wyświetlenia w linku. Drugi parametr jest to akcja, a trzeci parametr jest nazwa kontrolera. Na przykład pierwszy link wskazuje akcji indeksu w StudentsController. Rzeczywiste hiperłącze jest zbudowany z tych wartości. Pierwszy link ostatecznie przejście do **Index.cshtml** plików w ramach **widoków/uczniów** folderu.

## <a name="display-student-views"></a>Wyświetlanie widoków dla uczniów

Zweryfikuje kod poprawnie dodany do projektu wyświetla listę uczniów i umożliwia użytkownikom edytowanie, tworzenie lub usuwanie rekordów dla uczniów w bazie danych.

Kliknij prawym przyciskiem myszy **widoków** > **Home** > *Index.cshtml* pliku, a następnie wybierz **Pokaż w przeglądarce**. Na stronie głównej aplikacji wybierz **listę uczniów**.

![](generating-views/_static/image6.png)

Na **indeksu** strony, zwróć uwagę na listę uczniów i łącza, aby zmodyfikować te dane. Wybierz **Utwórz nowy** łącze i podanie kilku wartości dotyczących nowego studenta. Kliknij przycisk **Utwórz**i zwróć uwagę, dodaniu nowego studenta do listy.

Po powrocie **indeksu** wybierz opcję **Edytuj** połączyć, a następnie zmienić niektóre z wartości dla uczniów lub studentów. Kliknij przycisk **Zapisz**i zwróć uwagę, rekord dla uczniów został zmieniony.

Na koniec wybierz pozycję **Usuń** link i upewnij się, że chcesz usunąć rekord, klikając **Usuń** przycisku.

Bez pisania żadnego kodu, można dodać widoków, które wykonują typowe operacje na danych w tabeli dla uczniów.

Być może zauważono, że tekst etykiety dla pola jest oparty na właściwość bazy danych (takich jak **LastName**) który nie jest zawsze mają być wyświetlane na stronie sieci web. Na przykład, lepiej jest etykieta **nazwisko**. Naprawi ten problem wyświetlaną w dalszej części tego samouczka.

## <a name="display-enrollment-views"></a>Wyświetlanie widoków rejestracji

Baza danych zawiera relację jeden do wielu między tabelami, studentów i rejestracji i relacji jeden do wielu między tabelami kursu i rejestracji. Widoki dla rejestracji prawidłowo obsługiwać te relacje. Przejdź do strony głównej witryny i wybierz **listę rejestracje** link a następnie **Utwórz nowy** łącza.

W widoku wyświetlane formularz służący do tworzenia nowego rekordu rejestracji. W szczególności zwróć uwagę, że formularz zawiera **CourseID** listy rozwijanej i **StudentID** listy rozwijanej. Oba są wypełnione wartościami z powiązanych tabel.

Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowana zależności dla typu danych pola. **Klasa** musi zawierać cyfry, dzięki czemu jest wyświetlany komunikat o błędzie, jeśli zostanie podjęta próba zapewniają niezgodną wartość: *Pola klasy korporacyjnej musi być liczbą.*

Upewnieniu się, że widoki generowane automatycznie umożliwiają użytkownikom pracę z danymi w bazie danych. W następnym samouczku z tej serii możesz zaktualizować bazę danych i wprowadzić odpowiednie zmiany w aplikacji sieci web.

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dodano szkieletu
> * Dodano linki do nowych widoków
> * Widoki wyświetlanych dla uczniów
> * Widoki wyświetlane rejestracji

Przejdź do następnego artykułu, aby dowiedzieć się, jak zmienić bazę danych programu.
> [!div class="nextstepaction"]
> [Zmień bazę danych](changing-the-database.md)