---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Przy użyciu dostawców uwierzytelniania OAuth w MVC 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację sieci web platformy ASP.NET MVC 4, która umożliwia użytkownikom logowanie się przy użyciu poświadczeń z zewnętrznego dostawcy, takich jak Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: d0203b62c911056fc56ed103c1c42f67816cbbf0
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021758"
---
<a name="using-oauth-providers-with-mvc-4"></a>Korzystanie z dostawców uwierzytelniania OAuth we wzorcu MVC 4
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku dowiesz się, jak utworzyć aplikację sieci web platformy ASP.NET MVC 4, która umożliwia użytkownikom logowanie za pomocą poświadczeń z zewnętrznego dostawcy, takich jak Facebook, Twitter, Microsoft lub Google, a następnie zintegrować niektóre funkcje, które z tych dostawców w sieci Aplikacja sieci Web. Dla uproszczenia ten samouczek koncentruje się na pracy przy użyciu poświadczeń z usługi Facebook.
> 
> Aby użyć zewnętrznych poświadczeń w aplikacji sieci web programu ASP.NET MVC 5, zobacz [tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Włączanie tych poświadczeń w witrynach sieci web zapewnia znaczące korzyści, ponieważ milionom użytkowników mają już konta z tych dostawców zewnętrznych. Tacy użytkownicy mogą być bardziej skłonni się zarejestrować dla danej witryny, gdy ta osoba nie ma do tworzenia i Zapamiętaj nowego zestawu poświadczeń. Ponadto po użytkownik jest zalogowany za pomocą jednego z tych dostawców, można zastosować operacji społecznościowych od dostawcy.


## <a name="what-youll-build"></a>Będziesz tworzyć

Istnieją dwa główne cele, w tym samouczku:

1. Umożliwić użytkownikowi logowanie się przy użyciu poświadczeń z dostawcą uwierzytelniania OAuth.
2. Pobranie informacji o koncie z dostawcy i zintegrowania z rejestracji konta dla danej witryny.

Mimo że na potrzeby przykładów w tym samouczku skoncentrować się na temat korzystania z usługi Facebook jako dostawcy uwierzytelniania, można zmodyfikować kod, aby użyć innych dostawców. Kroki, aby wdrożyć u innego dostawcy są bardzo podobne do czynności, które będą widoczne w tym samouczku. Można zauważyć tylko między nimi istotne różnice bezpośrednie wywołania interfejsu API dostawcy zestawu.

## <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) lub [programu Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Lub

- Microsoft Visual Studio 2010 z dodatkiem SP1 lub [Visual Web Developer Express 2010 z dodatkiem SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Ponadto w tym temacie założono, że masz podstawową wiedzę na temat platformy ASP.NET MVC i programu Visual Studio. Wprowadzenie do platformy ASP.NET MVC 4, odwiedź stronę [wprowadzenie do platformy ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio Utwórz nową aplikację sieci Web 4 platformy ASP.NET MVC i nadaj mu &quot;OAuthMVC&quot;. Można kierować platformy .NET Framework 4.5 lub 4.

![Tworzenie projektu](using-oauth-providers-with-mvc/_static/image1.png)

W oknie nowego projektu programu ASP.NET MVC 4 Wybierz **aplikacji internetowej** i pozostawić **Razor** aparatu widoku.

![Wybierz aplikację internetową](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Włącz dostawcę

Gdy tworzysz aplikację sieci web MVC 4 przy użyciu szablonu aplikacji internetowej, projekt zostanie utworzony przy użyciu pliku o nazwie AuthConfig.cs w aplikacji\_folder początkowy.

![Plik AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Plik AuthConfig zawiera kod, aby zarejestrować klientów dla dostawcy uwierzytelniania zewnętrznego. Domyślnie ten kod został umieszczony w komentarzach, więc żaden z zewnętrznych dostawców są włączone.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Należy Usuń komentarz ten kod w celu korzystania z klienta zewnętrznego uwierzytelnienia. Usuń znaczniki komentarza dostawców, które mają zostać uwzględnione w lokacji. Na potrzeby tego samouczka umożliwi tylko poświadczeń serwisu Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Zwróć uwagę, w powyższym przykładzie, że metoda zawiera puste ciągi dla parametrów rejestracji. Jeśli zostanie podjęta próba uruchomienia aplikację teraz, aplikacja zgłasza wyjątek argumentu, ponieważ puste ciągi nie są dozwolone dla parametrów. Aby zapewnić prawidłowe wartości, należy zarejestrować witryny sieci web przy użyciu dostawców zewnętrznych, jak pokazano w następnej sekcji.

## <a name="registering-with-an-external-provider"></a>Rejestrowanie przy użyciu zewnętrznego dostawcy

Do uwierzytelniania użytkowników za pomocą poświadczeń z zewnętrznego dostawcy, należy zarejestrować witryny sieci web za pomocą dostawcy. Po zarejestrowaniu witryny, otrzyma parametry (takie jak klucz lub identyfikator i klucz tajny) do uwzględnienia podczas rejestrowania klienta. Musisz mieć konto z dostawcami, które chcesz użyć.

W tym samouczku nie są wyświetlane wszystkie kroki, które należy wykonać, aby zarejestrować za pomocą tych dostawców. Kroki nie są zwykle trudne. Aby pomyślnie zarejestrować witrynę, wykonaj instrukcje podane w tych witrynach. Aby rozpocząć pracę z zarejestrowaniem witryny, zobacz witryny dla deweloperów dla:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

W przypadku rejestrowania witryny za pomocą usługi Facebook, można zapewnić &quot;localhost&quot; dla domeny, lokacji i `&quot; http://localhost/&quot;` dla adresu URL, jak pokazano na poniższej ilustracji. Przy użyciu localhost pracuje z dostawcami większość, ale obecnie nie współpracujesz z dostawcy firmy Microsoft. W przypadku dostawcy firmy Microsoft musi zawierać prawidłową witrynę sieci web adres URL.

![Rejestracja witryny](using-oauth-providers-with-mvc/_static/image4.png)

Na poprzedniej ilustracji zostały usunięte wartości identyfikatora aplikacji, klucz tajny aplikacji i kontaktowy adres e-mail. Po zarejestrowaniu faktycznie witryny, te wartości będą obecne. Należy pamiętać wartości identyfikatora aplikacji i klucz tajny aplikacji, ponieważ spowoduje dodanie ich do aplikacji.

## <a name="creating-test-users"></a>Tworzenie użytkowników testowych

Jeśli możesz nie mieć nic przeciwko do Testuj swoją witrynę przy użyciu istniejącego konta serwisu Facebook, możesz pominąć tę sekcję.

Można łatwo utworzyć użytkowników testowych dla aplikacji w ramach strony zarządzania aplikacji usługi Facebook. Te konta, zaloguj się do witryny testowe. Utwórz użytkowników testowych, klikając pozycję **role** łącze w okienku nawigacji po lewej stronie, a kliknięcie **Utwórz** łącza.

![Utwórz użytkowników testowych](using-oauth-providers-with-mvc/_static/image5.png)

W witrynie Facebook automatycznie tworzy liczby kont testowych, które w przypadku żądania.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Dodawanie identyfikatora aplikacji i klucza tajnego od dostawcy

Teraz, gdy otrzymane identyfikatora i klucza tajnego z usługi Facebook, wróć do pliku AuthConfig i dodać je jako wartości parametrów. Poniższe wartości nie są rzeczywiste wartości.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Zaloguj się przy użyciu poświadczeń zewnętrznych

To wszystko, co trzeba zrobić, aby umożliwić zewnętrznych poświadczeń w lokacji. Uruchom aplikację i kliknij łącze Zaloguj się w prawym górnym rogu. Ten szablon automatycznie rozpoznaje zarejestrowano usługi Facebook jako dostawca i zawiera przycisk dla dostawcy. Jeśli zarejestrujesz wielu dostawców, przycisk dla każdej z nich jest automatycznie dołączane.

![Logowanie zewnętrzne](using-oauth-providers-with-mvc/_static/image6.png)

W tym samouczku opisano, jak dostosować logowania przycisków do zewnętrznych dostawców. Aby uzyskać te informacje, zobacz [Dostosowywanie interfejs użytkownika logowania w przypadku korzystania z uwierzytelniania OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Kliknij przycisk Facebook, aby zalogować się przy użyciu poświadczeń serwisu Facebook. Po wybraniu jednego z dostawców zewnętrznych są przekierowywane do tej lokacji i zostanie wyświetlony monit tej usługi, aby zalogować się.

Na poniższej ilustracji przedstawiono ekran logowania za pomocą konta Facebook. Stwierdza, że używasz konta w serwisie Facebook, zaloguj się do witryny o nazwie oauthmvcexample.

![Uwierzytelnianie serwisu Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Po zalogowaniu się przy użyciu poświadczeń serwisu Facebook, strony informuje użytkownika, że lokacji będą mieć dostęp do podstawowych informacji.

![żądanie uprawnień](using-oauth-providers-with-mvc/_static/image8.png)

Po wybraniu **przejdź do aplikacji**, użytkownik musi zarejestrować się dla witryny. Na poniższej ilustracji przedstawiono na stronie rejestracji po użytkownik zalogował się przy użyciu poświadczeń serwisu Facebook. Nazwa użytkownika jest zazwyczaj są wstępnie wypełnione nazwą od dostawcy.

![zarejestruj](using-oauth-providers-with-mvc/_static/image9.png)

Kliknij przycisk **zarejestrować** do ukończenia rejestracji. Zamknij przeglądarkę.

Widać, że dodano nowe konto z bazą danych. Otwórz w Eksploratorze serwera **DefaultConnection** bazy danych, a następnie otwórz **tabel** folderu.

![tabele bazy danych](using-oauth-providers-with-mvc/_static/image10.png)

Kliknij prawym przyciskiem myszy **UserProfile** tabeli, a następnie wybierz pozycję **Pokaż dane tabeli**.

![Pokaż dane](using-oauth-providers-with-mvc/_static/image11.png)

Zobaczysz nowe konto, którą dodałeś. Spójrz na dane w **strony sieci Web\_OAuthMembership** tabeli. Zobaczysz większej ilości danych związanych z zewnętrznego dostawcy dla konta, który właśnie został dodany.

Jeśli chcesz włączyć uwierzytelnianie zewnętrzne, wszystko będzie gotowe. Jednak można dodatkowo zintegrować informacje od dostawcy nowy proces rejestracji użytkownika, jak pokazano w poniższych sekcjach.

## <a name="create-models-for-additional-user-information"></a>Twórz modele, aby uzyskać dodatkowe informacje dotyczące użytkownika

Jak można zauważyć w poprzednich sekcjach, nie musisz pobierać żadnych dodatkowych informacji dotyczących rejestracji wbudowanego konta do pracy. Jednak większość zewnętrznych dostawców przekazać ponownie dodatkowe informacje o użytkowniku. Poniższe sekcje pokazują, jak zachować te informacje i zapisywanie w bazie danych. W szczególności zostaną zachowane, wartości pełną nazwę użytkownika, identyfikator URI Osobista strona sieci web przez użytkownika i wartość, która wskazuje, czy Facebook wykryła konta.

Użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) Dodawanie tabeli do przechowywania dodatkowych informacji dotyczących użytkowników. Dodajesz tabeli do istniejącej bazy danych, dlatego najpierw należy utworzyć migawkę bieżącej bazy danych. Tworząc migawkę bieżącej bazy danych, możesz później utworzyć migracji, który zawiera nową tabelę. Aby utworzyć migawkę bieżącej bazy danych:

1. Otwórz **Konsola Menedżera pakietów**
2. Uruchom polecenie **enable-migrations**
3. Uruchom polecenie **początkowej migracji Dodaj — IgnoreChanges**
4. Uruchom polecenie **Aktualizuj bazy danych**

Teraz należy dodać nowe właściwości. W folderze modeli Otwórz plik AccountModels.cs i znaleźć klasy RegisterExternalLoginModel. Klasa RegisterExternalLoginModel przechowuje wartości, które pochodzą dostawcy uwierzytelniania. Dodaj właściwości o nazwie FullName i łącza, jak wyróżniono poniżej.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Również w AccountModels.cs, Dodaj nową klasę o nazwie ExtraUserInformation. Ta klasa reprezentuje nową tabelę, która zostanie utworzona w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

W klasie UsersContext Dodaj wyróżniony kod poniżej, aby utworzyć właściwość DbSet dla nowej klasy.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Teraz możesz przystąpić do tworzenia nowej tabeli. Otwórz konsolę Menedżera pakietów, ponownie i tym razem:

1. Uruchom polecenie **AddExtraUserInformation Dodaj migracji**
2. Uruchom polecenie **Aktualizuj bazy danych**

Nowa tabela istnieje teraz w bazie danych.

## <a name="retrieve-the-additional-data"></a>Pobrania dodatkowych danych.

Istnieją dwa sposoby, aby pobrać dodatkowe dane użytkownika. Pierwszy sposób polega na przechowywanie danych użytkownika, przekazanych, domyślnie podczas żądania uwierzytelniania. Druga metoda jest specjalnie wywołanie dostawcy interfejsu API i uzyskasz więcej informacji. Wartości FullName i łącza są automatycznie przekazywane przez usługi Facebook. Wartość, która wskazuje, czy Facebook wykryła konta są pobierane za pomocą wywołania interfejsu API usługi Facebook. Najpierw zostanie wypełniony wartości FullName i Link i później, nastąpi pobranie wartości zweryfikowane.

Aby pobrać dodatkowe dane użytkownika, należy otworzyć **AccountController.cs** w pliku **kontrolerów** folderu.

Ten plik zawiera logikę do rejestrowania, rejestrowanie i zarządzania kontami. W szczególności zwróć uwagę, metody o nazwie **ExternalLoginCallback** i **ExternalLoginConfirmation**. W ramach tych metod możesz dodać kod, aby dostosować operacje logowania zewnętrznego dla aplikacji. W pierwszym wierszu **ExternalLoginCallback** metoda zawiera:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Dodatkowe dane użytkownika jest przekazywany w **ExtraData** właściwość **AuthenticationResult** obiekt, który jest zwracany z **VerifyAuthentication** metody. Klient usługi Facebook zawiera następujące wartości w **ExtraData** właściwości:

- identyfikator
- nazwa
- Link
- płeć
- accesstoken

Innych dostawców mają podobne, lecz nieco różni się danych we właściwości ExtraData.

Jeśli jesteś nowym użytkownikiem witryny jest użytkownik, będzie pobrać niektóre dodatkowe dane i przekazywać dane do widoku potwierdzenia. Ostatni blok kodu w metodzie jest uruchamiane tylko wtedy, gdy użytkownik jest nowym składnikiem w lokacji. Zastąp następujący wiersz:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

w tym:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Ta zmiana obejmuje tylko wartości dla właściwości FullName i łącza.

W **ExternalLoginConfirmation** metody, zmodyfikuj kod wyróżniony poniżej, aby zapisać dodatkowe informacje dotyczące użytkownika.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Dostosowywanie widoku

Pobranie z dostawcy danych dodatkowych użytkownika pojawi się na stronie rejestracji.

W **widoków**/**konta** folder, otwórz **ExternalLoginConfirmation.cshtml**. Poniżej istniejącego pola dla nazwy użytkownika należy dodać pola Imię i nazwisko, Link i PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Teraz jesteś już prawie gotowe uruchomić aplikację i zarejestrować nowego użytkownika o dodatkowe informacje, które są zapisywane. Konieczne jest posiadanie konta, które nie jest już zarejestrowany w witrynie. Możesz używać konta innego testu, lub usuwanie wierszy w **UserProfile** i **stron sieci Web\_OAuthMembership** tabel dla konta chcesz użyć ponownie. Przez usunięcie tych wierszy, użytkownik musi zagwarantować ponownie zarejestrować konto.

Uruchom aplikację i zarejestrować nowego użytkownika. Należy zauważyć, że tym razem strona potwierdzenia zawiera więcej wartości.

![zarejestruj](using-oauth-providers-with-mvc/_static/image12.png)

Po zakończeniu rejestracji, zamknij przeglądarkę. Szukaj w bazie danych, zwróć uwagę na nowe wartości w **ExtraUserInformation** tabeli.

## <a name="install-nuget-package-for-facebook-api"></a>Zainstaluj pakiet NuGet dla interfejsu API usługi Facebook

Udostępnia usługi Facebook [API](https://developers.facebook.com/docs/reference/apis/) , można wywołać w celu wykonania operacji. Możesz wywołać interfejs API usługi Facebook, kierując wysyłania żądań HTTP lub przy użyciu instalacji pakietu NuGet, który ułatwia wysyłanie tych żądań. Przy użyciu pakietu NuGet jest wyświetlany w tym samouczku, ale NuGet Instalowanie pakietu nie jest istotne. W tym samouczku przedstawiono sposób użycia pakietu zestawu SDK języka C# usługi Facebook. Istnieją inne pakiety NuGet uzyskanymi podczas wywoływania interfejsu API usługi Facebook.

Z **Zarządzaj pakietami NuGet** systemu windows, wybierz pakiet zestawu SDK języka C# usługi Facebook.

![Zainstaluj pakiet](using-oauth-providers-with-mvc/_static/image13.png)

Użyjesz Facebook zestawu SDK C# do wywołania operacji, która wymaga tokenu dostępu dla użytkownika. Następna sekcja pokazuje, jak uzyskać token dostępu.

## <a name="retrieve-access-token"></a>Pobieranie tokenu dostępu

Większość zewnętrznych dostawców przesłać token dostępu po zweryfikowaniu poświadczeń użytkownika. Ten token dostępu jest bardzo ważne, ponieważ umożliwia wywoływanie operacji, które są dostępne tylko dla uwierzytelnionych użytkowników. W związku z tym pobierania i przechowywania token dostępu jest niezbędne zapewnić więcej funkcji.

W zależności od zewnętrznego dostawcy tokenu dostępu może mieć okresu ważności ograniczoną ilość czasu. Aby upewnić się, że masz token dostępu nie jest ważna, można będzie pobrać każdorazowo użytkownik loguje się i zapisz go jako wartość sesji zamiast zapisywanie w bazie danych.

W **ExternalLoginCallback** metody token dostępu jest również przekazany z powrotem do **ExtraData** właściwość **AuthenticationResult** obiektu. Dodaj wyróżniony kod do **ExternalLoginCallback** zapisanie tokenu dostępu w **sesji** obiektu. Ten kod jest uruchamiany za każdym razem, gdy użytkownik zaloguje się za pomocą konta w serwisie Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Mimo że w tym przykładzie pobiera token dostępu z usługi Facebook, możesz pobrać token dostępu z dowolnego zewnętrznego dostawcy za pomocą tego samego klucza o nazwie &quot;accesstoken&quot;.

## <a name="logging-off"></a>Trwa wylogowywanie

Wartość domyślna **wylogowania** metody logowania użytkownika z aplikacji, ale nie rejestruje użytkownika poza zewnętrznego dostawcy. Aby zalogować użytkownika z usługi Facebook i uniemożliwić przechowywanie po wylogowaniu użytkownika tokenu, można dodać następujący wyróżniony kod do **wylogowania** metody w elementu AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Wartości podane `next` parametr jest adres URL do użycia po użytkownik zaloguje się z serwisem Facebook. Podczas testowania, na komputerze lokalnym, czy Podaj prawidłowy numer portu lokacji lokalnej. W środowisku produkcyjnym zapewni domyślną stronę, np. contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Pobierz informacje o użytkowniku, który wymaga tokenu dostępu

Skoro masz przechowywane token dostępu i zainstalować pakiet zestawu SDK języka C# usługi Facebook, możesz użyć ich do żądania dodatkowych informacji dotyczących użytkowników z usługi Facebook. W **ExternalLoginConfirmation** metody, Utwórz wystąpienie obiektu **FacebookClient** klasy, przekazując wartość tokenu dostępu. Wartość żądania **zweryfikować** właściwość obecnie uwierzytelniony użytkownik. **Zweryfikować** właściwość wskazuje, czy Facebook został zweryfikowany konta w inny sposób, na przykład wysłanie wiadomości na telefon komórkowy. Zapisz tę wartość w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Należy ponownie usunąć rekordy w bazie danych użytkownika albo użyj innego konta w serwisie Facebook.

Uruchom aplikację i zarejestrować nowego użytkownika. Przyjrzyj się **ExtraUserInformation** tabelę, aby sprawdzić wartość właściwości zweryfikowany.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzono lokację, która jest zintegrowana z usługą Facebook do uwierzytelniania użytkowników i dane rejestracji. Wiesz już o zachowanie domyślne, które zostało skonfigurowane do aplikacji sieci web MVC 4 oraz jak dostosować to zachowanie domyślne.

## <a name="related-topics"></a>Tematy pokrewne

- [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
