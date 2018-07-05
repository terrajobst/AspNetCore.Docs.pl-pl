---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Tworzenie warstwy dostępu do danych | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390043"
---
<a name="create-the-data-access-layer"></a>Tworzenie warstwy dostępu do danych
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku opisano sposób tworzenia, dostęp i przeglądanie danych z bazy danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. Ten samouczek opiera się na poprzednim samouczku "Tworzenie projektu" i jest częścią serii samouczków o nazwie Wingtip zabawki Store. Po ukończeniu tego samouczka, dlatego zostanie utworzone grupy klas dostępu do danych, które znajdują się w *modeli* folderu projektu.

## <a name="what-youll-learn"></a>Zawartość:

- Jak tworzyć modele danych.
- Jak zainicjować i Inicjowanie bazy danych.
- Jak zaktualizować i skonfigurować aplikację do obsługi bazy danych.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Poniżej przedstawiono funkcje wprowadzone w tym samouczku:

- Entity Framework Code pierwszy
- LocalDB
- Adnotacje danych

## <a name="creating-the-data-models"></a>Tworzenie modeli danych

[Entity Framework](https://msdn.microsoft.com/data/aa937723) to struktura mapowania obiektowo relacyjny (ORM). Dzięki temu można pracować z danymi relacyjnymi jako obiekty, eliminując większość kodu dostępu do danych, który będzie zazwyczaj należy napisać. Używający narzędzia Entity Framework, możesz wykonywać zapytania za pomocą [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), a następnie pobrać i manipulowanie danymi jako silnie typizowanych obiektów. LINQ zapewnia wzorce wykonywania kwerend i aktualizowania danych. Używający narzędzia Entity Framework pozwala skupić się na tworzeniu części aplikacji, zamiast koncentrowania się na dane podstawowe informacje dotyczące dostępu. W dalszej części tej serii samouczków pokażemy sposób użycia danych do wypełnienia zapytań nawigacji i produktu.

Entity Framework obsługuje paradygmat programowania, o nazwie *Code First*. Kod pozwala najpierw zdefiniować swoje modele danych przy użyciu klas. Klasa to konstrukcja, która pozwala na tworzenie własnych typach niestandardowych przez grupowanie zmienne innych typów, metod i zdarzeń. Można mapowania klas istniejącą bazę danych lub ich używać, aby wygenerować bazę danych. W tym samouczku utworzysz modele danych przez napisanie klasy modelu danych. Następnie będzie umożliwiają Entity Framework, utworzyć bazę danych na bieżąco z tych nowych klas.

Rozpocznie się przez utworzenie klas jednostek, które definiują modeli danych dla aplikacji formularzy sieci Web. Następnie utworzysz klasy kontekstu, który zarządza klas jednostek i zapewnia dostęp do danych w bazie danych. Zostanie również utworzony klasy inicjatora, który będzie używany do wypełniania bazy danych.

### <a name="entity-framework-and-references"></a>Entity Framework i odwołań

Domyślnie program Entity Framework jest dołączana w przypadku tworzenia nowego **aplikacji sieci Web ASP.NET** przy użyciu **formularzy sieci Web** szablonu. Entity Framework można zainstalowane, odinstalowane i zaktualizowane jako pakiet NuGet.

Ten pakiet NuGet obejmuje następujące elementy **środowiska uruchomieniowego** zespoły w obrębie projektu:

- EntityFramework.dll — wszystkie wspólne środowisko uruchomieniowe kod używany przez program Entity Framework
- EntityFramework.SqlServer.dll — dostawcy programu Microsoft SQL Server dla programu Entity Framework

### <a name="entity-classes"></a>Klas jednostek

Klasy, Utwórz zdefiniować schemat danych są nazywane klasami jednostki. Jeśli jesteś nowym użytkownikiem projekt bazy danych, pomyśl o klas jednostek, jak definicje tabel bazy danych. Każda właściwość klasy określa kolumnę w tabeli bazy danych. Te klasy oferują lekki, obiektowo relacyjny interfejs między kodem zorientowane obiektowo i struktury tabeli relacyjnej bazy danych.

W tym samouczku będziesz rozpoczyna pracę przez dodanie klasy proste jednostki reprezentujące schematów i kategorii produktów. Klasa produktów będzie zawierać definicje dla każdego produktu. Nazwa każdego z elementów członkowskich klasy produktu będzie `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, i `Category`. Klasa kategorii będzie zawierać definicje dla każdej kategorii, który produkt może należeć do, takie jak samochód, łodzi lub płaszczyzny. Nazwa każdego z członków klasy kategorii będzie `CategoryID`, `CategoryName`, `Description`, i `Products`. Każdy produkt będzie należeć do jednej z kategorii. W ramach tych zajęć jednostki zostanie dodany do istniejącego projektu *modeli* folderu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**. 

    ![Tworzenie warstwy dostępu do danych — nowy element Menu](create_the_data_access_layer/_static/image1.png)

   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. W obszarze **Visual C#** z **zainstalowane** w okienku po lewej stronie, wybierz opcję **kodu**. 

    ![Tworzenie warstwy dostępu do danych — nowy element Menu](create_the_data_access_layer/_static/image2.png)
3. Wybierz **klasy** ze środkowego okienka i nazwij ta nowa klasa *Product.cs*.
4. Kliknij przycisk **Dodaj**.  
   Plik nowej klasy jest wyświetlany w edytorze.
5. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Tworzenie innej klasy, powtarzając kroki od 1 do 4, jednak nadaj nowej klasie *Category.cs* i zastąpić domyślny kod następującym kodem:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Jak wcześniej wspomniano, `Category` klasy reprezentuje typ produktu, która aplikacja jest przeznaczona do sprzedaży (takie jak <a id="a"> </a> &quot;samochodów&quot;, &quot;łodzi&quot;, &quot;Rakiet&quot;i tak dalej), a `Product` klasa reprezentuje poszczególnych produktów (zabawki) w bazie danych. Każde wystąpienie `Product` obiektu odnoszą się do wiersza w tabeli relacyjnej bazy danych, a każda właściwość klasy produktu będzie zmapowana do kolumny w tabeli relacyjnej bazy danych. W dalszej części tego samouczka można przejrzeć dane produktu znajdujące się w bazie danych.

### <a name="data-annotations"></a>Adnotacje danych

Być może zauważono, że określone składowe klasy mają atrybuty określające szczegóły dotyczące elementu członkowskiego, takie jak `[ScaffoldColumn(false)]`. Są to *adnotacje danych*. Atrybuty adnotacji danych opisujący sposób sprawdzania poprawności danych wejściowych użytkownika dla tego elementu członkowskiego, aby określić, formatowanie i określić, jak modelowane po utworzeniu bazy danych.

### <a name="context-class"></a>Context — Klasa

Aby rozpocząć korzystanie z klas dostępu do danych, należy zdefiniować klasy kontekstu. Jak już wspomniano, klasy kontekstu zarządza klas obiektów (takich jak `Product` klasy i `Category` klasy) i zapewnia dostęp do danych w bazie danych.

Ta procedura dodaje nowe klasy C# kontekstu do *modeli* folderu.

1. Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz **klasy** ze środkowego okienka, nadaj jej nazwę *ProductContext.cs* i kliknij przycisk **Dodaj**.
3. Zastąp domyślny kod zawarty w klasie, z następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Ten kod dodaje `System.Data.Entity` przestrzeni nazw tak, aby mieć dostęp do wszystkich funkcji podstawowych programu Entity Framework, co obejmuje możliwość zapytania, wstawiania, aktualizowania i usuwania danych dzięki współpracy z silnie typizowanych obiektów.

`ProductContext` Klasa reprezentuje produktu kontekst bazy danych platformy Entity Framework, która obsługuje pobieranie, przechowywania i aktualizowania `Product` klasy wystąpień w bazie danych. `ProductContext` Klasa pochodzi od `DbContext` dostarczane przez program Entity Framework klasy bazowej.

### <a name="initializer-class"></a>Klasa inicjatora

Konieczne będzie uruchomienie niestandardowej logiki biznesowej można zainicjować kontekstu jest używana podczas pierwszej bazy danych. Umożliwi to inicjatora dane mają zostać dodane do bazy danych, tak, aby natychmiast wyświetlić produktów i kategorii.

Ta procedura dodaje nowe klasy C# inicjator do *modeli* folderu.

1. Utworzyć kolejną `Class` w *modeli* folder i nadaj mu nazwę *ProductDatabaseInitializer.cs*.
2. Zastąp domyślny kod zawarty w klasie, z następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Jak widać w powyższym kodzie, jeśli baza danych jest tworzony i zainicjowany, `Seed` właściwość jest zastąpione i ustawić. Gdy `Seed` właściwość jest ustawiona, wartości kategorii i produktów są używane do wypełniania bazy danych. Jeśli użytkownik podejmie próbę aktualizacji danych inicjatora, modyfikując kod powyżej, po utworzeniu bazy danych, nie zobaczysz żadnych aktualizacji po uruchomieniu aplikacji sieci Web. Przyczyną jest powyższy kod korzysta z implementacją `DropCreateDatabaseIfModelChanges` klasy, rozpoznawał, jeśli model (schemat) został zmieniony przed zresetowaniem danych inicjatora. Jeśli żadne zmiany nie zostaną wprowadzone do `Category` i `Product` klas jednostek, baza danych nie zostanie zainicjowana ponownie z danymi inicjatora.

> [!NOTE] 
> 
> Jeśli potrzebowała bazy danych, być odtwarzane przy każdym uruchomieniu aplikacji, można użyć `DropCreateDatabaseAlways` klasy zamiast `DropCreateDatabaseIfModelChanges` klasy. Jednak w tej serii samouczków, należy użyć `DropCreateDatabaseIfModelChanges` klasy.


W tym momencie w tym samouczku trzeba będzie *modeli* folder o cztery nowe klasy i klasa jeden domyślny:

![Tworzenie warstwy dostępu do danych — folderu modeli](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurowanie aplikacji do korzystania z modelu danych

Teraz, po utworzeniu klasy, które reprezentują dane, należy skonfigurować aplikację do korzystania z klasy. W *Global.asax* pliku, możesz dodać kod, który inicjuje modelu. W *Web.config* pliku, możesz dodać informacje o tym aplikacji, co baza danych będzie używane do przechowywania danych, który jest reprezentowany przez nowe klasy danych. *Global.asax* pliku może służyć do obsługi zdarzeń aplikacji lub metody. *Web.config* plik pozwala modyfikować konfigurację aplikacji sieci web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aktualizowanie pliku Global.asax

Aby zainicjować modeli danych, podczas uruchamiania aplikacji, spowoduje zaktualizowanie `Application_Start` obsługi w *Global.asax.cs* pliku.

> [!NOTE] 
> 
> W Eksploratorze rozwiązań, użytkownik może wybrać jedną *Global.asax* pliku lub *Global.asax.cs* plik do edycji *Global.asax.cs* pliku.


1. Dodaj następujący kod wyróżniony na żółto do `Application_Start` method in Class metoda *Global.asax.cs* pliku.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Przeglądarka musi obsługiwać HTML5, aby wyświetlić kod wyróżniony na żółto, oglądając tę serię samouczków w przeglądarce.


Jak pokazano w powyższym kodzie, podczas uruchamiania aplikacji, aplikacji określa, że inicjator, które będą uruchamiane podczas pierwszego danych jest dostępny. Dwa dodatkowe przestrzenie nazw są wymagane do dostępu `Database` obiektu i `ProductDatabaseInitializer` obiektu.

 Modyfikowanie pliku Web.Config 

Mimo że Entity Framework Code First wygeneruje bazy danych dla Ciebie w lokalizacji domyślnej, gdy baza danych jest wypełniana danymi inicjatora, dodawanie własnych informacji o połączeniu z aplikacją zapewnia kontrolę nad lokalizację bazy danych. Określ tego połączenia z bazą danych przy użyciu parametrów połączenia w aplikacji *Web.config* pliku w folderze głównym projektu. Dodając nowe parametry połączenia, można kierować lokalizacji bazy danych (*wingtiptoys.mdf*) ma zostać utworzony w katalogu danych aplikacji (*aplikacji\_danych*), zamiast domyślnej Lokalizacja. Wprowadzenie tej zmiany pozwoli na znalezienie i sprawdź plik bazy danych w dalszej części tego samouczka.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *Web.config* pliku.
2. Dodaj parametry połączenia, wyróżniony na żółto do `<connectionStrings>` części *Web.config* pliku w następujący sposób:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Gdy aplikacja jest uruchamiana po raz pierwszy, zostanie utworzona baza danych w lokalizacji określonej przez ciąg połączenia. Jednak przed uruchomieniem aplikacji, utworzymy ją najpierw.

## <a name="building-the-application"></a>Kompilowanie aplikacji

Aby upewnić się, czy działają wszystkie klasy i zmian w aplikacji sieci Web, należy skompilować aplikację.

1. Z **debugowania** menu, wybierz opcję **kompilacji WingtipToys**.  
 **Dane wyjściowe** zostanie wyświetlone okno, a jeśli wszystko poszło dobrze, zobaczysz *zakończyło się pomyślnie* wiadomości.  

    ![Tworzenie warstwy dostępu do danych — Windows danych wyjściowych](create_the_data_access_layer/_static/image4.png)

Jeśli napotkasz błąd, ponownie sprawdź powyższe kroki. Informacje przedstawione w **dane wyjściowe** okna będą wskazywać plik, który ma problem, gdzie w pliku zmian jest wymagana. Te informacje pozwoli określić, jaka część powyższej procedury muszą być przejrzane i stałej w projekcie.

## <a name="summary"></a>Podsumowanie

W tym samouczku tej serii możesz mieć utworzony modelu danych, a także, dodać kod, który będzie używany do inicjowania i Inicjowanie bazy danych. Skonfigurowano również aplikacji na używanie modeli danych, gdy aplikacja jest uruchomiona.

W następnym samouczku dodasz aktualizacji interfejsu użytkownika, nawigacji i pobierać dane z bazy danych. W rezultacie zostanie automatycznie tworzona w oparciu o klas jednostek, które utworzono w ramach tego samouczka bazy danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Omówienie programu Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Przewodnik dla początkujących programu ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Pierwszy programowania za pomocą platformy Entity Framework Code](http://www.msteched.com/2010/Europe/DEV212) (wideo)   
[Kod pierwszego relacje Fluent interfejsu API](https://msdn.microsoft.com/data/hh134698)   
[Adnotacje danych na pierwszym kodu](https://msdn.microsoft.com/data/gg193958)  
[Ulepszenia w zakresie produktywności dla programu Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Poprzednie](create-the-project.md)
> [dalej](ui_and_navigation.md)
