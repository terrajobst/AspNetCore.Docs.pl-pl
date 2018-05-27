# <a name="aspnet-background-tasks-sample-generic-host"></a>Tło ASP.NET zadań próbki (rodzajowego hosta)

Ten przykład przedstawia użycie [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). W tym przykładzie pokazano funkcje opisane w [zadania związane z usług hostowanych w ASP.NET Core w tle](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) tematu.

Podczas uruchamiania próbki [Visual Studio Code](https://code.visualstudio.com/)ustaw **konsoli** wartości konfiguracji konsoli w *.vscode/launch.json* albo `externalTerminal` lub `integratedTerminal`. Użycie `internalConsole` jest niezgodny z naciśnięcie klawisza dane wejściowe konsoli, która aplikacja używa do elementów roboczych tła umieścić w kolejce.
