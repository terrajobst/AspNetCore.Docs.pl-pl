---
title: Zarządzanie odwołaniami protobuf za pomocą programu dotnet-GRPC
author: juntaoluo
description: Dowiedz się więcej na temat dodawania, aktualizowania, usuwania i wyświetlania protobuf odwołań za pomocą narzędzia dotnet-GRPC Global.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290062"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>Zarządzanie odwołaniami protobuf za pomocą programu dotnet-GRPC

Przez [Jan Luo](https://github.com/juntaoluo)

`dotnet-grpc` to globalne narzędzie platformy .NET Core do zarządzania odwołaniami protobuf w ramach projektu .NET gRPC. Narzędzie może służyć do dodawania, odświeżania, usuwania i wyświetlania listy odwołań protobuf.

## <a name="installation"></a>Instalacja

Aby zainstalować [Narzędzie globalne programu .NET Core](/dotnet/core/tools/global-tools)`dotnet-grpc`, uruchom następujące polecenie:

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>Dodaj odwołania

`dotnet-grpc` może służyć do dodawania odwołań protobuf jako elementów `<Protobuf />` do pliku *. csproj* :

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

Odwołania do protobuf są używane do generowania zasobów C# klienta i/lub serwera. @No__t-0tool może:

* Utwórz odwołanie protobuf z plików lokalnych na dysku.
* Utwórz odwołanie protobuf z pliku zdalnego określonego przez adres URL.
* Upewnij się, że odpowiednie zależności pakietu gRPC są dodawane do projektu.

Na przykład pakiet `Grpc.AspNetCore` jest dodawany do aplikacji sieci Web. `Grpc.AspNetCore` zawiera gRPC serwer i biblioteki klienta oraz narzędzia obsługi. Alternatywnie do aplikacji konsolowej są dodawane pakiety `Grpc.Net.Client`, `Grpc.Tools` i `Google.Protobuf`, które zawierają tylko biblioteki i narzędzia klienckie gRPC.

### <a name="add-file"></a>Dodaj plik

Polecenie `add-file` służy do dodawania plików lokalnych na dysku jako odwołań protobuf. Podane ścieżki plików:

* Może być względna względem bieżącego katalogu lub ścieżek bezwzględnych.
* Mogą zawierać symbole wieloznaczne dla [obsługi symboli wieloznacznych](https://wikipedia.org/wiki/Glob_(programming))plików opartych na wzorcu.

Jeśli jakiekolwiek pliki znajdują się poza katalogiem projektu, zostanie dodany element `Link` w celu wyświetlenia pliku w folderze `Protos` w programie Visual Studio.

### <a name="usage"></a>Użycie

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>Argumenty

| Argument | Opis |
|-|-|
| plikach | Odwołuje się do pliku protobuf. Mogą to być ścieżki do globalizowania dla lokalnych plików protobuf. |

#### <a name="options"></a>Opcje

| Opcja krótka | Opcja Long | Opis |
|-|-|-|
| -p | --projekt | Ścieżka do pliku projektu, na którym mają być wykonywane działania. Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.
| -s | --usługi | Typ usług gRPC, które mają zostać wygenerowane. Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web. Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.
| -i | --dodatkowe-import-katalogów | Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf. Jest to rozdzielana średnikami lista ścieżek.
| | --dostęp | Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas. Wartość domyślna to `Public`. Akceptowane wartości to `Internal` i `Public`.

### <a name="add-url"></a>Dodaj adres URL

Polecenie `add-url` służy do dodawania pliku zdalnego określonego przez źródłowy adres URL jako odwołanie protobuf. Należy podać ścieżkę pliku, aby określić, gdzie pobrać plik zdalny. Ścieżka pliku może być względna względem bieżącego katalogu lub ścieżki bezwzględnej. Jeśli ścieżka pliku znajduje się poza katalogiem projektu, zostanie dodany element `Link` w celu wyświetlenia pliku w folderze wirtualnym `Protos` w programie Visual Studio.

### <a name="usage"></a>Użycie

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>Argumenty

| Argument | Opis |
|-|-|
| url | Adres URL pliku protobuf zdalnego. |

#### <a name="options"></a>Opcje

| Opcja krótka | Opcja Long | Opis |
|-|-|-|
| -o | --output | Określa ścieżkę pobierania pliku protobuf zdalnego. Jest to wymagana opcja.
| -p | --projekt | Ścieżka do pliku projektu, na którym mają być wykonywane działania. Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.
| -s | --usługi | Typ usług gRPC, które mają zostać wygenerowane. Jeśli określono `Default`, `Both` jest używany dla projektów sieci Web i `Client` jest używany dla projektów innych niż sieci Web. Akceptowane wartości to `Both`, `Client`, `Default`, `None`, `Server`.
| -i | --dodatkowe-import-katalogów | Dodatkowe katalogi, które mają być używane podczas rozpoznawania importu dla plików protobuf. Jest to rozdzielana średnikami lista ścieżek.
| | --dostęp | Modyfikator dostępu, który ma być używany dla C# wygenerowanych klas. Wartość domyślna to `Public`. Akceptowane wartości to `Internal` i `Public`.

## <a name="remove"></a>Usuń

Polecenie `remove` służy do usuwania odwołań protobuf z pliku *csproj* . Polecenie akceptuje argumenty ścieżki i źródłowe adresy URL jako argumenty. Narzędzie:

* Usuwa odwołanie protobuf.
* Nie usuwa pliku *. proto* , nawet jeśli został pierwotnie pobrany ze zdalnego adresu URL.

### <a name="usage"></a>Użycie

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>Argumenty

| Argument | Opis |
|-|-|
| wołują | Adresy URL lub ścieżki plików odwołań protobuf do usunięcia. |

### <a name="options"></a>Opcje

| Opcja krótka | Opcja Long | Opis |
|-|-|-|
| -p | --projekt | Ścieżka do pliku projektu, na którym mają być wykonywane działania. Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.

## <a name="refresh"></a>Odśwież

Polecenie `refresh` służy do aktualizowania odwołania zdalnego z najnowszą zawartością ze źródłowego adresu URL. Aby określić odwołanie, które ma zostać zaktualizowane, można użyć zarówno ścieżki pliku pobierania, jak i źródłowego adresu URL. Uwaga:

* Skróty zawartości plików są porównywane, aby określić, czy plik lokalny powinien zostać zaktualizowany.
* Nie są porównywane żadne informacje o znaczniku czasu.

Narzędzie zawsze zastępuje plik lokalny plikiem zdalnym, jeśli jest wymagana aktualizacja.

### <a name="usage"></a>Użycie

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>Argumenty

| Argument | Opis |
|-|-|
| wołują | Adresy URL lub ścieżki plików do zdalnych odwołań protobuf, które powinny zostać zaktualizowane. Pozostaw ten argument pusty, aby odświeżyć wszystkie odwołania zdalne. |

### <a name="options"></a>Opcje

| Opcja krótka | Opcja Long | Opis |
|-|-|-|
| -p | --projekt | Ścieżka do pliku projektu, na którym mają być wykonywane działania. Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.
| | --osuszyć-Run | Wyświetla listę plików, które zostaną zaktualizowane bez pobierania nowej zawartości.

## <a name="list"></a>Lista

Polecenie `list` służy do wyświetlania wszystkich odwołań protobuf w pliku projektu. Jeśli wszystkie wartości w kolumnie są wartościami domyślnymi, kolumna może zostać pominięta.

### <a name="usage"></a>Użycie

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>Opcje

| Opcja krótka | Opcja Long | Opis |
|-|-|-|
| -p | --projekt | Ścieżka do pliku projektu, na którym mają być wykonywane działania. Jeśli plik nie zostanie określony, polecenie przeszukuje bieżący katalog.

## <a name="additional-resources"></a>Zasoby dodatkowe

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
