# Дипломная работа по базовому курсу разработчик Python
Выполнена основная работа по дипломному заданию:
- Класс ImageSaver умеет загружать фотографии с указанного альбома (по умолчанию profile) произвольного пользователя Вконтакте на ЯндексДиск, вести лог работы и лог загруженных файлов, получать список файлов и информацию о файле на диске, получать статус и имя пользователя Вк, удалять и создавать папки на ЯндексДиске.
- Если не указывать ID пользователя Вконтакте при создании ImageSaver, класс будет инициалиизирован данными владельца токена. Если пользователь забанен или отключен, класс не будет инстанцирован.
- Если в методы не передавать ID пользователя, они будут применяться к тому, от имени кого инициализирован класс (а не от имени владельца токена).
- Класс ImageSaver сохраняет указанное количество фотографий на Яндекс диск, причем выбирает из них наибольшие копии двумя методами. Основной для фото с 2012 года перемножением ширины и высоты, и запасной метод - основанный на названии типа (если размеры не указаны то он автоматически подключается).
- ImageSaver называет фотографии по количеству их лайков, причем для ускорения при сохранении на Диск не проверяет на существование такого имени, потому как Яндекс переименовывает совпадения, возможно эта логика не совсем правильная, но у нас всегда есть возможность удалить папку с фото (кроме корневой). При совпадении назвинй файлов внутри одной выгрузки, включается умное переименование файлов и обновляется лог загруженных файлов.
- Запрос на удаление папки или ее перезапись.
- Ведение лога загруженных файлов в правильном JSON формате с двойными кавычками и выгрузка его в папку с фото с заданным именем.
- Прогресс бар со спиннером или tdqm не стал делать, но сделал подробный вывод действий программы в лог при включенном дебаге.
- Классы используют задержку при выполнении запросов в цикле, чтобы предотвратить бан или троттлинг.
- PEP8 и подробный docstring на каждый важный метод, также присутствуют inline комментарии.

## Немного подробностей:
- Класс ImageSaver инстанцируется вместе с классами VkClient и YaUploader и содержит ссылку во внутренних полях на их экземпляры. Все классы содержат признак успешной инициализации. Если хоть один из вложенных классов не стартует с токенами, ImageSaver также помечается как не инициализированный. Внутри каждого метода также есть проверка на инициализацию и выполнение метода прекращается.
- Все три класса в своих методах, работающих с сетью, возвращают словарь, содержащий значения по ключам: success - признак успешности выполнения метода, object - возвращаемый объект из API (обычно распарсенный JSON или даже вытянутый конкретный объект по переданному пути), message - сообщение об ошибке.
- По умолчанию включен режим дебага - он выводит максимум информации в консоль
- В YaUploader сделана поддержка асинхронных операций (но пока подключена только в методе удаления папки)
- Сделан режим демо, чтобы раскрыть по максимуму возможности класса. Если вы не укажете токены в скрипте, демка вас спросит и покажет линк ВК для получения токена.
- ID пользователей ВК могут быть переданы как цифрами так и строкой. В методы, где принимаются списки (получение друзей), могут передаваться смешанные списки. Статический метод prepare_params их переведет в правильную форму.

# Задание на дипломный проект «Резервное копирование» первого блока «Основы языка программирования Python».
Возможна такая ситуация, что мы хотим показать друзьям фотографии из социальных сетей, но соц. сети могут быть недоступны по каким-либо причинам. Давайте защитимся от такого.  
Нужно написать программу для резервного копирования фотографий с профиля(аватарок) пользователя vk в облачное хранилище Яндекс.Диск.  
Для названий фотографий использовать количество лайков, если количество лайков одинаково, то добавить дату загрузки.  
Информацию по сохраненным фотографиям сохранить в json-файл.

## Задание:
Нужно написать программу, которая будет:
1. Получать фотографии с профиля. Для этого нужно использовать метод [photos.get](https://vk.com/dev/photos.get).
2. Сохранять фотографии максимального размера(ширина/высота в пикселях) на Я.Диске.
3. Для имени фотографий использовать количество лайков. 
4. Сохранять информацию по фотографиям в json-файл с результатами. 

### Входные данные:
Пользователь вводит:
1. id пользователя vk;
2. токен с [Полигона Яндекс.Диска](https://yandex.ru/dev/disk/poligon/).
*Важно:* Токен публиковать в github не нужно!

### Выходные данные:
1. json-файл с информацией по файлу:
```javascript
    [{
    "file_name": "34.jpg",
    "size": "z"
    }]
```
2. Измененный Я.диск, куда добавились фотографии.
​
​
### Обязательные требования к программе:
1. Использовать REST API Я.Диска и ключ, полученный с полигона.
2. Для загруженных фотографий нужно создать свою папку.
3. Сохранять указанное количество фотографий(по умолчанию 5) наибольшего размера (ширина/высота в пикселях) на Я.Диске
4. Сделать прогресс-бар или логирование для отслеживания процесса программы.
5. Код программы должен удовлетворять PEP8.
​
### Необязательные требования к программе:
1. Сохранять фотографии и из других альбомов.
2. Сохранять фотографии из других социальных сетей. [Одноклассники](https://apiok.ru/) и [Инстаграмм](https://www.instagram.com/developer/)
3. Сохранять фотографии на Google.Drive.


Советы:
1. Для тестирования можно использовать аккаунт https://vk.com/begemot_korovin
2. Токен для VK api: 958eb5d439726565e9333aa30e50e0f937ee432e927f0dbd541c541887d919a7c56f95c04217915c32008
