Вывод справочной информации epm mark -h / epm mark -help / epm mark help ;

Маркировать программу как "Остановлено обновление" epm mark hold <программа>, выполняется Администратором (пользователь root);

Снять маркировку "Остановлено обновление" с программы epm mark unhold <программа>,  выполняется Администратором (пользователь root);

Вывести список всех программ с маркером  epm mark showhold ;

Проверить определённую программу на наличие маркера "Остановлено обновление" epm mark checkhold <программа> ;

Маркировать программу как "Установлено автоматически" epm mark auto <программа> / epm mark remove <программа> ,  выполняется Администратором (пользователь root);

Маркировать программу как "Установлено вручную" epm mark manual <программа> / epm mark install <программа>,  выполняется Администратором (пользователь root);

Вывести список всех программ с маркером "Установлено автоматически" epm mark showauto ,  выполняется Администратором (пользователь root);

Вывести список всех программ с маркером "Установлено вручную" epm mark showmanual ,  выполняется Администратором (пользователь root).

Использование:

epm mark [опции] [аргументы]

    epm mark hold wget 
При выполнении команды используется опция (действие) hold, над аргументом (программой) wget.
В результате выполнения команды epm mark hold wget, программа маркируется как "Остановлено обновление".

    epm mark chekhold mc

При выполнении команды используется опция (действие) checkhold, над аргументом (программой) mc.
В результате программа mc проверяется на наличие маркировки "Остановлено обновление" и в случае присутствия маркера, выводится название программы, в случае отсутствия маркера, информация не выводится.

    epm mark auto htop
При выполнении команды используется опция (действие) auto, над аргументом (программой) htop.
В результате выполнения команды epm mark auto htop, программа маркируется как "Установлено автоматически".

Из замеченного мной могу отметить, что часть команд доступна как под пользователем без прав su, так и под суперпользователем.
И по маркировке опциями hold, unhold, auto\remove и manual\install доступными только через суперпользователя не возникло.
Вот по ограничениям на отображение списка showauto и showmanual только суперпользователем возникло не понимание.
В чём приоритет вывода данного списка выше списка showhold или проверки программы с ограничением обновления checkhold.
