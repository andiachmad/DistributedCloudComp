# Stage 1: Build dependencies using Poetry
FROM python:3.12-alpine AS builder

ENV POETRY_VERSION=1.8.2
WORKDIR /app

# Install curl untuk install poetry
RUN apk add --no-cache curl && \
    curl -sSL https://install.python-poetry.org | python3 - && \
    apk del curl

ENV PATH="/root/.local/bin:$PATH"

COPY pyproject.toml poetry.lock ./

RUN poetry export -f requirements.txt --without-hashes --only main > requirements.txt

# Stage 2: Final image
FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /app/requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["sh", "-c", "python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]
