FROM       debian:jessie
MAINTAINER julien@rottenberg.info

WORKDIR   /opt/newsblur

COPY      requirements.txt /opt/newsblur/

RUN       BUILD_PACKAGES="locales build-essential gfortran libxslt1-dev libxml2-dev zlib1g-dev lib32ncurses5-dev libjpeg-dev libpq-dev libblas-dev liblapack-dev" && \
          RUNTIME_PACKAGES="python-pip python-requests-whl python-dev" && \
          apt-get update --fix-missing && \
          apt-get install -y $BUILD_PACKAGES $RUNTIME_PACKAGES   && \
          locale-gen en_GB.UTF-8 && \
          pip install -r requirements.txt && \
          pip install image psycopg2 numpy scipy && \
          rm -rf ~/.cache/pip && \
          apt-get remove --purge -y $BUILD_PACKAGES `apt-mark showauto` && \
          rm -rf /var/lib/apt/lists/*

ENV       LANG=en_GB.UTF-8 \
          LC_CTYPE=en_GB.UTF-8

# encoding
RUN       echo "import sys; reload(sys); sys.setdefaultencoding('utf-8')" | tee /usr/lib/python2.7/sitecustomize.py

COPY      . /opt/newsblur/
RUN       cp docker/local_settings.py .
CMD       gunicorn_django --bind 0.0.0.0:8000
