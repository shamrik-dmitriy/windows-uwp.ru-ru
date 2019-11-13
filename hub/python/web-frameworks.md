---
title: Веб-разработка с помощью Python в Windows
description: Как приступить к использованию Python для разработки веб-приложений в Windows, включая настройку для таких платформ, как Flask и Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python в Windows, Python Web с WSL, веб-приложение Python с подсистемой Windows для Linux, веб-разработка Python в Windows, приложение Flask в Windows, Django приложение в Windows, Python Web, Flask Web dev в Windows, Django Web dev в Windows, Windows Web Dev с Python, VS Code Python Web Dev, Remote WSL Extension, Ubuntu, WSL, венв, PIP, расширение Microsoft Python, запуск Python в Windows, использование Python в Windows, сборка с помощью Python в Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 285e5149778f2d5cb63554a5af63bb9ae23809dc
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314948"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Приступая к работе с Python для разработки веб-приложений в Windows

Ниже приведено пошаговое руководство по началу работы с Python для веб-разработки в Windows с помощью подсистемы Windows для Linux (WSL).

## <a name="set-up-your-development-environment"></a>Настройка среды разработки

Мы рекомендуем установить Python на WSL при создании веб-приложений. Многие руководства и инструкции для разработки веб-приложений на языке Python написаны для пользователей Linux и используют средства упаковки и установки на основе Linux. Большинство веб-приложений также развертываются в Linux, поэтому это обеспечит согласованность между средой разработки и рабочими средами.

Если вы используете Python не для разработки веб-приложений, мы рекомендуем установить Python непосредственно в Windows 10 с помощью Microsoft Store. WSL не поддерживает рабочие столы графического пользовательского интерфейса или приложения (например, PyGame, GNOME, KDE и т. д.). Установите и используйте Python непосредственно в Windows в этих случаях. Если вы не знакомы с Python, ознакомьтесь с нашим руководством: Начните [использовать Python в Windows для начинающих](./beginners.md). Если вы заинтересованы в автоматизации общих задач в операционной системе, ознакомьтесь с нашим руководством: Приступая [к работе с Python в Windows для написания сценариев и автоматизации](./scripting.md). В некоторых расширенных сценариях можно загрузить определенный выпуск Python непосредственно из [Python.org](https://www.python.org/downloads/windows/) или установить [другой вариант](https://www.python.org/download/alternatives), например Anaconda, Jython, PyPy, винписон, IronPython и т. д. Это рекомендуется только в том случае, если вы являетесь более сложным программистом Python с определенной причиной выбора альтернативной реализации.

## <a name="enable-windows-subsystem-for-linux"></a>Включение подсистемы Windows для Linux

WSL позволяет запускать среду GNU/Linux, включая большинство программ командной строки, служебные программы и приложения, непосредственно в Windows, неизмененные и полностью интегрированные с файловой системой Windows и избранными инструментами, такими как Visual Studio Code. Перед включением WSL убедитесь, что у вас установлена последняя [версия Windows 10](https://www.microsoft.com/software-download/windows10).

Чтобы включить WSL на компьютере, необходимо:

1. Перейдите в меню " **Пуск** " (нижний левый значок Windows), введите "Включение или отключение компонентов Windows" и выберите ссылку на **Панель управления** , чтобы открыть всплывающее меню " **компоненты Windows** ". Найдите в списке "подсистема Windows для Linux" и установите флажок, чтобы включить эту функцию.

2. При появлении соответствующего запроса перезагрузите компьютер.

## <a name="install-a-linux-distribution"></a>Установка дистрибутива Linux

Для работы в WSL доступно несколько дистрибутивов Linux. Вы можете найти и установить Избранное в Microsoft Store. Мы рекомендуем начать с [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , так как он является актуальным, популярным и хорошо поддерживаемым.

1. Откройте эту ссылку [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , откройте Microsoft Store и выберите **получить**. *(Это довольно большая загрузка, и для установки может потребоваться некоторое время.)*

2. После завершения скачивания выберите **запустить** из Microsoft Store или запустите, введя "Ubuntu 18,04 LTS" в меню " **Пуск** ".

3. При первом запуске распределения вам будет предложено создать имя учетной записи и пароль. После этого вы автоматически войдете в систему от имени этого пользователя по умолчанию. Можно выбрать любое имя пользователя и пароль. Они не влияют на имя пользователя Windows.

Вы можете проверить дистрибутив Linux, который используется в данный момент, введя: `lsb_release -d`. Чтобы обновить дистрибутив Ubuntu, используйте команду: `sudo apt update && sudo apt upgrade`. Рекомендуем регулярно обновлять, чтобы убедиться, что у вас есть самые последние пакеты. Windows не обрабатывает это обновление автоматически. Ссылки на другие дистрибутивы Linux, доступные в Microsoft Store, альтернативных методах установки или устранении неполадок см. в статье [руководство по установке подсистемы Windows для Linux для Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="set-up-visual-studio-code"></a>Настройка Visual Studio Code

Воспользуйтесь преимуществами [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [linting](https://code.visualstudio.com/docs/python/linting), [поддержки отладки](https://code.visualstudio.com/docs/python/debugging), [фрагментами кода](https://code.visualstudio.com/docs/editor/userdefinedsnippets)и [модульного тестирования](https://code.visualstudio.com/docs/python/unit-testing) с помощью VS Code. VS Code хорошо интегрируется с подсистемой Windows для Linux, предоставляя [встроенный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal) для создания простого рабочего процесса между редактором кода и командной строкой, а также поддержки [Git для управления версиями](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) с помощью общего Git команды (добавления, фиксации, принудительной отправки, извлечения), встроенные непосредственно в пользовательский интерфейс.

1. [Скачайте и установите VS Code для Windows](https://code.visualstudio.com). VS Code также доступен для Linux, но подсистема Windows для Linux не поддерживает приложения GUI, поэтому нам нужно установить ее в Windows. Не волнуйтесь, вы по-прежнему сможете интегрироваться с командной строкой и инструментами Linux с помощью расширения Remote-WSL.

2. Установите [расширение Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) на VS Code. Это позволяет использовать WSL в качестве интегрированной среды разработки и будет работать с совместимостью и путем. [Подробнее](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Если у вас уже установлен VS Code, то для установки [расширения Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)необходимо убедиться в наличии [версии 1,35](https://code.visualstudio.com/updates/v1_35) или более поздней версии. Не рекомендуется использовать WSL в VS Code без расширения Remote-WSL, так как будет потеряна поддержка автоматического завершения, отладки, linting и т. д. Забавный факт: Это расширение WSL устанавливается в $HOME/.вскоде-сервер/екстенсионс.

## <a name="create-a-new-project"></a>Создание нового проекта

Давайте создадим новый каталог проекта в нашей файловой системе Linux (Ubuntu), который затем будет работать с приложениями и инструментами Linux с помощью VS Code.

1. Закройте VS Code и откройте Ubuntu 18,04 (Командная строка WSL), перейдя в меню " **Пуск** " (нижний левый значок Windows) и введя: "Ubuntu 18,04".

2. В командной строке Ubuntu перейдите к папке, в которую нужно поместить проект, и создайте для нее каталог: `mkdir HelloWorld`.

![Терминал Ubuntu](../images/ubuntu-terminal.png)

> [!TIP]
> При использовании подсистемы Windows для Linux (WSL) важно помнить, что **теперь вы работаете между двумя различными файловыми системами**: 1) файловая система Windows и 2) файловая система Linux (WSL), которая является Ubuntu для нашего примера. Необходимо обратить внимание на место установки пакетов и файлов хранения. Можно установить одну версию средства или пакета в файловой системе Windows и совершенно другую версию в файловой системе Linux. Обновление средства в файловой системе Windows не будет оказывать влияния на средство в файловой системе Linux и наоборот. WSL подключает жесткие диски компьютера в папке `/mnt/<drive>` в дистрибутиве Linux. Например, диск Windows C: подключен к `/mnt/c/`. Вы можете получить доступ к файлам Windows с помощью терминала Ubuntu и использовать приложения и инструменты Linux для этих файлов и наоборот. Мы рекомендуем работать в файловой системе Linux для разработки веб-приложений Python, учитывая, что большая часть веб-инструментов изначально написана для Linux и развернута в рабочей среде Linux. Это также позволяет избежать смешанной семантики файловой системы (например, Windows не учитывает регистр в отношении имен файлов). С другой стороны, WSL теперь поддерживает переход между системами файлов Linux и Windows, что позволяет размещать файлы на одном из них. [Подробнее](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). Мы также рады поделиться этим [WSL2ом в ближайшее время до Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) и предложим некоторые полезные улучшения. Вы можете [опробовать его сейчас на этапе Windows Insider build 18917](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Установка Python, PIP и венв

Ubuntu 18,04 LTS поставляется с уже установленным Python 3,6, но не поставляется с некоторыми из модулей, которые могут быть получены с другими установками Python. Нам по-прежнему потребуется установить **PIP**, Стандартный диспетчер пакетов для Python и **венв**, стандартный модуль, используемый для создания и управления облегченными виртуальными средами.  

1. Убедитесь, что Python3 уже установлен, открыв терминал Ubuntu и введя: `python3 --version`. Это должно вернуть номер версии Python. Если вам нужно обновить версию Python, сначала обновите версию Ubuntu, введя: `sudo apt update && sudo apt upgrade`, а затем обновите Python с помощью `sudo apt upgrade python3`.

2. Установите **PIP** , введя: `sudo apt install python3-pip`. PIP позволяет устанавливать и управлять дополнительными пакетами, которые не входят в стандартную библиотеку Python.

3. Установите **венв** , введя: `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Создание виртуальной среды

Использование виртуальных сред — Рекомендуемая рекомендация для проектов разработки Python. Создав виртуальную среду, можно изолировать средства проекта и избежать конфликтов управления версиями с инструментами для других проектов. Например, вы можете обслуживать старые веб-проекты, для которых требуется веб-платформа Django 1,2, но тогда новый проект поступает с помощью Django 2,2. Если вы обновляете Django глобально, за пределами виртуальной среды, вы можете столкнуться с некоторыми проблемами управления версиями позже. В дополнение к предотвращению случайных конфликтов управления версиями виртуальные среды позволяют устанавливать пакеты и управлять ими без прав администратора.

1. Откройте терминал и в папке проекта *HelloWorld* используйте следующую команду, чтобы создать виртуальную среду с именем **. венв**: `python3 -m venv .venv`.

2. Чтобы активировать виртуальное окружение, введите: `source .venv/bin/activate`. Если он сработал, перед командной строкой должен отобразиться **(. венв)** . Теперь у вас есть автономная среда, готовая к написанию кода и установке пакетов. Завершив работу с виртуальной средой, введите следующую команду, чтобы отключить ее: `deactivate`.

    ![Создание виртуальной среды](../images/wsl-venv.png)

> [!TIP]
> Рекомендуется создать виртуальную среду в каталоге, в котором планируется использовать проект. Поскольку каждый проект должен иметь собственный отдельный каталог, каждый из них будет иметь собственную виртуальную среду, поэтому нет необходимости в уникальном именовании. Наше предложение заключается в использовании Name **. венв** для соблюдения соглашения Python. Некоторые средства (например, пипенв) также устанавливаются по умолчанию в каталог проекта. Вы не хотите использовать **. env** , так как это противоречит файлам определения переменных среды. Обычно не рекомендуется использовать имена, отличные от точек, так как вам не требуется `ls`, чтобы вы не задумали, что каталог существует. Мы также рекомендуем добавить **. венв** в файл gitignore. (Вот [шаблон gitignore по умолчанию GitHub для Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) для справки.) Дополнительные сведения о работе с виртуальными окружениями в VS Code см. [в разделе Использование окружений Python в VS Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Открытие WSL удаленного окна

VS Code использует расширение Remote-WSL (установлено ранее), чтобы обрабатывать подсистему Linux как удаленный сервер. Это позволяет использовать WSL в качестве интегрированной среды разработки. [Подробнее](https://code.visualstudio.com/docs/remote/wsl). 

1. Откройте папку проекта в VS Code из терминала Ubuntu, введя: `code .` (символ "." указывает VS Code открыть текущую папку).

2. Оповещение системы безопасности будет всплывающим из защитника Windows, выберите "разрешить доступ". После VS Code откроется индикатор узла удаленного подключения в нижнем левом углу, что позволит вам узнать, что вы редактируете в **WSL: Ubuntu-18.04 @ no__t-0.

    ![Индикатор узла удаленного подключения VS Code](../images/wsl-remote-extension.png)

3. Закройте терминал Ubuntu. Перемещаясь вперед, мы будем использовать терминальный WSL, интегрированный в VS Code.

4. Откройте терминал WSL в VS Code, нажав клавиши **CTRL + '** (с помощью символа обратной кавычки) или **Просмотр** > **терминал**. Откроется командная строка bash (WSL), открытая для пути к папке проекта, созданной в терминале Ubuntu.

    ![VS Code с терминалом WSL](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Установка расширения Microsoft Python

Вам потребуется установить все расширения VS Code для удаленного WSL. Расширения, уже установленные локально на VS Code, не будут доступны автоматически. [Подробнее](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Откройте окно расширения VS Code. для этого **нажмите клавиши CTRL + SHIFT + X** (или используйте меню, чтобы перейти к **просмотру** **расширений** > ).

2. В поле лучшие **расширения поиска в Marketplace** введите:  **Python**.

3. Найдите расширение **Python (MS-Python. Python) по** расширению Майкрософт и нажмите зеленую кнопку " **установить** ".

4. После завершения установки расширения необходимо нажать синюю кнопку Перезагрузить **обязательные** . Это приведет к перезагрузке VS Code и отображению **WSL: UBUNTU-18,04-установлен раздел @ no__t-0 в окне расширений VS Code, в котором показано, что вы установили расширение Python.

## <a name="run-a-simple-python-program"></a>Запуск простой программы Python

Python — интерпретируемый язык, поддерживающий различные типы интерпретаторов (python2, Anaconda, PyPy и т. д.). VS Code должен по умолчанию присвоить интерпретатору, связанному с проектом. Если у вас есть причина изменить ее, выберите интерпретатор, который сейчас отображается в синей строке в нижней части окна VS Code или откройте **палитру команд** (Ctrl + Shift + P) и введите команду **Python: Выберите интерпретатор @ no__t-0. Отобразится список установленных на данный момент интерпретаторов Python. Дополнительные [сведения о настройке окружений Python](https://code.visualstudio.com/docs/python/environments).

Давайте создадим и запустим простую программу Python в качестве теста и убедитесь, что выбран правильный интерпретатор Python.

1. Откройте VS Code окно проводника, **нажмите клавиши CTRL + SHIFT + E** (или используйте меню, чтобы перейти к **просмотру** > **Explorer**).

2. Если он еще не открыт, откройте встроенный терминал WSL, нажав **клавиши Ctrl + Shift + '** , и убедитесь, что выбрана папка **HelloWorld** Python Project.

3. Создайте файл Python, введя: `touch test.py`. Файл, который вы только что создали, появится в окне обозревателя в папках. венв и. vscode, которые уже находятся в каталоге проекта.

4. Выберите файл **Test.py** , который вы только что создали в окне обозревателя, чтобы открыть его в VS Code. Так как файл. Копировать в нашем имени файла сообщает VS Code, что это файл Python, ранее загруженное расширение Python автоматически выбирает и загружает интерпретатор Python, отображаемый в нижней части окна VS Code.

    ![Выбор интерпретатора Python в VS Code](../images/interpreterselection.gif)

5. Вставьте этот код Python в файл test.py, а затем сохраните файл (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. Чтобы запустить только что созданную программу Python "Hello World", выберите файл **Test.py** в окне обозревателя VS Code, а затем щелкните файл правой кнопкой мыши, чтобы открыть меню параметров. Выберите **запустить файл Python в терминале**. Кроме того, в окне терминала Integrated WSL введите: `python test.py`, чтобы запустить программу "Hello World". Интерпретатор Python выполнит печать "Hello World" в окне терминала.

Поздравляю. Все готово для создания и запуска программ Python! Теперь давайте попробуем создать Hello World приложение с двумя наиболее популярными веб-платформами Python: Flask и Django.

## <a name="hello-world-tutorial-for-flask"></a>Hello World учебник по Flask

[Flask](http://flask.pocoo.org/) — это платформа веб-приложений для Python. В этом кратком руководстве вы создадите небольшое приложение Flask "Hello World" с помощью VS Code и WSL.

1. Откройте Ubuntu 18,04 (Командная строка WSL), перейдите в меню " **Пуск** " (нижний левый значок Windows) и введите: "Ubuntu 18,04".

2. Создайте каталог для проекта: `mkdir HelloWorld-Flask`, затем `cd HelloWorld-Flask`, чтобы войти в каталог.

3. Создание виртуальной среды для установки инструментов проекта: `python3 -m venv .venv`

4. Откройте проект **HelloWorld-Flask** в VS Code, введя команду `code .`.

5. В VS Code откройте интегрированный терминал WSL (или bash), нажав **клавиши Ctrl + Shift + '** (папка проекта **HelloWorld-Flask** уже должна быть выбрана). *Закройте командную строку Ubuntu, так как мы будем работать в терминальном WSL, интегрированном с VS Code перемещения вперед.*

6. Активируйте виртуальную среду, созданную на шаге #3 с помощью терминала Bash в VS Code: `source .venv/bin/activate`. Если он сработал, перед командной строкой должен отобразиться (. венв).

7. Установите Flask в виртуальной среде, введя: `python3 -m pip install flask`. Убедитесь, что он установлен, введя: `python3 -m flask --version`.

8. Создайте новый файл для кода Python: `touch app.py`

9. Откройте файл **app.py** в проводнике VS Code (`Ctrl+Shift+E`, а затем выберите файл App.py). Будет активировано расширение Python для выбора интерпретатора. По умолчанию используется **Python 3.6.8 64-bit ('. венв ': венв)** . Обратите внимание, что она также обнаружила виртуальную среду.

    ![Активированная виртуальная среда](../images/virtual-environment.png)

10. В **app.py**добавьте код для импорта Flask и создайте экземпляр объекта Flask:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. Кроме того, в **app.py**добавьте функцию, которая возвращает содержимое, в данном случае — простую строку. С помощью декоратора **app. Route** Flask СОПОСТАВЬТЕ маршрут URL "/" с этой функцией:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Можно использовать несколько декораторов в одной и той же функции, по одной на одну строку, в зависимости от того, сколько разных маршрутов нужно сопоставлять с одной и той же функцией.

12. Сохраните файл **app.py** (**CTRL + S**).

13. В окне терминала запустите приложение, введя следующую команду:

    ```python
    python3 -m flask run
    ```

    Будет запущен сервер разработки Flask. Сервер разработки ищет **app.py** по умолчанию. При запуске Flask должны отобразиться выходные данные следующего вида:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Откройте веб-браузер по умолчанию на странице, подготовленной к просмотру, **удерживая клавишу CTRL, щелкните** в окне терминала URL-адрес http://127.0.0.1:5000/. В браузере должно отобразиться следующее сообщение:

    ![Привет, Flask!](../images/hello-flask.png)

15. Обратите внимание, что при посещении URL-адреса, например "/", в терминале отладки появится сообщение, показывающее HTTP-запрос:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Закройте приложение, нажав **клавиши CTRL + C** в окне терминала.

> [!TIP]
> Если вы хотите использовать имя файла, отличное от **app.py**, например **Program.py**, определите переменную среды с именем **FLASK_APP** и задайте в качестве значения свой выбранный файл. Сервер разработки Flask использует значение **FLASK_APP** вместо файла **app.py**по умолчанию. Дополнительные сведения см. в разделе [Документация по интерфейсу командной строки Flask](http://flask.pocoo.org/docs/1.0/cli/).

Поздравляем, вы создали веб-приложение Flask с помощью Visual Studio Code и подсистемы Windows для Linux! Более подробное руководство по использованию VS Code и Flask см. в разделе [Flask Tutorial в Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Hello World учебник по Django

[Django](https://www.djangoproject.com) — это платформа веб-приложений для Python. В этом кратком руководстве вы создадите небольшое приложение Django "Hello World" с помощью VS Code и WSL.

1. Откройте Ubuntu 18,04 (Командная строка WSL), перейдите в меню " **Пуск** " (нижний левый значок Windows) и введите: "Ubuntu 18,04".

2. Создайте каталог для проекта: `mkdir HelloWorld-Django`, затем `cd HelloWorld-Django`, чтобы войти в каталог.

3. Создание виртуальной среды для установки инструментов проекта: `python3 -m venv .venv`

4. Откройте проект **HelloWorld-DJango** в VS Code, введя команду `code .`.

5. В VS Code откройте интегрированный терминал WSL (или bash), нажав **клавиши Ctrl + Shift + '** (папка проекта **HelloWorld-Django** уже должна быть выбрана). *Закройте командную строку Ubuntu, так как мы будем работать в терминальном WSL, интегрированном с VS Code перемещения вперед.*

6. Активируйте виртуальную среду, созданную на шаге #3 с помощью терминала Bash в VS Code: `source .venv/bin/activate`. Если он сработал, перед командной строкой должен отобразиться (. венв).

7. Установите Django в виртуальной среде с помощью команды: `python3 -m pip install django`. Убедитесь, что он установлен, введя: `python3 -m django --version`.

8. Затем выполните следующую команду, чтобы создать проект Django:

    ```bash
    django-admin startproject web_project .
    ```

    Команда `startproject` предполагает (с помощью `.` в конце), что текущая папка является папкой проекта, и создает в ней следующее:

    - `manage.py`. Служебная программа командной строки Django для проекта. Вы запускаете административные команды для проекта, используя `python manage.py <command> [options]`.

    - Вложенная папка с именем `web_project`, которая содержит следующие файлы:
        - `__init__.py`: пустой файл, указывающий Python на то, что эта папка является пакетом Python.
        - `wsgi.py`: точка входа для веб-серверов, совместимых с WSGI, для обслуживания проекта. Обычно этот файл остается без изменений, так как он предоставляет обработчики для рабочих веб-серверов.
        - `settings.py`: содержит параметры для проекта Django, которые изменяются в процессе разработки веб-приложения.
        - `urls.py`: содержит оглавление для проекта Django, который также изменяется в процессе разработки.

9. Чтобы проверить проект Django, запустите сервер разработки Django с помощью команды `python3 manage.py runserver`. Сервер работает на порте 8000 по умолчанию, и в окне терминала должны отобразиться выходные данные, аналогичные приведенным ниже.

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    При первом запуске сервера он создает базу данных SQLite по умолчанию в файле `db.sqlite3`, который предназначен для целей разработки, но может использоваться в рабочей среде для веб-приложений с низким уровнем громкости. Кроме того, встроенный веб-сервер Django предназначен *только* для местных целей разработки. Однако при развертывании на веб-узле Django использует вместо этого веб-сервер узла. Модуль `wsgi.py` в проекте Django отвечает за подключение к рабочим серверам.

    Если вы хотите использовать порт, отличный от порта по умолчанию 8000, укажите номер порта в командной строке, например `python3 manage.py runserver 5000`.

10. `Ctrl+click` URL-адрес `http://127.0.0.1:8000/` в окне "вывод терминала", чтобы открыть браузер по умолчанию для этого адреса. Если Django установлен правильно и проект является допустимым, вы увидите страницу по умолчанию. В окне VS Code терминального вывода также отображается журнал сервера.

11. Когда все будет готово, закройте окно браузера и приостанавливаете работу сервера в VS Code с помощью `Ctrl+C`, как указано в окне "вывод терминала".

12. Теперь, чтобы создать приложение Django, запустите в папке проекта команду `startapp` программы администрирования (где `manage.py`):

    ```bash
    python3 manage.py startapp hello
    ```

    Команда создает папку с именем `hello`, которая содержит несколько файлов кода и одну вложенную папку. Из них часто приходится работать с `views.py` (которые содержат функции, определяющие страницы в веб-приложении) и `models.py` (которые содержат классы, определяющие объекты данных). Папка `migrations` используется программой администрирования Django для управления версиями базы данных, как описано далее в этом руководстве. Существуют также файлы `apps.py` (Конфигурация приложения), `admin.py` (для создания административного интерфейса) и `tests.py` (для тестов), которые здесь не рассматриваются.

13. Измените значение `hello/views.py`, чтобы оно соответствовало следующему коду, который создает одно представление для домашней страницы приложения:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Создайте файл `hello/urls.py` с содержимым ниже. В файле `urls.py` указываются шаблоны для маршрутизации различных URL-адресов в соответствующие представления. Приведенный ниже код содержит один маршрут для отображения корневого URL-адреса приложения (`""`) к функции `views.home`, которую вы только что добавили в `hello/views.py`:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. Папка `web_project` также содержит файл `urls.py`, где фактически обрабатывается маршрутизация URL-адресов. Откройте `web_project/urls.py` и измените его в соответствии с приведенным ниже кодом (при необходимости можно оставить комментарии с указанием). Этот код извлекает в приложении `hello/urls.py`, используя `django.urls.include`, который хранит маршруты приложения, содержащиеся в приложении. Это разделение полезно, если проект содержит несколько приложений.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Сохраните все измененные файлы.

17. В окне терминала VS Code запустите сервер разработки с `python manage.py runserver` и откройте браузер в `http://127.0.0.1:8000/`, чтобы увидеть страницу, которая визуализирует "Hello, Django".

Поздравляем, вы создали веб-приложение Django с помощью VS Code и подсистемы Windows для Linux! Более подробное руководство по использованию VS Code и Django см. в разделе [Django Tutorial в Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Учебник по Python с VS Code](https://code.visualstudio.com/docs/python/python-tutorial): Вводный учебник по VS Code в качестве среды Python, в основном способах редактирования, запуска и отладки кода.
- [Поддержка Git в VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Узнайте, как использовать основы управления версиями Git в VS Code.  
- [Узнайте об обновлениях, которые вскоре появятся в WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index): Эта новая версия изменяет, как дистрибутивы Linux взаимодействуют с Windows, повышая производительность файловой системы и добавляя полную совместимость системных вызовов.
- [Работа с несколькими дистрибутивами Linux в Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Узнайте, как управлять различными дистрибутивами Linux на компьютере Windows.