# DJANGO:
[примеры кода можно посмотреть здесь](https://github.com/Chudopal/Django_learning)

**Django - это фреймворк для backend разработки, написанный на python.**
+ ## [Настройка виртуальной среды](#settings_of_env)
+ ## [Основные термины](#basics_termins)
+ ## [Основные команды Django](#basic_commands_django)
+ ## [Создание проекта и приложения](#make_model_and_app)
+ ## [Модели Django](#models_of_django)
  + ### [Создание моделей](#create_model)
  + ### [Добавление моделей в abmin.py](#add_in_admin)
  + ### [Настройка отображения моделей](#display_of_models)
+ ## [Работа с оболочкой](#query_set)
+ ## [Обработчики Django](#handl)
  + ### [views.py](#look_of_site)
  + ### [Добавление шаблонов URL](#urltemp)

## <a name="settings_of_env"></a> Настройка виртуальной среды
+ Для того, чтобы начать Django-проект желательно сделать виртуальную среду и установить в ней Django.
+ Для того, чтобы уcтановить виртуальную среду необходимо создать папку, в которой в консоли прописать: `python3 -m venv name_of_env` 
+ Далее для работы необходимо активировать виртуальную среду:
`source name_of_env/bin/activate`
+ На то, что среда активированна укажет ее название перед именем пользователя в скобках в терминале:
`(name_of_env) alexandr@alexandr-VivoBook-15-ASUS-Laptop-X570UD:~/git_hub/django/blogsite$`
+ Чтобы деактивировать среду, надо просто прописать в терминале:
`deactivate`
+ Перед тем как установить Django желательно обновить pip;
`python3 -m pip install --upgrade pip`
+ Для установки Django версии 2.0.5 необходимо прописать в терминале:
`pip install Django==2.0.5`
+ Для того, чтобы посмотреть какие пакеты с их версиями установленны в среде необходимо выполнить команду:
`pip freeze`

## <a name="basics_termins"></a> Основные термины
В Django *проект* – это код, созданный с использoванием Django и содержащий некоторые настройки. *Приложение* – это набор
модулей, описывающих модели, обработчики запросов, шаблоны и конфигурации URL’ов. Приложение взаимодействует с фреймворком, предоставляя некоторую функциональность, и может быть многократно использовано в других проектах. Мы можем сопоставить проект с сайтом, который состоит из
нескольких приложений (блога, раздела вопросов, форума), каждое из которых
может быть использовано и в других проектах.
*Миграции* - это что-то вроде системы контроля версий для базы данных. Они позволяют команде программистов изменять структуру БД, в то же время оставаясь в курсе изменений других участников. Миграции обычно идут рука об руку с конструктором таблиц для более простого обращения с архитектурой вашего приложения.

## <a name="basic_commands_django"></a> Основные команды Django
Команды просто писать в терминале:

+ **`source name_of_env/bin/activate`** - активация виртуальной среды.
+ **`deactivate`** - деактивация виртуальной среды.
+ **`django-admin startproject mysite`** - создание проекта  mysite.
+ **`python manage.py runserver`** - запуск сервера. 
+ **`python manage.py startapp blog`** - создание приложения blog.
+ **`python manage.py makemigrations blog`** - инициализирующая миграция, ее нужно делать всякий раз после изменения models.py.
+ **`python manage.py sqlmigrate blog 0001`** - выводит SQL код миграции 0001 в консоль.
+ **`python manage.py migrate`** - синхронизация базы данных. Применение миграции для всех приложений, указанных в INSTALLED_APPS, файла setting.py.
+ **`python manage.py createsuperuser`** - создание сайта администрирования. На нем можно управлять всеми моделями, которые в нем определены.
+ **`python manage.py shell`** - открыть оболочку.

## <a name="make_model_and_app">  Создание проекта и приложения
**`django-admin startproject mysite`** - создание проекта  mysite.
После создания проекта в директории появится папка mysite, в которой будет следующее содержимое:
```
mysite/ 
    manage.py           - утилита командной строки для управления проектом.
    mysite/             - сама папка проекта.
        _init__.py      - говорит Python о том, что mysite является Python-пакетом.
        settings.py     - конфигурация проекта.
        urls.py         - здесь будут содержаться шаблоны адресов.
        wsgi.py         - конфигурация для запуска проекта как WSGI-приложения.

```
Далее можно посмотреть, успешно ли создался проект, для этого необходмо его запустить:
**`python manage.py runserver`** - запуск сервера, по умолчанию он доступен по адресу `127.0.0.1:8000` или `localhost:8000`, чтобы изменить адрес или файл кофигурации нужно в консоле запустить сервер следующим образом: `python manage.py runserver 127.0.0.1:8001 --settings=mysite.settings`

**`python manage.py startapp blog`** - создание приложения blog. Приложения - это блоки проекта.
```
blog/
    __init__.py
    admin.py            - регистрируем модели для добавления их в систему администрирования Django.
    apps.py             - файл, содержащий основную конфигурацию приложения blog.
    migrations/         - папка, содержащая миграции базы данных приложения.
        __init__.py
    models.py           - модели данных приложения.
    tests.py            - файл для тестов приложения.
    views.py            - вся логика хранится здесь.
```
Чтобы приложение blog было видно Django, необходимо добавить 
*blog.apps.BlogConfig* в настройку INSTALLED_APPS в файле settings.py. 
Класс BlogConfig – это конфигурация приложения.

## <a name="models_of_django"></a> Модели Django
**Модели** отображают информацию о данных, с которыми вы работаете. Они содержат поля и поведение ваших данных. Обычно одна модель представляет одну таблицу в базе данных.

Основы:
+ Каждая модель это класс унаследованный от *django.db.models.Model*.
+ Атрибут модели представляет поле в базе данных.
+ Django предоставляет автоматически созданное API для доступа к данным.
### <a name="create_model"></a> Создание моделей 
Модели описываются в файле models.py:
```python
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

# Create your models here.

class Post(models.Model):
    """Класс для формирования модели поста в базе данных.
    Если вдруг происходит изменение здесь,
    необходимо пересобрать базу данных"""

    STATUS_CHOICES = ( 
        ('draft','Draft'),  
        ('published','Published'),
    )
    title = models.CharField(max_length=250) #заголовок статьи, базе данных пребразуется в обычный VARCHAR
    slug = models.SlugField(max_length=250, unique_for_date='publish') #Поле для формаирования URL-ов. unique_for_date - для формирования уникальных имен
    author = models.ForeignKey(User, on_delete=models.CASCADE, #внешний ключ,определеят отношение "один ко многим".  
                               related_name='blog_posts')#on_delete -определяет поведение при удалиении пользователя. CASCADE - удалять все посты пользоватля.
                                                         #related_name - имя обратной связи от User к Post. Так можно найти вязанные посты автора
    body = models.TextField()#основное содержание статьи.
    publish = models.DateTimeField(default=timezone.now) #поле даты, которое сохраняет дату публикации. datetime.now - возвращает текущую дату.
    created = models.DateTimeField(auto_now_add=True)#поле даты. указывает, когда статья была создана. auto_now_add - авто сохранение даты при создании объекта
    updated = models.DateTimeField(auto_now=True)#дата и время, когда статья была отредактирована. auto_now - дата будет сохраняться автоматически при создании объекта
    status = models.CharField(max_length=10,choices=STATUS_CHOICES,default='draft')#отображает статус статьи.  STATUS_CHOICES - отсюда будут браться варианты
    class Meta:
        """Метаданные
        Здесь мы указали, что статьи должны
        сортироваться по дате публикации"""

        ordering = ('-publish',)#"-" - означает что сортировка должна происходить по убыванию
        def __str__(self):
            return self.title
```
Каждое поле определено как атрибут класса, и каждый атрибут соответствует полю таблицы в базе данных.
После изменения модели необходимо прописать в терминале:
+ `python manage.py makemigrations name_of_app`
+ `python manage.py migrate`

### <a name="add_in_admin"></a> Добавление моделей в admin.py
**`python manage.py createsuperuser`** - создание сайта администрирования. На нем можно управлять всеми моделями, которые в нем определены.
Чтобы приложение показывало свою модель на странице администрирования необходимо изменить
файл admin.py этого приложения таким образом:
```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
Где *Post* - это модель из приложения. Теперь на `localhost:8000/admin` можно добавить или удалить экземляры моделей данного приложения.

### <a name="display_of_models"></a> Настройка отображения моделей
Можно настривать отображение моделей в /admin, 
```python
from django.contrib import admin
from .models import Post

@admin.register(Post) #декоратор, то же самое что и admin.site.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('title', 'slug', 'author', 'publish','status') #столбцы таблицы
```
Так мы говорим Django, что наша модель зарегистрирована на сайте администрирования с помощью пользовательского класса, наследника `ModelAdmin`.

## <a name="query_set"></a> Работа с оболочкой
Django ORM основана на объектах запросов *QuerySet*. QuerySet – это коллекция объектов, полученных из базы данных. К ней могут быть применены фильтрация и сортировка.
+ Чтобы добавить объект в БД, необходимо открыть терминал и прописать `python manage.py shell`,затем прописать следующие строки:
```python
>>> from django.contrib.auth.models import User
>>> from blog.models import Post
>>> user = User.objects.get(username='admin') #ищем пользователя с таким именем
>>> post = Post(title='Another post', slug='another-post',
body='Post body.', author=user)
>>>post.save()#сохраняем объект в базу данных
``` 
+ Чтобы получить доступ ко все объектам БД необходимо прописать `>>> all_posts = Post.objects.all()`
+ Методы сортировки:
  + *filter()*:
    `Post.objects.filter(publish__year=2017)` - сортировка записей из 2017 года.
    **Условия фильтрации строятся с использованием двойного подчеркивания например publish__year.**
  + *exclude()*:
    фильтр имключения, т е мы исключаем все записи, которые подходят под него: `Post.objects.filter(publish__year=2017).exclude(title__startswith='Why')`
  + *order_by()*:
    для сортировки по полям: `Post.objects.order_by('title')` - алфавитном порядке, `Post.objects.order_by('-title')` - в обратном порядке.
+ Чтобы удалить объект:
    *delete()*: 
    ```python
    post = Post.objects.get(id=1)
    post.delete()
    ```

## <a name="handl"></a> Обработчики Django
Обработчик Django – это простая Python-функция, которая получает веб-запрос и возвращает веб-ответ. Вся логика, формирующая желаемый ответ, описывается внутри этой функции. Каждый обработчик генерирует шаблон, используя переменные контекста, и возвращает HTTP-ответ со сформированной HTML-страницей.

### <a name="look_of_site"></a> views.py
Обработчики необходимо описывать в `views.py`:
```python
def post_list(request):
    """Обработчик для отображения списка статей

    request - обязателен для всех обработчиков
    В этой функции мы запрашиваем 
    из БД все опубликованные статьи с помощью
    менеджера published
    """
    posts = Post.published.all()#запрос из бд
    return render(              #формирует шаблон со списка статей
        request,                #принимает объект запроса request,  
        'blog/post/list.html',  #путь к шаблону
        {'posts':posts}         #и переменные контекста для этого шаблона.     
    )

```
Во `views.py` мы описываем логику реакции на запрос, а frontend формируется в папке template приложения. Мы используем функцию render() для формирования шаблона. Она принимает объект запроса **request**, **путь к шаблону** и **переменные контекста** для этого шаблона. В ответ вернется объект HttpResponse со сформированным текстом (обычно это HTML-код). Функция render() использует переданный контекст при формировании шаблона, поэтому любая
переменная контекста будет доступна в шаблоне. *Процессоры контекста – это вызываемые функции, которые добавляют в контекст переменные.*

### <a name="urltemp"></a> Добавление шаблонов URL
Шаблоны URL’ов позволяют сопоставить адреса с обработчиками. Шаблон представляет собой комбинацию из строки, описывающей адрес, обработчика и необязательного названия, которое даст возможность обращаться к этому шаблону на всех уровнях проекта. Django проходит по порядку по всем шаблонам,пока не найдет первый подходящий, т. е. совпадающий с URL’ом запроса. Затем Django сможет импортировать соответствующий обработчик и выполнить его, передав внутрь объект запроса HttpRequest и ключевые слова или позиционные аргументы.
Для этого необходимо в папке приложени создать файл `urls.py`(Любое значение, определенное в шаблоне как `<parameter>`, возвраща-
ется в виде строки):
```python
    from django.urls import path
    from . import views
    app_name = 'blog'#ОПРЕДЕЛЕНИЕ ПРОСТРАНСТВА ИМЕН

    urlpatterns = [
    # post views
    path('', views.post_list, name='post_list'),#вызов обработчика
    path('<int:year>/<int:month>/<int:day>/<slug:post>/',
    views.post_detail, name='post_detail'), 
    ]
```
Так же необходимо отредактировать `urls.py`, который находится в радительском каталоге:
```python
    from django.urls import path, include
    from django.contrib import admin

    urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls', namespace='blog')),#именно здесь мы добавили юрлы приложения
    ]
```
**`include - подключает конфигурацию приложения.`**

В Django есть соглашение о том, что метод модели get_absolute_url() должен возвращать канонический URL объекта:
```python
    from django.urls import reverse
    class Post(models.Model):
    # ...
    def get_absolute_url(self):
        return reverse('blog:post_detail', args=[self.publish.year,
        self.publish.month, self.publish.day, self.slug])
```
