---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: generowanie widoków | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879754"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Baza danych EF najpierw o platformie ASP.NET MVC: generowanie widoków
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na przy użyciu funkcji szkieletów ASP.NET można wygenerować widoków i kontrolerów.


## <a name="add-scaffold"></a>Dodawanie szkieletu

Wszystko jest gotowe do generowania kodu, który zapewni danych standardowych operacji dla klasy modelu. Kod należy dodać przez dodawanie elementu szkieletu. Istnieje wiele opcji dla typu funkcja szkieletów, które można dodać; w tym samouczku szkieletu będzie zawierać kontrolera i widoki, które odpowiadają modeli dla użytkowników domowych i rejestracji, utworzony w poprzedniej sekcji.

Aby zachować spójność w projekcie, zostanie dodany nowy kontroler do istniejącego **kontrolerów** folderu. Kliknij prawym przyciskiem myszy **kontrolerów** folder, a następnie wybierz **Dodaj** — **nowy element szkieletu**.

![Dodawanie szkieletu](generating-views/_static/image1.png)

Wybierz **kontroler MVC 5 z widokami używający narzędzia Entity Framework** opcji. Ta opcja spowoduje wygenerowanie kontrolera oraz widoki dla aktualizowanie, usuwanie, tworzenie i wyświetlanie danych w modelu.

![Dodaj kontroler mvc](generating-views/_static/image2.png)

Wybierz **uczniowie** dla klasy modelu, a następnie wybierz **ContosoUniversityEntities** dla klasy kontekstu. Zachowaj nazwę kontrolera jako **StudentsController**,

![Określ kontroler](generating-views/_static/image3.png)

Kliknij przycisk **Dodaj**.

Jeśli wystąpi błąd, prawdopodobnie nie zbudować projektu w poprzedniej sekcji. Jeśli tak, spróbuj go skompilować, a następnie ponownie Dodaj element szkieletu.

Po zakończeniu procedury generowania kodu, zobaczysz nowego kontrolera i widoków w projekcie.

![Pokaż widoki](generating-views/_static/image4.png)

Ponownie wykonaj te same kroki, ale dodania szkieletu dla klasy rejestracji. Po zakończeniu powinien mieć **EnrollmentsController.cs** plików i folderów w obszarze **widoków** o nazwie **rejestracji** z Create, Delete, szczegóły, edycji i indeks Widoki.

![Pokaż widoki](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Dodawanie łączy do nowych widoków

Aby ułatwić przejdź do nowych widoków, można dodać kilka hiperłącza do widoków indeksu dla uczniów lub studentów i rejestracji. Otwórz plik **Views/Home/Index.cshtml**, która jest strona główna witryny. Dodaj następujący kod poniżej jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

W przypadku metody ActionLink pierwszym parametrem jest tekst do wyświetlenia w łączu. Drugi parametr jest akcję, a trzeci parametr jest nazwa kontrolera. Na przykład pierwszy link wskazuje akcji indeksu w StudentsController. Rzeczywiste hiperłącze jest tworzony z tych wartości. Pierwszy link ostatecznie powoduje przejście do **Index.cshtml** pliku w ramach **widoków/uczniów lub studentów** folderu.

## <a name="display-student-views"></a>Wyświetlanie widoków dla użytkowników domowych

Zostanie Sprawdź, czy kod poprawnie dodane do projektu zostanie wyświetlona lista studentów i umożliwia użytkownikom edytowanie, tworzenie lub usuwanie rekordów uczniów w bazie danych.

Kliknij prawym przyciskiem myszy **Views/Home/Index.cshtml** pliku, a następnie wybierz **Wyświetl w przeglądarce**. Na tej stronie kliknij łącze listy studenta.

![](generating-views/_static/image6.png)

Na tej stronie zostanie wyświetlona lista studentów i łącza, aby zmodyfikować te dane.

![Lista dla uczniów lub studentów](generating-views/_static/image7.png)

Kliknij przycisk **Utwórz nowy** i podanie kilku wartości dotyczących nowej uczniów łącza.

![Utwórz nowy uczniów](generating-views/_static/image8.png)

Kliknij przycisk **Utwórz**i zwróć uwagę, nowe uczniów zostanie dodany do listy.

![Lista z nowego uczniów](generating-views/_static/image9.png)

Wybierz **Edytuj** łącza, a następnie zmień niektóre wartości w studenta.

![Edytuj studentów](generating-views/_static/image10.png)

Kliknij przycisk **zapisać**i zwróć uwagę, uczniów rekord został zmieniony.

Na koniec wybierz **usunąć** i upewnij się, że chcesz usunąć rekord, klikając łącza **usunąć** przycisku.

![Usuń studentów](generating-views/_static/image11.png)

Bez pisania żadnego kodu, dodano widoków służących do wykonywania typowych operacji na danych w tabeli studenta.

Można zauważyć, że tekst etykiety dla pola jest oparty na właściwość bazy danych (takich jak **nazwisko**) która nie jest zawsze ma mają być wyświetlane na stronie sieci web. Na przykład, lepiej jest etykieta **nazwisko**. Później w samouczku naprawi ten problem wyświetlania.

## <a name="display-enrollment-views"></a>Wyświetlanie widoków rejestracji

Baza danych zawiera relacji jeden do wielu między tabelami studentów i rejestracji i relacji jeden do wielu między tabelami przebiegu i rejestrowania. Widoki dla rejestracji poprawnie obsługiwać te relacje. Przejdź do strony głównej witryny i wybierz **listy rejestracji** łącza, a następnie **Utwórz nowy** łącza. W widoku wyświetlane formularz służący do tworzenia nowego rekordu rejestracji. W szczególności należy zauważyć, że formularz zawiera dwie listy rozwijanej, które są wypełniane przy użyciu wartości z powiązanych tabel.

![Tworzenie rejestracji](generating-views/_static/image12.png)

Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowane na podstawie na typ danych pola. Klasy wymaga numeru, więc Jeśli spróbujesz Podaj niezgodną wartość jest wyświetlany komunikat o błędzie.

![Sprawdzanie poprawności komunikatu](generating-views/_static/image13.png)

Upewnieniu się, że widoki generowane automatycznie umożliwiają użytkownikom pracę z danymi w bazie danych. W następnym samouczku z tej serii możesz zaktualizować bazę danych i wprowadzić odpowiednie zmiany w aplikacji sieci web.

> [!div class="step-by-step"]
> [Poprzednie](creating-the-web-application.md)
> [dalej](changing-the-database.md)
