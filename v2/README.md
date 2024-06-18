# Para rodar o ambiente

## Banco de Dados Relacional

Para rodar o banco de dados relacional em contêiner, é necessário criar no diretório `db`:

- Diretório `data`: para persistência dos dados das bases;
- Arquivo `.env` com o seguinte conteúdo: `POSTGRES_PASSWORD=<senha>`, onde `<senha>` deve ser definida a senha mestre do PostgreSQL.
