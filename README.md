# Стартовая структура для проекта с использованием GULP 4

#### После клонирования запускаем для установки всех пакетов
`$ npm install`

## Все файлы для разработки лежат в папке `/gulp-project/app`

`$ cd gulp-project/app`

## Содержимое файла `gulpfile.js`:

Задача `browser-sync` для синхронизации с браузером.

Задача `styles` компилирует стили из scss в css, сжимает, переименовывает и загружает полученный файл css в папку 'app/css'.

Задача `scripts` объединяет указанные js-файлы в один файл с минификацией и помещает в папку 'app/js'.

Задача `code` включается внутри задачи `watch` для отслеживания изменений в файлах .html для последующей синхронизации с браузером.

Задача `minify-html` используется в задаче `build` для минификации файлов html с последующим помещением минифицированных html в папку для продакшена 'dist'.

Задача `sprite` позволяет создавать спрайты с иконками. Если спрайты не используются, тогда нужно удалить задачу из тасок `build` и `default`, удалить файлы 'app\scss\_sprite.scss', 'images\sprite-icons' и 'images\sprite.png' 'images\sprite@2x.png' - Это файлы для частного случая, когда нужны спрайты.

Если спрайты используются, но не используются спрайты для ретины, нужно закомментировать строки, в которых упоминается retina внутри 'spritesmith({...})'

**imgName** — имя генерируемой картинки.

**imgPath** — путь к спрайту, будет записываться в CSS.

**cssName** — имя css файла, который получится на выходе.

**retinaSrcFilter** - директория с иконками для создания спрайта.

Пример использорвания иконки со спрайтом:

`<i class="icon icon-fb"></i>`

Правило для всех иконок:

`.icon {
  display: inline-block;
}`

При использовании scss, создаются миксины для использования спрайтов в scss.

Сгенерирует полностью все стили для всех иконок:

`@include sprites($spritesheet-sprites);`

Сгенерирует полностью все стили для всех иконок для ретины:

`@include retina-sprites($retina-groups);`

Вывести стили для отдельной иконки:

`.icon-fb {
  @include sprite($icon-fb);
}`

Вывести стили для отдельной иконки для ретины:

`.icon-fb {
  @include retina-sprite($icon-fb-group);
}`

Задача `img`  используется в задаче 'build' - оптимизирует все изображения и помещает их в готовый дистрибутив продакшена 'dist'.

Задача `clean` удаляет папку продакшена 'dist' с готовой сборкой всех файлов.

Задача `clear` чистит кеш, кеш используется для сжатия картинок, чтобы не сжимать их при каждом билде. Если в готовой сборке наблядаются глюки, нужно очистить кеш.

Задача `build-dist` используется в задаче 'build' для копирования скомпилированных файлов css и js в папку продакшена 'dist'.

Задача `watch` отслеживает изменения в указанных файлах, но без использования 'browser-sync' не будет синхронизации в баузере. Задача 'watch' используется в задаче 'default'.

Задача `build` выполняет поочередно задачи `clean`, `sprite`, `styles`, `scripts`, `minify-html`, `img`, `build-dist` и результатом будет собранный готовый сайт в папке продакшена 'dist'.

Задача `default` запускает задачи `sprite`, `styles`, `scripts`, `browser-sync`, `watch` и результатом будет запуск в браузере http://localhost:3000/ с синхронизацией и отслеживанием изменений в формате livereload.

#### При разработке запускаем для синхронизации браузера с изменениями в коде:

`$ gulp`

или 

`$ gulp default`

#### Для сборки продакшена запускаем :

`$ gulp build`

И получаем рабочую сборку всех файлов, минифицированных, сжатых и оптимизированных в каталоге `dist`.