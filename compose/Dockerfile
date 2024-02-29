FROM python:3.9-slim-buster as base

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

ARG BUILD_ENV=local

WORKDIR /app


# Instala dependencias comunes
# RUN apk update && apk add --no-cache \
#     python3 \
#     python3-dev \
#     gcc \
#     libc-dev \
#     libpq \
#     postgresql-client\
#     py3-pip



RUN apt-get update && apt-get install --no-install-recommends -y \
  # dependencies for building Python packages
  build-essential \
  libpq-dev  \
  nano gcc \
  libcap2 libcap2-bin libpam-cap iputils-ping gettext \
  python3-dev  graphviz  

RUN addgroup --system django \
   && adduser --system --ingroup django django 

RUN  chmod +x   /app



# Requirements are installed here to ensure they will be cached.

# Use wheels to install Python dependencies
# COPY ../requirements .

# RUN pip install -r requirements/${BUILD_ENV}.txt

COPY ../config/entrypoint /config/entrypoint
RUN sed -i 's/\r$//g' /config/entrypoint
RUN chmod +x /config/entrypoint

COPY ./start.sh /start
RUN sed -i 's/\r$//g' /start
RUN chmod +x /start

COPY ../ ./app

USER django
# CMD ["/app/start.sh"]
ENTRYPOINT ["config/entrypoint"]