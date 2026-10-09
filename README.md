# InksJar
A InksJar é uma empresa de tecnologia e design que utiliza modelagem e impressão 3D para transformar ideias em objetos reais, com foco em personalização e sustentabilidade.

Este projeto implementa a API responsável por:

- Cadastro e gestão de produtos e categorias
- Gerenciamento de clientes
- Criação de pedidos com controle de estoque
- Validações e regras de negócio

---

## Tecnologias utilizadas

- Node.js + Express
- MySQL
- Joi (validações)
- bcryptjs
- dotenv

---

## Como rodar o projeto

### Pré-requisitos
- Node.js instalado
- MySQL rodando

### Passos

1. Clone o repositório:
```bash
git clone https://github.com/SEU_USUARIO/inksjar-backend.git
cd inksjar-backend

Instale as dependências:

Bashnpm install

Configure o arquivo .env:

Bashcp .env.example .env
Edite com seus dados do MySQL.

Crie o banco de dados:

Bashnpm run db:init

Inicie o servidor:

Bashnpm run dev
A API ficará disponível em: http://localhost:3000

Principais endpoints

MétodoRotaDescriçãoGET/api/produtosListar produtosPOST/api/produtosCadastrar produtoPUT/api/produtos/:idAtualizar produtoDELETE/api/produtos/:idInativar produtoGET/api/clientesListar clientesPOST/api/clientesCadastrar clientePOST/api/pedidosCriar pedido (com baixa de estoque)GET/api/pedidosListar pedidos

Regras implementadas

Não permite preço ou estoque negativo
Não permite e-mails duplicados
Impede a compra se não houver estoque suficiente
Baixa automática no estoque ao finalizar o pedido
Uso de transação no banco para garantir consistência
