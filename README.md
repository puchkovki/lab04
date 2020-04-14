## Laboratory work IV

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Выполнить инструкцию учебного материала
- [x] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=puchkovki #initialize value GITHUB_USERNAME
$ export GITHUB_TOKEN=6eeb83b2805cfc3d1ae428ba598de34d1f6ca74f #initialize value GITHUB_TOKEN
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace #go to the workspace directory
$ pushd . #save the current working directory in memory so it can be returned to at any time
/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace ~
$ source scripts/activate #read and execute file activate
```

```ShellSession
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles # update packages
Turning on ignore dotfiles mode.
Downloading https://github.com/rvm/rvm/archive/master.tar.gz
Installing RVM to /home/puchkovki/.rvm/
Installation of RVM in /home/puchkovki/.rvm/ is almost complete:

  * To start using RVM you need to run `source /home/puchkovki/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ . scripts/activate
$ rvm autolibs disable # disable automatic dependency installation
$ rvm install ruby-2.4.2 # install ruby
Searching for binary rubies, this might take some time.
Found remote file https://rubies.travis-ci.org/ubuntu/18.04/x86_64/ruby-2.4.2.tar.bz2
ruby-2.4.2 - #configure
ruby-2.4.2 - #download
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
100 17.5M  100 17.5M    0     0   134k      0  0:02:13  0:02:13 --:--:-- 95172
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.4.2 - #validate archive
ruby-2.4.2 - #extract
ruby-2.4.2 - #validate binary
ruby-2.4.2 - #setup
ruby-2.4.2 - #gemset created /home/puchkovki/.rvm/gems/ruby-2.4.2@global
ruby-2.4.2 - #importing gemset /home/puchkovki/.rvm/gemsets/global.gems..................................
ruby-2.4.2 - #generating global wrappers.......
ruby-2.4.2 - #gemset created /home/puchkovki/.rvm/gems/ruby-2.4.2
ruby-2.4.2 - #importing gemsetfile /home/puchkovki/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.4.2 - #generating default wrappers.......
$ rvm use 2.4.2 --default # set a default ruby language manager
Using /home/puchkovki/.rvm/gems/ruby-2.4.2
$ gem install travis # install travis configurations
Done installing documentation for multipart-post, faraday, faraday_middleware, highline, backports, net-http-pipeline, net-http-persistent, addressable, multi_json, gh, launchy, ffi, ethon, typhoeus, websocket, pusher-client, travis after 191 seconds
17 gems installed
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04 #clone the previous lab
Cloning into 'projects/lab04'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 22 (delta 2), reused 22 (delta 2), pack-reused 0
Unpacking objects: 100% (22/22), done.
$ cd projects/lab04 #go to the current lab directory
$ git remote remove origin #remove remote origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04 #add remote origin from the url
```

```ShellSession
$ cat > .travis.yml <<EOF #install instruments required for the compilation of cpp files
language: cpp
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF #what to do in the build phase 

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF #add extra packages

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

```ShellSession
$ travis login --pro --github-token ${GITHUB_TOKEN}  #command line utility for the optimisation work with Travis
```

```ShellSession
$ travis lint #check the yml file for the validity
Hooray, .travis.yml looks valid :)
```

```ShellSession
$ ex -sc '1i|[![Build Status](https://travis-ci.com/puchkovki/lab04.svg?branch=master)](https://travis-ci.com/puchkovki/lab04)' -cx README.md
```

```ShellSession
$ git add .travis.yml # add changes
$ git add README.md # add changes
$ git commit -m"added CI" #commit changes
$ git push origin master #push changes
Username for 'https://github.com': puchkovki
Password for 'https://puchkovki@github.com':
Counting objects: 26, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (26/26), 4.25 KiB | 31.00 KiB/s, done.
Total 26 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), done.
To https://github.com/puchkovki/lab04.git
 * [new branch]      master -> master
```

```ShellSession
$ travis lint #check the yml file for the validity
Hooray, .travis.yml looks valid :)
$ travis accounts --pro #show accounts on Travis
puchkovki (Киря): educational account, 10 repositories
$ travis sync --pro #synchronize repos on Github and Travis
synchronizing: . done
$ travis repos --pro #show repos
puchkovki/AMV (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

puchkovki/Distributed-systems (active: yes, admin: yes, push: yes, pull: yes)
Description: Семестровый курс по параллельному программированию для студентов 3 курса ФУПМ, МФТИ.

puchkovki/Industrial-programming (active: yes, admin: yes, push: yes, pull: yes)
Description: Industrial programming course for MIPT

puchkovki/Interviews (active: yes, admin: yes, push: yes, pull: yes)
Description: Tasks for the interviews

puchkovki/MathCompl (active: yes, admin: yes, push: yes, pull: yes)
Description: Here are practical exercises of the Computational Math

puchkovki/Microsha (active: yes, admin: yes, push: yes, pull: yes)
Description: First homework of IT

puchkovki/lab02 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

puchkovki/lab03 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

puchkovki/lab04 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

puchkovki/parallel-programming-class (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

trmigor/CppLA (active: no, admin: no, push: yes, pull: yes)
Description: C++ Linear algebra library

trmigor/Judex (active: no, admin: no, push: yes, pull: yes)
Description: Contest system
$ travis enable --pro #which repos are enable now
Detected repository as puchkovki/lab04, is this correct? |yes| yes
puchkovki/lab04: enabled :)
$ travis whatsup --pro #projects status                                          
puchkovki/lab04 passed: #3
$ travis branches --pro #infromation about branches
master:  #3    passed     Set theme jekyll-theme-hacker
$ travis history --pro #show building history

#3 passed:       master Set theme jekyll-theme-hacker
#2 passed:       master added CI
#1 passed:       master added CI
$ travis show --pro #show last building status

Job #3.1:  Set theme jekyll-theme-hacker
State:         passed
Type:          push
Branch:        master
Compare URL:   https://github.com/puchkovki/lab04/compare/88ce364c23be...c893996500a7
Duration:      38 sec
Started:       2020-03-25 22:24:16
Finished:      2020-03-25 22:24:54
Allow Failure: false
Config:        os: linux
```

## Report

```ShellSession
$ popd # get the directory from the stack
/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace ~
$ export LAB_NUMBER=04 #initialize the variable LAB_NUMBER
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} # clone lab
Cloning into 'tasks/lab04'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 69 (delta 0), reused 0 (delta 0), pack-reused 66
Unpacking objects: 100% (69/69), done.
$ mkdir reports/lab${LAB_NUMBER} # make directory
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md # copy and rename file
$ cd reports/lab${LAB_NUMBER} # change directory
$ edit REPORT.md # make report
$ gist REPORT.md
```
## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2015-2019 The ISC Authors
```
