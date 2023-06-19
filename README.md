# CI/CD Gitlab
## Домашнее задание. Горбунов Владимир

## Содержание

- [Задание 1. Gitlab на локальном сервере](#Задание-1)
- [Задание 2. Настройка пайплайна в Gitlab](#Задание-2-3)  

## Задание 1

>Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в этом репозитории.</br>
Создайте новый проект и пустой репозиторий в нём.</br>
Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на  которой запущен GitLab.</br>
В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

![](./img/gitlab-runner.jpg)`

## Задание 2-3

>Запушьте репозиторий на GitLab, изменив origin. Это изучалось на занятии по Git.</br>
Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.</br>
В качестве ответа в шаблон с решением добавьте:</br>
файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне;</br>
скриншоты с успешно собранными сборками.</br>
Измените CI так, чтобы:</br>
этап сборки запускался сразу, не дожидаясь результатов тестов; </br>
тесты запускались только при изменении файлов с расширением *.go. </br>

Чтобы билд запускался не дожидаясь теста, в билде указал:
```
  dependencies: []
```
Тест запускается только при изменении файлов *.go:
```
  only:
   changes:
     - '*.go'
```

```
stages:
  - test
  - build
  - analysis

test:
  stage: test
  image: golang:1.17
  script: 
   - go test .
  only:
   changes:
     - '*.go'
     
analysis:
  stage: test
  image:
    name: sonarsource/sonar-scanner-cli
    entrypoint: [""]
  variables:
  script:
  - sonar-scanner -Dsonar.projectKey=gitlab -Dsonar.sources=. -Dsonar.host.url=http://gitlab.localdomain:9000 -Dsonar.login=sqa_4508113f8a900343ddc6fa99d02dc81282d4bd05
 

build:
  stage: build
  image: golang:1.17
  script:
   - go build -a -installsuffix nocgo -o goapp .
  dependencies: []
```

https://github.com/Night-N/7-gitlab/blob/master/img/gitlab-ci.yml

![](./img/gitlab%20with%20sonar.jpg)`