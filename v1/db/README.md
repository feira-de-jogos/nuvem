# Banco de Dados Relacional

Para rodar o banco de dados relacional em contêiner, é necessário criar o arquivo `.env` com as seguintes variáveis de ambiente:

- `MQTT_URI`: para a conexão com o *broker* MQTT;
- `PGUSER`, `PGPASSWORD`, `PGHOST` e `PGDATABASE`: para a conexão com o banco de dados relacional. 

Exemplo de arquivo:

```ini
MQTT_URI=mqtt://<usuário_mqtt>:<senha_mqtt>@mqtt-broker
PGUSER=<usuário_db>
PGPASSWORD=<senha_db>
PGHOST=<host>
PGDATABASE=<database>
```

onde os parâmetros sensíveis (entre sinais `<` e `>`) devem ser definidos.
