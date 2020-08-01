# Flask
**микрофреймворк python для создания веб-приложений**

+ [Установка Flask](#installing)
+ [Приложение на Flask](#app)
+ [Маршруты](#paths)
+ [HTTP-методы](#methods)
+ [Шаблоны](#templates)
+ [Объект request](#request)

### <a name="installing"></a> Установка Flask
+ Установить виртуальную среду: 
    ```bash
    python3 -m venv env
    ```
+ Активировать виртуальную среду:
    ```bash
    source env/bin/activate
    ```
+ Установка Flask:
    ```
    pip insltall flask
    ```
+ Запустить приложение flsk:
    ```bash
    flask start
    ```

### <a name="app"> </a> Приложение на Flask 
+ Создать файл `app.py` с кодом:
    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        return 'Hello World!'

    if __name__ == '__main__':
        app.run(debug=True)
    ```
    1. Импортируем Flask. Экземлпяр этого класса и будет WSGI-приложением.
    2. При помощи декоратора `@app.route("/")` указываем URL, по которому будет доступно данное отображение.
    3. При запуске приложения ему передается именованный параметр `debug=True`, что обозначает, что приложение необходимо запустить в режиме отладки.

+ Запустить это приложение через консоль
+ Перейти на `localhost:5000`

### <a name="paths"></a> Маршруты
+ Функции-обработчики работают с запросами, поступающими на конкретный URL. Этот URL определяется маршрутом.
+ Маршруты задаются с помощью декоратора `@app.route("")` над фунцией-обработчиком.
+ Маршруты могут меняться в зависимости от того, на какой странице находиться, например путь `authors/` будет соответствовать списку авторов, а вот путь `authors/john` будет соответствовать определенному автору, Джону. Реализуется это через изменяемые маршруты:
    ```py
    @app.route('/user/<username>')#изменяемый URL
    def show_user_profile(username):
        return f'User {username}'
    ```
    ```py
    @app.route('/post/<int:post_id>')#изменяемый URL
    def show_post(post_id):
        return f'Post {post_id}'
    ```
### <a name="methods"></a> HTTP-методы
+ в `app.route()` можно добавлять методы, которые обрабатывает функция:
    ```py
    from flask import request

    @app.route('/login', methods=['GET', 'POST'])
    def login():
        if request.method == 'POST':
            do_the_login()
        else:
            show_the_login_form()
    ```
    + `request` берется из глобального контекста

### <a name="templates"> </a> Шаблоны
+ Для визуализации шоблонов используется метод `render_template()`, все, что необходимо это передать название шаблона, а так же передать список именованных аргументов. Flask ищет шаблоны в папке **templates** пакета, в котором находится сам файл, из котрого этот шаблон вызывается: 
    ```py
    from flask import render_template

    @app.route('/hello/<name>')
    def hello(name=None):
        return render_template('hello.html', name=name)
    ```
    + исполняемый файл может находиться в модуле:
    ```
    /application.py
    /templates
        /hello.html
    ```
    + исполняемый файл может находиться в пакете:
    ```
    /application
        /__init__.py
        /templates
            /hello.html
    ```
+ для приведенного выше кода можно придумать такой шаблон:
    ```html
    <title>Hello from Flask</title>
    {% if name %}
      <h1>Hello {{ name }}!</h1>
    {% else %}
      <h1>Hello World!</h1>
    {% endif %}
    ```
    + для шаблонизации используется движое Jinja2, тот же что и для Django, потому правила переменных, блоков и наследования шаблонов сохраняются как и для Django. В приведенном выше примере видно, что в шаблоне используется переданный именованный аргумент `name`.

### <a name="request"></a> Объект request
+ за запрос, приходящий функциям на обработку отвечает глобальный обьект **`request`**:
    ```py
    from flask import request
    ```
    + В нем хранится все данные, которые передает клиент, чтобы получить корректные данные с сервера.
    + Глобальный объект запроса `request` является локальным для определенного контекста. Новый контекст создается каждый раз для нового потока(или не потока, так как flask имеет дело с параллельным выполнением).
+ можно получать рвзличные данные из объекта запроса, например метод(`method`) или данные фармы(`form[]`) :
    ```py
    @app.route('/login', methods=['POST', 'GET'])
    def login():
        error = None
        if request.method == 'POST':
            if valid_login(request.form['username'],
                           request.form['password']):
                return log_the_user_in(request.form['username'])
            else:
                error = 'Invalid username/password'
        return render_template('login.html', error=error)
    ```
+ можно получать данные из URL `?ключ=значение`:
    ```py
    serchword = request.args.get('key', '')
    ```