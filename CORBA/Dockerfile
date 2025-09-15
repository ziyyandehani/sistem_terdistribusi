FROM python:3.11-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends omniidl omniORB && rm -rf /var/lib/apt/lists/*
COPY CORBA/ /app/CORBA/
WORKDIR /app/CORBA
# Generate Python stubs from IDL
RUN omniidl -bpython hello.idl || true
EXPOSE 2809
CMD ["tail", "-f", "/dev/null"]