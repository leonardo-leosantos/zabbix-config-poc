# Monitoria com Zabbix, Grafana e Prometheus

Este projeto sobe uma stack de monitoria usando Docker Compose para coletar, armazenar e visualizar metricas.

## Servicos da stack

- `postgres_zabbix`: banco PostgreSQL usado pelo Zabbix para armazenar configuracoes, historicos, eventos e inventario.
- `zabbix-server`: componente central do Zabbix; processa coleta, regras, triggers, alertas e grava dados no banco.
- `zabbix-web`: interface web do Zabbix para administracao, visualizacao de hosts, itens, triggers e historico.
- `zabbix-agent`: agente de monitoramento instalado no host/container monitorado; envia metricas para o Zabbix Server.
- `grafana`: plataforma de visualizacao para criar dashboards personalizados com dados do Zabbix e Prometheus.
- `prometheus`: servidor de metricas no modelo pull; raspa endpoints dos exporters e disponibiliza consultas.
- `node-exporter`: exporter de metricas de sistema (CPU, memoria, disco, rede) do ambiente host.
- `cadvisor`: exporter de metricas de containers (uso de CPU/memoria, filesystem, rede e runtime Docker).
- `apache-exporter`: exporter de metricas do Apache (requisicoes, workers, bytes, uptime) via mod_status do zabbix-web.

## Como os componentes se encaixam

- O `Zabbix` cobre monitoramento tradicional orientado a hosts, itens e alertas.
- O `Prometheus` cobre series temporais por scraping de exporters.
- O `Grafana` unifica a visualizacao, permitindo dashboards para as duas fontes.
