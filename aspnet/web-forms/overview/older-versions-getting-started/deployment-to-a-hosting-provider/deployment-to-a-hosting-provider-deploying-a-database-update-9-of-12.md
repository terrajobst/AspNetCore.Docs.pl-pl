---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji bazy danych — 9 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio lub Visual Web Developer programu SQL Server Compact: Wdrażanie aktualizacji bazy danych — 9 12
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tej serii samouczków opisano sposób wdrażania platformy ASP.NET (publikowanie) projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC i Visual Studio Express 2012 RC for Web. W razie musisz zainstalować aktualizację publikowania w sieci Web, można również używać programu Visual Studio 2010. Aby obejrzeć wprowadzenie do serii, zobacz [pierwszy samouczek z tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Samouczek zawiera funkcje wdrażania dodane po wydaniu programu Visual Studio 2012 RC, pokazuje, jak wdrożyć wersjach programu SQL Server innych niż SQL Server Compact, która przedstawia sposób wdrażania aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku wprowadzić zmianę w bazie danych i zmiany kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację do środowiska produkcyjnego i badania.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej sekcji, Dodaj kolumnę daty urodzenia `Person` klasę podstawową dla `Student` i `Instructor` jednostek. Następnie należy zaktualizować strona, wyświetlająca instruktora danych, tak aby wyświetlone nowej kolumny.

W *ContosoUniversity.DAL* otwarciu projektu *Person.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następnego):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Następnie zaktualizuj Seed — metoda, dzięki czemu zapewnia wartość dla nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następujący blok kodu, który obejmuje informacje daty urodzenia:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz *Instructors.aspx* i Dodaj nowe pole do szablonu, aby wyświetlić datę urodzenia. Dodaj go między te przypisania zatrudnienia daty i pakietu office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Jeśli wcięcia kodu jest niezsynchronizowana, możesz nacisnąć klawisze CTRL-K, a następnie klawisz CTRL-D automatycznie sformatuj ponownie plik.)

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna. Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **domyślny projekt**.

W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **domyślny projekt**, a następnie wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, nadal wybrano projektu ContosoUniversity.DAL):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Po zakończeniu działania polecenia, uruchom aplikację, a następnie wybierz stronę instruktorów. Podczas ładowania strony widać, że ma nowy Data urodzenia.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska testowego

W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.

W **jeden kliknij publikowania w sieci Web** narzędzi, wybierz opcję **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**. (Wyłączenie pasku narzędzi wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)

Program Visual Studio wdroży zaktualizowaną aplikację, a następnie otwiera przeglądarkę na stronę główną. Uruchom stronę instruktorów, aby zweryfikować, że aktualizacja została pomyślnie wdrożona. Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizacje schematu bazy danych i uruchamia `Seed` metody. Gdy zostanie wyświetlona strona, można znaleźć oczekiwanej **Data urodzenia** kolumny zawierającej daty w nim.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska produkcyjnego

Teraz można wdrożyć w środowisku produkcyjnym. Jedyną różnicą jest to, że będziesz używać *aplikacji\_offline.htm* aby uniemożliwić użytkownikom dostęp do witryny i w związku z tym uaktualnienie bazy danych, podczas gdy wdrażasz zmiany. W przypadku wdrożenia produkcyjnego, wykonaj następujące czynności:

- Przekaż *aplikacji\_offline.htm* pliku do miejsca produkcji.
- W programie Visual Studio, wybierz profil produkcji w **jeden kliknij publikowania w sieci Web** narzędzi i kliknij przycisk **publikowanie w sieci Web**.
- Usuń *aplikacji\_offline.htm* plik z miejsca produkcji.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym powinny być wdrażanie planu tworzenia kopii zapasowych. Oznacza to, powinny być okresowo kopiowane *służbowe-Prod.sdf* i *aspnet Prod.sdf* plików z produkcji lokacji do lokalizacji bezpiecznego magazynu i należy zachować kilka generacji takich Tworzenie kopii zapasowych. Podczas aktualizowania bazy danych należy kopii zapasowej z bezpośrednio przed zmianą. Następnie jeśli popełnienia błędu, a nie odnalezienia dopiero po wdrożeniu w środowisku produkcyjnym, nadal będzie można odzyskać bazy danych do stanu sprzed przed jego uległa uszkodzeniu.


Gdy program Visual Studio otworzy adres URL strony głównej w przeglądarce, *aplikacji\_offline.htm* zostanie wyświetlona strona. Po usunięciu *aplikacji\_offline.htm* pliku, możesz przejść do strony głównej ponownie, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Teraz wdrożeniu aktualizacji aplikacji, która obejmuje zmianę bazy danych do testu i produkcji. Następny samouczek pokazuje, jak przeprowadzić migrację bazy danych z programu SQL Server Compact do programu SQL Server Express i programu SQL Server.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [dalej](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
