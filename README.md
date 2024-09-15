![alt text](https://github.com/mezhibo/ansible_expluatation/blob/043bbea12f1bc0c292f9f734f16b404762a73998/IMG/1.jpg)

**Подготовка к выполнению**

1. В Yandex Cloud создайте новый инстанс (4CPU4RAM) на основе образа jetbrains/teamcity-server.

2. Дождитесь запуска teamcity, выполните первоначальную настройку.

3. Создайте ещё один инстанс (2CPU4RAM) на основе образа jetbrains/teamcity-agent. Пропишите к нему переменную окружения SERVER_URL: "http://<teamcity_url>:8111".

4. Авторизуйте агент.

5. Сделайте fork репозитория.

6. Создайте VM (2CPU4RAM) и запустите playbook.


**Основная часть**

1. Создайте новый проект в teamcity на основе fork.

2. Сделайте autodetect конфигурации.

3. Сохраните необходимые шаги, запустите первую сборку master.

4. Поменяйте условия сборки: если сборка по ветке master, то должен происходит mvn clean deploy, иначе mvn clean test.

5. Для deploy будет необходимо загрузить settings.xml в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus.

6. В pom.xml необходимо поменять ссылки на репозиторий и nexus.

7. Запустите сборку по master, убедитесь, что всё прошло успешно и артефакт появился в nexus.

8. Мигрируйте build configuration в репозиторий.

9. Создайте отдельную ветку feature/add_reply в репозитории.

10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово hunter.

11. Дополните тест для нового метода на поиск слова hunter в новой реплике.

12. Сделайте push всех изменений в новую ветку репозитория.

13. Убедитесь, что сборка самостоятельно запустилась, тесты прошли успешно.

14. Внесите изменения из произвольной ветки feature/add_reply в master через Merge.

15. Убедитесь, что нет собранного артефакта в сборке по ветке master.

16. Настройте конфигурацию так, чтобы она собирала .jar в артефакты сборки.

17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны.

18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity.

19. В ответе пришлите ссылку на репозиторий.



**Решение**

Создаем 3 ВМ под наши нужды

![alt text](https://github.com/mezhibo/teamcity/blob/c0bf5a11056e6cf9a7b924048df5f52475ab6308/IMG/1.jpg)


Авторизуем агента и делаем autodeteck конфигурации


![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/2.jpg)


Запускаем билд

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/3.jpg)


Смотрим результат выполнения ![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/4.jpg)


Создаем 2 шага по условиям сборки: если сборка по ветке master, то должен происходит mvn clean deploy, иначе mvn clean test

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/5.jpg)


Содержимое первого шага 

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/6.jpg)


Содержимое второго шага 

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/7.jpg)


Запускаем билд и видим что все отработало 

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/8.jpg)

Перейдем в Nexus и првоерим наличие созданного нами артефакта 

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/9.jpg)



Теперь смигрируем build configuration в репозиторий и вот [ССЫЛКА](https://github.com/mezhibo/example-teamcity/tree/4731b69beb2204494467a8a4b02ba39e122ceb01/.teamcity/MezhiboBild)


Теперь создадим новую ветку feature/add_reply

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/10.jpg)



Дополним новый метод для приложения в папках main и test

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/11.jpg)


![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/12.jpg)


И изменим версию приложения для корректного отображения в дальнейшем на нашем Nexus-репозитории

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/18.jpg)


Теперь пушим все в новую ветку feature/add_reply

Переходим в teamcity и визим что у нас автоматически при пуше запустилась сборка по ветке feature/add_reply

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/13.jpg)


Посмотрим какой коммит отработал, и вилим что тот, который только что сделал


![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/14.jpg)


Перключаемся на нашу основную ветку master и делаем слияние с ветки feature/add_reply в ветку master


![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/15.jpg)


Перейдем обратно в проект, видим что опять пошлша сборка, но уже в ветке master

![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/16.jpg)


Перейдем в Nexus и видим что у нас появилась новая версия нашего артефакта, которую мы создавали в рамках ветки feature/add_reply, но потом сделали merge в master и получили новый артефакт.


![alt text](https://github.com/mezhibo/teamcity/blob/83c321410f1cbf3c2fbc5c3a74459a7bda5f63f6/IMG/17.jpg)



Так как из условия выше мы создали 2 правила, то у нас при пуше в ветку отличную от master, деплой приложения в nexus производиться не будет

Если же у нас изменения запушены в ветку master то наше приложение отправится в хранилище артефактов nexus

[ССЫЛКА НА РЕПОЗИТОРИЙ](https://github.com/mezhibo/example-teamcity.git)









