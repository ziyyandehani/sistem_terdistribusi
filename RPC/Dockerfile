FROM python:3.11-slim
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY RPC/requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY RPC/ /app/
EXPOSE 4000
CMD ["tail", "-f", "/dev/null"]