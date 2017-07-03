Новостной сайт на базе Yii2 basic
=================================

**Описание задачи:**
--------------------
Используя basic шаблон фреймворка Yii2 нужно написать простейший новостной сайт
с авторизацией и оповещением пользователей о событиях.

**Junior Developer:**

- Регистрация и авторизация пользователей (можно использовать готовые

модули/расширения) с подтверждением почтового ящика.

- Постраничный вывод превью новостей на главной странице с дальнейшим

полным просмотром. Количество превью на странице должно быть

изменяемым.

- CRUD управление новостями и пользователями с разграничением прав.

Анонимный пользователь может просматривать только превью, пользователь

может просматривать полные новости, модератор может добавлять новости, а

администратор еще и управлять пользователями.

- Оповещать пользователя по e-mail при изменении пароля или создания нового

пользователя администратором (выслать новому пользователю на e-mail

ссылку для активации профиля и ввода нового пароля для дальнейшей

авторизации) и оповещать администратора при регистрации нового

пользователя.

-; Автоматическая авторизация на сайте при активации профиля.


**Для установки приложения необходимо:**
----------------------------------------
1. Скопировать все файлы из архива в директорию сайта
2. Создать базу данных и настроить к ней подключение в файле /config/db.php
        ```php
        return [
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host=localhost;dbname=dbName',
            'username' => 'username',
            'password' => 'password for username',
            'charset' => 'utf8',    
        ];
        ```
3. **Выполнить миграции в указанном порядке:**
- ```yii migrate --migrationPath=@mdm/admin/migrations```
- ```yii migrate --migrationPath=@yii/rbac/migrations```
- ```yii migrate``` 
4.	Выполнить инициализацию модуля Rbac ```yii rbac/init```, в результате инициализации будут добавлены роли (User, Moderator, Admin) и разрешения (userAccess, modAccess, adminAccess)
5.	По умолчанию роль Admin будет установлена для первого зарегистрированного пользователя с id = 1
6.	Для добавления маршрутов необходимо:
- Раскомментировать строку в файле ```/config/web.php```
    ```php
    'as access' => [
        'class' => 'mdm\admin\components\AccessControl',
        'allowActions' => [
           ...
            //'rbac/*',
               ...
        ]
    ],
    ```
 Теперь модуль Rbac доступен по маршруту ```ваш_домен/rbac```
- В меню *маршруты* добавляем маршруты - ```rbac/*, admin/*, news/view```
- Добавляем маршруты разрешениям ```userAccess = news/view,
	modAccess = admin/* + userAccess, adminAccess = rbac/* + modAccess```
- Строку из пункта а необходимо закомментировать 
7.	Регистрация пользователей - по умолчанию первый пользователь будет админ, укажите при регистрации свой email , на который придёт ссылка для активации аккаунта, после перехода по ссылке вам будет доступен пункт меню Админка. 
Из Админки будет доступно управление ролями и разрешениями (пункт меню - Пользователи)
8.	В файле ```/config/params.php``` укажите email администратора на него будут приходить уведомления о регистрации новых пользователей
        ```php
        return [
               …
            'adminEmail' => 'admin@example.com',    
             …
        ];
        ```

Требования
------------

**Минимальные системные требования:**
- web-server: Apache
- php 5.4

