Для проведения тестирования откроем терминал и подключимся к серверу с установленным менеджером пакетов EPM версии 3.64.15-eter0.p10.1

Далее вводим команды для проверки корректности выводимой информации.

Test №1

	epm mark -h wget

Для первого теста воспользуемся стандартным выведением помощи epm merk -h с дополнительным аргументом wget.

В консоли (терминале) введем команду epm mark -h wget

mark — это интерфейс для маркировки пакетов

   -h|--help|help        help
   
   hold                  mark the given package(s) as held back
   
   unhold                unset the given package(s) as held back
   
   showhold              print the list of packages on hold
   
   checkhold             return true if the package is on hold
   
   auto|remove           mark the given package(s) as automatically installed
   
   manual|install        mark the given package(s) as manually installed
   
   showauto              print the list of automatically installed packages
   
   showmanual            print the list of manually installed packages

Примеры:
  
  epm mark hold mc
  
  epm manual mc

Test №1 отработал корректно (у epm mark -h не должно быть аргумента)

Test №2

Проведём тест на корректность отработки команды epm mark help.

В консоли (терминале) введем команду epm mark help

	epm mark help

mark — это интерфейс для маркировки пакетов

   -h|--help|help        help
   
   hold                  mark the given package(s) as held back
   
   unhold                unset the given package(s) as held back
   
   showhold              print the list of packages on hold
   
   checkhold             return true if the package is on hold
   
   auto|remove           mark the given package(s) as automatically installed
   
   manual|install        mark the given package(s) as manually installed
   
   showauto              print the list of automatically installed packages
   
   showmanual            print the list of manually installed packages

Примеры:

  epm mark hold mc
  
  epm manual mc
  
Test №2 отработал корректно

Test №3

Проверим как отработает команда epm mark hold с аргументом git, который не установлен в систему.

В консоли (терминале) введем команду epm merk hold git

	epm mark hold git
 
 # rpm -q --queryformat %{name}\n -- git

пакет git не установлен

Test №3 отработал корректно (в списке установленных пакетов отсутсвует пакет git)

Test №4

Проверим как отработает команда epm mark hold c аргументом wget, который присутсвует в системе.

В консоли (терминале) введем команду epm mark hold wget

	epm mark hold wget

 # rpm -q --queryformat %{name}\n -- wget
 
 # echo "RPM::Hold {"^wget";};" > /etc/apt/apt.conf.d/hold-wget.conf

Test №4 отработал корректно (у программы остановлено обновление и информация занесена в файл hold-wget.conf)

Test №5

Проверим как отработает команда epm mark unhold с аргументом wget, который уже маркирован как "Остановить обновления".

В консоли (терминале) введем команду epm mark unhold wget

	epm mark unhold wget

 # rpm -q --queryformat %{name}\n -- wget
 
 # rm -fv /etc/apt/apt.conf.d/hold-wget.conf
 
 Test №5 отработал корректно (у программы возобновлено обновление и из файла hold-wget.conf удалено ограничение)

Test №6

Проверим как отработает команда epm mark showhold.

В консоли (терминале) введем команду epm mark showhold

	epm mark showhold

wget

mc

Test №6 отработал корректно (выведен список всех программ с отключенным обновлением)

Test №7

Проверим как отработает команда epm mark checkhold с аргументом wget, который уже маркирован как "Остановить обновления"

В консоли (терминале) введем команду epm mark checkhold wget

	epm mark checkhold wget

Test №7 отработал некорректно (при наличии в списке всех программ с отключенным обновлением wget, данную информацию команда не предоставила)

Далее произведён анализ файла epm-mark по пути /usr/share/eepm/

epm_mark_checkhold()
{
# workaround against epm assure questions
case $PMTYPE in
    dnf-rpm)
        # there is no hold entries without versionlock
        __dnf_is_supported_versionlock || return 1
        __dnf_assure_versionlock
        load_helper epm-query
        docmd dnf versionlock list | grep "^$1" | sed -e 's|\.\*$||' | grep -v " " | filter_pkgnames_to_short | grep -q "^$1$"
        return
	;;
esac

epm_mark_showhold | grep -q "^$1$"

}

Присутсвуют ошибки в строках:

10: docmd dnf versionlock list | grep "^$1" | sed -e 's|\.\*$||' | grep -v " " | filter_pkgnames_to_short | grep -q "^$1$"

15: epm_mark_showhold | grep -q "^$1$"

Согласно данным https://www.altlinux.org/Grep_start требуется делать поиск слова опцией -i

После замены опции с -q на -i команда epm mark checkhold отработала.

Test №8 после внесения исправлений в файл epm-mark

	epm mark checkhold wget
wget

Test №8 отработал корректно

Test №9

Проверим как отработает команда epm mark unhold с аргументом wge*, который уже маркирован как "Остановить обновления".

В консоли (терминале) введем команду epm mark unhold wge*

	epm mark unhold wge*

WARNING: В имени пакета wge* есть запрещенные символы. Пропущено.

ERROR: Query: package name is missed  (you can discuss this problem (epm 3.64.15-eter0.p10.1 on ALTLinux/p10) in Telegram: https://t.me/useepm)

Test №9 отработал корректно

Test №10

epm mark unhold wge?

Проверим как отработает команда epm mark unhold с аргументом wge?, который уже маркирован как "Остановить обновления".

В консоли (терминале) введем команду epm mark wnhold wge?

# rpm -q --queryformat %{name}\n -- wge?

пакет wge? не установлен

Test №10 отработал некорректно.(согласно функции __alt_test_glob() символы * и ? не допустимы)

Функция отвечающая за проверку и выведение ошибки.

__alt_test_glob()
{
    echo "$*" | grep -q "\.[*?]" && warning "Only glob symbols * and ? are supported. Don't use regexp here!"
}

Присутсвует ошибка в grep -q "\.[*?]" . Но я пока не могу ответить, точнее.
