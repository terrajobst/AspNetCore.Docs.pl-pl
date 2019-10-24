# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz pozycję **Usługa procesu roboczego**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Wybierz pozycję **Utwórz**.
1. W oknie dialogowym **Tworzenie nowej usługi procesu roboczego** wybierz pozycję **Utwórz**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. Utwórz nowy projekt.
1. Na pasku bocznym wybierz pozycję **aplikacja** na **platformie .NET Core** .
1. Wybierz pozycję **proces roboczy** w obszarze **ASP.NET Core**. Wybierz opcję **Dalej**.
1. Wybierz pozycję **.NET Core 3,0** dla **platformy docelowej**. Wybierz opcję **Dalej**.
1. Podaj nazwę w polu **Nazwa projektu** . Wybierz pozycję **Utwórz**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń. W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorker`. Folder dla `ContosoWorker` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.

```dotnetcli
dotnet new worker -o ContosoWorker
```