
# 📘 Documentação Swagger - API Filmes JSON Server

## 1️⃣ Introdução

Esta documentação descreve uma **API simulando CRUD completo de filmes** usando JSON Server.
Ela permite:

* Listar todos os filmes
* Buscar filme por ID
* Criar, atualizar e deletar filmes

Todas as rotas usam o JSON Server como backend e os dados de exemplo são baseados nos seguintes filmes:

```json
[
  { "id": 1, "titulo": "O Poderoso Chefão", "ano": 1972, "duracao": 175, "generoId": 2 },
  { "id": 2, "titulo": "Vingadores: Ultimato", "ano": 2019, "duracao": 181, "generoId": 1 },
  { "id": 3, "titulo": "Interestelar", "ano": 2014, "duracao": 169, "generoId": 4 },
  { "id": 4, "titulo": "Coringa", "ano": 2019, "duracao": 122, "generoId": 2 },
  { "id": 5, "titulo": "Toy Story", "ano": 1995, "duracao": 81, "generoId": 8 },
  { "id": 6, "titulo": "Titanic", "ano": 1997, "duracao": 195, "generoId": 6 },
  { "id": 7, "titulo": "O Senhor dos Anéis: A Sociedade do Anel", "ano": 2001, "duracao": 178, "generoId": 7 },
  { "id": 8, "titulo": "Pantera Negra", "ano": 2018, "duracao": 134, "generoId": 1 },
  { "id": 9, "titulo": "Forrest Gump", "ano": 1994, "duracao": 142, "generoId": 2 },
  { "id": 10, "titulo": "O Exorcista", "ano": 1973, "duracao": 122, "generoId": 5 },
  { "id": 11, "titulo": "Homem-Aranha: Sem Volta Para Casa", "ano": 2021, "duracao": 148, "generoId": 1 },
  { "id": 12, "titulo": "Divertida Mente", "ano": 2015, "duracao": 95, "generoId": 8 },
  { "id": 13, "titulo": "A Origem", "ano": 2010, "duracao": 148, "generoId": 4 },
  { "id": 14, "titulo": "Os Caça-Fantasmas", "ano": 1984, "duracao": 105, "generoId": 5 },
  { "id": 15, "titulo": "O Diabo Veste Prada", "ano": 2006, "duracao": 109, "generoId": 3 }
]
```

---

## 2️⃣ Estrutura básica do Swagger

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "API Filmes JSON Server",
    "version": "1.0.0",
    "description": "CRUD completo de filmes com JSON Server."
  },
  "servers": [
    {
      "url": "http://localhost:3000",
      "description": "Servidor local JSON Server"
    }
  ],
  "paths": {},
  "components": {}
}
```

---

## 3️⃣ Schemas (modelos de dados)

### Filme

```json
"Filme": {
  "type": "object",
  "required": ["id", "titulo", "ano", "duracao", "generoId"],
  "properties": {
    "id": { "type": "integer", "example": 1 },
    "titulo": { "type": "string", "example": "O Poderoso Chefão" },
    "ano": { "type": "integer", "example": 1972 },
    "duracao": { "type": "integer", "example": 175 },
    "generoId": { "type": "integer", "example": 2 }
  }
}
```

### FilmeInput (para criação/atualização)

```json
"FilmeInput": {
  "type": "object",
  "required": ["titulo", "ano", "duracao", "generoId"],
  "properties": {
    "titulo": { "type": "string", "example": "Novo Filme" },
    "ano": { "type": "integer", "example": 2025 },
    "duracao": { "type": "integer", "example": 120 },
    "generoId": { "type": "integer", "example": 1 }
  }
}
```

> ✅ Explicação:
>
> * `Filme` → usado nas respostas do servidor (inclui ID)
> * `FilmeInput` → usado nas requisições POST/PUT (JSON Server gera ID automaticamente)

---

## 4️⃣ Rotas (paths)

### a) GET /filmes

* Lista todos os filmes

```json
"/filmes": {
  "get": {
    "summary": "Lista todos os filmes",
    "responses": {
      "200": {
        "description": "Lista de filmes",
        "content": {
          "application/json": {
            "schema": {
              "type": "array",
              "items": { "$ref": "#/components/schemas/Filme" }
            }
          }
        }
      }
    }
  }
}
```

---

### b) GET /filmes/{id}

* Busca um filme pelo ID

```json
"/filmes/{id}": {
  "get": {
    "summary": "Busca um filme pelo ID",
    "parameters": [
      { "name": "id", "in": "path", "required": true, "schema": { "type": "integer" } }
    ],
    "responses": {
      "200": {
        "description": "Filme encontrado",
        "content": {
          "application/json": {
            "schema": { "$ref": "#/components/schemas/Filme" }
          }
        }
      },
      "404": { "description": "Filme não encontrado" }
    }
  }
}
```

---

### c) POST /filmes

* Cria um novo filme

```json
"/filmes": {
  "post": {
    "summary": "Cria um novo filme",
    "requestBody": {
      "required": true,
      "content": {
        "application/json": { "schema": { "$ref": "#/components/schemas/FilmeInput" } }
      }
    },
    "responses": {
      "201": {
        "description": "Filme criado",
        "content": {
          "application/json": { "schema": { "$ref": "#/components/schemas/Filme" } }
        }
      }
    }
  }
}
```

---

### d) PUT /filmes/{id}

* Atualiza um filme existente

```json
"/filmes/{id}": {
  "put": {
    "summary": "Atualiza um filme",
    "parameters": [
      { "name": "id", "in": "path", "required": true, "schema": { "type": "integer" } }
    ],
    "requestBody": {
      "required": true,
      "content": {
        "application/json": { "schema": { "$ref": "#/components/schemas/FilmeInput" } }
      }
    },
    "responses": {
      "200": { "description": "Filme atualizado" },
      "404": { "description": "Filme não encontrado" }
    }
  }
}
```

---

### e) DELETE /filmes/{id}

* Deleta um filme existente

```json
"/filmes/{id}": {
  "delete": {
    "summary": "Deleta um filme",
    "parameters": [
      { "name": "id", "in": "path", "required": true, "schema": { "type": "integer" } }
    ],
    "responses": {
      "200": { "description": "Filme deletado com sucesso" },
      "404": { "description": "Filme não encontrado" }
    }
  }
}
```

---

## 5️⃣ Como usar na aula

1. Abra o **Swagger UI** apontando para esse JSON.
2. Mostre a rota **GET /filmes** primeiro.
3. Depois, GET /filmes/{id}, explicando **parâmetros de path**.
4. Faça POST para criar filme, mostrando o `requestBody`.
5. Mostre PUT para atualizar e DELETE para remover.
6. Explique **schemas**, como `Filme` e `FilmeInput`, e o uso do `$ref`.

---

