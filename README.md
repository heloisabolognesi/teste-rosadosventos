# 🧭 Rosa dos Ventos — Sistema de Planejamento e Gestão de Viagens

Rosa dos Ventos é uma aplicação full-stack para planejamento de viagens, permitindo que o usuário organize viagens, monte roteiros de atividades, controle gastos e salve locais favoritos (restaurantes, hotéis, pontos turísticos). Projeto acadêmico da disciplina **Desenvolvimento de Sistemas com Banco de Dados**.

> Desenvolvido por [Heloísa Bolognesi](https://github.com/heloisabolognesi).

---

## 📋 Sumário

- [Sobre o projeto](#-sobre-o-projeto)
- [Funcionalidades](#-funcionalidades)
- [Tecnologias utilizadas](#-tecnologias-utilizadas)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Modelo de dados](#-modelo-de-dados)
- [Arquitetura da API](#-arquitetura-da-api)
- [Pré-requisitos](#-pré-requisitos)
- [Como rodar o projeto localmente](#-como-rodar-o-projeto-localmente)
- [Variáveis de ambiente](#-variáveis-de-ambiente)
- [Scripts disponíveis](#-scripts-disponíveis)
- [Autenticação e segurança](#-autenticação-e-segurança)
- [Roadmap](#-roadmap)
- [Contribuindo](#-contribuindo)
- [Licença](#-licença)
- [Contato](#-contato)

---

## 📖 Sobre o projeto

O **Rosa dos Ventos** é um sistema web completo (frontend + backend + banco de dados relacional) para gestão de viagens pessoais. Cada usuário pode cadastrar suas viagens com destino, datas e orçamento, montar um roteiro cronológico de atividades, registrar gastos por categoria e salvar uma lista de lugares favoritos com avaliação — tudo protegido por autenticação e vinculado à própria conta do usuário.

O projeto foi construído como projeto integrador acadêmico, com foco em modelagem relacional (chaves primárias/estrangeiras, integridade referencial, tipos de dados adequados) e em uma API REST autenticada via JWT.

---

## ✨ Funcionalidades

- **🔐 Autenticação** — cadastro e login de usuários com senha criptografada (bcrypt) e sessão via JWT.
- **✈️ Gestão de Viagens** — CRUD completo de viagens, com nome, destino, descrição, orçamento, datas de ida/volta, imagem e status (`planejada`, `em_andamento`, `concluida`, `cancelada`).
- **🗓️ Roteiro de Atividades** — montagem de um roteiro cronológico por viagem, com data, horário e marcação de atividade concluída.
- **💰 Controle de Gastos** — lançamento de despesas por viagem, categorizadas (`hospedagem`, `transporte`, `alimentacao`, `passeio`, `compras`, `outro`), com valores em formato monetário preciso.
- **⭐ Favoritos** — lista de locais salvos por viagem (restaurante, hotel, ponto turístico, local interessante), com endereço, observações e avaliação de 1 a 5 estrelas.
- **📊 Dashboard** — estatísticas consolidadas do usuário (resumo de viagens, gastos, etc.).
- **🌗 Tema claro/escuro** — alternância de tema com preferência salva no navegador.
- **📱 Landing page pública** — página inicial de apresentação do sistema.

---

## 🛠 Tecnologias utilizadas

### Backend
- [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/)
- [MySQL](https://www.mysql.com/) via [mysql2](https://github.com/sidorares/node-mysql2) (pool de conexões)
- [JWT (jsonwebtoken)](https://github.com/auth0/node-jsonwebtoken) — autenticação
- [bcryptjs](https://github.com/dcodeIO/bcrypt.js) — hash de senhas
- [cors](https://github.com/expressjs/cors) / [dotenv](https://github.com/motdotla/dotenv)
- [nodemon](https://nodemon.io/) (ambiente de desenvolvimento)

### Frontend
- [React 19](https://react.dev/) + [Vite](https://vitejs.dev/)
- [React Router DOM 7](https://reactrouter.com/) — roteamento
- [Axios](https://axios-http.com/) — consumo da API (com interceptor de JWT)
- [Lucide React](https://lucide.dev/) — ícones
- ESLint

### Banco de dados
- MySQL — modelagem relacional com `AUTO_INCREMENT`, `FOREIGN KEY`, `ENUM`, `DECIMAL(10,2)` e `ON DELETE CASCADE`

---

## 📁 Estrutura do projeto

```
teste-rosadosventos/
├── backend/
│   ├── src/
│   │   ├── app.js                    # Ponto de entrada da API Express
│   │   ├── config/
│   │   │   └── db.js                 # Pool de conexões MySQL
│   │   ├── controllers/
│   │   │   ├── authController.js
│   │   │   ├── viagensController.js
│   │   │   ├── roteiroController.js
│   │   │   ├── gastosController.js
│   │   │   ├── favoritosController.js
│   │   │   └── dashboardController.js
│   │   ├── middlewares/
│   │   │   ├── authMiddleware.js     # Validação do JWT
│   │   │   └── errorMiddleware.js    # Tratamento global de erros
│   │   └── routes/
│   │       ├── authRoutes.js
│   │       ├── viagensRoutes.js
│   │       ├── roteiroRoutes.js
│   │       ├── gastosRoutes.js
│   │       ├── favoritosRoutes.js
│   │       └── dashboardRoutes.js
│   ├── .env                          # Variáveis de ambiente (não versionar)
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── LandingPage.jsx
│   │   │   ├── LoginPage.jsx
│   │   │   ├── RegisterPage.jsx
│   │   │   ├── DashboardPage.jsx
│   │   │   ├── ViagensPage.jsx
│   │   │   └── ViagemDetalhePage.jsx
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   ├── TripCard.jsx
│   │   │   ├── ActivityItem.jsx
│   │   │   ├── GastoItem.jsx
│   │   │   ├── FavoritoCard.jsx
│   │   │   ├── StatCard.jsx
│   │   │   └── Modal.jsx
│   │   ├── context/
│   │   │   └── AuthContext.jsx       # Contexto de autenticação
│   │   ├── hooks/
│   │   │   └── useAuth.js
│   │   ├── services/
│   │   │   └── api.js                # Instância do Axios + interceptor JWT
│   │   └── App.jsx                   # Rotas públicas/privadas
│   └── package.json
└── schema.sql                        # Script de criação do banco (MySQL)
```

---

## 🗄 Modelo de dados

O script `schema.sql` cria o banco `rosa_dos_ventos` no MySQL com as tabelas abaixo:

| Tabela                | Descrição                                                                 |
|------------------------|----------------------------------------------------------------------------|
| `usuarios`             | Usuários do sistema (nome, e-mail único, senha com hash, foto de perfil) |
| `viagens`               | Viagens do usuário (destino, orçamento, datas, status)                   |
| `roteiro_atividades`   | Atividades do roteiro de cada viagem (data, horário, conclusão)          |
| `gastos`                | Despesas por viagem, categorizadas e com valor em `DECIMAL(10,2)`        |
| `favoritos`             | Locais salvos por viagem, com tipo, endereço e avaliação (1–5)           |

**Relacionamentos:**

```
usuarios (1) ───< (N) viagens
viagens  (1) ───< (N) roteiro_atividades
viagens  (1) ───< (N) gastos
viagens  (1) ───< (N) favoritos
```

Todas as chaves estrangeiras usam `ON DELETE CASCADE`, garantindo que a exclusão de um usuário ou viagem remova automaticamente seus dados relacionados, sem deixar registros órfãos.

---

## 🔌 Arquitetura da API

Base URL local: `http://localhost:5000/api`

| Método | Rota                          | Descrição                                  | Autenticação |
|--------|--------------------------------|----------------------------------------------|:---:|
| POST   | `/auth/register`              | Cria um novo usuário                          | ❌ |
| POST   | `/auth/login`                 | Autentica e retorna um token JWT              | ❌ |
| GET    | `/viagens`                    | Lista as viagens do usuário logado            | ✅ |
| POST   | `/viagens`                    | Cria uma nova viagem                          | ✅ |
| GET    | `/viagens/:id`                | Detalha uma viagem                            | ✅ |
| PUT    | `/viagens/:id`                | Atualiza uma viagem                           | ✅ |
| DELETE | `/viagens/:id`                | Remove uma viagem                             | ✅ |
| GET    | `/viagens/:id/roteiro`        | Lista o roteiro de uma viagem                 | ✅ |
| POST   | `/viagens/:id/roteiro`        | Cria uma atividade no roteiro                 | ✅ |
| PUT    | `/roteiro/:id`                | Atualiza uma atividade do roteiro             | ✅ |
| DELETE | `/roteiro/:id`                | Remove uma atividade do roteiro               | ✅ |
| GET    | `/viagens/:id/gastos`         | Lista os gastos de uma viagem                 | ✅ |
| POST   | `/viagens/:id/gastos`         | Registra um gasto                             | ✅ |
| PUT    | `/gastos/:id`                 | Atualiza um gasto                             | ✅ |
| DELETE | `/gastos/:id`                 | Remove um gasto                               | ✅ |
| GET    | `/viagens/:id/favoritos`      | Lista os favoritos de uma viagem              | ✅ |
| POST   | `/viagens/:id/favoritos`      | Adiciona um favorito                          | ✅ |
| PUT    | `/favoritos/:id`              | Atualiza um favorito                          | ✅ |
| DELETE | `/favoritos/:id`              | Remove um favorito                            | ✅ |
| GET    | `/dashboard`                  | Retorna estatísticas consolidadas do usuário  | ✅ |

Rotas marcadas com ✅ exigem o header `Authorization: Bearer <token>`.

---

## ✅ Pré-requisitos

- [Node.js](https://nodejs.org/) 18 ou superior
- [MySQL](https://www.mysql.com/) instalado e em execução localmente (ou acessível remotamente)
- npm (ou yarn/pnpm, se preferir adaptar os comandos)

---

## 🚀 Como rodar o projeto localmente

1. **Clone o repositório**

   ```bash
   git clone https://github.com/heloisabolognesi/teste-rosadosventos.git
   cd teste-rosadosventos
   ```

2. **Crie o banco de dados**

   Execute o script `schema.sql` no seu servidor MySQL (via terminal, MySQL Workbench, ou outra ferramenta de sua preferência):

   ```bash
   mysql -u root -p < schema.sql
   ```

   Isso cria o banco `rosa_dos_ventos` com todas as tabelas, chaves e relacionamentos.

3. **Configure e rode o backend**

   ```bash
   cd backend
   npm install
   ```

   Crie um arquivo `.env` na pasta `backend/` (veja [Variáveis de ambiente](#-variáveis-de-ambiente)) e depois inicie o servidor:

   ```bash
   npm run dev
   # ou, em produção:
   npm start
   ```

   A API sobe por padrão em `http://localhost:5000`.

4. **Configure e rode o frontend**

   Em outro terminal:

   ```bash
   cd frontend
   npm install
   npm run dev
   ```

   O frontend (Vite) sobe por padrão em `http://localhost:5173`, já configurado para consumir a API em `http://localhost:5000/api`.

5. Acesse `http://localhost:5173` no navegador, crie uma conta e comece a planejar suas viagens.

---

## 🔑 Variáveis de ambiente

Crie um arquivo `.env` dentro da pasta `backend/` com as seguintes chaves:

```env
PORT=5000
DB_HOST=localhost
DB_USER=seu_usuario_mysql
DB_PASS=sua_senha_mysql
DB_NAME=rosa_dos_ventos
JWT_SECRET=uma_chave_secreta_forte_para_o_jwt
```

> ⚠️ O arquivo `.env` contém credenciais sensíveis e não deve ser versionado. Se ele estiver presente no repositório, é recomendável adicioná-lo ao `.gitignore` e trocar as credenciais expostas.

O frontend, por padrão, aponta para `http://localhost:5000/api` (definido em `frontend/src/services/api.js`). Caso o backend rode em outra URL/porta, ajuste esse valor.

---

## 📜 Scripts disponíveis

### Backend (`backend/`)

| Script        | Descrição                                         |
|---------------|------------------------------------------------------|
| `npm run dev` | Inicia o servidor com nodemon (recarrega automaticamente) |
| `npm start`   | Inicia o servidor em modo produção                 |

### Frontend (`frontend/`)

| Script          | Descrição                                  |
|-----------------|----------------------------------------------|
| `npm run dev`   | Inicia o servidor de desenvolvimento (Vite)  |
| `npm run build` | Gera o build de produção                     |
| `npm run preview` | Serve localmente o build de produção       |
| `npm run lint`  | Executa o ESLint no projeto                  |

---

## 🔐 Autenticação e segurança

- As senhas dos usuários são armazenadas com **hash bcrypt** (`bcryptjs`), nunca em texto puro.
- O login retorna um **token JWT**, assinado com a chave definida em `JWT_SECRET`.
- O frontend guarda o token no `localStorage` (`rosa_dos_ventos_token`) e o envia automaticamente em todas as requisições via interceptor do Axios (`Authorization: Bearer <token>`).
- No backend, o `authMiddleware` valida o token em todas as rotas privadas (viagens, roteiro, gastos, favoritos e dashboard), extraindo o `id`, `nome` e `email` do usuário autenticado.
- No banco de dados, a integridade referencial (`FOREIGN KEY` + `ON DELETE CASCADE`) garante que os dados de viagens, roteiros, gastos e favoritos estejam sempre vinculados a um usuário/viagem válidos.

---

## 🗺 Roadmap

Ideias de evolução para o projeto:

- [ ] Upload de imagens (viagens e foto de perfil) para um serviço de storage
- [ ] Paginação e filtros nas listagens de viagens e gastos
- [ ] Gráficos de gastos por categoria no dashboard
- [ ] Compartilhamento de viagens entre usuários (viagens em grupo)
- [ ] Testes automatizados (backend e frontend)
- [ ] Deploy da API e do frontend em produção

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Para contribuir:

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/minha-feature`)
3. Faça commit das suas alterações (`git commit -m 'feat: adiciona minha feature'`)
4. Faça push para a branch (`git push origin feature/minha-feature`)
5. Abra um Pull Request

---

## 📄 Licença

Este projeto ainda não possui uma licença definida. Caso deseje torná-lo open source formalmente, considere adicionar um arquivo `LICENSE` (por exemplo, [MIT](https://choosealicense.com/licenses/mit/)).

---

## 📬 Contato

Desenvolvido por **Heloísa Bolognesi**
GitHub: [@heloisabolognesi](https://github.com/heloisabolognesi)

---

<p align="center">🧭 Planeje. Explore. Registre cada viagem com a Rosa dos Ventos.</p>
