# InksJar 

A **InksJar Corporation** é uma empresa de tecnologia e design que transforma ideias criativas em objetos reais por meio da modelagem e impressão 3D.  

Este repositório contém o **back-end** da plataforma de e-commerce, responsável por:

- Gestão completa do **catálogo de produtos** e **categorias**
- Cadastro e gestão de **clientes**
- Processamento de **pedidos** com baixa automática de estoque
- Validações de negócio e integridade transacional

---

## Estrutura do Projeto

```
inksjar-backend/
├── sql/
│   └── schema.sql              # Script de criação do banco + dados de exemplo
├── src/
│   ├── config/
│   │   └── database.js         # Pool de conexões MySQL
│   ├── controllers/
│   │   ├── produtosController.js
│   │   ├── categoriasController.js
│   │   ├── clientesController.js
│   │   └── pedidosController.js  # Lógica de checkout + transação
│   ├── routes/
│   │   ├── produtos.js
│   │   ├── categorias.js
│   │   ├── clientes.js
│   │   └── pedidos.js
│   ├── utils/
│   │   └── validators.js       # Schemas Joi
│   ├── database/
│   │   └── init.js             # Script de inicialização do banco
│   └── server.js               # Entrada da aplicação
├── .env.example
├── package.json
└── README.md
```

---

## Configuração e Execução

### 1. Pré-requisitos

- Node.js 18+ instalado
- MySQL 8.x (ou MariaDB) rodando localmente
- Git

### 2. Clonar o repositório

```bash
git clone https://github.com/SEU_USUARIO/inksjar-backend.git
cd inksjar-backend
```

### 3. Instalar dependências

```bash
npm install
```

### 4. Configurar variáveis de ambiente

Copie o arquivo de exemplo e edite com suas credenciais:

```bash
cp .env.example .env
```

Edite o `.env`:

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=sua_senha_mysql
DB_NAME=inksjar_db

PORT=3000
JWT_SECRET=uma_chave_secreta_qualquer
```

### 5. Criar o banco de dados e tabelas

```bash
npm run db:init
```

Este comando:
- Cria o banco `inksjar_db`
- Cria todas as tabelas
- Insere dados de exemplo (produtos, categorias, clientes)
- Cria o usuário admin (`admin@inksjar.com` / `admin123`)

### 6. Iniciar o servidor

**Modo desenvolvimento (com auto-reload):**
```bash
npm run dev
```

**Modo produção:**
```bash
npm start
```

A API estará disponível em: **http://localhost:3000**

---

## 📡 Endpoints da API

### Health Check
| Método | Rota            | Descrição              |
|--------|-----------------|------------------------|
| GET    | `/api/health`   | Status da API          |

### Produtos (CRUD)
| Método | Rota                  | Descrição                          |
|--------|-----------------------|------------------------------------|
| GET    | `/api/produtos`       | Lista produtos (filtros: nome, categoria, status) |
| GET    | `/api/produtos/:id`   | Busca produto por ID               |
| POST   | `/api/produtos`       | Cria novo produto                  |
| PUT    | `/api/produtos/:id`   | Atualiza produto                   |
| DELETE | `/api/produtos/:id`   | Inativa produto (soft delete)      |

**Exemplo de body (POST/PUT):**
```json
{
  "codigo_barras": "7891962056500",
  "nome": "Rolamento PETG",
  "descricao": "Filamento reciclado",
  "id_categoria": 4,
  "preco": 45.90,
  "quantidade_estoque": 10,
  "unidade": "Caixas",
  "cor": "Transparente",
  "material": "PETG"
}
```

### Categorias (CRUD)
| Método | Rota                    | Descrição                    |
|--------|-------------------------|------------------------------|
| GET    | `/api/categorias`       | Lista categorias ativas      |
| GET    | `/api/categorias/:id`   | Busca categoria por ID       |
| POST   | `/api/categorias`       | Cria categoria               |
| PUT    | `/api/categorias/:id`   | Atualiza categoria           |
| DELETE | `/api/categorias/:id`   | Inativa categoria            |

### Clientes (CRUD)
| Método | Rota                  | Descrição                    |
|--------|-----------------------|------------------------------|
| GET    | `/api/clientes`       | Lista clientes (filtros: nome, email) |
| GET    | `/api/clientes/:id`   | Busca cliente por ID         |
| POST   | `/api/clientes`       | Cria cliente                 |
| PUT    | `/api/clientes/:id`   | Atualiza cliente             |
| DELETE | `/api/clientes/:id`   | Inativa cliente              |

**Exemplo de body (POST):**
```json
{
  "cpf": "123.456.789-00",
  "nome": "João Silva",
  "email": "joao.silva@gmail.com",
  "telefone": "(11) 1234-5678",
  "endereco": "Rua A, 100 - São Paulo/SP",
  "idade": 30,
  "senha": "senha123"
}
```

### Pedidos (Checkout + Consultas)
| Método | Rota                        | Descrição                                      |
|--------|-----------------------------|------------------------------------------------|
| POST   | `/api/pedidos`              | **Cria pedido + baixa estoque (transação)**    |
| GET    | `/api/pedidos`              | Lista pedidos (filtros: status, id_cliente)    |
| GET    | `/api/pedidos/:id`          | Busca pedido com itens                         |
| PATCH  | `/api/pedidos/:id/status`   | Atualiza status do pedido                      |

**Exemplo de body para criar pedido (POST `/api/pedidos`):**
```json
{
  "id_cliente": 1,
  "endereco_entrega": "Rua A, 100 - São Paulo/SP",
  "forma_pagamento": "Cartão de Crédito",
  "observacao": "Entregar no período da manhã",
  "itens": [
    {
      "id_produto": 1,
      "quantidade": 2
    },
    {
      "id_produto": 3,
      "quantidade": 5
    }
  ]
}
```

**Resposta de sucesso:**
```json
{
  "sucesso": true,
  "mensagem": "Pedido criado com sucesso! Estoque atualizado automaticamente.",
  "dados": {
    "id_pedido": 1,
    "cliente": "João Silva",
    "valor_total": 104.30,
    "status": "Pendente",
    "itens": [...]
  }
}
```

---

## 🔒 Regras de Negócio Implementadas

### Validações de Entrada
- Preços **não podem ser negativos**
- Quantidade em estoque **não pode ser negativa**
- E-mails **não podem ser duplicados**
- Campos obrigatórios não podem ficar em branco
- Quantidade de item no pedido deve ser ≥ 1

### Lógica de Checkout (Transacional)
1. Validação completa dos dados
2. Verificação de existência e estoque de **todos** os produtos
3. **Impede finalização** se qualquer item tiver quantidade > estoque disponível
4. Inicia **transação MySQL**
5. Grava cabeçalho do pedido (status inicial: `Pendente`)
6. Grava cada item na tabela `itens_pedido`
7. **Baixa automática** da quantidade em estoque
8. Atualiza status do produto (`Disponível` / `Baixo estoque` / `Em falta`)
9. Commit (ou Rollback completo em caso de erro)

---

## 📊 Modelo de Dados (principais tabelas)

```
categorias 1 ─────── N produtos
clientes   1 ─────── N pedidos
pedidos    1 ─────── N itens_pedido N ─────── 1 produtos
```

---

## 👥 Participação da Equipe

Este repositório demonstra o histórico de commits de todos os membros do grupo.  
Cada membro deve realizar commits com suas contribuições.

---

## 📝 Licença

Projeto educacional — Uso livre para fins acadêmicos.
