FROM python:3.11-slim
WORKDIR /app
COPY oneway/ /app/
EXPOSE 12345/udp
CMD ["tail", "-f", "/dev/null"]