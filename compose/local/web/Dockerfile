FROM python:3.8-slim-buster

ENV PYTHONUNBUFFERED 1
# PYTHONDONTWRITEBYTECODE=1 tell Python to not write bytecode (.pyc) and __pycache__ directory on local env.
ENV PYTHONDONTWRITEBYTECODE 1

# Install system packages required by Wagtail and Django.
RUN apt-get update --yes --quiet && apt-get install --yes --quiet --no-install-recommends \
    build-essential \
    libpq-dev \
    libmariadbclient-dev \
    libjpeg62-turbo-dev \
    zlib1g-dev \
    libwebp-dev \
 && rm -rf /var/lib/apt/lists/*

# Requirements are installed here to ensure they will be cached.
COPY ./requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

COPY ./compose/local/web/entrypoint /entrypoint
# RUN sed -i 's/\r$//g' /entrypoint is used to process the line endings of the shell scripts, which converts Windows line endings to UNIX line endings.
RUN sed -i 's/\r$//g' /entrypoint
RUN chmod +x /entrypoint

COPY ./compose/local/web/start /start
RUN sed -i 's/\r$//g' /start
RUN chmod +x /start

# In docker-compose.yml, we config docker volume .:/app, so here we set WORKDIR /app. If we edit code on host machine, then the code change can also been seen in /app of the docker container.
WORKDIR /app

ENTRYPOINT ["/entrypoint"]