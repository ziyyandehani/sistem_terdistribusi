FROM python:3.11-slim
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY ZMQ/requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY ZMQ/ /app/
EXPOSE 5555 12345
CMD ["tail", "-f", "/dev/null"]