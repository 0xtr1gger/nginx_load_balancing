FROM python:3.12-slim

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5002

CMD ["gunicorn", "-b", "0.0.0.0:5002", "app:app"]