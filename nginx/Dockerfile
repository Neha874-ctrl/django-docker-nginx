FROM python:3.11-slim

# system deps for mysqlclient
RUN apt-get update && apt-get install -y \
    gcc \
    default-libmysqlclient-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# collect static AFTER mysqlclient works
RUN python manage.py collectstatic --noinput

CMD ["gunicorn", "notesapp.wsgi:application", "--bind", "0.0.0.0:8000"]
