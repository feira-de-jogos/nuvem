# MQTT Broker

Deve-se criar o arquivo de senha `pwfile` para autenticação MQTT.

Exemplo via comando `mosquitto_passwd`:

```sh
mosquitto_passwd -c pwfile <usuário> <senha>
```

onde `<usuário>` e `<senha>` devem ser definidos.
