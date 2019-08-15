Polecenie aktualizacji bazy danych generuje następujące ostrzeżenie: 

   "Nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".

Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

Schemat bazy danych jest oparty na modelu określonym w `MvcMovieContext` klasie (w pliku *Data/MvcMovieContext. cs* ). `InitialCreate` Argument jest nazwą migracji. Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.

`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.