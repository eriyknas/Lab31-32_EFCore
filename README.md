# Лабораторная работа №31-32. Введение в SQLite и Entity Framework Core
## Информация о студенте
**Студент:** Чешуина А.Д., Антонова П.А  
**Группа:** ИСП-231  
**Дата:** 28.04.2026
## Краткое описание работы
В данной лабораторной работе реализовано веб-API для управления задачами с использованием:
- ASP.NET Core Web API
- Entity Framework Core
- SQLite
- Swagger
## Полезные команды dotnet ef
- `dotnet ef migrations add Имя` - Создать новую миграцию
- `dotnet ef database update` - Применить миграции к БД
- `dotnet ef migrations list` - Показать список миграций
- `dotnet ef migrations remove` - Удалить последнюю миграцию
- `dotnet ef database update ИмяМиграции` - Откатиться до указанной миграции
## Список всех реализованных маршрутов
| Метод | Маршрут | Описание |
|-------|---------|----------|
| GET | /api/tasks | Получить все задачи  |
| GET | /api/tasks/{id} | Получить задачу по ID |
| GET | /api/tasks/search | Поиск задач по тексту, приоритету, статусу |
| GET | /api/tasks/stats | Статистика по задачам |
| GET | /api/tasks/paged | Пагинированный список задач |
| GET | /api/tasks/overdue | Просроченные задачи |
| POST | /api/tasks | Создать новую задачу |
| PUT | /api/tasks/{id} | Полностью обновить задачу |
| PATCH | /api/tasks/{id}/complete | Переключить статус выполнения |
| PATCH | /api/tasks/complete-all | Отметить все задачи как выполненные |
| DELETE | /api/tasks/{id} | Удалить задачу |
| DELETE | /api/tasks/completed | Удалить все выполненные задачи |
## Таблица применённых миграций
| Миграция | Описание |
|----------|----------|
| InitialCreate | Создание таблицы Tasks с начальными данными |
| AddDueDateToTask | Добавление поля DueDate (срок выполнения) |
## Сравнительная таблица LINQ vs SQL
| LINQ | SQL |
|------|-----|
| `.Where(t => t.IsCompleted == false)` | `WHERE is_completed = 0` |
| `.OrderBy(t => t.CreatedAt)` | `ORDER BY created_at ASC` |
| `.OrderByDescending(t => t.CreatedAt)` | `ORDER BY created_at DESC` |
| `.Take(10)` | `LIMIT 10` |
| `.Skip(20).Take(10)` | `OFFSET 20 LIMIT 10` |
| `.Count()` | `SELECT COUNT(*)` |
| `.Any(t => t.Priority == "High")` | `SELECT EXISTS(...)` |
| `.Max(t => t.Id)` | `SELECT MAX(id)` |
| `.GroupBy(t => t.Priority)` | `GROUP BY priority` |
## Итоговая сравнительная таблица
| Концепция | Хранение в памяти | EF Core + SQLite |
|-----------|-------------------|------------------|
| Хранение данных | `static List<T>` в RAM | Файл .db на диске |
| После перезапуска | Данные пропадают | Данные сохраняются |
| Поиск по условию | LINQ to Objects | LINQ to Entities → SQL |
| Создание структуры | Не нужно | Миграции (`dotnet ef`) |
| Начальные данные | Хардкод в коде | `HasData()` в миграции |
| Получение данных | `list.FirstOrDefault()` | `await db.Table.FindAsync(id)` |
| Добавление | `list.Add(item)` | `db.Table.Add(item)` + `SaveChangesAsync()` |
| Удаление | `list.Remove(item)` | `db.Table.Remove(item)` + `SaveChangesAsync()` |
| Масштабируемость | Ограничена RAM | Гигабайты данных |
| Транзакции | Нет | Встроены в EF Core |
## Главные выводы
1. EF Core — это переводчик между C# и SQL. Вы пишете LINQ, он пишет SQL.
2. Миграции — это система контроля версий для структуры БД. Так же, как Git для кода.
3. Code First удобнее, чем писать SQL вручную: изменил класс → создал миграцию
→ база обновлена.
4. SaveChangesAsync() — ключевой момент. До него все изменения живут только в памяти.
5. async/await при работе с БД — это не опционально, а стандарт. Блокировать поток на время ожидания БД — плохая практика.
