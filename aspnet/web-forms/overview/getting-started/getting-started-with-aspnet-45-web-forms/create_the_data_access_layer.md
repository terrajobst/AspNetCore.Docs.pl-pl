---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: "Utwórz Warstwa dostępu do danych | Dokumentacja firmy Microsoft"
author: Erikre
description: "Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 809609155b06c4632bd4f450082d84c432c7a46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="create-the-data-access-layer"></a>Utwórz Warstwa dostępu do danych
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


Ten samouczek zawiera opis sposobu tworzenia, dostępu i przejrzyj dane z bazy danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. W tym samouczku jest oparty na poprzednich samouczek "Utwórz projekt" i jest częścią serii samouczek Wingtip zabawka magazynu. Po ukończeniu tego samouczka, dlatego zostanie utworzone grupy klas dostępu do danych, które znajdują się w *modele* folderu projektu.

## <a name="what-youll-learn"></a>Zawartość:

- Jak utworzyć modeli danych.
- Jak zainicjować i inicjatora bazy danych.
- Jak zaktualizować i skonfigurować aplikację do obsługi bazy danych.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Poniżej przedstawiono funkcje wprowadzone w samouczku:

- Entity Framework Code First
- LocalDB
- Adnotacji danych

## <a name="creating-the-data-models"></a>Tworzenie modeli danych

[Entity Framework](https://msdn.microsoft.com/data/aa937723) to struktura mapowania obiektów relacyjnych (ORM). Umożliwia pracę z danych relacyjnych jako obiekty, eliminując większość kodu dostępu do danych, który zazwyczaj będzie potrzebny do zapisania. Przy użyciu programu Entity Framework, można wystawiać zapytań za pomocą [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), następnie pobrać i manipulowanie danymi jako silnie typizowanych obiektów. LINQ zawiera wzorce wykonywania kwerend i aktualizowanie danych. Przy użyciu programu Entity Framework pozwala skupić się na tworzenie pozostała część aplikacji, a nie koncentrujących się na dane podstawowe informacje na temat dostępu. W dalszej części tego samouczka serii pokażemy ci sposobu używania danych do wypełnienia zapytań nawigacji i produktu.

Model programowania, o nazwie obsługuje programu Entity Framework *Code First*. Kod pozwala najpierw zdefiniować modeli danych przy użyciu klasy. Klasa jest konstrukcję, która umożliwia tworzenie własnych niestandardowych typów grupując zmienne innych typów, metod i zdarzeń. Można mapowania klas istniejącą bazę danych lub użyj ich, aby wygenerować bazę danych. W tym samouczku utworzysz modeli danych pisząc klasy modelu danych. Następnie otrzymasz stosowne utworzyć bazę danych na bieżąco z tych nowych klas programu Entity Framework.

Zaczniesz przez utworzenie klas jednostek, które definiują modeli danych dla aplikacji formularzy sieci Web. Następnie utworzy klasę kontekstu, która zarządza klas jednostek i zapewnia dostęp do danych w bazie danych. Zostanie również utworzona klasa inicjatora, który będzie używany do wypełniania bazy danych.

### <a name="entity-framework-and-references"></a>Entity Framework i odwołań

Domyślnie program Entity Framework jest dołączana w przypadku tworzenia nowego **aplikacji sieci Web ASP.NET** przy użyciu **formularzy sieci Web** szablonu. Entity Framework można zainstalowana, odinstalowane i zaktualizować jako pakietu NuGet.

Ten pakiet NuGet zawiera następujące **środowiska uruchomieniowego** zestawy w projekcie:

- EntityFramework.dll — wszystkie wspólne środowiska uruchomieniowego kod używany przez program Entity Framework
- EntityFramework.SqlServer.dll — dostawcy programu Microsoft SQL Server dla programu Entity Framework

### <a name="entity-classes"></a>Klas jednostek

Klasy utworzyć aby zdefiniować schemat danych są nazywane klas jednostek. Jeśli nowy projekt bazy danych, należy traktować klas jednostek jako definicje tabel bazy danych. Każda właściwość klasy określa kolumnę w tabeli bazy danych. Te klasy udostępniają niewielka, obiektów relacyjnych interfejs między kodem zorientowane obiektowo i struktura tabeli relacyjnej bazy danych.

W tym samouczku będzie uruchomić przez dodanie jednostki prostej klasy reprezentujące schematów dla produktów i kategorii. Klasa produkty będą zawiera definicje dla każdego produktu. Nazwa każdego z elementów członkowskich klasy produktu będzie `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, i `Category`. Klasa kategorii będzie zawierać definicji dla każdej kategorii, że produkt może należeć do, takie jak samochodów, łodzi lub płaszczyzny. Nazwa każdego z elementów członkowskich klasy kategorii będzie `CategoryID`, `CategoryName`, `Description`, i `Products`. Każdy produkt będą należeć do jednej z następujących kategorii. Klasy te jednostki zostaną dodane do istniejącego projektu *modele* folderu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**. 

    ![Tworzenie nowego elementu Menu Warstwa dostępu do danych-](create_the_data_access_layer/_static/image1.png)

 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. W obszarze **Visual C#** z **zainstalowana** w okienku po lewej stronie, wybierz opcję **kod**. 

    ![Tworzenie nowego elementu Menu Warstwa dostępu do danych-](create_the_data_access_layer/_static/image2.png)
3. Wybierz **klasy** w środkowym okienku i nazwa ta nowa klasa *Product.cs*.
4. Kliknij przycisk **Dodaj**.  
 Nowy plik klasy jest wyświetlany w edytorze.
5. Zastąp następujący kod w kodzie domyślnym:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Utwórz inną klasę, powtarzając kroki od 1 do 4, jednak nowej klasie nazwę *Category.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Jak wcześniej wspomniano, `Category` klasy reprezentuje typ produktu, którego aplikacja jest przeznaczona do sprzedaży (takich jak <a id="a"> </a> &quot;samochodów&quot;, &quot;łodzi&quot;, &quot;Rakiet&quot;i tak dalej) i `Product` klasa reprezentuje produkty indywidualne (toys) w bazie danych. Każde wystąpienie `Product` obiektu odpowiada wiersz w tabeli relacyjnej bazy danych, a każda właściwość klasy produktu przypisze do kolumny w tabeli relacyjnej bazy danych. W dalszej części tego samouczka można przejrzeć dane produktu znajdujące się w bazie danych.

### <a name="data-annotations"></a>Adnotacji danych

Można zauważyć, że niektóre elementy członkowskie klas określenie szczegółów dotyczących elementu członkowskiego, takich jak atrybuty `[ScaffoldColumn(false)]`. Są to *adnotacji danych*. Atrybuty adnotacji danych opisujący sposób sprawdzania poprawności danych wejściowych użytkownika dla tego elementu członkowskiego, aby określić, formatowanie i określić, jak jest modelowana po utworzeniu bazy danych.

### <a name="context-class"></a>Context — Klasa

Aby rozpocząć korzystanie z klas dla dostępu do danych, należy zdefiniować klasy kontekstu. Jak już wspomniano, klasy kontekstu zarządza klas jednostek (takich jak `Product` klasy i `Category` klasy) i zapewnia dostęp do danych w bazie danych.

Ta procedura dodaje nowe C# kontekstu klasę do *modele* folderu.

1. Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **klasy** w środkowym okienku, nadaj jej nazwę *ProductContext.cs* i kliknij przycisk **Dodaj**.
3. Zastąp domyślny kod zawarty w klasie z następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Ten kod dodaje `System.Data.Entity` przestrzeni nazw, aby mieć dostęp do wszystkich podstawowych funkcji programu Entity Framework, w tym możliwość zapytania, wstawiania, aktualizowania i usuwania danych Praca z silnie typizowanych obiektów.

`ProductContext` Klasa reprezentuje produktu kontekst bazy danych programu Entity Framework, który obsługuje pobieranie, przechowywania i aktualizowania `Product` klasy wystąpień w bazie danych. `ProductContext` Pochodną klasy `DbContext` podstawowa klasy Entity Framework.

### <a name="initializer-class"></a>Klasa inicjatora

Konieczne będzie uruchomienie niektórych niestandardowej logiki zainicjować kontekst jest używany podczas pierwszego bazy danych. Pozwoli to inicjatora dane mają zostać dodane do bazy danych, tak aby natychmiast wyświetlić produktów i kategorii.

Ta procedura dodaje nowe C# inicjatora klasę do *modele* folderu.

1. Utworzyć inną `Class` w *modele* folder i nadaj mu nazwę *ProductDatabaseInitializer.cs*.
2. Zastąp domyślny kod zawarty w klasie z następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Jak widać z powyższego kodu podczas tworzenia i inicjowania bazy danych `Seed` właściwości jest zastąpione i ustawić. Gdy `Seed` właściwość jest ustawiona, wartości kategorii i produktów są używane do wypełniania bazy danych. Jeśli podjęto próbę aktualizacji danych inicjatora, modyfikując powyższy kod po utworzeniu bazy danych, nie zobaczysz jakiekolwiek aktualizacje po uruchomieniu aplikacji sieci Web. Przyczyną jest powyższym kodzie użyto implementacja `DropCreateDatabaseIfModelChanges` klasy do rozpoznawania, jeśli model (schemat) został zmieniony przed zresetowaniem danych inicjatora. Jeśli zostały wprowadzone żadne zmiany `Category` i `Product` klas jednostek, bazy danych nie zostanie zainicjowana ponownie z danymi inicjatora.

> [!NOTE] 
> 
> Jeśli potrzebujesz bazy danych do odtworzenia zawsze uruchomienia aplikacji, można użyć `DropCreateDatabaseAlways` klasy zamiast `DropCreateDatabaseIfModelChanges` klasy. Jednak dla tej serii samouczek, przy użyciu `DropCreateDatabaseIfModelChanges` klasy.


W tym momencie w tym samouczku będzie mieć *modele* folder z czterech nowe klasy i jeden domyślny klasy:

![Utwórz Warstwa dostępu do danych - folderu modeli](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurowanie aplikacji do korzystania z modelu danych

Teraz, po utworzeniu klasy, które przedstawiają dane, musisz skonfigurować aplikację do korzystania z klasy. W *Global.asax* pliku, Dodaj kod, który inicjuje modelu. W *Web.config* pliku należy dodać informacje o tym aplikacji, co baza danych będzie używane do przechowywania danych reprezentowanego przez nowe klasy danych. *Global.asax* plików może służyć do obsługi zdarzeń aplikacji lub metody. *Web.config* plików umożliwia kontrolowanie konfiguracji aplikacji sieci web platformy ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aktualizowanie pliku Global.asax

Aby zainicjować modeli danych podczas uruchamiania aplikacji, spowoduje zaktualizowanie `Application_Start` obsługi w *Global.asax.cs* pliku.

> [!NOTE] 
> 
> W Eksploratorze rozwiązań, można wybrać dowolną *Global.asax* pliku lub *Global.asax.cs* plik, aby edytować *Global.asax.cs* pliku.


1. Dodaj następujący kod wyróżnione kolorem żółtym do `Application_Start` metody w *Global.asax.cs* pliku.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Przeglądarka musi obsługiwać HTML5, aby wyświetlić kod wyróżnione kolorem żółtym podczas wyświetlania tego samouczka serii w przeglądarce.


Jak pokazano w powyższym kodzie podczas uruchamiania aplikacji, aplikacji określa inicjatora, który będzie uruchamiany podczas pierwszego danych jest dostępny. Dwie dodatkowe przestrzenie nazw są wymagane, aby uzyskać dostęp do `Database` obiektu i `ProductDatabaseInitializer` obiektu.

 Zmodyfikowanie pliku Web.Config 

Mimo że Entity Framework Code First będzie generowania bazy danych dla Ciebie w domyślnej lokalizacji bazy danych jest wypełniana inicjatora danych, dodawanie danych połączenia do swojej aplikacji zapewnia kontrolę nad lokalizacji bazy danych. Określ to połączenie z bazą danych przy użyciu parametrów połączenia z aplikacją *Web.config* pliku w katalogu głównym projektu. Dodając nowe parametry połączenia, można kierować lokalizacji bazy danych (*wingtiptoys.mdf*) ma zostać utworzony w katalogu danych aplikacji (*aplikacji\_danych*), zamiast domyślnej Lokalizacja. Wprowadzenie tej zmiany umożliwi znajdowanie i sprawdź plik bazy danych w dalszej części tego samouczka.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Web.config* pliku.
2. Dodaj wyróżnione kolorem żółtym do ciągu połączenia `<connectionStrings>` sekcji *Web.config* plików w następujący sposób:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Gdy aplikacja jest uruchamiana po raz pierwszy, zostanie utworzona w lokalizacji określonej przez ciąg połączenia bazy danych. Jednak przed uruchomieniem aplikacji, utworzymy ją najpierw.

## <a name="building-the-application"></a>Kompilowanie aplikacji

Aby upewnić się, czy działają wszystkie klasy i zmian w aplikacji sieci Web, należy skompilować aplikację.

1. Z **debugowania** menu, wybierz opcję **kompilacji WingtipToys**.  
 **Dane wyjściowe** okno jest wyświetlane, a jeśli wszystkie poszło dobrze, zobacz *zakończyło się pomyślnie* wiadomości.  

    ![Utwórz Warstwa dostępu do danych — w oknie danych wyjściowych](create_the_data_access_layer/_static/image4.png)

Jeśli napotkasz błąd, ponownie sprawdź powyższe kroki. Informacje zawarte w **dane wyjściowe** okno będzie wskazywać plik, który ma problem, gdzie w pliku zmiany jest wymagane. Te informacje będą umożliwiają określenia, jaka część powyższe kroki muszą zostać sprawdzone i stałym w projekcie.

## <a name="summary"></a>Podsumowanie

W tym samouczku serii można mieć utworzone modelu danych, a także, dodaje kod, który będzie używany do inicjowania i inicjatora bazy danych. Skonfigurowano również aplikacji na używanie modeli danych po uruchomieniu aplikacji.

W następnym samouczku będzie aktualizacji interfejsu użytkownika, Dodaj nawigacji i pobierania danych z bazy danych. Spowoduje to bazy danych są automatycznie tworzone w oparciu klas jednostek, które zostały utworzone w tym samouczku.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie struktury jednostek](https://msdn.microsoft.com/library/bb399567.aspx)   
[Przewodnik dla początkujących do programu ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Programowanie pierwszej aplikacji za pomocą programu Entity Framework Code](http://www.msteched.com/2010/Europe/DEV212) (klip wideo)   
[Kod pierwszego relacje interfejsu API Fluent](https://msdn.microsoft.com/data/hh134698)   
[Kod pierwszego adnotacji danych](https://msdn.microsoft.com/data/gg193958)  
[Ulepszenia wydajności programu Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

>[!div class="step-by-step"]
[Poprzednie](create-the-project.md)
[dalej](ui_and_navigation.md)
