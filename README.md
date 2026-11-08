# DevJobs

Plataforma simples para divulgação e gerenciamento de vagas de tecnologia. O projeto possui uma interface estática e uma API REST em Node.js com SQLite, autenticação JWT e consulta de endereço por CEP.

## Tecnologias

- HTML, CSS e JavaScript
- Node.js e Express
- SQLite (`better-sqlite3`)
- JWT (`jsonwebtoken`)
- ViaCEP

## Como executar a API

```bash
cd api
npm install
```

Crie o arquivo `api/.env` a partir de `api/.env.example`:

```env
JWT_SECRET=uma-chave-secreta-segura
PORT=3001
```

Opcionalmente, popule o banco com os dados de exemplo:

```bash
node src/database/seed.js
```

Inicie o servidor:

```bash
npm start
```

A API estará disponível em `http://localhost:3001`.

## Rotas principais

| Método | Rota | Descrição |
| --- | --- | --- |
| `GET` | `/api/health` | Verifica o status da API |
| `GET` | `/api/vagas` | Lista as vagas |
| `GET` | `/api/vagas/:id` | Busca uma vaga por ID |
| `POST` | `/api/auth/login` | Autentica uma empresa e retorna um JWT |
| `POST` | `/api/vagas` | Cria uma vaga (requer JWT) |
| `PUT` | `/api/vagas/:id` | Atualiza uma vaga |
| `DELETE` | `/api/vagas/:id` | Remove uma vaga da empresa autenticada (requer JWT) |
| `GET` | `/api/cep/:cep` | Consulta endereço pelo ViaCEP |

## Autenticação

Após executar o seed, use a empresa de exemplo para gerar um token:

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "contato@technova.com",
  "senha": "123456"
}
```

Envie o token retornado no cabeçalho das rotas protegidas:

```http
Authorization: Bearer SEU_TOKEN
```

Exemplo de criação de vaga:

```http
POST /api/vagas
Authorization: Bearer SEU_TOKEN
Content-Type: application/json

{
  "titulo": "Desenvolvedor Node.js",
  "tipo": "remoto",
  "tecnologias": "Node.js, Express, SQLite"
}
```

## Consulta de CEP

```http
GET /api/cep/01310100
```

Resposta esperada:

```json
{
  "cep": "01310-100",
  "cidade": "São Paulo",
  "estado": "SP",
  "bairro": "Bela Vista",
  "logradouro": "Avenida Paulista"
}
```

## Segurança

O arquivo `.env`, o banco de dados local e `node_modules` não são versionados. Use uma chave JWT própria fora do ambiente de desenvolvimento.
