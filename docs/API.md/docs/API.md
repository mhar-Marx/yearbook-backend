# API do Yearbook — Documentação de Endpoints

    Base URL (produção): `https://yearbook-backend.vercel.app`

    ## Convenções

    - Todas as respostas são em JSON
    - Rotas protegidas exigem header `Authorization: Bearer <token>`
    - O campo `senhaHash` nunca é retornado em nenhuma resposta
    - Erros seguem o formato `{ "erro": "mensagem descritiva" }`

    ## Auth

    ### POST /auth/register
    ### POST /auth/login

    Autentica um aluno e retorna um token JWT.

    - **Autenticação:** Não
    - **Body:**

    ```json
    {
      "email": "maria@email.com",
      "senha": "minhasenha123"
    }
    ```

    - **Resposta de sucesso:** `200 OK`

    ```json
    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
    ```

    - **Erros:**
      - `401` — Credenciais inválidas (email não existe ou senha incorreta)

    Cria uma nova conta de aluno.

    - **Autenticação:** Não
    - **Body:**

    ```json
    {
      "nome": "Maria Silva",
      "email": "maria@email.com",
      "senha": "minhasenha123",
      "cidade": "Salinas",
      "frase": "Aqui começa o futuro.",
      "planosFuturos": "Cursar Ciência da Computação na UFMG"
    }
    ```

    - **Resposta de sucesso:** `201 Created`

    ```json
    {
      "id": 1,
      "nome": "Maria Silva",
      "email": "maria@email.com",
      "cidade": "Salinas",
      "frase": "Aqui começa o futuro.",
      "planosFuturos": "Cursar Ciência da Computação na UFMG",
      "fotoUrl": null,
      "role": "USER",
      "criadoEm": "2026-04-03T10:30:00.000Z"
    }
    ```

    - **Erros:**
      - `400` — Campos obrigatórios ausentes
      - `409` — Email já cadastrado
    
    ## Alunos

    ### GET /alunos

Lista todos os alunos.

- **Autenticação:** Não
- **Body:** Nenhum
- **Resposta de sucesso:** `200 OK`

```json
[
  {
    "id": 1,
    "nome": "Maria Silva",
    "email": "maria@email.com",
    "cidade": "São Paulo",
    "frase": "Aprender é viver!",
    "planosFuturos": "Ser desenvolvedora full-stack",
    "fotoUrl": "https://exemplo.com/maria.jpg",
    "role": "ALUNO",
    "criadoEm": "2026-05-12T10:00:00.000Z"
  },
  {
    "id": 2,
    "nome": "João Souza",
    "email": "joao@email.com",
    "cidade": "Rio de Janeiro",
    "frase": "Sempre em frente",
    "planosFuturos": "Abrir minha startup",
    "fotoUrl": "https://exemplo.com/joao.jpg",
    "role": "ALUNO",
    "criadoEm": "2026-05-12T10:05:00.000Z"
  }
]

### GET /alunos/:id

Busca um aluno pelo ID.

- **Autenticação:** Não
- **Body:** Nenhum
- **Resposta de sucesso:** `200 OK`

```json
{
  "id": 1,
  "nome": "Maria Silva",
  "email": "maria@email.com",
  "cidade": "São Paulo",
  "frase": "Aprender é viver!",
  "planosFuturos": "Ser desenvolvedora full-stack",
  "fotoUrl": "https://exemplo.com/maria.jpg",
  "role": "ALUNO",
  "criadoEm": "2026-05-12T10:00:00.000Z"
}

### PUT /alunos/:id

Atualiza o próprio perfil do aluno.

- **Autenticação:** Bearer token
- **Body:** (todos os campos são opcionais)

```json
{
  "nome": "Maria Silva",
  "cidade": "São Paulo",
  "frase": "Aprender é viver!",
  "planosFuturos": "Ser desenvolvedora full-stack",
  "fotoUrl": "https://exemplo.com/maria-atualizada.jpg"
}

### DELETE /alunos/:id

Remove um aluno (somente ADMIN).

- **Autenticação:** Bearer token (admin)
- **Body:** Nenhum
- **Resposta de sucesso:** `204 No Content`
- **Erros:**
  - `401` — Não autenticado
  - `403` — Não é admin
  - `404` — Aluno não encontrado

### GET /mensagens

Lista todas as mensagens do mural.

- **Autenticação:** Não
- **Body:** Nenhum
- **Resposta de sucesso:** `200 OK`

```json
[
  {
    "id": 1,
    "texto": "Parabéns a todos!",
    "imagemUrl": "https://exemplo.com/festa.jpg",
    "autorId": 1,
    "autor": {
      "id": 1,
      "nome": "Maria Silva",
      "fotoUrl": "https://exemplo.com/maria.jpg"
    },
    "criadoEm": "2026-05-12T12:00:00.000Z"
  },
  {
    "id": 2,
    "texto": "Que saudade das aulas!",
    "imagemUrl": null,
    "autorId": 2,
    "autor": {
      "id": 2,
      "nome": "João Souza",
      "fotoUrl": "https://exemplo.com/joao.jpg"
    },
    "criadoEm": "2026-05-12T12:05:00.000Z"
  }
]

### POST /mensagens

Cria uma nova mensagem no mural.

- **Autenticação:** Bearer token
- **Body:**

```json
{
  "texto": "Mensagem nova",
  "imagemUrl": "https://exemplo.com/imagem.jpg"
}

### DELETE /mensagens/:id

Exclui uma mensagem do mural.

- **Autenticação:** Bearer token
- **Body:** Nenhum
- **Resposta de sucesso:** `204 No Content`
- **Erros:**
  - `401` — Não autenticado
  - `403` — Não é dono da mensagem nem ADMIN
  - `404` — Mensagem não encontrada

