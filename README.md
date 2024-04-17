# PicPay Simplificado - API Laravel

Este projeto é uma implementação simplificada de uma plataforma de pagamentos, permitindo depósitos e transferências de dinheiro entre usuários e lojistas.

## Instalação e Configuração

Siga estas etapas para configurar o ambiente de desenvolvimento:

1 - Clone o Repositório
```bash
  git clone https://github.com/seu-usuario/seu-repositorio.git
  cd seu-repositorio
```
2 - Construa e Inicie os Contêineres do Docker
```bash
  docker-compose up --build
```
Isso irá configurar e iniciar todos os serviços necessários, incluindo o servidor web e o banco de dados.

3 - Configuração Adicional do Laravel
Após os contêineres estarem rodando, você pode precisar executar migrações e qualquer outra configuração inicial.
```bash
  docker-compose exec app php artisan migrate
```    

## Documentação da API

#### Autenticação
A autenticação é realizada via tokens JWT. Utilize o endpoint de login para obter o token, que deve ser incluído no cabeçalho das requisições que requerem autenticação.

### Endpoints
#### Registro de Usuário
``` http
 POST /api/register
```

Autenticação Necessária: Não


| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
|name: String | Obrigatório | Nome do Usuário/Mercador
email: String | Obrigatório | Email do Usuário/Mercador
password: String | Obrigatório | Senha do Usuário/Mercador
is_merchant: Boolean | Obrigatório | Se é mercador = true |

#### Exemplo de Requisição:
``` curl
curl -X POST http://localhost:8000/api/register \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePassword123",
    "is_merchant": false
  }'

```
#### Resposta de Sucesso:

Código: 201 CREATED

Response:
``` http
{
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "is_merchant": false
  },
  "token": "jwt.token.here"
}

```

#### Login

```http
  POST /api/login
```
Autenticação Necessária: Não

| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
|email: String | Obrigatório | Email do Usuário/Mercador
password: String | Obrigatório | Senha do Usuário/Mercador |

#### Exemplo de Requisição:

``` http
curl -X POST http://localhost:8000/api/login \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "john@example.com",
    "password": "securePassword123"
  }'
```
#### Resposta de Sucesso:
Código: 200 OK

``` http
{
  "token": "jwt.token.here"
}
```

#### Transferência de Dinheiro

``` http
POST /api/transfer
```
Autenticação Necessária: Sim

| Parâmetro   | Tipo       | Descrição                           |
| :---------- | :--------- | :---------------------------------- |
|value: Decimal | Obrigatório | Valor Transferencia
payer: Integer | Obrigatório | ID do pagador (usuário autenticado) 
payee: Integer | Obrigatório | ID do recebedor|

#### Exemplo de Requisição:
``` curl
curl -X POST http://localhost:8000/api/transfer \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {token}' \
  -d '{
    "value": 100.0,
    "payer": 1,
    "payee": 2
  }'
```
#### Resposta de Sucesso:
Código: 201 CREATED

``` http
{
  "message": "Transfer successful",
  "transaction": {
    "id": 123,
    "value": 100.0,
    "payer_id": 1,
    "payee_id": 2
  }
}
```

## Endpoints da API

Aqui estão os principais endpoints disponíveis na API. Você pode utilizar curl para fazer requisições a esses endpoints.

Registrar Usuário
POST /api/register
Cria um novo usuário.

```bash
  curl -X POST http://localhost:8000/api/register \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePassword123",
    "is_merchant": false
}'

```

Login de Usuário
POST /api/login
Autentica um usuário.

```bash
  curl -X POST http://localhost:8000/api/login \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "john@example.com",
    "password": "securePassword123"
}'

```

Transferência de Dinheiro
POST /api/transfer
Realiza uma transferência de dinheiro de um usuário para outro.

```bash
  curl -X POST http://localhost:8000/api/transfer \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {token}' \
  -d '{
    "value": 100,
    "payer": 1,
    "payee": 2
}'

```

Consultar Usuário
GET /api/user
Retorna informações do usuário autenticado.

```bash
  curl -X GET http://localhost:8000/api/user \
  -H 'Authorization: Bearer {token}'

```

## Considerações Finais

- Utilize a autenticação JWT para todas as requisições após o login.
- Assegure que os dados enviados nas requisições estejam conforme os formatos requeridos para evitar erros de validação.

## Dúvidas e Sugestões

Por gentileza enviar para [rafamerola@gmail.com].