---
ms.openlocfilehash: 1e848a6779a0c68754215ead35d27df5b371f8ed
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265382"
---
<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e4451-101">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e4451-101">Test the app</span></span>

* <span data-ttu-id="e4451-102">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e4451-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e4451-103">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="e4451-103">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e4451-105">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="e4451-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e4451-106">Jeśli zostanie wyświetlony następujący błąd, sprawdź masz Uruchom migracje i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="e4451-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
