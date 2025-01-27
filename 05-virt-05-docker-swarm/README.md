# Домашнее задание к занятию 5. «Оркестрация кластером Docker контейнеров на примере Docker Swarm»

## Как сдавать задания

Обязательны к выполнению задачи без звёздочки. Их нужно выполнить, чтобы получить зачёт и диплом о профессиональной переподготовке.

Задачи со звёдочкой (*) — это дополнительные задачи и/или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---


## Важно

1. Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

2. [Ссылки для установки открытого ПО](https://github.com/netology-code/devops-materials/blob/master/README.md).

---

## Задача 1

Дайте письменые ответы на вопросы:

- В чём отличие режимов работы сервисов в Docker Swarm-кластере: replication и global?

> global - означает, что данный сервис будет запущен ровно в одном экземпляре на всех возможных нодах. А replicated означает, что n-ое кол-во контейнеров для данного сервиса будет запущено на всех доступных нодах.

- Какой алгоритм выбора лидера используется в Docker Swarm-кластере?

> алгоритм поддержания распределенного консенсуса — Raft. 
  Когда кластер теряет лидера, то другие ноды (фолловеры) переходят в статус кандидатов, после чего с задержкой в диапазоне 150-300ms они голосуют за себя, как за нового лидера и отправляют это сообщение другим кандидатам, если кандидат получивший такое сообщение ещё не проголосовал в этот момент, то он 
  отдаёт свой голос тому кандидату от которого получил сообщение. Далее, после выбора лидера, он отправляет кандидатам сообщения (heartbeat timeout), подтверждающие доступность лидера, если лидер окажется недоступен - ноды не получат такого сообщения и голосование начнётся вновь, будет выбран новый лидер.

- Что такое Overlay Network?

> Эти сети служат общей подсетью для контейнеров единой сети, в которой они могут обмениваться данными напрямую (даже если они запущены на разных физических хостах).
  Контейнер, запущенный на swarm-кластере, по умолчанию может быть соединён с тремя и более overlay-сетями. Первая сеть, docker_gwbridge, позволяет контейнерам поддерживать связь с внешним миром. Сеть ingress нужна только для того, чтобы устанавливать входящие соединения из внешнего мира. И сети overlay, 
  которые создает сам пользователь и их можно прикрепить к контейнерам. 
 
## Задача 2

Создайте ваш первый Docker Swarm-кластер в Яндекс Облаке.

Чтобы получить зачёт, предоставьте скриншот из терминала (консоли) с выводом команды:
```
docker node ls
```

![screenshot](https://i.ibb.co/vd23XYM/Screenshot-from-2023-09-18-13-21-40.png)

## Задача 3

Создайте ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Чтобы получить зачёт, предоставьте скриншот из терминала (консоли), с выводом команды:
```
docker service ls
```

![screenshot](https://i.ibb.co/4Vms4CQ/Screenshot-from-2023-09-18-13-23-34.png)

## Задача 4 (*)

Выполните на лидере Docker Swarm-кластера команду, указанную ниже, и дайте письменное описание её функционала — что она делает и зачем нужна:
```
# см.документацию: https://docs.docker.com/engine/swarm/swarm_manager_locking/
docker swarm update --autolock=true
```
> Эта команда шифрует ключи TSL и журналы Raft хранящиеся у каждого экземпляра менеджера кластера (и у каждого вновь добавленного менеджера), пока Docker находится в состоянии покоя. Это делается в том числе для того, чтобы обеспечить использование Docker-secrets, который хранит конфиденциальные данные в журнале Raft.
При каждом перезапуске Docker-swarm будет необходимо ввести ключ для разблокировки вручную.

