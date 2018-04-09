---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrowanie danych uniwersalnych dostawcy dla członkostwa i profilów użytkownika dla tożsamości ASP.NET (C#) | Dokumentacja firmy Microsoft
author: rustd
description: W tym samouczku opisano kroki, które są niezbędne do przeprowadzenia migracji użytkowników i danych roli i danych profilów użytkowników utworzone przy użyciu dostawców uniwersalnych z istniejącej aplikacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrowanie danych uniwersalnych dostawcy dla członkostwa i profilów użytkownika dla tożsamości ASP.NET (C#)
====================
przez [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Roberta Mcmurraya](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku opisano kroki, które są niezbędne do migracji użytkowników i dane roli i danych profilów użytkowników utworzone przy użyciu dostawców uniwersalnych istniejącej aplikacji do modelu tożsamości ASP.NET. Podejście powieść do migracji danych profilu użytkownika mogą być używane w aplikacji przy użyciu również członkostwa SQL.


Wraz z wydaniem programu Visual Studio 2013, ASP.NET team wprowadzono nowy system ASP.NET Identity i więcej o tej wersji [tutaj](../../index.md). Po tego artykułu, aby przeprowadzić migrację aplikacji sieci web z [członkostwa SQL do nowego systemu tożsamości](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), w tym artykule przedstawiono kroki, aby przeprowadzić migrację istniejących aplikacji, które należy wykonać modelu dostawców do zarządzania użytkownikami i rola w nowym modelu tożsamości. Fokus w tym samouczku będą się głównie na temat migracji dane profilu użytkownika, aby bezproblemowo Połącz je w nowym systemie. Migrowanie informacje użytkownika i roli są podobne do członkostwa SQL. Podejście do migracji danych profilu można w aplikacji z również członkostwa SQL.

Na przykład Rozpoczniemy z aplikacją sieci web utworzony za pomocą programu Visual Studio 2012, który korzysta z modelu dostawców. Firma Microsoft będzie, a następnie dodaj kod do zarządzania profilami, zarejestruj użytkownika, dodać dane profilu dla użytkowników, migracji schemat bazy danych, a następnie Zmień aplikację do korzystania z systemu tożsamości użytkownika i roli zarządzania. Jako test migracji użytkowników utworzone przy użyciu dostawców uniwersalnych powinny mieć możliwość logowania i nowych użytkowników powinny mieć możliwość rejestrowania.

> [!NOTE]
> Znajduje się pełny przykład w [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Podsumowanie migracji danych profilu

Przed rozpoczęciem z migracja, poinformuj nas przyjrzeć się obsługi przechowywania danych profilu w modelu dostawców. Dane profilu dla aplikacji, które użytkownicy mogą być przechowywane na wiele sposobów, najczęściej między nimi są przy użyciu dostawców wbudowanych profilu dostarczanego wraz z dostawców uniwersalnych. Obejmuje kroki

1. Dodaj klasę, która ma właściwości używany do przechowywania danych profilu.
2. Dodaj klasę, która rozszerza "ProfileBase" i implementuje metody w celu uzyskania powyższe dane profilu dla użytkownika.
3. Włącz przy użyciu domyślnego dostawcy profilu w *web.config* plików i zdefiniuj klasy zadeklarowanej w kroku 2 # ma być używana podczas uzyskiwania dostępu do informacji o profilu.

Informacje o profilu jest przechowywana jako serializacji xml i dane binarne w tabeli "Profilów" w bazie danych.

Po przeprowadzeniu migracji aplikacji do użycia w nowym systemie tożsamości platformy ASP.NET, informacje o profilu jest zdeserializować i przechowywane jako właściwości klasy użytkownika. Każda właściwość następnie mogą być mapowane na kolumny w tabeli użytkownika. W tym miejscu zaletą jest, że właściwości można pracować bezpośrednio za pomocą klasy użytkownika oprócz nie ma potrzeby serializacji/deserializacji danych informacji jednocześnie, gdy uzyskuje do niej dostęp.

## <a name="getting-started"></a>Wprowadzenie

1. Tworzenie nowej aplikacji formularzy sieci Web programu ASP.NET 4.5 w programie Visual Studio 2012. Bieżący przykładzie użyto szablonu formularzy sieci Web, ale można również użyć aplikacji MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Utwórz nowy folder modeli, można zapisać informacji o profilu  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Na przykład Daj nam przechowywania daty urodzenia, miasta, wysokości i wagi użytkownika w profilu. Wysokość i wagi są przechowywane jako niestandardowej klasy o nazwie "PersonalStats". Do przechowywania i pobierania profilu, potrzebujemy klasę, która rozszerza "ProfileBase". Umożliwia utworzenie nowej klasy "AppProfile", aby uzyskać i przechowywać informacje o profilu.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Włącz profil *web.config* pliku. Wprowadź nazwę klasy, która ma być używany do przechowywania/pobierania informacji o użytkowniku utworzonego w kroku #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Dodawanie strony formularzy sieci web w folderze "Konto", aby pobrać dane profilu użytkownika i zapisze go. Kliknij prawym przyciskiem myszy projekt i wybierz opcję "Dodaj nowy element". Dodawanie nowej strony formularzy sieci Web z strony wzorcowej "AddProfileData.aspx". Skopiuj następujące w sekcji "Znacznika":

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Dodaj następujący kod w kodzie:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Dodaj obszar nazw, w których AppProfile zdefiniowana jest klasa, aby usunąć błędy kompilacji.
6. Uruchom aplikację i utworzenie nowego użytkownika z nazwą użytkownika "**olduser".** Przejdź do strony "AddProfileData" i Dodaj informacje o profilu użytkownika.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Aby sprawdzić, czy dane są przechowywane jako serializacji xml w tabeli "Profilów" w oknie Eksploratora serwera. W programie Visual Studio z menu "Widok" Wybierz "Eksploratora serwera". Powinien być połączenia danych dla bazy danych zdefiniowanych w *web.config* pliku. Kliknięcie połączenie danych pokazuje pod różnych kategorii. Rozwiń węzeł "Tabele" Pokaż różnych tabel w bazie danych, a następnie kliknij prawym przyciskiem "Profilów" i wybierz opcję "Pokaż tabeli danych" do wyświetlania danych profilu przechowywane w tabeli profilów.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrowanie schematu bazy danych

Aby istniejąca baza danych działa w systemie tożsamości, musimy zaktualizować schemat w bazie danych tożsamości do obsługi pola, które dodano do oryginalnej bazy danych. Można to zrobić za pomocą skryptów SQL, aby utworzyć nowe tabele i skopiuj istniejące informacje. W oknie "Eksploratora serwera" rozwiń parametru "DefaultConnection" do wyświetlenia w tabelach. Kliknij prawym przyciskiem myszy kliknij tabele i wybierz pozycję "Nowe zapytanie"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Wklej skrypt SQL z [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) i uruchom go. Jeśli połączenia "DefaultConnection" zostanie odświeżona, możemy stwierdzić, że nowe tabele są dodawane. Można sprawdzić danych wewnątrz tabel, aby zobaczyć, że dane zostały poddane migracji.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrowanie aplikacji do korzystania z tożsamości platformy ASP.NET

1. Instalowanie pakietów Nuget potrzebne dla tożsamości ASP.NET:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Więcej informacji na temat zarządzania pakietami Nuget można znaleźć [tutaj](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Aby pracować z istniejących danych w tabeli, należy utworzyć klasy modelu, które mapowania tabeli, a Podłączanie w systemie tożsamości. W ramach umowy tożsamości klasy modelu albo powinien implementować interfejsów zdefiniowanych w bibliotece dll Identity.Core lub rozszerzyć istniejącą implementacją tych interfejsów, które są dostępne w Microsoft.AspNet.Identity.EntityFramework. Firma Microsoft będzie używać istniejących klas dla roli, logowania użytkowników i oświadczeń użytkownika. Należy użyć użytkownika niestandardowego dla naszej próbki. Kliknij prawym przyciskiem myszy projekt i Utwórz nowy folder "IdentityModels". Dodaj nową klasę "User", jak pokazano poniżej:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Zauważ, że ProfileInfo teraz jest właściwością klasy użytkownika. Dlatego możemy użyć klasy użytkownika bezpośrednio pracować z danymi profilu.

Skopiuj pliki w **IdentityModels** i **IdentityAccount** folderów ze źródła pobierania ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Mają one pozostałe klasy modelu i nowych stron wymaganych dla użytkowników i zarządzania rolami przy użyciu interfejsów API tożsamości ASP.NET. Podejście, używane jest podobny do członkostwa SQL i szczegółowy opis można znaleźć [tutaj](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

## <a name="copying-profile-data-to-the-new-tables"></a>Kopiowanie danych profilu do nowych tabel

Jak wspomniano wcześniej, musimy deserializacji danych xml w tabelach profile i zapisze go w kolumnach tabeli AspNetUsers. Nowe kolumny zostały utworzone w tabeli użytkowników w poprzednim kroku, tak aby pozostało do wypełnienia tych kolumn niezbędnych danych. W tym celu użyjemy aplikacji konsoli, w którym jest uruchamiane jeden raz, aby wypełnić nowo utworzony kolumn w tabeli użytkowników.

1. Utwórz nową aplikację konsoli w istniejącym rozwiązania.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Zainstaluj najnowszą wersję pakietu Entity Framework.
3. Dodaj aplikację sieci web utworzone powyżej jako odwołanie do aplikacji konsoli. Aby zrobić to kliknij prawym przyciskiem myszy projekt, a następnie "Dodaj odwołania", następnie rozwiązania, następnie kliknij na projekt i kliknij przycisk OK.
4. Kopiuj poniżej kod w klasie Program.cs. Tę logikę odczytuje dane profilu dla każdego użytkownika, serializuje go jako obiekt "ProfileInfo" i zapisuje je w bazie danych.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Niektóre modele są definiowane w folderze "IdentityModels" projektu aplikacji sieci web, więc należy dołączyć odpowiednie przestrzenie nazw.
5. Powyższy kod działa na plik bazy danych w aplikacji\_utworzyć folderu dane projektu aplikacji sieci web w poprzednich krokach. Aby odwołać, który, należy zaktualizować parametry połączenia w pliku app.config aplikacji konsoli przy użyciu parametrów połączenia w pliku web.config aplikacji sieci web. Ponadto podaj pełną ścieżkę fizyczną we właściwości 'AttachDbFilename'.
6. Otwórz wiersz polecenia i przejdź do folderu bin powyżej aplikacji konsoli. Uruchom plik wykonywalny i przejrzyj dane wyjściowe dziennika, jak pokazano na poniższej ilustracji.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Otwórz tabelę "AspNetUsers" w Eksploratorze serwera i sprawdź dane w nowych kolumn, które posiadają właściwości. One powinien być zaktualizowany o wartości odpowiednich właściwości.

## <a name="verify-functionality"></a>Sprawdzenie działania

Użyj strony nowo dodanego członkostwo, które są implementowane za pomocą ASP.NET Identity logowania użytkownika z utrzymując bazę danych. Użytkownik powinien mieć możliwość Zaloguj się za pomocą tych samych poświadczeń. Inne funkcje, takie jak dodawanie OAuth, tworzenie nowego użytkownika, zmiana haseł, Dodawanie ról, spróbuj dodać użytkowników do ról, itp.

Dane profilu dla użytkownika starych i nowych użytkowników, należy pobrać i przechowywane w tabeli użytkowników. Nie jest już starej tabeli ma być utworzone odwołanie.

## <a name="conclusion"></a>Wniosek

Tego artykułu opisano proces migrowanie aplikacji sieci web, które używane modelu dostawcy dla członkostwa w produkcie ASP.NET Identity. Artykuł opisane również migrację danych profilu dla użytkowników być punktem zaczepienia systemu tożsamości. Zostaw komentarze poniżej pytania i problemów napotkanych podczas migracji aplikacji.

*Dzięki użyciu Rick Anderson i Roberta Mcmurraya do przeglądania tego artykułu.*
