# Distributed Systems Practice - Containerized

This repository demonstrates multiple distributed system communication patterns. Each section now has an isolated Docker / Docker Compose setup so the host Python environment remains untouched.

## Prerequisites
- Docker Engine + Docker Compose plugin

## Sections & How to Run

All compose files are under `compose/`.

| Pattern | Compose File | Run Command | Notes |
|---------|--------------|-------------|-------|
| REST (Flask) | `compose/rest.yml` | `docker compose -f compose/rest.yml up -d` | Exposes 5151 |
| JSON-RPC | `compose/rpc.yml` | `docker compose -f compose/rpc.yml up -d` | Server + auto client |
| SOAP | `compose/soap.yml` | `docker compose -f compose/soap.yml up -d` | WSDL at `:8000/?wsdl` |
| MQTT (Mosquitto) | `compose/mqtt.yml` | `docker compose -f compose/mqtt.yml up -d` | Broker + pub/sub |
| TCP Req/Resp | `compose/reqresp.yml` | `docker compose -f compose/reqresp.yml up -d` | Echo demo |
| UDP One-way | `compose/udp.yml` | `docker compose -f compose/udp.yml up -d` | Uses UDP 12345 |
| ZeroMQ (REQ/REP + PUB/SUB) | `compose/zmq.yml` | `docker compose -f compose/zmq.yml up -d` | Two patterns |
| ZooKeeper Leader Election | `compose/zookeeper_election.yml` | `docker compose -f compose/zookeeper_election.yml up -d` | 1 ZK + 3 nodes |
| Processes & Threads | `compose/code_process.yml` | `docker compose -f compose/code_process.yml up -d` | Multi-proc & threads |
| CORBA | `compose/corba.yml` | `docker compose -f compose/corba.yml up -d` | omniORB server |
| Upcall Demo | `compose/upcall.yml` | `docker compose -f compose/upcall.yml up -d` | Client/server callback |
| Lamport Clock (partial) | `compose/lamport.yml` | `docker compose -f compose/lamport.yml up -d` | Needs counterpart |

Stop with Ctrl+C or:
```bash
docker compose -f compose/<file>.yml down
```

## Container Details
Each service has its own image build (Python 3.11 slim) with minimal dependencies via per-module `requirements.txt` when needed.

## Logs & Interaction
Use a second terminal to attach logs, e.g.:
```bash
docker logs -f rest-server
```

## Extending
- Add healthchecks for production usage.
- Combine into a single multi-service `docker-compose.yml` if orchestration across patterns is desired.

## Limitations / Future Work
- ICE not containerized yet (needs Ice runtime + slice compilation steps).
- Lamport demo lacks the server-side counterpart; current container will fail connection until added.
- Security (TLS, auth) intentionally omitted for clarity.

## Cleanup
Remove all images:
```bash
docker image prune -f
```

## License
Educational use.
