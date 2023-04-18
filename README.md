# Чек-лист по ПЗ Витомского
Создано для работы с MySQL базы данных
## Этапы выполения
- [ ] Подготовка оборудования
    - [ ] Скачать Аmpps

      После установки будет доступен следующий интерфейс

      ![img_1.png](img_1.png)
    - [ ] Скачать Microsoft Visual Studio
        - [ ] При установке выбрать

      ![img.png](img.png)
- [ ] Подготовка базы данный
    - [ ] Войти в phpMyAdmin
        - [ ] В интерфейсе ampps нажать на домик, который откроет сайт
        - [ ] На сайте нажать на кнопку phpMyAdmin
        - [ ] Войти в систему по логину `root` и паролю `mysql`
    - [ ] Создать базу данных с любым названием. Например, `mzd`.
      Можно создать следующими способами:
      - Через интерфейс
        - [ ] Слева нажать на кнопу `Создать БД`
        - [ ] В поле `Имя базы данных` ввести название базы данных 
      - Через SQL-запрос: `CREATE DATABASE ``mzd``;`
    - [ ] Создать три таблицы, связанные между собой.
      Пример таблиц
      - Таблица `users` - таблица сохраненных лиц в базе данный,
        состоящая из стоблцов: `user_id` - индефикатор пользователя в базе (`PRIMARY KEY`),
        `firstname` - имя пользователя,
        `surname` - фамилия пользователя.
      - Таблица `phones` - таблица сохраненных номеров,
        которыми владеют пользователи из таблицы `users`.
        Состоит из столбцов: `phone_id` (`PRIMARY KEY`), `phone_number`, `user`.
      - Таблица `social_networks` - таблица социальных сетей.
        Состоит из `social_network_id` (`PRIMARY KEY`), `name`, `url`.
      - Таблица `rigistrations` хранит в себе регистрации пользователей во всех соц. сетях.
      Столбцы: `rigistration_id` (PK), `socical_network`, `phone`, `username`
      
      Можно создать таблицы данных следующими способами:
      - Через интерфейс:
        - [ ] Слева выбрать базу данных
        - [ ] В поле `Имя таблицы` ввести название и брыть необходимое количество стобов.
        - [ ] Заполнить столбы `Имя` и `Тип`. 
        Для создания PK необходимо нажать на чекбокс под `A_I`
        - [ ] Если необходимо создать зависимость
          - [ ] Зайти в структуру таблицы, перейти в вкладку `Связи`
          - [ ] В поле столбец выбрать столбец,
          который ссылается к таблица
          - [ ] В поле таблица выбрать ту таблицу,
          на которую будет ссылаться столбец
          - [ ] Выбрать PK той таблицы
      - SQL запросом:
      ```
      CREATE TABLE `mzd`.`users` 
      ( `user_id` INT NOT NULL AUTO_INCREMENT , `firstname` TEXT NOT NULL , 
      `surname` TEXT NOT NULL, PRIMARY KEY (`user_id`)
      ) ENGINE = InnoDB;
      CREATE TABLE `mzd`.`phones` 
      ( `phone_id` INT NOT NULL AUTO_INCREMENT,
        `user` INT NOT NULL,
      `phone` TEXT NOT NULL, PRIMARY KEY (`phone_id`)
      ) ENGINE = InnoDB;
      ALTER TABLE `phones` ADD FOREIGN KEY (`user`) REFERENCES `users`(`user_id`) 
      ON DELETE NO ACTION ON UPDATE NO ACTION;
      CREATE TABLE `mzd`.`social_networks` 
      ( `social_network_id` INT NOT NULL AUTO_INCREMENT,
        `name` TEXT NOT NULL, `url` TEXT NOT NULL,
         PRIMARY KEY (`social_network_id`)
      ) ENGINE = InnoDB;
      ```
- [ ] Заполнить базу данных. 
Заполнять в порядке: `users`, `phones`, `social_networks`, `rigistrations`
- [ ] Создание тонкого клиента
  - [ ] Запустить Visual Studio
  - [ ] Нажать Создание проекта
  - [ ] Выбрать `Консольное приложение (.NET Framework)`
  - [ ] В `Обозреватель решений` найти зависимости.
  Нажать правой кнопкой на Ссылки/`Зависимости` 
  и выбрать в списке `Управление пакетами NuGet`
  - [ ] Переходим в Обзор и вбиваем в поиск `Mysql`
  - [ ] Выбираем `Mysql.Data` от Oracle и нажимаем `Добавить пакеты` / `Установить`
  - [ ] Открываем `Program.cs`, который сейчас выглядит примерно так
    ```cs
    using System;
    using System.Data;
    
    namespace SqlExample
    {
       public class Program
       {
           static void Main()
           {
                Console.WriteLine("Hello, World!");
           }
       }
    }
    ```
    - [ ] В начале файла добавить `using MySql.Data;`
      - [ ] В функции `main` добавить строки:
        - [ ] Параметры подключения:
        ```cs
        string server   = "localhost"; // Адресс сервера
        string database = "mzd";       // Используемая база данных
        string user     = "root";      // Логин
        string password = "mysql";     // Пароль
        string connectionString = $"server={server};user={user};database={database};password={password};";
        ```
        - [ ] Подключение к базе данных:
        ```cs
        // создаём объект для подключения к БД
        MySqlConnection conn = new MySqlConnection(connStr);
        // устанавливаем соединение с БД
        conn.Open();
        ```
        - [ ] Создаем константный запрос
        ```cs
        // создаем запрос с константой
        string sql = "SELECT * FROM users;";
        // объект для выполнения SQL-запроса
        MySqlCommand command = new MySqlCommand(sql, conn);
        // объект для чтения ответа сервера
        MySqlDataReader reader = command.ExecuteReader();
        // читаем результат построчно
        while (reader.Read())
        {
        Console.WriteLine($"{reader[0].ToString().PadRight(3, ' ')}" +
                 $" {reader[1].ToString().PadRight(20, ' ')}" +
                 $" {reader[2].ToString().PadRight(20, ' ')}");
        }
        // закрываем reader
        reader.Close(); 
        ```
        - [ ] Создаем запрос c переменной
        ```cs
        // создаем запрос с константой
        string sql = "SELECT * FROM phones WHERE phones.user = @var_user;";
        // объект для выполнения SQL-запроса
        MySqlCommand command = new MySqlCommand(sql, conn);
        // добавляем значение параметра
        command.Parameters.AddWithValue("@var_user", 2);
        // объект для чтения ответа сервера
        MySqlDataReader reader = command.ExecuteReader();
        // читаем результат построчно
        while (reader.Read())
        {
        Console.WriteLine($"{reader[0].ToString().PadRight(3, ' ')}" +
               $" {reader[1].ToString().PadRight(20, ' ')}");
        }
        // закрываем reader
        reader.Close(); 
        ```
        - [ ] Создаем датасет запрос
        ```cs
        // объект для выполнения SQL-запроса
        MySqlDataAdapter adr = new MySqlDataAdapter(sql, conn);
        // добавляем значение параметра
        adr.SelectCommand.Parameters.AddWithValue("@param", somevalue);
        // заполнение dataset
        DataTable dt = new DataTable();
        adr.Fill(dt); 
        //Вывод таблицы полученный через датасет
        foreach (DataRow row in dt.Rows)
        {
            Console.WriteLine($"{row[column]}");
        }
        // закрываем dataset
        adr.Dispose(); 
        ```
        - [ ] Закрыть соединение с сервером
        ```cs
        // закрываем соединение с БД
        conn.Close();
        ```
        - Можно добавить `Console.ReadLine();` чтобы окно не закрывалось 
  - [ ] Включить воображение и дописать код
- [ ] Проверить работоспособность кода
- [ ] Отправить отчет Витомского
## FAQ
### Как выполнить SQL запрос
Сверху выбрать нажать на кнопку SQL, 
в большом текстовом поле ввести необходимый запрос
и нажиьть на кнопку `Вперёд`