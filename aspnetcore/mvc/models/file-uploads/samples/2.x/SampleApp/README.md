# <a name="upload-files-sample-app"></a>Przykładowa aplikacja przekazywania plików

Ta przykładowa aplikacja pokazuje Koncepcje opisane w temacie [przekazywanie plików w ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads) tematu.

## <a name="security-considerations"></a>Zagadnienia związane z zabezpieczeniami

Należy zachować ostrożność, zapewniając użytkownikom możliwość przekazywania plików na serwer. Osoby atakujące mogą wykonywać ataki [typu "odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) , próbować przekazać wirusy lub złośliwe oprogramowanie lub próbować złamać sieci i serwery w inny sposób.

Kroki zabezpieczeń, które zmniejszają prawdopodobieństwo pomyślnego ataku:

* Przekaż pliki do dedykowanego obszaru przekazywania plików w systemie, najlepiej do dysku niesystemowego. Użycie dedykowanej lokalizacji ułatwia nakładanie środków zabezpieczeń na przekazane pliki. Wyłącz uprawnienia do wykonywania w lokalizacji przekazywania pliku.&dagger;
* Nigdy nie Utrwalaj przekazanych plików w tym samym drzewie katalogów co aplikacja.&dagger;
* Użyj bezpiecznej nazwy pliku, która jest określana przez aplikację. Nie używaj nazwy pliku dostarczonej przez dane wejściowe użytkownika lub niezaufanej nazwy pliku przekazanego pliku.&dagger;
* Zezwala tylko na określony zestaw zatwierdzonych rozszerzeń plików.&dagger;
* Sprawdź podpis formatu pliku, aby uniemożliwić użytkownikowi przekazywanie zamaskowanego pliku (na przykład przekazanie pliku *exe* z rozszerzeniem *. txt* ).&dagger;
* Sprawdź, czy na serwerze są również wykonywane sprawdzenia po stronie klienta. Sprawdzanie po stronie klienta można łatwo obejść.&dagger;
* Sprawdź rozmiar przekazywania i Zapobiegaj przeciążom, które są większe niż oczekiwano.&dagger;
* Jeśli pliki nie powinny być zastąpione przez przekazany plik o tej samej nazwie, przed przekazaniem pliku Sprawdź nazwę pliku względem bazy danych lub magazynu fizycznego.
* **Przed zapisaniem pliku Uruchom skaner wirusów/złośliwego oprogramowania dla przekazanej zawartości.**

&dagger;przykładowej aplikacji przedstawiono podejście, które spełnia kryteria.

> [!WARNING]
> Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:
>
> * Całkowicie przejęcia w systemie.
> * Przeciąż system z wynikiem awarii systemu.
> * Naruszyć bezpieczeństwo danych użytkownika lub systemu.
> * Zastosuj graffiti do publicznego interfejsu użytkownika.
>
> Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:
>
> * [Przekazywanie pliku bez ograniczeń](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Zabezpieczenia platformy Azure: Upewnij się, że odpowiednie formanty są stosowane podczas akceptowania plików od użytkowników](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

Aby uzyskać dodatkowe informacje, zobacz [przekazywanie plików w ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="how-to-use-the-sample"></a>Jak używać przykładu

W pliku *appSettings. JSON* :

1. Ustaw ścieżkę dla przechowywanych plików (`StoredFilesPath`).

   * Aplikacja Przykładowa ustawia wartość na `c:\\files`, w której znajduje się folder o nazwie *Files* w katalogu głównym dysku C:.
   * Ścieżka musi istnieć. Utwórz folder *Files* na dysku C: lub Ustaw ścieżkę do odpowiedniej lokalizacji.
   * Proces aplikacji wymaga uprawnień do odczytu/zapisu dla ścieżki.
   * **WAŻNE!** Wyłącz uprawnienia do wykonywania dla wszystkich użytkowników w ścieżce.

1. Ustaw limit rozmiaru pliku (`FileSizeLimit`) w bajtach. Domyślna wartość `2097152` aplikacji przykładowej (2 097 152 bajtów) umożliwia przekazywanie plików do 2 MB.
