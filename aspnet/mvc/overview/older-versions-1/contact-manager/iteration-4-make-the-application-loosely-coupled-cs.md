---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iteracja #4 — zastosować luźno (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Dla...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 33221c6c3326c7034fe013f152579828e2fc8a3a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873982"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iteracja #4 — zastosować luźno (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji platformy ASP.NET MVC kontaktu administracyjnego (C#)

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.

## <a name="this-iteration"></a>Tej iteracji

W czwartym iteracji aplikacji skontaktuj się z Menedżera możemy Refaktoryzuj aplikacji, aby wprowadzić więcej luźno powiązane z aplikacji. Gdy aplikacja jest luźno, można zmodyfikować kod w jednej części aplikacji bez konieczności modyfikowania kodu w innych częściach aplikacji. Luźno powiązanych aplikacji są bardziej odporne na zmiany.

Obecnie wszystkie logika dostępu i sprawdzanie poprawności danych używany przez aplikację kontaktów Menedżerze znajduje się w klasy kontrolera. Dobrym pomysłem jest. Zawsze, gdy należy zmodyfikować jedną część aplikacji, istnieje ryzyko wprowadzenia usterki w innej części aplikacji. Jeśli zmodyfikujesz logiki sprawdzania poprawności, istnieje ryzyko na przykład wprowadzenie nowych usterek w danych logiki dostępu lub kontrolera.

> [!NOTE] 
> 
> (SRP), klasę nigdy nie powinien mieć kilka przyczyn, aby zmienić. Mieszanie kontroler, weryfikacji i logiki bazy danych to naruszenie ogromną jednej zasady odpowiedzialności.


Istnieje kilka przyczyn, które może być konieczne zmodyfikowanie aplikacji. Może być konieczne dodanie nowej funkcji do aplikacji, konieczne może być naprawa błędów w aplikacji lub może być konieczne zmodyfikować implementowania funkcji aplikacji. Aplikacje są rzadko statyczne. Charakteryzują się wzrostu i zmodyfikować wraz z upływem czasu.

Załóżmy, że zdecydujesz się zmienić sposobu implementacji warstwą dostępu do danych. Prawo teraz, skontaktuj się z Menedżera aplikacja używa programu Entity Framework firmy Microsoft dostęp do bazy danych. Jednak można zdecydować przeprowadzić migrację do technologii dostępu do nowych lub alternatywne danych, takich jak usług danych ADO.NET lub NHibernate. Kod dostępu do danych nie jest odizolowana od kodu sprawdzania poprawności i kontrolera, istnieje jednak nie można zmodyfikować kod dostępu do danych w aplikacji bez konieczności modyfikacji innego kodu, który nie jest bezpośrednio powiązana z dostępu do danych.

Gdy aplikacja jest luźno, z drugiej strony, należy wprowadzić zmiany do części aplikacji bez użycia innych części aplikacji. Na przykład można przełączać technologii dostępu do danych bez modyfikowania logiki sprawdzania poprawności lub kontrolera.

W tym iteracji możemy korzystać z kilku wzorce projektowe oprogramowania, które umożliwiają Refaktoryzuj naszą aplikację kontaktów Menedżerze do bardziej luźno powiązanych aplikacji. Gdy firma Microsoft gotowe won t Menedżera skontaktuj się z tym niczego go t są przed. Jednak firma Microsoft będzie można zmienić aplikację łatwiej w przyszłości.

> [!NOTE] 
> 
> Refaktoryzacja jest procesem dostosowywania aplikacji w taki sposób, że nie utracić wszelkie istniejące funkcje.


## <a name="using-the-repository-software-design-pattern"></a>Przy użyciu wzorca projektowego oprogramowania repozytorium

Nasze pierwsza zmiana jest przeprowadzać wzorca projektowego oprogramowania o nazwie wzorca repozytorium. Użyjemy wzorca repozytorium do izolowania naszego kodu dostępu do danych od pozostałej części naszej aplikacji.

Implementacja wzorca repozytorium musimy wykonać następujące dwa kroki:

1. Tworzenie interfejsu
2. Utwórz konkretną klasę, która implementuje interfejs

Najpierw należy utworzyć interfejs, który opisano wszystkie metody dostępu do danych, które należy wykonać. Interfejs IContactManagerRepository znajduje się w 1 wyświetlania. Ten interfejs opisano metody pięć: CreateContact(), DeleteContact() EditContact(), GetContact i ListContacts().

**Wyświetlanie listy 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Następnie należy utworzyć konkretną klasę, która implementuje interfejs IContactManagerRepository. Ponieważ używamy programu Entity Framework firmy Microsoft dostęp do bazy danych, utworzymy nową klasę o nazwie EntityContactManagerRepository. Ta klasa znajduje się lista 2.

**Wyświetlanie listy 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Zwróć uwagę, że klasa EntityContactManagerRepository implementuje interfejs IContactManagerRepository. Klasa implementuje wszystkich pięciu z metod opisanych w tym interfejsie.

Być może zastanawiasz się, dlaczego musimy odblokowane przy użyciu interfejsu. Dlaczego musimy tworzyć interfejs oraz klasy, która implementuje go?

Z jednym wyjątkiem w pozostałej części aplikacji będzie współpracować z interfejsu oraz klasy konkretnej. Zamiast wywoływania metody ujawnione przez klasę EntityContactManagerRepository, zadzwonimy metody ujawnione przez interfejs IContactManagerRepository.

Dzięki temu firma Microsoft implementować interfejs z nową klasę bez konieczności modyfikowania w pozostałej części naszej aplikacji. Na przykład w przyszłości, firma Microsoft może być do zaimplementowania klasy DataServicesContactManagerRepository, który implementuje interfejs IContactManagerRepository. Klasa DataServicesContactManagerRepository może umożliwia dostęp do bazy danych zamiast Microsoft Entity Framework ADO.NET Data Services.

Jeśli zaplanowano naszego kodu aplikacji interfejsu IContactManagerRepository zamiast klasy konkretnej EntityContactManagerRepository następnie możemy przełączyć konkretnych klas bez modyfikowania pozostałe naszego kodu. Na przykład firma Microsoft może przełączyć się z klasy EntityContactManagerRepository do klasy DataServicesContactManagerRepository bez modyfikowania logiki weryfikacji lub dostępu do danych.

Programowanie dla interfejsów (obiektów abstrakcyjnych) zamiast klas konkretnych sprawia, że naszej aplikacji bardziej odporne na zmiany.

> [!NOTE] 
> 
> Interfejs z konkretną klasę w programie Visual Studio może szybko utworzyć przez wybranie opcji menu refaktoryzacji, Wyodrębnij interfejsu. Na przykład można najpierw Utwórz klasę EntityContactManagerRepository, a następnie użyć wyodrębniania interfejsu do generowania interfejsu IContactManagerRepository automatycznie.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Przy użyciu wzorca projektowego oprogramowania iniekcji zależności

Teraz, możemy zostały zmigrowane naszego kodu dostępu do danych do oddzielnego klasy repozytorium, należy zmodyfikować skontaktuj się z kontrolera do użycia tej klasy. Firma Microsoft będzie korzystać z oprogramowania wzorca projektowego o nazwie iniekcji zależności do użycia klasy repozytorium w kontrolera.

Zmodyfikowane kontrolera kontaktu znajduje się w 3 wyświetlania.

**Wyświetlanie listy 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Należy zauważyć, że kontroler skontaktuj się z 3 wyświetlania ma dwa konstruktory. Pierwszy konstruktora przekazuje konkretne wystąpienie interfejsu IContactManagerRepository drugi Konstruktor. Używa klasy kontrolera skontaktuj się z *iniekcji zależności konstruktora*.

Jeden i tylko miejsce służy klasa EntityContactManagerRepository znajduje się w pierwszym konstruktora. W pozostałej części klasa korzysta z interfejsu IContactManagerRepository zamiast klasy konkretnej EntityContactManagerRepository.

Dzięki temu można łatwo zmienić implementacji klasy IContactManagerRepository w przyszłości. Jeśli chcesz użyć klasy DataServicesContactRepository zamiast klasy EntityContactManagerRepository modyfikować tylko pierwszy konstruktora.

Iniekcji zależności konstruktora powoduje klasy kontrolera skontaktuj się z bardzo testować. Testy jednostkowe można utworzyć wystąpienie kontrolera kontaktu przez przekazanie implementację testową klasy IContactManagerRepository. Ta funkcja iniekcji zależności jest bardzo ważne dla nas w następnej iteracji, jeśli mamy utworzyć testów jednostkowych dla aplikacji, skontaktuj się z Menedżera.

> [!NOTE] 
> 
> Jeśli chcesz całkowicie oddziel klasy kontrolera kontakt z określonej implementacji interfejsu IContactManagerRepository następnie można wykorzystasz platforma, która obsługuje iniekcji zależności, takich jak StructureMap lub firmy Microsoft Entity Framework (MEF). Dzięki wykorzystaniu framework iniekcji zależności, nigdy nie należy do odwoływania się do klasy konkretnej w kodzie.


## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

Zwróć uwagę, że logiki sprawdzania poprawności jest nadal zamienione z naszych logiką kontrolera klasy kontrolera zmodyfikowane w 3 wyświetlania. Z tego samego powodu jest dobrym pomysłem jest odizolowanie logiki dostępu do danych jest dobrym pomysłem jest odizolowanie logiki sprawdzania poprawności.

Aby rozwiązać ten problem, można utworzyć oddzielne [ *warstwy usług*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Warstwy usług jest oddzielne warstwy, który firma Microsoft może wstawić między naszych kontrolera i klasy repozytorium. Warstwy usługi zawiera logiki biznesowej wraz ze wszystkimi logiki sprawdzania poprawności.

ContactManagerService znajduje się w listę 4. Zawiera logikę weryfikacji z klasy controller kontaktu.

**Wyświetlanie listy 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Zwróć uwagę, że Konstruktor ContactManagerService wymaga ValidationDictionary. Warstwy usług komunikuje się z warstwą kontrolera za pośrednictwem tego ValidationDictionary. Omówiono ValidationDictionary szczegółowo w następnej sekcji, gdy omówimy wzorzec Dekoratora.

Zwróć uwagę, ponadto, że ContactManagerService implementuje interfejs IContactManagerService. Należy zawsze starań, aby program względem interfejsy zamiast konkretnych klas. Inne klasy w aplikacji kontaktów Menedżerze nie wchodzą w interakcje z klasy ContactManagerService bezpośrednio. Zamiast tego z jednym wyjątkiem w pozostałej części aplikacji skontaktuj się z Menedżera zaplanowano interfejsu IContactManagerService.

Interfejs IContactManagerService znajduje się w 5 wyświetlania.

**Wyświetlanie listy 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Zmodyfikowane klasy kontrolera kontaktu znajduje się w 6 wyświetlania. Należy zauważyć, że kontroler skontaktuj się z już współdziała z repozytorium ContactManager. Zamiast tego kontrolera skontaktuj się z wchodzi w interakcję z usługą ContactManager. Każda warstwa jest izolowana możliwie często z innych warstw.

**Wyświetlanie listy 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Naszej aplikacji już nie uruchamia afoul o jednej zasady odpowiedzialność (SRP). Kontroler kontaktu w 6 wyświetlania została usunięta co odpowiedzialności niż sterowanie przepływem wykonywania aplikacji. Całą logikę weryfikacji została usunięta z kontrolera kontaktu i przypisany do warstwy usług. Wszystkie logiki bazy danych ma został przeniesiony do warstwy repozytorium.

## <a name="using-the-decorator-pattern"></a>Przy użyciu wzorca Dekoratora

Chcemy można było całkowicie oddziel naszych warstwy usług z naszych warstwy kontrolera. Zasadniczo możemy należy skompilować naszych warstwy usług w osobny zestaw z naszych warstwy kontrolera bez konieczności Dodaj odwołanie do naszej aplikacji MVC.

Jednak naszych warstwy usług musi przekazywać komunikaty o błędach weryfikacji do warstwy kontrolera. Jak można włączyć warstwy usług do komunikowania się komunikaty o błędach weryfikacji bez sprzężenia kontrolera i warstwy usług? Firma Microsoft może korzystać z wzorca projektowego oprogramowania o nazwie [wzorzec Dekoratora](http://en.wikipedia.org/wiki/Decorator_pattern).

Kontroler używa ModelStateDictionary, o nazwie ModelState do reprezentowania błędy sprawdzania poprawności. W związku z tym może się wydawać do przekazania ModelState z kontrolera warstwy do warstwy usług. Jednak w warstwie usługi za pomocą ModelState spowodowałoby, warstwą usługi zależne od funkcji platformy ASP.NET MVC. Powinien to być nieprawidłowy, ponieważ kiedyś, warto użyć warstwy usług z aplikacji WPF zamiast aplikacji platformy ASP.NET MVC. W takim przypadku wouldn t ma dotyczyć odwołanie platformę ASP.NET MVC do użycia klasy ModelStateDictionary.

Wzorzec Dekoratora umożliwia zawijanie istniejącej klasy w nową klasę, aby mogła implementować interfejs. Nasze projektu Manager skontaktuj się z zawiera klasę ModelStateWrapper zawartych w wyświetlania 7. Klasa ModelStateWrapper implementuje interfejs wyświetlania 8.

**Wyświetlanie listy 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Wyświetlanie listy 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Jeśli Zamknij Spójrz na listę 5 następnie zobaczysz warstwy usług ContactManager wyłącznie używa interfejsu IValidationDictionary. Usługa ContactManager nie jest zależna od klasy ModelStateDictionary. Gdy kontroler kontaktu tworzy usługi ContactManager, kontrolera opakowuje jego ModelState następująco:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Podsumowanie

W tym iteracji firma Microsoft nie dodano żadnych nowych funkcji do Menedżera skontaktuj się z aplikacji. Celem tej iteracji było Refaktoryzuj wtedy ułatwiają obsługiwanie i modyfikowanie aplikacji menedżera kontaktu.

Po pierwsze zaimplementowano wzorzec projektowy oprogramowania repozytorium. Firma Microsoft cały kod dostępu do danych migracji do osobnej klasy ContactManager repozytorium.

Możemy również samodzielnie logiki sprawdzania poprawności z logiki kontrolera. Utworzyliśmy oddzielne usługi warstwy, która zawiera wszystkie naszego kodu sprawdzania poprawności. Warstwa kontrolera współpracuje z warstwy usług i warstwy usług wchodzi w interakcję z warstwą repozytorium.

Tworząc warstwy usług, wybraliśmy zaletą wzorca Dekoratora odizolowanie ModelState od warstwy naszych usług. W naszym warstwy usług firma Microsoft zaprogramowane interfejsu IValidationDictionary zamiast ModelState.

Na koniec Wybraliśmy zaletą wzorca projektowego oprogramowania o nazwie wzorca iniekcji zależności. Ten wzorzec umożliwia nam program względem interfejsów (abstrakcje) zamiast konkretnych klas. Implementowanie wzorca projektowego iniekcji zależności powoduje naszego kodu więcej testować. W następnej iteracji dodania do projektu naszych testów jednostkowych.

> [!div class="step-by-step"]
> [Poprzednie](iteration-3-add-form-validation-cs.md)
> [dalej](iteration-5-create-unit-tests-cs.md)
