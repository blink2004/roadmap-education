# Roadmap of education
Roadmap of education as Automation QA: integration, optimization and scaling.

- e. 	  - estimation
- s.t. 	- spent time

For example, in brackets indicates `(e. 5h, s.t. 1d)` or `(5h 1d)`. This means that estimated time for this part is 5 hours but spent time is 1 day.

## Allure with Jenkins
### PHPUnit
* [Execute from local files](#execute-from-local-files) (e. 3h, s.t. 2d )
* Execute from GitHub (2d)
* Execute from local files by pipeline (3d)
* Execute from GitHub by pipeline (3d)
### TestNG
* Execute from local files (e. 3d)
* Execute from GitHub (e. 3d)
* Execute from local files by pipeline (3d)
* Execute from GitHub by pipeline (3d)
## PostMan
### Primarily
* Parameterize of tests (2d)
* Pre-requests (3d)
* Run one request before another, for example authorization (3d)
* Validating the JSON response against the appropriate schema (3d)
### Secondarily
* Mock-services (1w)
## Performance testing through JMeter (1M)
## Grafana monitoring (2w)
## Data base
### Primarily
* database backup, archiving and send by email (3d + 1h + 3h)
* JOIN - types (2d)
* indexes, keys (3d)
* B-tree (3d)
* EXPLAIN (3d)
* EXTENDED EXPLAIN (2d)
* stored procedures, triggers, etc.(1w)
### Secondarily
* pull of connects (3d)
* master - slave (4d)
## Jenkins
* Создать несколько jobs и запустить их по очереди/параллельно в одной job'e (1w)
* Написание своих Pipelines (2w)
* Запустить тесты Postman'a (3d)
## WebServer
### Primarily
* Own mail client (3d)
## Security
* Code defactoring
* Kali Linux
* Xenotix
* SQLMap
* nMap

## Allure
### Execute from local files
You have PHP project with PHPUnit tests. For example located on `/var/www/html/project/demo`. 

1. Add Allure to PHPUnit test https://github.com/allure-framework/allure-phpunit.
2. Install in Jenkins plugins `Allure Jenkins Plugin`.
3. Create new free configuration Job.
4. - [x] `Set delete old builds`
5. Build -> Execute shell command:
```shell
cd /var/www/html/project/demo
php ./vendor/bin/phpunit --coverage-html ./tests/report/ --log-junit ./build/allure-results/junit.xml
/var/lib/jenkins/tools/ru.yandex.qatools.allure.jenkins.tools.AllureCommandlineInstallation/2.13.5/bin/allure generate -c -o ./build/allure-report -- ./build/allure-results
cp -r ./build/allure-results ${WORKSPACE}/
```
6. Add `after build step` -> `Allure report`. Set:
```
Results: ${WORKSPACE}/allure-results
Report path: allure-report
```
Add required access rights for files and folders.
