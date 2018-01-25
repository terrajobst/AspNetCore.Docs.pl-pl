---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publikowanie witryny MVC Database First Azure | Dokumentacja firmy Microsoft
author: tfitzmac
description: "Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publikowanie MVC bazy danych pierwszej lokacji na platformie Azure
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych. Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.
> 
> Ta część serii koncentruje się na publikowania aplikacji sieci web i bazy danych do platformy Azure. W tym temacie, aby dowiedzieć się więcej na temat publikowania aplikacji sieci web i bazy danych, ale do rzeczywistego wykonania czynności, które należy uruchomić na początku tego samouczka można znaleźć. Zobacz [wprowadzenie](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Wdrażanie aplikacji sieci web na platformie Azure

Potrzebne jest konto platformy Azure do ukończenia tego samouczka:

- Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- Możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

Aby opublikować aplikację sieci web, kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.

![Publikowanie witryny](publish-to-azure/_static/image1.png)

Wybierz witryny sieci Web platformy Microsoft Azure.

![Wybierz pozycję Azure](publish-to-azure/_static/image2.png)

Jeśli użytkownik nie jest zalogowany na platformie Azure, podaj poświadczenia konta Azure. Następnie wybierz nowy, aby utworzyć nową aplikację sieci web.

![nową lokację](publish-to-azure/_static/image3.png)

Utworzyć unikatową nazwę aplikacji sieci web. Jeśli zostanie wyświetlony zielony znacznik wyboru z prawej strony pola Nazwa będzie wiadomo się, że nazwa jest unikatowa. Wybierz region aplikacji sieci web. Wybierz **Utwórz nowy serwer** dla bazy danych i podaj nazwę użytkownika i hasło dla nowego serwera bazy danych. Gdy skończysz, kliknij przycisk **Utwórz**.

![Tworzenie witryny](publish-to-azure/_static/image4.png)

Wartości połączenia są teraz gotowe. Możesz pozostawić te wartości bez zmian.

![Połączenia](publish-to-azure/_static/image5.png)

Kliknij przycisk **Dalej**.

W przypadku ustawień, powiadomienia, że dwa połączenia z bazą danych są określone — ApplicationDBContext i ContosoUniversityDataEntities. ApplicationDBContext jest połączenie dla tabel konta użytkownika. Te wartości Pokaż tylko ciągi połączenia dla bazy danych. Nie oznacza to, że te bazy danych zostanie opublikowana, podczas publikowania witryny. Po zakończeniu publikowania aplikacji sieci web, będą publikować projektu bazy danych.

![](publish-to-azure/_static/image6.png)

Wielokropek (...) obok połączenie z bazą danych zawiera szczegółowe informacje o parametrach połączenia. Kliknij przycisk wielokropka obok ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Zanotuj nazwę serwera bazy danych i bazy danych. Nazwa serwera jest generowany losowo. Nazwa bazy danych jest prosta nazwa witryny za pomocą  **\_db** dołączane. Obie nazwy będą potrzebne później podczas publikowania bazy danych.

Kliknij przycisk **OK** aby zamknąć okno ciąg połączenia bazy danych.

W oknie Publikowanie w sieci Web kliknij **dalej** Aby wyświetlić podgląd.

![](publish-to-azure/_static/image8.png)

Można kliknąć, aby uruchomić Podgląd, aby wyświetlić listę plików do opublikowania. Ponieważ jest to po raz pierwszy po opublikowaniu tej witryny, listy jest każdego odpowiedniego pliku w projekcie.

Kliknij przycisk **publikowania**.

W okienku dane wyjściowe zostaną wyświetlone wynik publikacji.

![](publish-to-azure/_static/image9.png)

Po opublikowaniu witryna jest immedialely uruchamiane w przeglądarce sieci web. Witryna została wdrożona i można zarejestrować nowego użytkownika do witryny; Jednak tabele w projekcie ContosoUniversityData nie jeszcze zostały opublikowane. Po kliknięciu na liście studentów łącza zostanie zwrócony błąd.

## <a name="publish-database-to-sql-azure"></a>Publikowanie bazy danych SQL Azure

Przed publikowania bazy danych, należy się upewnić, czy komputer lokalny może połączyć się z serwerem bazy danych. Ogranicza zapory dla serwera bazy danych, które maszyny można podłączyć do bazy danych. Należy dodać adres IP komputera do dozwolonych adresów IP dla zapory.

Zaloguj się do konta platformy Azure za pośrednictwem portalu Azure.

Wybierz nową bazę danych i wybierz **Zarządzaj**.

![Zarządzanie](publish-to-azure/_static/image10.png)

Należy skonfigurować serwer bazy danych umożliwia nawiązywanie połączeń z komputera. Po wybraniu Zarządzaj należy dodać bieżącego adresu IP, jako dozwolone na serwerze bazy danych. Wybierz opcję Tak.

![Dodaj adres ip](publish-to-azure/_static/image11.png)

Istnieje ryzyko, że adres IP dodanym w poprzednim kroku nie jest tylko adres IP, który należy skonfigurować dla połączenia. Możesz spróbować zalogować się do bazy danych, aby sprawdzić, czy połączenia są poprawnie skonfigurowane. Podaj użytkownika i hasła utworzonego wcześniej.

![Logowanie](publish-to-azure/_static/image12.png)

Jeśli zostanie wyświetlony komunikat o błędzie, należy dodać inny adres IP. Kliknij komunikat o błędzie, aby zobaczyć bardziej szczegółowe informacje o błędzie. W szczegółach zobaczysz adres IP, który należy dodać. Należy pamiętać, ten adres IP.

![niedozwolone](publish-to-azure/_static/image13.png)

Zamknij to okno logowania, a następnie wróć do portalu Azure.

Przejdź do pulpitu nawigacyjnego dla Twojej bazy danych. Kliknij przycisk **Zarządzaj dozwolonymi adresami IP**.

![Zarządzanie adresami ip](publish-to-azure/_static/image14.png)

Teraz należy dodać adres IP z komunikat o błędzie. Zmiana zakresu dozwolonych adresów IP, aby uwzględnić z komunikat o błędzie albo dodać ten adres IP jako oddzielny wpis.

![Dodaj nowy adres](publish-to-azure/_static/image15.png)

Zapisz zmiany w dozwolonych adresów IP.

Kliknij przycisk Zarządzaj, a następnie spróbuj ponownie zalogować się do bazy danych. Konieczne może być Poczekaj kilka minut, zanim dozwolone adresy IP są poprawnie skonfigurowane dla zapory. Jeśli możesz pomyślnie zalogować się w bazie danych, możesz Ukończono konfigurowanie połączenia z bazą danych.

To okno zarządzania można pozostaw otwarte, ponieważ w wyniku wdrożenia bazy danych będzie sprawdzać wkrótce.

Wróć do projektu bazy danych. Kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.

![Publikowanie bazy danych](publish-to-azure/_static/image16.png)

W oknie publikowania bazy danych, wybierz **Edytuj**.

![Edytuj](publish-to-azure/_static/image17.png)

Podaj nazwę serwera bazy danych i poświadczeń uwierzytelniania dla serwera. Po podaniu poświadczenia, wybierz bazę danych utworzoną z listy dostępnych baz danych. Domyślnie Visual Studio ustawia nazwę pola bazy danych do nazwy projektu, który nie może być taka sama jak bazy danych, który został utworzony.

![](publish-to-azure/_static/image18.png)

Kliknij przycisk OK.

Prawdopodobnie można zapisać tego profilu, aby można opublikować aktualizacje w przyszłości, bez konieczności ponownego wprowadzania wszystkich informacji o połączeniu. Wybierz **Utwórz profil**.

![Zapisywanie profilu](publish-to-azure/_static/image19.png)

W projekcie o nazwie zostanie utworzony plik **ContosoUniversityData.publish.xml**. Przy następnym, którą chcesz opublikować bazy danych na platformie Azure, po prostu załadować tego profilu.

Teraz, kliknij przycisk **publikowania** utworzyć bazę danych na platformie Azure.

![Publikowanie](publish-to-azure/_static/image20.png)

Po uruchomieniu przez pewien czas, publikowania wyniki są wyświetlane.

![wyniki](publish-to-azure/_static/image21.png)

Teraz można przejść wstecz do portalu zarządzania dla Twojej bazy danych. Odśwież widok projektu i należy zauważyć, że zostały wdrożone 3 tabel z danymi wstępnie wypełnione.

![nowe tabele](publish-to-azure/_static/image22.png)

Teraz można przystąpić do testowania aplikacji sieci web, który jest wdrożony na platformie Azure. Przejdź do aplikacji sieci web na platformie Azure (na przykład http://contosositeexample.azurewebsites.net/). Kliknij łącze do listy studentów i powinien zostać wyświetlony widok indeksu dla uczniów lub studentów.

![Widok](publish-to-azure/_static/image23.png)

Czasami połączenia i bazy danych należy trochę czasu na zostać prawidłowo skonfigurowane. Jeśli wystąpi błąd, zaczekaj kilka minut, a następnie spróbuj ponownie.

## <a name="conclusion"></a>Wniosek

Ta seria podać prosty przykład sposobu generowanie kodu na podstawie istniejącej bazy danych, która umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych. ASP.NET MVC 5, Entity Framework i ASP.NET szkieletów on użyty do utworzenia projektu.

Przykład wprowadzające programowanie Code First, zobacz [wprowadzenie do platformy ASP.NET MVC 5](../introduction/getting-started.md).

Na przykład bardziej zaawansowanych, zobacz [tworzenia modelu danych struktury jednostek dla aplikacji ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Należy pamiętać, że API DbContext, służące do pracy z danymi w pierwszym bazy danych jest taki sam jak interfejs API, można użyć do pracy z danymi w Code First. Nawet jeśli zamierzasz użyć pierwszej bazy danych, można poznać sposoby obsługi złożonych scenariuszy, takich jak odczytywanie i aktualizowanie danych powiązanych konfliktów współbieżności, i z kodu pierwszy samouczek. Jedyną różnicą jest w sposób tworzenia bazy danych, klasa kontekstu i klas jednostek.

>[!div class="step-by-step"]
[Poprzednie](enhancing-data-validation.md)
