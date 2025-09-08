FROM python:3.11-slim
WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
COPY Sync/zookep/election/requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY Sync/zookep/election/ /app/
CMD ["sh", "-c", "python election_node.py ${NODE_ID}"]