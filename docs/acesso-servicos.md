# Acessando os serviços da stack

## Contexto

Os containers da stack estão conectados numa rede interna Docker chamada `zabbix-network`.
Dentro dessa rede, cada container é acessível pelo nome do serviço definido no `docker-compose.yml`.
Fora dessa rede (seu navegador, terminal do Mac), os serviços são acessados via `localhost` e a porta publicada.

---

## Acesso externo: navegador ou terminal do host

Use sempre `localhost` com a porta mapeada no `docker-compose.yml`:

| Servico          | URL no navegador/terminal           |
|------------------|-------------------------------------|
| Zabbix Web       | `http://localhost`                  |
| Grafana          | `http://localhost:3000`             |
| Prometheus UI    | `http://localhost:9090`             |
| Prometheus metrics | `http://localhost:9090/metrics`   |
| Prometheus targets | `http://localhost:9090/targets`   |
| node-exporter    | `http://localhost:9100/metrics`     |
| cAdvisor         | `http://localhost:8080/metrics`     |
| apache-exporter  | `http://localhost:9117/metrics`     |
| Apache server-status | `http://localhost/server-status?auto` |

> Tentar acessar `http://prometheus:9090` no navegador retorna erro `DNS_PROBE_FINISHED_NXDOMAIN`
> porque `prometheus` é um hostname interno Docker, não resolvido fora da rede de containers.

---

## Acesso interno: comunicacao entre containers

Dentro da rede Docker, use o **nome do servico** definido no `docker-compose.yml` no lugar de `localhost`:

| Quem acessa       | Quem e acessado       | URL interna                                      |
|-------------------|-----------------------|--------------------------------------------------|
| Grafana           | Prometheus            | `http://prometheus:9090`                         |
| Prometheus        | apache-exporter       | `http://apache-exporter:9117`                    |
| Prometheus        | node-exporter         | `http://node-exporter:9100`                      |
| Prometheus        | cAdvisor              | `http://cadvisor:8080`                           |
| apache-exporter   | Zabbix Web (Apache)   | `http://zabbix-web-frontend:8080/server-status?auto` |

---

## Erro comum: configurando datasource no Grafana

Ao adicionar o Prometheus como datasource no Grafana, usar `http://localhost:9090` resulta no erro:

```
Post "http://localhost:9090/api/v1/query": dial tcp [::1]:9090: connect: connection refused
```

Isso acontece porque `localhost` dentro do container Grafana aponta para o proprio container Grafana,
nao para o host ou para o Prometheus.

**Solucao:** usar o nome do servico Docker:

```
http://prometheus:9090
```

---

## Regra geral

| Contexto                        | Use             |
|---------------------------------|-----------------|
| Navegador ou terminal no Mac    | `localhost:PORTA` |
| Configuracao entre containers   | `nome-do-servico:PORTA` |
