# [Firefox Developer Tools Docs](https://docs.firefox-dev.tools/) [![Travis tests][travis-image]][travis-url]

This repository contains scripts that download the sources, *render* and publish the docs, which are hosted elsewhere (currently in the `devtools/docs/` folder of [this repository](https://github.com/mozilla/gecko-dev)).

The automation happens on Travis CI. You can check the [status of the builds](https://travis-ci.org/firefox-devtools/docs) if you're curious.

## Available npm scripts

### `npm run update`

Updates the external repository we use as One Source Of Truth™.

Due to the way the external repository is generated (it is a Mercurial to Git conversion at the moment), we have to delete and clone again on each run, as the commit hashes don't match each time the process runs, and git cannot `pull`. In the future, we might be able to run `git pull` instead.

### `npm run build`

Renders the documentation, using the `docs` folder in the cloned repository as source and [GitBook](https://github.com/GitbookIO/gitbook) as renderer.

The rendered contents are saved in the `output` folder.

### `npm run travis-build`

Shortcut to run `update` and `build` sequentially, for a more succcinct `.travis.yml` file.

### `npm run travis-publish`

Publishes the contents of `output` to the `gh-pages` branch, using the [gh-pages-travis](https://www.npmjs.com/package/gh-pages-travis) module.

The scripts whose name starts with `travis-` are intended to be run in Travis CI, just in case it wasn't obvious.

In Travis, `travis-build` will be run first. If it's successful, `travis-publish` is called. Have a look at [.travis.yml](./.travis.yml) for more details.

## How was this set up?

* Clone repo
* Run `npm install`
* Go to **Repository settings**, make sure *GitHub pages* are enabled, and select the `gh-pages` branch
* You should be able to access your repo using the address displayed by GitHub (something like `[yourusername].github.io/[repositoryname]`)
* Enable this repository in Travis, from [your Travis dashboard](https://travis-ci.org/profile). If you don't have one, you'll have to create an account there. If you do, you might need to *Sync account*, so the repository shows up in the list. Then flick the switch on!
* Generate and set up deploy keys for the repository (run these commands when inside the repo folder):
  * `ssh-keygen -f id_rsa -t rsa -C "your_email@example.com"` (when asked, leave the passphrase empty).
  * Add the contents of `id_rsa.pub` as a deploy key for your Github repository: in `https://github.com/<your name>/<your repo>/settings/keys` - make sure to allow 'write' access or Travis won't be able to push
  * Install Travis CLI client: `gem install travis`
  * Log into the CLI and encrypt the private key you generated, `id_rsa`:
    * `travis login`
    * `travis encrypt-file id_rsa --add` (will add the decrypt command to recreate `id_rsa` in the current folder as a `before_install` script in `.travis.yml`)
  * Delete `id_rsa` and `id_rsa.pub`: `rm id_rsa id_rsa.pub`
  * Add `id_rsa.enc` to git and commit: `git add id_rsa.enc; git commit -m 'add encrypted key'`
* Make sure the values in the `env` section in `.travis.yml` are correct, update where necessary

That should be it!

If you push a commit to GitHub and Travis runs the script successfully and a new version is published on the `gh-pages` branch, looks like you're in a good position to enable Travis' `Cron` from the project settings in Travis. And then the process will be run automatically for you.

**Note** that if you revoke the deploy key in GitHub, then Travis won't be able to push to the repo. You'll need to generate a new one again, encrypt it with the Travis client, and update, commit and publish the `id_rsa.enc` file.

**Also note** that we are copying the `CNAME` file to the `output` folder before publishing because we use a custom domain and for some obscure reason GitHub keeps forgetting it (or resetting it to blank) if the domain is not set in the repository's `gh-pages` branch.



[travis-image]: https://travis-ci.org/firefox-devtools/docs.svg?branch=master
[travis-url]: https://travis-ci.org/firefox-devtools/doc

--------------------------------------------------------------
# [Документация по инструментам разработчика Firefox] (https://docs.firefox-dev.tools/) [! [Travis tests] [travis-image]] [travis-url]

 Этот репозиторий содержит скрипты, которые загружают исходники, * визуализируют * и публикуют документы, которые размещены в другом месте (в настоящее время в папке `devtools / docs /` [этого репозитория] (https://github.com/mozilla/gecko-  dev)).
 Автоматизация происходит на Travis CI.  Если вам интересно, вы можете проверить [статус сборок] (https://travis-ci.org/firefox-devtools/docs).

 ## Доступные сценарии npm

 ### `npm run update`

 Обновляет внешний репозиторий, который мы используем как One Source Of Truth ™.
 Из-за способа создания внешнего репозитория (на данный момент это преобразование Mercurial в Git), мы должны удалять и клонировать снова при каждом запуске, так как хэши фиксации не совпадают каждый раз при запуске процесса, а git не может  `тянуть`.  В будущем мы могли бы вместо этого запускать `git pull`.

 ### `npm run build`

 Визуализирует документацию, используя папку `docs` в клонированном репозитории в качестве источника и [GitBook] (https://github.com/GitbookIO/gitbook) в качестве средства визуализации.
 Визуализированное содержимое сохраняется в папке output.

 ### `npm run travis-build`

 Ярлык для последовательного запуска `update` и` build` для более лаконичного файла `.travis.yml`.

 ### `npm run travis-publish`

 Публикует содержимое `output` в ветке` gh-pages`, используя модуль [gh-pages-travis] (https://www.npmjs.com/package/gh-pages-travis).
 Скрипты, имя которых начинается с «travis-», предназначены для запуска в Travis CI, на всякий случай, если это не было очевидно.
 В Travis сначала запускается travis-build.  В случае успеха вызывается `travis-publish`.  См. [.Travis.yml] (./. Travis.yml) для получения более подробной информации.

 ## Как это было настроено?

 * Клонировать репо
 * Запустите `npm install`
 * Перейдите в ** Настройки репозитория **, убедитесь, что * страницы GitHub * включены, и выберите ветку `gh-pages`
 * Вы должны иметь возможность получить доступ к своему репо, используя адрес, отображаемый GitHub (что-то вроде `[yourusername] .github.io / [repositoryname]`)
 * Включите этот репозиторий в Travis с [вашей панели управления Travis] (https://travis-ci.org/profile).  Если у вас его нет, вам придется создать там учетную запись.  Если вы это сделаете, вам может потребоваться * Синхронизировать учетную запись *, чтобы репозиторий появился в списке.  Затем включите переключатель!
 * Сгенерируйте и настройте ключи развертывания для репозитория (запустите эти команды, находясь в папке репозитория):
   * `ssh-keygen -f id_rsa -t rsa -C" your_email@example.com "` (при запросе оставьте кодовую фразу пустой).
   * Добавьте содержимое `id_rsa.pub` в качестве ключа развертывания для вашего репозитория Github: в` https://github.com/<ваше имя> / <ваше репо> / settings / keys` - обязательно разрешите запись  'доступ, иначе Трэвис не сможет нажать
   * Установите клиент Travis CLI: `gem install travis`
   * Войдите в интерфейс командной строки и зашифруйте сгенерированный вами закрытый ключ, id_rsa:
     * `трэвис логин`
     * `travis encrypt-file id_rsa --add` (добавит команду дешифрования для воссоздания id_rsa в текущей папке в виде сценария before_install в .travis.yml)
   * Удалите id_rsa и id_rsa.pub: rm id_rsa id_rsa.pub
   * Добавьте `id_rsa.enc` в git и зафиксируйте:` git add id_rsa.enc;  git commit -m 'добавить зашифрованный ключ' '
 * Убедитесь, что значения в разделе `env` в` .travis.yml` верны, при необходимости обновите

 Так и должно быть!
 Если вы отправляете фиксацию в GitHub, и Трэвис успешно запускает скрипт, и новая версия публикуется в ветке gh-pages, похоже, вы в хорошем положении, чтобы включить Travis 'Cron' из настроек проекта в Travis.  .  И тогда процесс будет запущен автоматически.

 ** Обратите внимание **: если вы отзовете ключ развертывания в GitHub, Трэвис не сможет отправить его в репо.  Вам нужно будет снова сгенерировать новый, зашифровать его с помощью клиента Travis, а также обновить, зафиксировать и опубликовать файл id_rsa.enc.
 ** Также обратите внимание **, что мы копируем файл `CNAME` в папку` output` перед публикацией, потому что мы используем собственный домен и по какой-то непонятной причине GitHub продолжает его забывать (или сбрасывать его на пустой), если домен не  устанавливается в ветке репозитория `gh-pages`.



 [travis-image]: https://travis-ci.org/firefox-devtools/docs.svg?branch=master
 [travis-url]: https://travis-ci.org/firefox-devtools/docs
