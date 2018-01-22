# Файл настройки
----------

ORCHID использует стандартную систему настроек для Laravel.
Все параметры располагаются в директории `config`, а основным файлом для платформы является 
файл `platform.php`. Каждая настройка поставляется с комментарием поясняющим основную суть.


## Безголовый режим

```php
'headless' => false,
```

Платформа прекрасно понимает, что не может покрыть всех задач разработчика, поэтому предоставляет возможность
отключение графического интерфейса. 
Это может пригодится если вы создаёте приложение, которому не требуется стороннее администрирование, а весь контент создаётся пользователями. 
Так же это отличное решение, если вы хотите использовать свой собственный графический интерфейс (Например, встроить панель администрирования в дизайн вашего приложения)


## Адрес платформы

```php
'domain' => env('DASHBOARD_DOMAIN', parse_url(config('app.url'))['host']),
```

Для многих проектов адрес расположения панели администрирования играет важную роль.
Например, приложение располагается по адресу `example.com`, а платформа на `admin.example.com` или на стороннем домене.

Для этого требуется указать по какому адресу вы хотели её открывать. 

```php
'domain' => 'admin.example.com',
```
 
 Помните, что ваши параметры веб сервера должны быть настроены должным образом.




## Префикс платформы


```php
'prefix' => env('DASHBOARD_PREFIX', 'dashboard'),
```

По префиксу к графическому интерфейсу обычно можно выявить какая система установлена на веб сайте,
например `wp-admin` для WordPress, что может дать возможность автоматически искать старые уязвимые версии ПО и получать над ними контроль.
 
Есть и другие причины, но рассматривать в этом разделе мы не будем. 
Главное ORCHID предоставляет возможность смены префикса `dashboard` на любое другое название, например `admin` или `administrator`



## Промежуточные слои

```php
'middleware' => [
    'public'  => ['web'],
    'private' => ['web', 'dashboard'],
],
```

Можете добавлять/изменять промежуточные слои (middleware) для графического интерфейса. 
На данный момент предусмотрены две группы `public`, которую может видеть не авторизованный пользователь, 
например, страницу "Входа" или "Востановление пароля" и `private` которую наоборот видят только авторизованные пользователи.


Вы можете добавить сколько угодно новых промежуточных слоёв, 
например для предоставления возможностей администрирования из белого списка IP адресов



## Страница авторизации

```php
'auth' => [
    'display' => true,
    'image'   => '/orchid/img/background.jpg',
    //'slogan'  => '',
],
```

Страница авторизации имеет несколько настроек, таких как фоновое изображение и слоган вашего проекта.
Так же существует возможность и вовсе отключить поставляемую форму авторизации и сделать собственную, например с помощью команды:

```php
php artisan make:auth
```



## Локализация записей

```php
'locales' => [
    'en' => [
        'name'     => 'English',
        'script'   => 'Latn',
        'dir'      => 'ltr',
        'native'   => 'English',
        'regional' => 'en_GB',
    ],
],
```

Стандартные записи созданные с помощью `поведений` имеют возможность встроенной локализации, то есть 
вы можете создавать одни и те же записи на разных языках, что бы добавить язык, требуется лишь добавить
новый элемент массива.



## Поля

```php
'fields' => [
    'textarea'     => Orchid\Platform\Fields\Types\TextAreaField::class,
    'input'        => Orchid\Platform\Fields\Types\InputField::class,
    'list'         => Orchid\Platform\Fields\Types\ListField::class,
    'tags'         => Orchid\Platform\Fields\Types\TagsField::class,
    'robot'        => Orchid\Platform\Fields\Types\RobotField::class,
    'relationship' => Orchid\Platform\Fields\Types\RelationshipField::class,
    'place'        => Orchid\Platform\Fields\Types\PlaceField::class,
    'picture'      => Orchid\Platform\Fields\Types\PictureField::class,
    'datetime'     => Orchid\Platform\Fields\Types\DateTimerField::class,
    'checkbox'     => Orchid\Platform\Fields\Types\CheckBoxField::class,
    'code'         => Orchid\Platform\Fields\Types\CodeField::class,
    'wysiwyg'      => Orchid\Platform\Fields\Types\TinyMCEField::class,
    'password'     => Orchid\Platform\Fields\Types\PasswordField::class,
    'markdown'     => Orchid\Platform\Fields\Types\SimpleMDEField::class,
],
```

В конфигурации полей указываются алиасы для поля, например `wysiwyg` редактор, это позволяет
абстрагироваться от используемых элементов. Например в ходе работы вы поняли, что функций редактора
не хватает, вместо того, чтобы изменять редактор в каждом используемом файле, вам достаточно будет изменить 
класс на который ссылается тот самый алиас.

[Подробнее о полях](/ru/docs/field/)


## Одиночные поведения

```php
'single' => [
    App\Core\Behaviors\Single\DemoPage::class,
],
```

Одиночные поведения, это один из типов множественных `поведений` существующих лишь в одном единственном экземпляре.
Это отлично решение например для создания уникальных (Не типовых!) страниц для веб-сайтов.


## Множественные поведения


```php
'single' => [
    App\Core\Behaviors\Single\DemoPage::class,
],
```

Множественные поведения созданы для сокращения времени на создание типовых данных которые могут иметь множество записей.
Например, если требуется создать какие-либо справочники для данных или каталоги данные которых очень похожи.


## Стандартные поведения

```php
'common' => [
    'user'     => \Orchid\Platform\Behaviors\Base\UserBase::class,
    'category' => \Orchid\Platform\Behaviors\Base\CategoryBase::class,
],
```

Платформа поставляется с некоторым набором стандартных CRUD записей, например создание `Пользователя`, что бы изменить
или добавить новые поля для заполнения и вывода, требуется заменить стандартные классы.

## Пользовательское меню

```php
'menu' => [
    'header'  => 'Header menu',
    'sidebar' => 'Sidebar menu',
    'footer'  => 'Footer menu',
],
```

Конфигурация меню требует лишь указание ключа и значение, которое будет отображаться пользователю. 
По умолчанию добавлены три типа меню, верхнее, боковое и нижнее. 
Вы можете добавлять или удалять в зависимости от потребностей.

Пример использования меню можно посмотреть [здесь](/ru/docs/tutorial_blog/#vidzhet).

## Изображения

```php
'images' => [
    'low'    => [
        'width'   => '50',
        'height'  => '50',
        'quality' => '50',
    ],
    'medium' => [
        'width'   => '600',
        'height'  => '300',
        'quality' => '75',
    ],
    'high'   => [
        'width'   => '1000',
        'height'  => '500',
        'quality' => '95',
    ],
],
```

Вложения имеют возможность обработки изображений, создавая нужные копии подходящие для формата.


## Вложенные типы

```php
'attachment' => [
    'image' => [
        'png',
        'jpg',
        'jpeg',
        'gif',
    ],
    'video' => [
        'mp4',
        'mkv',
    ],
    'docs'  => [
        'doc',
        'docx',
        'pdf',
        'xls',
        'xlsx',
        'xml',
        'txt',
        'zip',
        'rar',
        'svg',
        'ppt',
        'pptx',
    ],
],
```

Вложения так же имеют возможность группировки, это отлично подходит например если, требуется выбрать только документы
или только видио записи.

## Виджеты панели управления

```php
'main_widgets' => [
    Orchid\Platform\Http\Widgets\UpdateWidget::class,
],
```

Главная страница панели управления по умолчанию имеет только один единственный виджет, который оповещает о
выходе новой стабильной версии ПО. Так как ORCHID не нацелен на решение конкретных проблематик, то не может
предоставлять какие-либо метрики (Например сколько создано записей или количество посетивших пользователей)

Вы можете сами создать и добавить любые виджеты для их отображения. Более подробно смотрите раздел "Виджеты"


## Ресурсы панели управления


```php
'resource' => [
    'stylesheets' => [],
    'scripts'     => [],
],
```

В ходе работы вам может потребоваться добавить свои собственные таблицы стили или javascript сценарии
глобально, на каждую страницу, то необходимо добавить пути для них в соответствующие массивы.