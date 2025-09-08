FROM python:3.11-slim
WORKDIR /app
COPY upcall/ /app/
# No EXPOSE needed (internal network) and no default CMD so container stays idle via compose override
CMD ["python", "servercall.py"]