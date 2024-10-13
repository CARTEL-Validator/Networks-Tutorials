# Установка и настройка GAIA

Gaia — это основная реализация протокола Cosmos SDK, который позволяет создавать блокчейны. Она служит демонстрационным примером для разработчиков, показывая, как использовать возможности Cosmos SDK для создания собственных децентрализованных приложений и систем.

Gaia включает в себя функциональность, необходимую для управления цифровыми активами, обработки транзакций и взаимодействия между различными блокчейнами через протокол IBC (Inter-Blockchain Communication). Это делает его важной частью экосистемы Cosmos, которая стремится к созданию сети взаимосвязанных блокчейнов.

С помощью Gaia разработчики могут легко разрабатывать и развертывать свои собственные блокчейны с минимальными усилиями.

## Основной сайт
[hub.cosmos.network](https://hub.cosmos.network/main)

---

## Тестовая сборка и установка
1. Заходим на сайт [https://github.com/finteh/gaia](https://github.com/finteh/gaia) и делаем форк репозитария к себе.

![github_fork.png](images/gaia/github_fork.png)

> Обязательно снимите флажок _Copy the main branch only_
![copy_main_branch.png](images/gaia/copy_main_branch.png)

2. Копируем адрес репозитория и клонируем его командой 

![clone_repo.png](images/gaia/clone_repo-1.png)

и клонируем его командой
``` Shell
git clone <repo-path>
```
![clone_repo-2.png](images/gaia/clone_repo-2.png)

3. Узнаём нужную Binary Version (ветку) по адресу  [https://www.mintscan.io/cosmos/parameters/](https://www.mintscan.io/cosmos/parameters/)

![binary_version.png](images/gaia/binary_version.png)
 
4. Переключаемся на нужную версию командой _git checkout <branch>_

``` Shell
git checkout v20.0.0
```

>Список версий можно посмотреть командой: `git branch`

>Загрузить последние обновление с удалённого репозитория можно командой: `git pull `


5. Проверяем версию GO
``` Shell
go version
```
Если GO не установлена или версия не актуальная, устанавливаем по ссылке [https://go.dev/doc/install](https://go.dev/doc/install)
(проверяем номер версии на актуальность)

Команды:
``` Shell
 wget https://go.dev/dl/go1.23.2.linux-amd64.tar.gz
 rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.2.linux-amd64.tar.gz
 export PATH=$PATH:/usr/local/go/bin
```
> Если GO установлен, но не находится - проверьте (и обновите пути)
``` Shell
export PATH=$PATH:/usr/local/go/bin
```
6. Заходим в репозиторий gaia через консоль
``` Shell
cd  <path>/gaia/
```
и запускаем сборку  
``` Shell
make build
```
В папке __build__ должен появиться бинарный файл _gaiad_

7. Проверяем версию командой 
``` Shell
./gaiad version
```
Если указана нужная версия, то бинарный файл собран успешно
![build_ok.png](images/gaia/build_ok.png)

---

## Настройка GAIA для Genesis-Валидатора (новая сеть)
### Делаем gentx-файл
1. Инициализируем ноду
```Shell
gaiad init <NAME_of_VALIDATOR> --chain-id <chain_id>
```
Пример:
```Shell
gaiad init Cartel --chain-id dvs42
```
>После этого в домашней директории создаётся папка `.gaia` с конфигурационными файлами

2. Создаём кошелёк
```Shell
gaiad keys add <wallet_name>
```
> Можно использовать аппаратный кошелёк Ledger

3. Создаём genesis-аккаунт с балансом в минимальных единицах
```Shell
gaiad genesis add-genesis-account <wallet_name> 10000000uatom
```

>Внимание!
> 
>Наименование монет может быть sputnik и stake
> 
>Зависит от ... 
 

Пример:
```Shell
./gaiad genesis add-genesis-account wallet 10000000stake
```

3.
```Shell
gaiad genesis gentx <wallet_name> 10000000uatom --chain-id <chain-id>
```
Пример:
```Shell
./gaiad genesis gentx wallet 10000000stake--chain-id dvs42
```


---

## Настройка GAIA для подключения нового валидатора к уже существующей сети



