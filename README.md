# Ansible таски для развертывания инфраструктуры в Selectel
Такски поделены на две части:

- Создание проекта
- Разворот серверов

P.S. Идемпотентность не сохраняется, сервера создаются через раз. Эти таски скорее для первоначального создания инфраструктуры, а не для управления ей.

### Подготовка окружения
------

##### Установка модулей


```
virtualenv --no-site-packages env
source env/bin/activate
```

```
pip install git+https://github.com/selectel/ansible-selvpc-modules
pip install shade jmespath
```

##### Настройка доступа 


Для работы с Resell API нужны ключи. 
Зарегистрированные пользователи Selectel могут получить их здесь https://my.selectel.ru/profile/apikeys/

```
export SEL_URL=https://api.selectel.ru/vpc/resell/
export SEL_TOKEN=<ваш API-ключ полученный выше в панели>
export OS_PROJECT_DOMAIN_NAME=<ваш логин на my.selectel.ru>
export OS_USER_DOMAIN_NAME=<аналогично предыдущему>
```

Для облегчения жизни и хождений на хосты через Ansible выставим переменную окружения ANSIBLE_HOST_KEY_CHECKING в значение False:

`export ANSIBLE_HOST_KEY_CHECKING=False`

Подготовить и переименовать файл в infra.yml:

`vars/infra.yml.exmaple`

### Процесс разворота
------

1. `ansible-playbook infra-create.yml`
2. Руками, в личном кабинете необходимо создать локальную сеть, в Москве с названием nat
3. `ansible-playbook server-create.yml`
