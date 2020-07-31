# Flask
**микрофреймворк python для создания веб-приложений**

+ [Установка Flask](#installing)
+ [Приложение на Flask](#app)


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