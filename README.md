# Patterns For Large-Scale JavaScript Application Architecture
####  Addy Osmani

Переклад на українську мову. Addy Osmani[1]
Використаний [переклад][4]

### Запуск сайту локально

    
    cd project-directory/
    # Один раз встановити залежності
    bundle install
    # Запустити блог. Буде доступний на 4000-му порті localhost'а
    jekyll serve --watch --drafts

Для просмотра черновиков используйте ключ `--drafts`, якщо ж чернетки
не потрібні - просто пропустіть цей ключ.

### Файли перекладу

Глави англійською мовою і українською лежать [тут][2]

### Гайдлайн

Ми дотримуємося гайдлайна по оформленню markdown від Frontender Magazine.
Ознайомитися з ним можна [тут][3]



### Верстка і все таке

    # Запуск Stylus
    stylus -w -o assets/css/ ./_stylus/main.styl

    # Запуск jekyll
    jekyll serve --watch --drafts


[1]: https://twitter.com/addyosmani
[2]: https://github.com/x51xxx/Patterns-For-Large-Scale-JavaScript-Application-Architecture/tree/gh-pages/_includes/translation
[3]: https://github.com/FMRobot/FM-guidelines
[3]: https://github.com/shuvalov-anton/largescaleJS_ru
