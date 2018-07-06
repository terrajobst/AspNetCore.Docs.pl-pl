---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych — 9 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5edb3cae785d578674f47f1aa3d5be7dfa6a8791
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822098"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych — 9 12
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web. Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć w usłudze Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku, wprowadzić zmianę w bazie danych i zmian kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację na środowisk testowych i produkcyjnych.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej sekcji Dodaj kolumnę daty urodzenia `Person` klasa podstawowa dla `Student` i `Instructor` jednostek. Następnie należy zaktualizować Strona która wyświetla dane przez instruktorów, tak, aby wyświetlał nowej kolumny.

W *ContosoUniversity.DAL* otwarty projekt *osoba.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następujący):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Następnie zaktualizuj Seed — metoda, dzięki czemu zapewnia wartości dla nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z poniższy blok kodu, który zawiera informacje o dacie urodzenia:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz *Instructors.aspx* i dodać nowe pole do szablonu, aby wyświetlić datę urodzenia. Dodać go między tymi zatrudnienia przypisania daty i pakietu office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Jeśli wcięcia kodu o szerokość jest niezsynchronizowana, można nacisnąć klawisze CTRL-K, a następnie CTRL-D do automatycznego ponownego sformatowania pliku.)

Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna. Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **projekt domyślny**.

W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **projekt domyślny**, a następnie wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, projekt ContosoUniversity.DAL została ona zaznaczona):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Po zakończeniu wykonywania polecenia Uruchom aplikację, a następnie wybierz stronę instruktorów. Po załadowaniu strony zobaczysz, że ma nowe pole daty urodzenia.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska testowego

W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.

W **Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz opcję **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**. (Jeśli pasek narzędzi jest wyłączona, wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)

Program Visual Studio wdroży zaktualizowaną aplikację i otwarciu przeglądarki do strony głównej. Uruchom stronę instruktorów, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie. Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizuje schemat bazy danych i uruchamia `Seed` metody. Jeśli zostanie wyświetlona strona, zobaczysz oczekiwany **Data urodzenia** kolumnie znajdują się daty w nim.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych do środowiska produkcyjnego

Teraz można wdrożyć do produkcji. Jedyna różnica polega na tym, których będziesz używać *aplikacji\_offline.htm* aby uniemożliwić użytkownikom dostęp do witryny i w ten sposób aktualizowania bazy danych, podczas gdy wdrażasz zmiany. Wdrożenia produkcyjnego należy wykonać następujące czynności:

- Przekaż *aplikacji\_offline.htm* plik do miejsca produkcji.
- W programie Visual Studio, wybierz profil produkcji w **Web publikacja w pojedynczym kliknięciem** paska narzędzi i kliknij przycisk **publikowanie w sieci Web**.
- Usuń *aplikacji\_offline.htm* plik z witryny produkcyjnej.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym powinny być implementowanie planu tworzenia kopii zapasowych. Oznacza to, że powinny być okresowo kopiowane *School-Prod.sdf* i *aspnet Prod.sdf* pliki z produkcji lokacji do lokalizacji bezpiecznego magazynu, a powinien utrzymywanie generacje kilka takich Tworzenie kopii zapasowych. Po zaktualizowaniu bazy danych powinno się wykonanie kopii zapasowej z bezpośrednio przed zmianą. Następnie jeśli popełnienia błędu, a nie odnaleźć go do czasu, po wdrożeniu do środowiska produkcyjnego, nadal będzie można odzyskać bazy danych do stanu, w jakim był, zanim zostanie ona uszkodzona.


Gdy program Visual Studio otwiera adres URL strony głównej w przeglądarce, *aplikacji\_offline.htm* zostanie wyświetlona strona. Po usunięciu *aplikacji\_offline.htm* pliku, możesz przejść do strony głównej ponownie, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Teraz udało Ci się wdrożyć aktualizację aplikacji, która obejmuje zmianę bazy danych testowych i produkcyjnych. Następnym samouczku dowiesz się, jak przeprowadzić migrację bazy danych z programu SQL Server Compact do programu SQL Server Express i SQL Server.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [dalej](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
