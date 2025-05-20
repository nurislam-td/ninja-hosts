# for startup build
FROM 3.12.10-slim AS base


FROM base AS builder
COPY --from=ghcr.io/astral-sh/uv:0.5.2 /uv  /bin/
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy VIRTUAL_ENV=/opt/venv
WORKDIR /app

COPY uv.lock pyproject.toml /app/


RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project --no-dev

COPY . /app

RUN mv /app/.venv ${VIRTUAL_ENV}

# -----------------------------------------------------------------------------------------------------------------------------
FROM base AS runtime

EXPOSE 8000


COPY ./compose/local/django/start /start
RUN sed -i 's/\r$//g' /start
RUN chmod +x /start

COPY ./compose/local/django/celery/worker/start /start-celeryworker
RUN sed -i 's/\r$//g' /start-celeryworker
RUN chmod +x /start-celeryworker

COPY ./compose/local/django/celery/beat/start /start-celerybeat
RUN sed -i 's/\r$//g' /start-celerybeat
RUN chmod +x /start-celerybeat

COPY ./compose/local/django/celery/flower/start /start-flower
RUN sed -i 's/\r$//g' /start-flower
RUN chmod +x /start-flower


COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"


COPY --from=builder /app /app

WORKDIR /app




