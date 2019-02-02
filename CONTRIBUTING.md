# <a name="contribute-to-the-aspnet-documentation"></a>Współtworzenie dokumentacji platformy ASP.NET

W tym dokumencie opisano proces współtworzenia artykułów i przykłady kodu, które są hostowane na [witrynie dokumentacji platformy ASP.NET](https://docs.microsoft.com/aspnet/). Błąd pisowni poprawki i nowe artykuły są powitalnej wkładów.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>Jak utworzyć proste korekcji lub sugestię

Artykuły są przechowywane w repozytorium jako pliki Markdown. Proste do zawartości pliku Markdown zmian w przeglądarce, wybierając **Edytuj** linku w prawym górnym rogu okna przeglądarki. (W oknie wąskie okno przeglądarki, rozwiń węzeł **opcje** paska, aby zobaczyć **Edytuj** łącza.) Postępuj zgodnie z instrukcjami, aby utworzyć żądanie ściągnięcia (PR). Firma Microsoft będzie recenzję żądania Ściągnięcia i zaakceptuj je lub zasugerować zmiany.

## <a name="how-to-make-a-more-complex-submission"></a>Jak tworzyć bardziej złożone przesyłania

Potrzebujesz podstawową wiedzę na temat [Git i GitHub.com](https://guides.github.com/activities/hello-world/).

* Otwórz [problem](https://github.com/aspnet/Docs/issues/new) opisujący, co chcesz zrobić, takich jak zmiana istniejącego artykułu lub utworzeniem nowej. Prosimy często konspekt dla nowej sugestii tematu. Oczekiwania na zatwierdzenie przez zespół inwestować dużo czasu.
* Rozwidlenia [aspnet/Docs](https://github.com/aspnet/Docs/) repozytorium i utwórz gałąź dla Twoich zmian.
* Prześlij żądanie Ściągnięcia do gałęzi głównej, z uwzględnieniem zmienionych uprawnień.
* Jeśli żądanie Ściągnięcia ma etykietę "wymagana przez cla" przypisane, [ukończenia wkładu umowę licencji (CLA)](https://cla.dotnetfoundation.org/).
* Odpowiadanie na żądania Ściągnięcia, opinie.

Aby uzyskać przykład, gdzie ten proces doprowadził do publikacji nowego artykułu, zobacz [problem &num;67](https://github.com/dotnet/docs/issues/67) i [żądania ściągnięcia &num;798](https://github.com/dotnet/docs/pull/798) w repozytorium dokumentacji platformy .NET. Nowy artykuł jest [dokumentowanie kodu](https://docs.microsoft.com/dotnet/articles/csharp/codedoc).

## <a name="docs-authoring-pack-extension-in-visual-studio-code"></a>Docs Authoring Pack rozszerzenia programu Visual Studio Code 

Jeśli używasz programu Visual Studio Code na potrzeby współtworzenia dokumentacji platformy ASP.NET, możesz zwiększyć produktywność, instalując [Docs Authoring Pack](https://marketplace.visualstudio.com/items?itemName=docsmsft.docs-authoring-pack) rozszerzenia. Rozszerzenie udostępnia szeroką gamą narzędzi, który pomaga w języku znaczników Markdown Zaznaczanie błędów, sprawdzanie pisowni kodu i szablony artykułów.

## <a name="markdown-syntax"></a>Składnia języka markdown

Artykuły są pisane w [DocFx składni języka Markdown](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), który jest nadzbiorem [Markdown połączonego z usługą GitHub (GFM)](https://guides.github.com/features/mastering-markdown/). Przykłady dla funkcji interfejsu użytkownika często używane w dokumentacji platformy ASP.NET przy użyciu składni DFM, zobacz [metadanych i szablonu języka znaczników Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) w przewodniku stylistycznym repozytorium dokumentacji platformy .NET. 

## <a name="folder-structure-conventions"></a>Konwencje struktury folderów

Dla każdego pliku Markdown może istnieć folder obrazów i folder do przykładowego kodu. Jeśli artykuł jest [fundamentals/configuration/index.md](https://github.com/aspnet/Docs/blob/master/aspnetcore/fundamentals/configuration/index.md), obrazy znajdują się w [podstawy/configuration/index/\_statyczne](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/_static) i przykładowych plików projektu aplikacji znajdują się w [ podstawy/configuration / / przykładowy indeks](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample). Obraz w *fundamentals/configuration/index.md* pliku jest renderowany przez następujący kod Markdown:

```
![description of image for alt attribute](configuration/index/_static/imagename.png)
```

Wszystkie obrazy powinny mieć [tekst alternatywny (alt)](https://wikipedia.org/wiki/Alt_attribute). Porady dotyczące określania tekst alternatywny, znaleźć zasoby online, takich jak [WebAIM: Tekst alternatywny](https://webaim.org/techniques/alttext/).

Na użytek małe nazw plików języka Markdown i nazwy plików obrazów.

## <a name="internal-links"></a>Łączy wewnętrznych

Skorzystaj z łączy wewnętrznych `uid` artykułu docelowej z łączem xref (tekst łącza jest ustawiona na tytuł połączonej zawartości):

```
<xref:uid_of_the_topic>
```

Tytuł artykułu nie nadaje się do tekstu łącza (na przykład wyraz lub frazę w zdaniu jest tekst łącza), z następującymi określić link odsyłaczy i tekst łącza:

```
[link text](xref:uid_of_the_topic)
```

Aby uzyskać więcej informacji, zobacz [odsyłacz DocFX](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#cross-reference).

## <a name="images-and-screenshots"></a>Obrazów oraz zrzuty ekranu

Nie dołączaj obrazów z artykułami, z wyjątkiem:

* W samouczkach podstawowe dołączania (dla początkujących).
* Gdy obraz jest potrzebny do przejrzystości.

Te ograniczenia zmniejszyć rozmiar repozytorium.

Jako opcjonalny krok upewnij się, że wszystkie obrazy i zrzuty ekranu użyte w dokumentacji są kompresowane, co ułatwia spełnienie wydajność ładowania strona i rozmiaru pliku. Kilka popularnych narzędzi obejmują TinyPNG (przy użyciu [TinyPNG witryny sieci Web](https://tinypng.com/) lub [TinyPNG API](https://tinypng.com/developers)) lub [Image Optimizer](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.ImageOptimizer) rozszerzenia programu Visual Studio. 

## <a name="code-snippets"></a>Fragmenty kodu

Artykuły często zawierają fragmenty kodu, aby zilustrować punktów. Język DFM umożliwia skopiuj kod do pliku Markdown, lub można znaleźć w osobnym pliku kodu. Firma Microsoft wolą używać plików osobnego kodu zawsze, gdy jest to możliwe zminimalizować ryzyko wystąpienia błędów w kodzie. Pliki kodu są przechowywane w repozytorium, przy użyciu struktury folderów opisanej wcześniej dla przykładowych projektów. 

Poniższe przykłady ilustrują [składni fragment kodu języka DFM](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet) do użycia w *configuration/index.md* pliku.

Do renderowania całego pliku z kodem jako fragment kodu:

```
[!code-csharp[](configuration/index/sample/Program.cs)]
```

Aby renderować część pliku jako fragment, używając numerów wierszy:

```
[!code-csharp[](configuration/index/sample/Program.cs?range=1-10,20,30,40-50]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=1-10,20,30,40-50]
```

Aby uzyskać C# fragmenty kodu, dokumentacja [ C# region](https://docs.microsoft.com/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region). Jeśli to możliwe, należy użyć regionów, a nie numery wierszy ponieważ numery wierszy w pliku kodu mogą zmienić i odwołania do numerów wierszy w języku znaczników Markdown zsynchronizowany. C#regiony mogą być zagnieżdżone. Jeśli odwołanie do regionu zewnętrzny, wewnętrzny `#region` i `#endregion` dyrektywy nie są renderowane przy użyciu fragmentu kodu. 

Aby renderować C# regionu o nazwie "snippet_Example":

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example)]
```

Aby wyróżnić wybrane wiersze w renderowanym fragmentu kodu (zazwyczaj renderowany jako kolor tła żółty):

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
[!code-csharp[](configuration/index/sample/Program.cs?range=10-20&highlight=1-3]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=10-20&highlight=1-3]
[!code-javascript[](configuration/index/sample/UsingOptionsSample.csproj?range=10-20&highlight=1-3]
```

## <a name="test-changes-with-docfx"></a>Testowanie zmian przy użyciu rozszerzenia DocFX

Przetestować zmiany z [narzędzia wiersza polecenia DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), co powoduje utworzenie lokalnie obsługiwanych wersji witryny. DocFX nie renderuje rozszerzeń stylu i lokacji utworzone w witrynie docs.microsoft.com.

Wymaga DocFX:

* .NET framework na Windows.
* Środowiska mono dla systemu Linux lub macOS. 

### <a name="windows-instructions"></a>Instrukcje Windows

* Pobierz i Rozpakuj *docfx.zip* z [zwalnia DocFX](https://github.com/dotnet/docfx/releases).
* DocFX należy dodać do ścieżki.
* W powłoce poleceń, przejdź do folderu, który zawiera *docfx.json* pliku (*aspnet* zawartości platformy ASP.NET lub *aspnetcore* zawartości platformy ASP.NET Core) i uruchom następujące polecenie:

  ```console
  docfx --serve
  ```
* W przeglądarce przejdź do `http://localhost:8080/group1-dest/`.

### <a name="mono-instructions"></a>Instrukcje platformy mono

* Zainstaluj platformę Mono za pośrednictwem Homebrew:

  ```console
  brew install mono
  ```
* Pobierz [najnowszą wersję DocFX](https://github.com/dotnet/docfx/releases).
* Wyodrębnij archiwum do *$ głównej/bin/docfx*.
* Tworzenie aliasów dla pary **docfx** w powłoce bash. Pierwszy alias jest używany do tworzenia w dokumentacji. Drugi aliasu jest używane do tworzenia i obsługi dokumentacji.

  ```console
  alias docfx='mono $HOME/bin/docfx/docfx.exe'
  alias docfx-serve='mono $HOME/bin/docfx/docfx.exe --serve'
  ```
* W powłoce poleceń, przejdź do folderu, który zawiera *docfx.json* pliku (*aspnet* zawartości platformy ASP.NET lub *aspnetcore* zawartości platformy ASP.NET Core) i uruchom następujące polecenie, aby tworzyć i obsługiwać dokumenty za pomocą jego aliasu:

  ```console
  docfx-serve
  ```
* W przeglądarce przejdź do `http://localhost:8080/group1-dest/`.

## <a name="voice-and-tone"></a>Głos i ton

Naszym celem jest pisanie dokumentacji, która jest łatwe do zrozumienia przez najszerszych możliwych odbiorców. W tym celu możemy ustanowić wytyczne dotyczące stylu, które poprosimy postępuj zgodnie z naszym współautorzy pisania. Aby uzyskać więcej informacji, zobacz [wytyczne dotyczące połączeń głosowych i ton](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) w repozytorium .NET.

## <a name="microsoft-writing-style-guide"></a>Przewodnik po stylu pisania firmy Microsoft

[Przewodnik po stylu pisania firmy Microsoft](https://docs.microsoft.com/style-guide/welcome/) umożliwia pisanie styl i terminologii wskazówki dotyczące wszystkich rodzajów komunikacji technologii, w tym dokumentacji platformy ASP.NET Core.

## <a name="redirects"></a>Przekierowuje

Jeśli usuwać artykuły, zmień jej nazwę pliku lub przenieść je do innego folderu, utworzyć przekierowanie, aby nie otrzymywać osób, które zakładek artykuł *404 Nie znaleziono* błędu. Dodanie przekierowań do [głównego pliku przekierowania](https://github.com/aspnet/Docs/blob/master/.openpublishing.redirection.json).
