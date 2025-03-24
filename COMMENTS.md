Данный пакет состоит из функций:
__alt_mark_hold_package() Помечает программу как "Остановлено обновление", делает соответствующую запись в файле /etc/apt/apt.conf.d/hold-$pkg.conf
__alt_test_glob() проверка на наличие недопустимых символов в команде "*" и "?", в случае нахождения выводит сообщение об ошибке (пример epm mark hold *get)
__alt_mark_hold() дополнительно проверяет программы apt и далее выполняет функцию __alt_mark_hold_package() 
__alt_mark_unhold() Снимает ограничение по обновлению программы и делает корректировки в файле /etc/apt/apt.conf.d/hold-$pkg.conf 
__alt_mark_showhold() скинуть ошибки в /dev/null, отобразить список программ с остановленным обновлением
epm_mark_hold() функция отвечающая за команду epm mark hold
epm_mark_unhold() функция отвечающая за команду epm mark unhold
epm_mark_showhold() функция отвечающая за команду epm mark showhold
epm_mark_checkhold() функция отвечающая за команду epm mark checkhold
epm_mark_auto() функция отвечающая за команду epm mark auto
epm_mark_manual() функция отвечающая за команду epm mark manual
epm_mark_showauto() функция отвечающая за команду epm mark showauto
epm_mark_showmanual() функция отвечающая за команду epm mark showmanual
epm_mark_help() функция отвечающая за команды  epm mark help / epm mark -h / epm mark --help
