---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: "Migrowanie istniejącej witryny sieci Web z członkostwa SQL do tożsamości ASP.NET | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "W tym samouczku przedstawiono kroki, aby dokonać migracji istniejącej aplikacji sieci web z użytkownika i roli dane utworzone za pomocą członkostwa SQL do nowej tożsamości programu ASP.NET s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 3638c6779a0fcedaaa49623126b28ecf09a4954f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrowanie istniejącej witryny sieci Web z członkostwa SQL do tożsamości platformy ASP.NET
====================
przez [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> W tym samouczku przedstawiono kroki migracji istniejącej aplikacji sieci web z użytkownika i roli dane utworzone za pomocą członkostwa SQL do nowego systemu tożsamości ASP.NET. Ta metoda obejmuje Zmiana istniejącego schematu bazy danych na jeden zapotrzebowaniem ASP.NET Identity i haku starych i nowych klas do niego. Po przyjmuje takie podejście, po migracji bazy danych bez wysiłku obsługi przyszłych aktualizacji do tożsamości.


W tym samouczku teraz nastąpi przekierowanie do szablonu aplikacji sieci web (formularze sieci Web) utworzone za pomocą programu Visual Studio 2010, aby utworzyć dane użytkownika i roli. Następnie użyjemy skrypty SQL do migracji istniejącej bazy danych do tabel wymaganych przez system tożsamości. Firma Microsoft będzie następnie instalowanie wymaganych pakietów NuGet i dodawanie nowych stron zarządzania konta, które korzystanie z systemu tożsamości zarządzanie członkostwem. Jako test migracji użytkowników utworzone za pomocą członkostwa SQL powinny mieć możliwość logowania i nowych użytkowników powinny mieć możliwość rejestrowania. Pełny przykład można znaleźć [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Zobacz też [migrowania członkostwa ASP.NET do tożsamości ASP.NET](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Wprowadzenie

### <a name="creating-an-application-with-sql-membership"></a>Tworzenie aplikacji z członkostwa SQL

1. Należy uruchomić z istniejącej aplikacji, która korzysta z członkostwa SQL i zawiera dane użytkownika i roli. Na potrzeby tego artykułu Utwórzmy aplikację sieci web w programie Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Za pomocą narzędzia Konfiguracja programu ASP.NET, tworzenie użytkowników 2: **oldAdminUser** i **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Utwórz rolę o nazwie administratora i Dodaj "oldAdminUser" jako użytkownik w tej roli.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Utwórz sekcję administratora witryny o Default.aspx. Wartość tagu autoryzacji w pliku web.config, aby umożliwić dostęp tylko do użytkowników pełniących role administratora. Więcej informacji można znaleźć tutaj [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Wyświetl bazy danych w Eksploratorze serwera, aby zrozumieć tabele utworzone przez system członkostwa SQL. Dane logowania użytkownika są przechowywane w aspnet\_użytkowników i aspnet\_tabele członkostwa, podczas gdy rola dane są przechowywane w aspnet\_tabeli ról. Informacje o tym, które użytkownicy są w rolach, które są przechowywane w aspnet\_UsersInRoles tabeli. Do zarządzania podstawowe członkostwo jest wystarczająca do portu dane w tabelach powyżej systemu ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migracja do programu Visual Studio 2013

1. Instalowanie programu Visual Studio Express 2013 dla sieci Web lub Visual Studio 2013 wraz z [najnowsze aktualizacje](https://www.microsoft.com/download/details.aspx?id=44921).
2. Otwórz projekt powyżej w zainstalowanej wersji programu Visual Studio. Jeśli na komputerze nie zainstalowano programu SQL Server Express, monit jest wyświetlany po otwarciu projektu, ponieważ ciąg połączenia używany program SQL Express. Można albo zainstalować program SQL Express lub jako obejścia Zmień parametry połączenia do LocalDb. W tym artykule zmienimy jej do LocalDb.
3. Otwórz plik web.config, a następnie zmień parametry połączenia z. SQLExpess do v11.0 (LocalDb). Usuń "wystąpienia użytkownika = true" z ciągu połączenia.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Otwórz Eksploratora serwera i sprawdź, czy można zaobserwować schemat tabeli i danych.
5. System tożsamości ASP.NET działa w wersji 4.5 lub nowszej platformy. Przekieruj aplikację do 4.5 lub nowszego.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.

### <a name="installing-the-nuget-packages"></a>Instalowanie pakietów Nuget

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt &gt; **Zarządzaj pakietami NuGet**. W polu wyszukiwania wprowadź "Asp.net Identity". Wybierz pakiet, na liście wyników, a następnie kliknij przycisk Instaluj. Zaakceptuj Umowę licencyjną, klikając przycisk "Zgadzam się". Należy pamiętać, że ten pakiet zostanie zainstalowany pakietów zależności: EntityFramework i Microsoft ASP.NET Identity Core. Podobnie można zainstalować następujących pakietów (Pomiń 4 ostatnie pakiety OWIN, jeśli nie chcesz włączyć logowania OAuth):

    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrację bazy danych do nowego systemu tożsamości

Następnym krokiem jest do migracji istniejącej bazy danych do schematu systemu tożsamości ASP.NET. Umożliwia to przeprowadzana SQL skryptu, która zawiera zestaw poleceń, aby utworzyć nowe tabele i migrować istniejące informacje o użytkowniku do nowych tabel. Można znaleźć pliku skryptu [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Ten przykład dotyczy tego pliku skryptu. Jeśli schemat dla tabel utworzony za pomocą członkostwa programu SQL jest dostosowane lub zmodyfikowane potrzebę skryptów można odpowiednio zmienić.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Sposób generowania skryptu SQL do migracji schematu

Dla klas ASP.NET Identity pracy fabrycznej przy użyciu danych istniejących użytkowników musimy migracji schemat bazy danych na jeden zapotrzebowaniem tożsamości ASP.NET. Firma Microsoft to zrobić, dodając nowe tabele i kopiowanie istniejące informacje o tych tabel. Domyślnie tożsamości ASP.NET używa EntityFramework do mapowania tożsamości klasy modelu bazy danych do przechowywania/pobierania informacji. Te klasy modelu implementować interfejsów tożsamości core, definiowania użytkownika i roli obiektów. Tabele i kolumny w bazie danych są oparte na tych klas modelu. Klasy modeli EntityFramework v2.1.0 tożsamości i ich właściwości są określone poniżej

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | string | Id | RoleId | Kluczem ProviderKey | Id |
| Nazwa użytkownika | string | Nazwa | Nazwa użytkownika | Nazwa użytkownika | Typ oświadczenia |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Użytkownik\_Id |
| Adres e-mail | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Numer telefonu | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DataGodzina |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Musimy mieć tabel dla każdego z tych modeli z kolumnami odpowiadającej właściwości. Mapowanie między klasami i tabel jest zdefiniowany w `OnModelCreating` metody `IdentityDBContext`. Jest to określane jako metodę interfejsu API fluent konfiguracji i więcej informacji można znaleźć [tutaj](https://msdn.microsoft.com/data/jj591617.aspx). Konfiguracja dla klas jest wymienione poniżej

| **Klasy** | **Tabela** | **Klucz podstawowy** | **Klucz obcy** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | Identyfikator UserId + RoleId | Użytkownik\_identyfikator -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Providerkey lub + UserId + LoginProvider | Nazwa użytkownika -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Użytkownik\_identyfikator -&gt;AspnetUsers |

Dzięki tym informacjom można utworzyć instrukcji SQL, aby utworzyć nowe tabele. Możemy zapisać każda instrukcja indywidualnie lub wygenerować cały skrypt za pomocą poleceń programu PowerShell platformy EntityFramework, które firma Microsoft może następnie edytować zgodnie z wymaganiami. Aby to zrobić, w PORÓWNANIU z Otwórz **Konsola Menedżera pakietów** z **widoku** lub **narzędzia** menu

- Uruchom polecenie "Enable-Migrations", aby włączyć migracje platformy EntityFramework.
- Uruchom polecenie "Add-migration początkowego", który tworzy kod początkowej konfiguracji, aby utworzyć bazę danych w języku C# / VB.
- Ostatnim krokiem jest uruchomienie "Update-Database — skryptu" polecenie, które generuje skrypt SQL w oparciu klasy modelu.

Ten skrypt generowania bazy danych może służyć jako start, w której będzie można czynione dodatkowe zmiany, aby dodać nowe kolumny, a następnie skopiować dane. Dzięki temu jest firma Microsoft Generowanie `_MigrationHistory` tabeli, które jest używane przez EntityFramework do modyfikacji schematu bazy danych, gdy zmiany w przyszłych wersjach programu wersjach tożsamości klasy modelu. 

Informacje o użytkowniku członkostwa SQL ma inne właściwości oprócz tych tożsamości użytkownika modelu klasy mianowicie poczty e-mail, prób wprowadzenia hasła, datę ostatniego logowania, Data ostatniej blokady itp. Jest to przydatne informacje i chcemy, aby zostać przeniesione do systemu tożsamości. Można to zrobić przez dodanie dodatkowych właściwości do modelu użytkownika i mapowanie ich kolumn tabeli w bazie danych. Firma Microsoft w tym dodawanie klasy tego podklasy `IdentityUser` modelu. Firma Microsoft dodać właściwości do tej klasy niestandardowe i przeprowadź edycję pliku skryptu SQL do dodania na odpowiednie kolumny podczas tworzenia tabeli. Kod dla tej klasy jest dalsze opisane w artykule. Skrypt SQL do tworzenia `AspnetUsers` tabeli po będzie dodanie nowych właściwości

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Obok należy skopiować istniejące informacje z członkostwa w bazie danych SQL do nowo dodanego tabel dla tożsamości. Można to zrobić za pomocą programu SQL przez skopiowanie danych bezpośrednio z jednej tabeli. Dodawanie danych do wierszy w tabeli, używamy `INSERT INTO [Table]` utworzenia. Aby skopiować z innej tabeli, możemy użyć `INSERT INTO` instrukcji wraz z programem `SELECT` instrukcji. Aby uzyskać wszystkie informacje o użytkowniku musimy zbadać *aspnet\_użytkowników* i *aspnet\_członkostwa* tabele i skopiuj dane na *AspNetUsers*tabeli. Używamy `INSERT INTO` i `SELECT` wraz z `JOIN` i `LEFT OUTER JOIN` instrukcje. Aby uzyskać więcej informacji na temat kwerend i kopiowanie danych między tabelami, zapoznaj się [to](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) łącza. Ponadto w tabelach AspnetUserLogins i AspnetUserClaims są puste rozpoczynać się od znaku, ponieważ nie ma żadnych informacji w członkostwie SQL, który jest mapowany na to domyślnie. Tylko informacje, kopiowane są przeznaczone dla użytkowników i ról. Projekt utworzony w poprzednich krokach będzie zapytanie SQL, aby skopiować informacje do tabeli użytkowników

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

W powyższych instrukcji SQL, informacje o poszczególnych użytkowników z *aspnet\_użytkowników* i *aspnet\_członkostwa* tabel jest kopiowana do kolumny  *AspnetUsers* tabeli. Modyfikacja tylko wykonać w tym miejscu jest firma Microsoft kopiowania hasło. Ponieważ algorytmu szyfrowania haseł w członkostwie SQL użyte "PasswordSalt" i "PasswordFormat", firma Microsoft skopiować który zbyt wraz ze skrótem hasła, aby może służyć do odszyfrowywania hasła przez tożsamości. Jest to wyjaśnić bardziej szczegółowo w artykule, gdy Podłączanie hasher hasła niestandardowego. 

Ten przykład dotyczy tego pliku skryptu. Dla aplikacji, które znajdują się dodatkowe tabele deweloperzy mogą wykonaj podejście podobne do dodawania dodatkowych właściwości dla klasy modelu użytkownika i mapować je do kolumn w tabeli AspnetUsers. Aby uruchomić skrypt,

1. Otwórz Eksploratora serwera. Rozwiń węzeł połączenia "ApplicationServices", aby wyświetlić tabele. Kliknij prawym przyciskiem myszy węzeł tabele i wybierz opcję "Nowe zapytanie"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. W oknie zapytania skopiuj i Wklej cały skrypt SQL z pliku Migrations.sql. Uruchom plik skryptu przez naciśnięcie przycisku STRZAŁKA "Execute".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Odśwież okno Eksploratora serwera. Pięć nowe tabele są tworzone w bazie danych.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Poniżej przedstawiono sposób informacje w tabeli członkostwa SQL są mapowane do nowego systemu tożsamości.

    ASPNET\_ról--&gt; AspNetRoles

    ASP\_netUsers i asp\_netMembership —&gt; AspNetUsers

    ASPNET\_UserInRoles —&gt; AspNetUserRoles

    Jak opisano w powyższej sekcji, w tabelach AspNetUserClaims i AspNetUserLogins są puste. Pole "Dyskryminatora" w tabeli AspNetUser powinna odpowiadać nazwy klasy modelu, która jest zdefiniowana jako następnego kroku. Również kolumna PasswordHash jest w formie "zaszyfrowane hasło | zaburzającą hasła | format hasła". Pozwala na użycie specjalnych logiki kryptograficzny członkostwa SQL, dzięki czemu można ponownie użyć starego hasła. Który znajduje się w dalszej części tego artykułu.

### <a name="creating-models-and-membership-pages"></a>Tworzenie modeli i stron członkostwa

Jak wspomniano wcześniej, funkcja tożsamości używa programu Entity Framework do komunikowania się z bazą danych do przechowywania informacji o koncie domyślnie. Aby pracować z istniejących danych w tabeli, należy utworzyć klasy modelu, które mapowania tabeli, a Podłączanie w systemie tożsamości. W ramach umowy tożsamości klasy modelu albo powinien implementować interfejsów zdefiniowanych w bibliotece dll Identity.Core lub rozszerzyć istniejącą implementacją tych interfejsów, które są dostępne w Microsoft.AspNet.Identity.EntityFramework.

W naszym przykładzie tabele AspNetRoles, AspNetUserClaims, AspNetLogins i AspNetUserRole mają kolumn, które są podobne do istniejącego wdrożenia systemu tożsamości. Dlatego można ponownie użyć istniejących klas do mapowania na tych tabel. Tabela AspNetUser zawiera niektóre dodatkowe kolumny, które są używane do przechowywania dodatkowych informacji z tabelami SQL członkostwa. To mogą być mapowane, tworząc klasę modelu rozszerzyć istniejącą implementację "IdentityUser" i Dodaj dodatkowe właściwości.

1. Modele tworzy folder w projekcie i dodać klasę użytkownika. Nazwa klasy powinna odpowiadać danych do kolumny "Dyskryminatora" w tabeli "AspnetUsers".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Klasa użytkownika powinny rozszerzać klasy IdentityUser w *Microsoft.AspNet.Identity.EntityFramework* biblioteki dll. Deklarowanie właściwości w klasie, które mapują AspNetUser kolumn. Właściwości Identyfikatora, nazwy użytkownika i PasswordHash SecurityStamp są zdefiniowane w IdentityUser, a więc zostały pominięte. Oto kod klasy użytkownika, który ma wszystkie właściwości

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Klasy Entity Framework DbContext jest wymagany do utrwalenia danych w tabelach modeli i pobierania danych z tabel, aby wypełnić modeli. *Microsoft.AspNet.Identity.EntityFramework* dll definiuje klasy kontekstu IdentityDbContext, która współdziała z tabele tożsamości, aby pobrać i przechowywać informacje. Kontekst IdentityDbContext&lt;tuser&gt; przyjmuje klasy "TUser", który może być dowolną klasę, która rozszerza klasę IdentityUser.

    Utwórz nową klasę ApplicationDBContext, rozszerzający IdentityDbContext w folderze "Modele" przekazywanie w klasie "User" utworzonego w kroku 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Zarządzanie użytkownikami w nowym systemie tożsamości jest wykonywane przy użyciu interfejs UserManager&lt;tuser&gt; klas zdefiniowanych w *Microsoft.AspNet.Identity.EntityFramework* biblioteki dll. Należy utworzyć niestandardowe klasy, która rozszerza interfejs UserManager, przekazując klasy "User" utworzonego w kroku 1.

    W folderze modeli Utwórz nową klasę interfejs UserManager, który rozszerza interfejs UserManager&lt;użytkownika&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Hasła użytkowników aplikacji są szyfrowane i przechowywane w bazie danych. Algorytm kryptograficzny używany w członkostwie SQL jest inna niż wersja w nowym systemie tożsamości. Aby ponownie użyć starego hasła, musimy selektywnie odszyfrować hasła, gdy stary użytkownicy logują się przy użyciu algorytmu członkostwa SQL, używając algorytmu kryptograficznego w tożsamości dla nowych użytkowników.

    Klasa interfejs UserManager ma właściwość PasswordHasher, który zapisuje wystąpienia klasy, który implementuje interfejs "Funkcji IPasswordHasher". Służy do szyfrowania/odszyfrowywania hasła podczas transakcji uwierzytelniania użytkownika. W tej klasy interfejs UserManager jest zdefiniowany w kroku 3, Utwórz nową klasę SQLPasswordHasher i skopiuj poniższego kodu.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Usuń błędy kompilacji importując System.Text i System.Security.Cryptography przestrzeni nazw.

    Metoda EncodePassword szyfruje hasło zgodnie z domyślną implementację kryptograficzny członkostwa SQL. To jest pobierana z biblioteki dll System.Web. Jeśli stary korzystać z aplikacji niestandardowej implementacji, a następnie powinny być uwzględnione w tym miejscu. Należy zdefiniować dwa inne metody *HashPassword* i *VerifyHashedPassword* używające *EncodePassword* metody wyznaczania wartości skrótu podanym hasłem lub sprawdź, czy jako zwykły tekst hasło przy użyciu jednego istniejących w bazie danych.

    System członkostwa SQL używane PasswordHash, PasswordSalt i PasswordFormat skrótu hasła wprowadzonego przez użytkowników podczas rejestrowania lub zmienić swoje hasło. Podczas migracji wszystkie trzy pola są przechowywane w kolumnie PasswordHash w tabeli AspNetUser oddzielone ' |' znaków. Po zalogowaniu użytkownika i hasło jest tych pól, aby sprawdzić hasła, a używamy kryptograficznego członkostwa SQL w przeciwnym razie używamy kryptograficznego domyślne systemu tożsamości można zweryfikować hasło. Dzięki temu użytkownicy nie musi zmieniać swoje hasła, gdy aplikacja jest migrowana.
5. Zadeklaruj Konstruktor dla klasy interfejs UserManager i przekazany jako SQLPasswordHasher do właściwości w konstruktorze.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Utwórz nowe konto zarządzania stron

Następnym krokiem w procesie migracji jest dodać konto zarządzania stron, które umożliwia użytkownikowi rejestracji i logowania. Stare konto strony z członkostwa SQL użyj formantów, które nie działają z nowym systemem tożsamości. Aby dodać nowego użytkownika strony zarządzania zgodne samouczek łącze [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) od kroku " Dodawanie do rejestracji użytkowników do aplikacji formularzy sieci Web ", ponieważ firma Microsoft już utworzeniu projektu i dodaniu pakietów NuGet.

Należy wprowadzić kilka zmian, na przykład, aby pracować z projektem, który mamy tutaj.

- Register.aspx.cs i Login.aspx.cs kodzie Użyj klasy `UserManager` z pakietów tożsamości, aby utworzyć użytkownika. W tym przykładzie należy użyć interfejs UserManager, dodane w folderze modeli, wykonując kroki wymienione wcześniej.
- Należy użyć klasy użytkownika utworzone zamiast IdentityUser w kodzie Register.aspx.cs i Login.aspx.cs klasy. Przechwytuje to w naszej klasy użytkownika niestandardowego dla systemu tożsamości.
- Część do utworzenia bazy danych można pominięte.
- Deweloper musi ustawić identyfikator aplikacji dla nowego użytkownika, aby dopasować bieżący identyfikator aplikacji. Można to zrobić podczas badania identyfikator aplikacji dla tej aplikacji, przed utworzeniem obiektu użytkownika w klasie Register.aspx.cs i ustawiając go przed utworzeniem użytkownika. 

    Przykład:

    Zdefiniuj metody na stronie Register.aspx.cs do badania aspnet\_aplikacji tabeli i uzyskać identyfikator aplikacji według nazwy aplikacji

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Teraz pobrać Ustaw tę wartość na obiekt użytkownika

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Użyj stara nazwa użytkownika i hasło istniejącego użytkownika logowania. Użyj strony rejestru do utworzenia nowego użytkownika. Sprawdź także, że użytkownicy są w rolach zgodnie z oczekiwaniami.

Eksportowanie do systemu tożsamości pozwala użytkownikowi na dodanie otwarte uwierzytelnianie (OAuth) do aplikacji. Zapoznaj się z przykładem [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) mającego OAuth włączone.

## <a name="next-steps"></a>Następne kroki

W tym samouczku będziemy pokazano, jak port użytkownikom z SQL członkostwa ASP.NET Identity, ale nie możemy portu danych profilu. W następnym samouczku wyjaśniono do przenoszenia danych profilu z członkostwa SQL do nowego systemu tożsamości.

Możesz pozostawić opinii w dolnej części tego artykułu.

*Dzięki użyciu Tomasz Dykstra i Rick Anderson do przeglądania tego artykułu.*
