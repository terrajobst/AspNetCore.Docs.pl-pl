# <a name="aspnet-core-health-check-sample"></a>Przykładowe sprawdzania kondycji platformy ASP.NET Core

Ten przykład ilustruje użycie kondycji Sprawdź oprogramowanie pośredniczące i usługi. W tym przykładzie przedstawiono scenariusz opisany w [kontroli kondycji w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks) tematu.

Aby uruchomić przykładową aplikację dla scenariusza opisanego w temacie, należy użyć [dotnet, uruchom](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) polecenia z folderu projektu w powłoce poleceń. Przekaż przełącznika scenariusza, w którym Eksplorowanie. Domyślnie aplikacja `basic` konfiguracji przełącznika nie jest udostępniane na `dotnet run`.

| Scenariusz                                               | Polecenie aplikacji przykładowej               | Opis |
| ------------------------------------------------------ | -------------------------------- | ----------- |
| Sonda kondycji podstawowe (ustawienie domyślne)                           | `dotnet run --scenario basic`    | Potwierdza, że aplikacja może przetwarzać żądania HTTP. |
| Sondowanie bazy danych                                         | `dotnet run --scenario db`       | Sprawdza, czy połączenie z bazą danych programu SQL Server. Zobacz [sondowanie bazy danych](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#database-probe) sekcji tego tematu, aby uzyskać instrukcje. |
| Sondy żywotności/gotowości                              | `dotnet run --scenario liveness` | Sprawdza stan aplikacji na żywo (*żywotności*) i aplikacji, przygotowywanie do stają się na żywo (*gotowości*). |
| Funkcję badania opartą na metryce (pamięci) /<br>Moduł zapisujący niestandardową odpowiedź | `dotnet run --scenario writer`   | Sprawdza, czy przed użyciem pamięci, a następnie zapisuje się niestandardowy JSON po zaznaczeniu tej opcji punkt końcowy kondycji. |
| Filtruj według portów                                         | `dotnet run --scenario port`     | Służy do przefiltrowania kontrole kondycji do danego portu. Zobacz [filtru według portów](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#filter-by-port) sekcji tego tematu, aby uzyskać instrukcje. |

Scenariusze filtrowania badania i port bazy danych wymagają dodatkowej konfiguracji. Zobacz [kontroli kondycji](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks) tematu, aby uzyskać szczegółowe informacje.
