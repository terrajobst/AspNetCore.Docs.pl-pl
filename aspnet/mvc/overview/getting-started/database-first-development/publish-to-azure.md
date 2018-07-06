---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publikowanie witryny MVC Database First na platformie Azure | Dokumentacja firmy Microsoft
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 0aaa8e2a586a89f6ea5eaeb4f3d280993342b2f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835751"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publikowanie witryny MVC Database First na platformie Azure
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na publikowania aplikacji sieci web i bazy danych na platformie Azure. Możesz przeczytać ten temat, aby dowiedzieć się więcej o publikowaniu aplikacji sieci web i bazy danych, ale aby faktycznie wykonać kroki, które należy uruchomić na początku tego samouczka. Zobacz [wprowadzenie](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Wdrażanie aplikacji sieci web na platformie Azure

Potrzebujesz konta platformy Azure do ukończenia tego samouczka:

- Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

Aby opublikować aplikację sieci web, kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.

![Publikowanie witryny](publish-to-azure/_static/image1.png)

Wybierz platformy Microsoft Azure Websites.

![Wybierz platformy Azure](publish-to-azure/_static/image2.png)

Jeśli użytkownik nie jest zalogowany na platformie Azure, wprowadź swoje poświadczenia konta platformy Azure. Następnie wybierz nowy, aby utworzyć nową aplikację sieci web.

![Nowa witryna](publish-to-azure/_static/image3.png)

Utwórz unikatową nazwę aplikacji sieci web. Jeśli zostanie wyświetlony zielony znacznik wyboru z prawej strony pola Nazwa będzie wiadomo, że nazwa jest unikatowa. Wybierz region dla aplikacji sieci web. Wybierz **Utwórz nowy serwer** bazy danych i podaj nazwę użytkownika i hasło dla tego nowego serwera bazy danych. Po zakończeniu kliknij przycisk **Utwórz**.

![Tworzenie witryny](publish-to-azure/_static/image4.png)

Wartości połączenia teraz wszystko jest ustawione. Możesz pozostawić te wartości bez zmian.

![połączenia](publish-to-azure/_static/image5.png)

Kliknij przycisk **Dalej**.

W przypadku ustawień, zwróć uwagę, że dwa połączenia z bazą danych są określone - ApplicationDBContext i ContosoUniversityDataEntities. ApplicationDBContext jest połączenie w przypadku tabel konta użytkownika. Te wartości Pokaż tylko parametry połączenia dla baz danych. Nie oznacza to, że te bazy danych zostaną opublikowane podczas publikowania witryny. Po zakończeniu publikowania aplikacji sieci web, opublikuje Twój projekt bazy danych.

![](publish-to-azure/_static/image6.png)

Wielokropek (...) obok połączenia z bazą danych zawiera szczegółowe informacje o parametrach połączenia. Kliknij wielokropek obok ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Zanotuj nazwę serwera bazy danych i bazy danych. Nazwa serwera jest generowany losowo. Nazwa bazy danych jest prosta nazwa witryny przy użyciu  **\_db** dołączane. Obie nazwy będą potrzebne później podczas publikowania bazy danych.

Kliknij przycisk **OK** aby zamknąć okno ciąg połączenia bazy danych.

W oknie Publikowanie w sieci Web kliknij **dalej** się korzystania z wersji zapoznawczej.

![](publish-to-azure/_static/image8.png)

Możesz kliknąć, aby uruchomić Podgląd, aby wyświetlić listę plików do opublikowania. Ponieważ jest to po raz pierwszy zostały opublikowane tej witryny, listy jest co odpowiedni plik w projekcie.

Kliknij przycisk **publikowania**.

W okienku danych wyjściowych zostaną wyświetlone wynik publikacji.

![](publish-to-azure/_static/image9.png)

Po opublikowaniu lokacja znajduje się immedialely uruchomiony w przeglądarce sieci web. Witryna została wdrożona i można zarejestrować nowego użytkownika do danej lokacji; jednak tabel w projekcie ContosoUniversityData nie jeszcze zostały opublikowane. Po kliknięciu na liście łączy uczniów, zostanie wyświetlony błąd.

## <a name="publish-database-to-sql-azure"></a>Publikowanie bazy danych SQL Azure

Przed opublikowaniem bazy danych, upewnij się, że komputer lokalny może łączyć się z serwerem bazy danych. Ogranicza zapory dla serwera bazy danych, której maszyny mogą łączyć z bazą danych. Należy dodać adres IP komputera do dozwolonych adresów IP dla zapory.

Zaloguj się do konta platformy Azure za pośrednictwem witryny Azure portal.

Wybierz nową bazę danych, a następnie wybierz pozycję **Zarządzaj**.

![Zarządzanie](publish-to-azure/_static/image10.png)

Należy skonfigurować serwer bazy danych, aby zezwolić na połączenia z komputera. Po wybraniu zarządzanie, zostanie wyświetlony monit, aby dodać bieżący adres IP do serwera bazy danych. Wybierz pozycję Tak.

![Dodaj adres ip](publish-to-azure/_static/image11.png)

Istnieje możliwość, że adres IP, zostały dodane w poprzednim kroku nie jest tylko adres IP, który należy skonfigurować dla połączeń. Możesz spróbować zalogować się do bazy danych, aby sprawdzić, czy połączenia zostały prawidłowo skonfigurowane. Podaj użytkownika i hasła, która została utworzona wcześniej.

![Zaloguj się](publish-to-azure/_static/image12.png)

Jeśli zostanie wyświetlony komunikat o błędzie, należy dodać inny adres IP. Kliknij komunikat o błędzie, aby zobaczyć więcej szczegółów o błędzie. W obszarze szczegółów zostanie wyświetlony adres IP, który chcesz dodać. Należy pamiętać, ten adres IP.

![niedozwolone](publish-to-azure/_static/image13.png)

Zamknij to okno logowania, a następnie wróć do witryny Azure portal.

Przejdź do pulpitu nawigacyjnego dla bazy danych. Kliknij przycisk **Zarządzaj dozwolonymi adresami IP**.

![Zarządzanie adresami ip](publish-to-azure/_static/image14.png)

Teraz należy dodać adres IP, z komunikatu o błędzie. Zmień zakresu dozwolonych adresów IP obejmujący jeden z komunikatu o błędzie lub dodać adres IP jako oddzielny wpis.

![Dodawanie nowego adresu](publish-to-azure/_static/image15.png)

Zapisz zmiany dozwolonych adresów IP.

Kliknij przycisk Zarządzaj, a następnie spróbuj zalogować się ponownie z bazą danych. Może być konieczne Poczekaj kilka minut, zanim dozwolone adresy IP są poprawnie skonfigurowane dla zapory. Jeśli możesz pomyślnie zalogować się w bazie danych, ukończono połączenie z bazą danych.

To okno Zarządzanie może pozostaw otwarte, ponieważ wynikiem wdrożenia bazy danych będzie sprawdzać wkrótce.

Wróć do projektu bazy danych. Kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.

![Publikowanie bazy danych](publish-to-azure/_static/image16.png)

W oknie publikowania bazy danych wybierz **Edytuj**.

![Edytuj](publish-to-azure/_static/image17.png)

Podaj nazwę serwera bazy danych i poświadczeń uwierzytelniania dla serwera. Po podaniu poświadczeń, wybierz bazę danych, utworzone na podstawie listy dostępnych baz danych. Domyślnie program Visual Studio ustawia nazwę pola bazy danych do nazwy projektu, który nie może być taka sama jak bazy danych, który został utworzony.

![](publish-to-azure/_static/image18.png)

Kliknij przycisk OK.

Prawdopodobnie należy zapisać ten profil, aby można było opublikować aktualizacje w przyszłości, bez konieczności ponownego wprowadzania wszystkie informacje o połączeniu. Wybierz **Utwórz profil**.

![Zapisz profil](publish-to-azure/_static/image19.png)

Utworzy plik w projekcie o nazwie **ContosoUniversityData.publish.xml**. Następnym razem, które chcesz opublikować bazę danych na platformę Azure, po prostu załadować ten profil.

Teraz, kliknij pozycję **Publikuj** utworzyć bazę danych na platformie Azure.

![Publikowanie](publish-to-azure/_static/image20.png)

Po uruchomieniu od pewnego czasu, publikowanie wyniki są wyświetlane.

![wyniki](publish-to-azure/_static/image21.png)

Teraz można wrócisz do portalu zarządzania bazy danych. Odśwież widok projektu i zwróć uwagę, że zostały wdrożone 3 tabel wstępnie wypełniony danymi.

![nowe tabele](publish-to-azure/_static/image22.png)

Teraz można przystąpić do testowania aplikacji sieci web, który jest wdrożony na platformie Azure. Przejdź do aplikacji sieci web na platformie Azure (takie jak http://contosositeexample.azurewebsites.net/). Kliknij link do listy studentów i powinien zostać wyświetlony widok indeksu dla uczniów lub studentów.

![Widok](publish-to-azure/_static/image23.png)

Od czasu do czasu połączenia i bazy danych należy trochę czasu, aby zostać prawidłowo skonfigurowane. Jeśli otrzymasz komunikat o błędzie, zaczekaj kilka minut, a następnie spróbuj ponownie.

## <a name="conclusion"></a>Wniosek

Ta seria podać prosty przykład sposobu generowania kodu z istniejącej bazy danych, która umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych. ASP.NET MVC 5, platformy Entity Framework i ASP.NET tworzenie szkieletów on używany do tworzenia projektu.

Wprowadzający przykład rozwiązania deweloperskiego Code First, zobacz [wprowadzenie do ASP.NET MVC 5](../introduction/getting-started.md).

Na przykład bardziej zaawansowanych, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji platformy ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Pamiętaj, że interfejs API typu DbContext, służące do pracy z danymi w pierwszej bazy danych jest taki sam jak interfejs API, można użyć do pracy z danymi w Code First. Nawet wtedy, gdy użytkownik zamierza użyć pierwszej bazy danych, możesz dowiedzieć się, jak do obsługi bardziej złożonych scenariuszy, takich jak odczytywanie i aktualizowanie powiązanych danych, obsługa konfliktów współbieżności, i innych elementów z samouczka Code First. Jedyną różnicą jest w sposób tworzenia bazy danych, klasy kontekstu i klas jednostek.

> [!div class="step-by-step"]
> [Poprzednie](enhancing-data-validation.md)
