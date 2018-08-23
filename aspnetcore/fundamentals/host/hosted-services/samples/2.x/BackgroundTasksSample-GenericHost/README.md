# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Próbki (Host ogólnego) zadań w tle programu ASP.NET Core

Ten przykład ilustruje użycie [pomocą interfejsu IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). W tym przykładzie przedstawiono funkcje opisane w [zadania za pomocą usług hostowanych w programie ASP.NET Core w tle](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) tematu.

Podczas uruchamiania przykładu [programu Visual Studio Code](https://code.visualstudio.com/)ustaw **konsoli** wartość konfiguracji konsoli w *.vscode/launch.json* do jednej `externalTerminal` lub `integratedTerminal`. Korzystanie z `internalConsole` jest niezgodna z danymi wejściowymi naciśnięcia klawisza konsoli przez aplikację do elementów roboczych w tle umieścić w kolejce.
