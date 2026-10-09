# InksJar

A InksJar é uma empresa de tecnologia e design que utiliza modelagem e impressão 3D para transformar ideias em objetos reais, com foco em personalização e sustentabilidade.

API responsável por:

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

1. 
```bash
git clone https://github.com/isabellemellooliveira/inksjar-backend.git
cd inksjar-backend

2.
npm install

3.
cp .env.example .env

4.
npm run db:init

5.
npm run dev

Método,Rota,Descrição
GET,/api/produtos,Listar produtos
POST,/api/produtos,Cadastrar produto
PUT,/api/produtos/:id,Atualizar produto
DELETE,/api/produtos/:id,Inativar produto
GET,/api/clientes,Listar clientes
POST,/api/clientes,Cadastrar cliente
POST,/api/pedidos,Criar pedido (com baixa de estoque)
GET,/api/pedidos,Listar pedidos

Regras
Não permite preço ou estoque negativo
Não permite e-mails duplicados
Impede a compra se não houver estoque suficiente
Baixa automática no estoque ao finalizar o pedido
Uso de transação no banco para garantir consistência****

Isabelle Helena
Alice Oliveira
Beatriz Pereira
3A
