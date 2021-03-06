# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Tworzenie nowego projektu.
1. Wybierz pozycję **Usługa procesu roboczego**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Wybierz pozycję **Utwórz**.
1. W oknie dialogowym **Tworzenie nowej usługi procesu roboczego** wybierz pozycję **Utwórz**.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

1. Tworzenie nowego projektu.
1. Na pasku bocznym wybierz pozycję **aplikacja** na **platformie .NET Core** .
1. Wybierz pozycję **proces roboczy** w obszarze **ASP.NET Core**. Wybierz opcję **Dalej**.
1. Wybierz **platformę .NET Core 3,0** lub nowszą dla **platformy docelowej**. Wybierz opcję **Dalej**.
1. Podaj nazwę w polu **Nazwa projektu** . Wybierz pozycję **Utwórz**.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj szablonu usługi procesu roboczego (`worker`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń. W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorker`. Folder dla aplikacji `ContosoWorker` jest tworzony automatycznie podczas wykonywania polecenia.

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
