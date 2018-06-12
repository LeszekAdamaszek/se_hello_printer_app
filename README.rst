Simple Flask App
================

Aplikacja Dydaktyczna wyświetlająca imię i wiadomość w różnych formatach dla zajęć
o Continuous Integration, Continuous Delivery i Continuous Deployment.

- Rozpocząnając pracę z projektem (wykorzystując virtualenv). Hermetyczne środowisko dla pojedyńczej aplikacji w python-ie:

  ::

    source /usr/bin/virtualenvwrapper.sh
    mkvirtualenv wsb-simple-flask-app
    pip install -r requirements.txt
    pip install -r test_requirements.txt

- Uruchamianie applikacji:

  ::

    # jako zwykły program
    python main.py

    # albo:
    PYTHONPATH=. FLASK_APP=hello_world flask run

- Uruchamianie testów (see: http://doc.pytest.org/en/latest/capture.html):

  ::

    PYTHONPATH=. py.test
    PYTHONPATH=. py.test  --verbose -s


- Uruchamianie testów z coverage:

  PYTHONPATH=. py.test --verbose -s --cov=.

Smoke test:

  curl --fail 127.0.0.1:5000
  curl -s -o /dev/null -w "%{http_code}" --fail 127.0.0.1:5000



- Kontynuując pracę z projektem, aktywowanie hermetycznego środowiska dla aplikacji py:

  ::

    source /usr/bin/virtualenvwrapper.sh
    workon wsb-simple-flask-app


- Integracja z TravisCI:

    docker_build:
    docker build -t $(MY_DOCKER_NAME) .

    docker_run: docker_build
      docker run \
      --name $(SERVICE_NAME)-dev \
      -p 5000:5000 \
      -d $(MY_DOCKER_NAME)

      docker_stop:
      docker stop $(SERVICE_NAME)-dev

    USERNAME=LeszekAdamaszek
    TAG=$(USERNAME)/$(MY_DOCKER_NAME)

    docker_push: docker_build
    @docker login --username $(USERNAME) --password $${DOCKER_PASSWORD}; \
    docker tag $(MY_DOCKER_NAME) $(TAG); \
    docker tag $(MY_DOCKER_NAME) $(TAG):$$(cat VERSION); \
    docker push $(TAG); \
    docker logout;

W TravisCI dodajemy zmienną o nazwie: DOCKER_PASSWORD, gdzie podajemy nasze hasło do dockera

Odpalanie komend z pliku Makefile:

  make deps # Sprawdź co robi komenda w pliku Makefile
  make lint
  make test
  make run
  make docker_build
  make docker_run
  make docker_stop
  make docker_push


Pomocnicze
==========

- Instalacja python virtualenv i virtualenvwrapper:

  ::

    yum install python-pip
    pip install -U pip
    pip install virtualenv
    pip install virtualenvwrapper

- Instalacja docker-a:

  ::

    yum remove docker \
        docker-common \
        container-selinux \
        docker-selinux \
        docker-engine

    yum install -y yum-utils

    yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo

    yum makecache fast
    yum install docker-ce
    systemctl start docker

Materiały
=========

- https://virtualenvwrapper.readthedocs.io/en/latest/

.. image:: https://travis-ci.org/LeszekAdamaszek/se_hello_printer_app.svg?branch=master
    :target: https://travis-ci.org/LeszekAdamaszek/se_hello_printer_app

.. image:: https://app.statuscake.com/button/index.php?Track=XclvE1NdNk&Days=1&Design=1
    :target: https://www.statuscake.com
