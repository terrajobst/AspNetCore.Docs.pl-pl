---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: generowanie widoków | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835331"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF bazy danych, najpierw z platformą ASP.NET MVC: generowanie widoków
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.


## <a name="add-scaffold"></a>Dodaj szkielet

Jesteś gotowy do generowania kodu, który zapewni operacji danych w warstwie standardowa dla klasy modelu. Możesz dodać kod przez dodawanie elementu szkieletu. Istnieje wiele opcji dla typu tworzenia szkieletów, które można dodać; w tym samouczku szkieletu obejmuje kontrolera i widoki, które odnoszą się do modeli dla uczniów i rejestracji, utworzony w poprzedniej sekcji.

Aby zachować spójność w projekcie, dodasz nowy kontroler do istniejącej **kontrolerów** folderu. Kliknij prawym przyciskiem myszy **kontrolerów** folder, a następnie wybierz **Dodaj** — **nowy element szkieletu**.

![Dodaj szkielet](generating-views/_static/image1.png)

Wybierz **kontroler MVC 5 z widokami używający narzędzia Entity Framework** opcji. Ta opcja spowoduje wygenerowanie kontrolera i widoki dla aktualizowanie, usuwanie, tworzenie i wyświetlanie danych w modelu.

![Dodaj kontroler mvc](generating-views/_static/image2.png)

Wybierz **uczniów** dla klasy modelu, a następnie wybierz **ContosoUniversityEntities** dla klasy kontekstu. Zachowaj nazwę kontrolera jako **StudentsController**,

![Określ kontroler](generating-views/_static/image3.png)

Kliknij przycisk **Dodaj**.

Jeśli otrzymasz komunikat o błędzie, może to być, ponieważ nie skompilowano projekt w poprzedniej sekcji. Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.

Po zakończeniu procedury generowania kodu zobaczysz nowy kontroler i widoków w projekcie.

![Pokaż widoki](generating-views/_static/image4.png)

Ponownie wykonaj te same czynności, ale Dodaj szkielet dla klasy rejestracji. Po zakończeniu powinien mieć **EnrollmentsController.cs** plików i folderów w obszarze **widoków** o nazwie **rejestracje** o tworzenie, usuwanie, szczegółowe informacje, edycji i indeksu Widoki.

![Pokaż widoki](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Dodawanie łączy do nowych widoków

Aby ułatwić przejście do nowych widoków, można dodać kilka hiperłącza do widoków indeksu dla uczniów i rejestracji. Otwórz plik w rozmiarze **Views/Home/Index.cshtml**, który jest stroną główną witryny. Dodaj następujący kod poniżej jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

W przypadku metody ActionLink pierwszy parametr jest tekst do wyświetlenia w linku. Drugi parametr jest to akcja, a trzeci parametr jest nazwa kontrolera. Na przykład pierwszy link wskazuje akcji indeksu w StudentsController. Rzeczywiste hiperłącze jest zbudowany z tych wartości. Pierwszy link ostatecznie przejście do **Index.cshtml** plików w ramach **widoków/uczniów** folderu.

## <a name="display-student-views"></a>Wyświetlanie widoków dla uczniów

Zweryfikuje kod poprawnie dodany do projektu wyświetla listę uczniów i umożliwia użytkownikom edytowanie, tworzenie lub usuwanie rekordów dla uczniów w bazie danych.

Kliknij prawym przyciskiem myszy **Views/Home/Index.cshtml** pliku, a następnie wybierz **Pokaż w przeglądarce**. Na tej stronie kliknij łącze do listy studentów.

![](generating-views/_static/image6.png)

Na tej stronie zostanie wyświetlona lista studentów i łącza, aby zmodyfikować te dane.

![listy studentów](generating-views/_static/image7.png)

Kliknij przycisk **Utwórz nowy** łącze i podanie kilku wartości dotyczących nowego studenta.

![Tworzenie nowego studenta](generating-views/_static/image8.png)

Kliknij przycisk **Utwórz**i zwróć uwagę, dodaniu nowego studenta do listy.

![Lista z nowego studenta](generating-views/_static/image9.png)

Wybierz **Edytuj** połączyć, a następnie zmienić niektóre z wartości dla uczniów lub studentów.

![Edytowanie ucznia](generating-views/_static/image10.png)

Kliknij przycisk **Zapisz**i zwróć uwagę, rekord dla uczniów został zmieniony.

Na koniec wybierz pozycję **Usuń** link i upewnij się, że chcesz usunąć rekord, klikając **Usuń** przycisku.

![Usuń ucznia](generating-views/_static/image11.png)

Bez pisania żadnego kodu, można dodać widoków, które wykonują typowe operacje na danych w tabeli dla uczniów.

Być może zauważono, że tekst etykiety dla pola jest oparty na właściwość bazy danych (takich jak **LastName**) który nie jest zawsze mają być wyświetlane na stronie sieci web. Na przykład, lepiej jest etykieta **nazwisko**. Naprawi ten problem wyświetlaną w dalszej części tego samouczka.

## <a name="display-enrollment-views"></a>Wyświetlanie widoków rejestracji

Baza danych zawiera relację jeden do wielu między tabelami, studentów i rejestracji i relacji jeden do wielu między tabelami kursu i rejestracji. Widoki dla rejestracji prawidłowo obsługiwać te relacje. Przejdź do strony głównej witryny i wybierz **listę rejestracje** link a następnie **Utwórz nowy** łącza. W widoku wyświetlane formularz służący do tworzenia nowego rekordu rejestracji. W szczególności zwróć uwagę, że formularz zawiera dwie listy rozwijane, które są wypełnione wartościami z powiązanych tabel.

![Tworzenie rejestracji](generating-views/_static/image12.png)

Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowana zależności dla typu danych pola. Klasy korporacyjnej musi zawierać cyfry, dzięki czemu jest wyświetlany komunikat o błędzie, jeśli zostanie podjęta próba zapewniają niezgodną wartość.

![komunikat sprawdzania poprawności](generating-views/_static/image13.png)

Upewnieniu się, że widoki generowane automatycznie umożliwiają użytkownikom pracę z danymi w bazie danych. W następnym samouczku z tej serii możesz zaktualizować bazę danych i wprowadzić odpowiednie zmiany w aplikacji sieci web.

> [!div class="step-by-step"]
> [Poprzednie](creating-the-web-application.md)
> [dalej](changing-the-database.md)
