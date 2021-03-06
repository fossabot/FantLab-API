## Миникарточки элементов базы

Основная краткая выдача информации по базе в виде лид-карточек. Разбита по блоки соответвующие сегментам базы: авторы/произведения/издания. Отдается с учетом релевантности найденнему по запросу.  
Структура ответа - сокращенный вариант отдельных позиций библиобазы. Исспользуется в выдачи листингов/подборок (в первую очередь - в поиске).  

Основные поля миникарточки (есть у всех сегментов):
```
        type: String,            # сегмент (сущность/раздел) базы: work, edition, author/autor, art и пр. 
                                 # полный список сегментов см. в общем README)
        id: Int,                 # id позиции данного сегмента (вместе с type дают уникальный fantlab-id: {type}{id}
                                 # примеры: work101, edition14215, author12 и т.д.

        title: String,           # заголовок (часто составное как "автор «название»")
        description: String,     # описание (краткое, не более 500 символов)
        image: Url,              # титульная картинка
        image_preview: Url,      # ссылка на превью-картинка (50х70 px)
        url: Url,                # ссылка на страницу в фл-базе

        name: String,            # имя персоны (название)
        name_orig: String,       # имя или название в оргинале (это поле есть не всегда)
        is_opened: Boolean,      # карточка открыта для просмотра (это поле есть не всегда, обычно есть только у авторов/персон)
```



Полный формат отдачи миникарточек:
```
{
  authors: [
    {
        type: String,            # сущность/раздел базы (autor)
        id: Int,                 # id позиции данного раздела (в данном случае - ID автора)
        name: String,            # имя
        name_orig: String,       # имя в оригинале
        title: String,           # имя рус и оргинальное: "Джон Смит (Jonh Smith)"
        description: String,     # краткое описание (не более 500 символов)
        image: Url,              # ссылка на фото автора
        image_preview: Url,      # ссылка на превью 50х70
        url: Url,                # ссылка на страницу автора в ФЛ-базе

        birthday: Data,          # дата или год рождения
        sex: String,             # пол (m/w)
        is_opened: Boolean,      # открыта ли страница автора
    },
    ...
  ],

  works: [
    {
        type: String,            # сущность/раздел базы (work)
        id: Int,                 # id произведения
        name: String,            # имя
        name_orig: String,       # имя в оригинале
        title:                   # заглавие: авторы и название произведения в одну строку
        description: String,     # краткая аннотация произведения (не более 500 символов)
        image: Url,              # ссылка на обложку-постер для произведения
        image_preview: Url,      # ссылка на его превью
        url: Url,                # ссылка на страницу произведения в ФЛ-базе

        year: Year,              # год публикации
        name_type: String,       # тип произведения на русском
        name_type_icon: String,  # тип произведения как тип (novel, cycle и пр.)
        published: Boolean,      # опубликовано или нет (1 - опубликовано / 0 - запланировано к выходу)
        
        creators: {              # массив авторов проивезедений.
            authors: [              # авторы произвдений
                {
                    type: String,         # сущность. могут быть autor или art (для графических)
                    id: Int,              # id автора
                    name: String,         # имя автора
                    is_opened: Boolean,   # открыт ли автор
                },
                ...
            ],

        saga: {                  # цикл, краткая информация о родительском цикле
             type: String,           # сущность/раздел базы (work)
             id: Int,                # work_id
             name: String,           # название цикла
             name_type: String,      # тип (цикл, роман-эпопея, межавторский цикл и пр.)
        },

        stat: {                  # рейтинг и статистика произведения
            rating: Float,           # рейтинг (пример: "8.79")
            voters: Int,             # кол-во оценок
            responses: Int,          # кол-во отзывов
        },
    },
    ...
  ],

  editions: [
    {
        type: String,            # сущность/раздел базы (edition)
        id: Int,                 # id издания
        name: String,            # наименование издания
        name_type: null,         # тип издания (если пусто - то обычное бумажное издание произведения)
        title: String,           # заголовок собранный из автора+названия+типа издания, пример: "Стивен Кинг «Волки Кальи» (издание)"
        description: String,     # описание издания. текст может содержать a-тэги на сущности базы
        image: Url,              # ссылка на обложку издания
        image_preview: Url,      # ссылка на превью обложки 60х~90
        url: Url,                # ссылка на страницу издания в ФЛ-базе

        year: Year,              # год выхода издания
        published: Boolean,      # издание вышло (1 - опубликовано / 0 - запланировано к выходу)
        lang: String,            # язык издания
        lang_code: String,       # код языка издания
        copies: Int,             # тираж
        pages: Int,              # количество страниц
        isbns: [ ],              # строковый массив ISBN

        creators: {              # массив авторов/составителей/издательств - т.е. тех, кто принял участие в выходе издания
            authors: [              # авторы произведений
                {
                    type: String,         # сущность. могут быть "autor" или "art" (для графических)
                    id: Int,              # id автора
                    name: String,         # имя автора
                    is_opened: Boolean,   # открыт ли автор
                },
                ...
            ],
            compiler: [],           # составители издания (сборника/антологии). структура такая же как в authors
            publishers: [           # издательства
                {
                    type: String,         # сущность. тут только тип "publisher"
                    id: Int,              # id издательства
                    name: String,         # наименование издательства
                },
                ...
            ]
        },
        series: [                    # серии, в которые входит издание
            {
                type: String              # тип (series) 
                id: Int,                  # id серии
                name: String,             # название серии
                is_opened: Boolean,       # открыта ли страница серии
            },
            ...
        ],
    },
    ...
  ],

}
```



## Получение миникарточек по поисковому запросу

Основная json-выдача поиска по базе. Разбита по блоки соответвующие сегментам базы: авторы/произведения/издания. Отдается с учетом релевантности найденнему по запросу.  
Структура ответа - формат миникарточек.

Запрос
```
GET /search-txt?q={text}
```

Параметры
```
text        - слово или фраза для поиска по фл-базе (название произведения, имя автора)
```

Пример
> [/search-txt?q=Мартин](https://api.fantlab.ru/search-txt?q=Мартин) - Выдача по запросу "Мартин"

Ответ
```
соответвует выдачи миникарточек (см. выше на этой странице)
```


## Получение миникарточек по их ID

Запрос на множественную выдачу позиций библиобазы (авторов/произведений/изданий) по id.  

Запрос
```
GET /search-ids?a={autor_ids}&w={work_ids}&e={edition_ids}
```

Параметры
```
autor_ids   - список id-авторов
work_ids    - список id-произведений
edition_ids - список id-издания
```
id передаются через запятую. Не более 200 позиций.

Пример
> [/search-ids?a=1,2&w=1,11&e=3,31,1033](https://api.fantlab.ru/search-ids?a=1,2&w=1,11&e=3,31,1033) - Выдача данных карточек авторов с id 1 и 2, произведений 1 и 11, издания 3, 31, 1033 (3 не выдастся, т.к. издания с таким ID нет в базе)

_Примечание: данный api-запрос используются на страницах самого ФЛ для формирование всплывающих подсказок (превью)  ссылок на сегменты базы._

Ответ
```
соответвует выдачи миникарточек (см. выше на этой странице)
```
