# dependency_resolver
Тестовое задание для собеседования в организацию

## Описание и функции
Микросервис для разрешения зависимостей между задачами и билдами.
Вводится имя билда из файла builds.yaml, затем формируется json ответ
в виде списка из тасок по введенному билду из файла tasks.yaml.



## Технологии
- Python 3.11.1
- FastAPI 0.99.1

## Запуск проекта в dev-режиме
Эти инструкции помогут вам запустить и запустить копию проекта на вашем локальном компьютере для целей разработки и тестирования.

*Клонируем репозиторий:*
```
git clone https://github.com/Eltimccc/dependency_resolver.git
```

*Переходим в папку с проектом и устанавливаем виртуальное окружение:*

```
python3 -m venv venv
```

*Активируем виртуальное окружение:*
```
source venv/bin/activate
```

*Устанавливаем зависимости:*
```
python3 -m pip install --upgrade pip
```
```
pip install -r requirements.txt
```

*ЗАПУСК!*
```
uvicorn app.main:app --reload
```

## Использование
Для корректного использования данного микросервиса в директории проекта должны присутствовать два файла: builds.yaml и tasks.yaml, которые содержат задачи и билды соответственно.

При запуске сервиса он загружает указанные файлы для получения информации и дальнейшей работе с ней.

Для выполнения запросов к микросервису можно использовать документацию Swagger, которая доступна по адресу http://127.0.0.1:8000/docs.

Для выполнения POST-запроса на эндпоинт /get_tasks в Swagger необходимо указать имя билда из файла builds.yaml.

Ответ на запрос представляет собой JSON-объект со списком задач, отсортированных с учетом их зависимостей.

В данном микросервисе для сортировки задач используется рекурсивный подход, который обрабатывает зависимости задач и определяет правильный порядок их добавления в список.

Структура этого микросервиса, рекомендованная документацией FastAPI и его автором, способствует масштабируемости и упрощает поддержку проекта.

### Пример
Вводим пост запрос:
```
{
  "build": "number_sort"
}
```
В файле builds.yaml есть билд:

```
builds:
- name: number_sort
  tasks:
  - five
  - ten
```
Таски с зависимостями из task.yaml:
```
tasks:
- name: ten
  dependencies:
  - six
  - seven
  - eight
  - nine
- name: five
  dependencies:
  - one
  - two
  - three
  - four
```

Выводом является JSON-объект со списком задач, отсортированных с учетом их зависимостей. <p> 
Задачи не могут быть выполнены перед зависимостями, поэтому сначала идет зависимость, потом задача к этой зависимости. Если в зависимостях есть задача, которая также содержит зависимости, то они обрабатываются по такому же принципу.

```
{
  "tasks": [
    "one",
    "two",
    "three",
    "four",
    "five",
    "six",
    "seven",
    "eight",
    "nine",
    "ten"
  ]
}
```

### Автор
[Денис М. (Python-разработчик)](https://github.com/Eltimccc "Денис М (Python-разработчик)")
