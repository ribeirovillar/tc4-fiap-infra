# TC4 FIAP - Plataforma de Pedidos

Repositório de infraestrutura para orquestração e documentação dos microserviços do projeto FIAP TC4 usando Docker Compose.

Esta plataforma completa de pedidos é composta por múltiplos microserviços que trabalham em conjunto para fornecer uma solução robusta de e-commerce.

---

## 🏗️ Arquitetura da Plataforma

A plataforma é baseada em microserviços, cada um com sua responsabilidade específica:

### Microserviços

| Serviço | Tecnologia | Porta | Descrição |
|---------|------------|-------|-----------|
| **Cliente Service** | Spring Boot 3.5.3 + Java 24 | 8080 | Gerenciamento de clientes e endereços |
| **Produto Service** | Quarkus 3.24.4 + Java 21 | 8081 | Catálogo de produtos |
| **Estoque Service** | Quarkus 3.24.4 + Java 21 | 8082 | Controle de estoque e baixas |
| **Pedido Receiver** | Spring Boot + Java | 8083 | Recebimento assíncrono de pedidos |
| **Pedido Service** | Spring Boot + Java | 8084 | Processamento de pedidos |
| **Pagamento Service** | Spring Boot 3.5.4 + Java 24 | 8085 | Processamento de pagamentos |

### 📂 Repositórios dos Microserviços

Para desenvolvimento individual ou análise detalhada, cada serviço possui seu próprio repositório:

| Serviço | Repositório |
|---------|-------------|
| **Cliente Service** | [tc4-fiap-cliente-service](https://github.com/ribeirovillar/tc4-fiap-cliente-service) |
| **Produto Service** | [tc4-fiap-produto-service](https://github.com/ribeirovillar/tc4-fiap-produto-service) |
| **Estoque Service** | [tc4-fiap-estoque-service](https://github.com/ribeirovillar/tc4-fiap-estoque-service) |
| **Pedido Receiver** | [tc4-fiap-pedido-receiver](https://github.com/ribeirovillar/tc4-fiap-pedido-receiver) |
| **Pedido Service** | [tc4-fiap-pedido-service](https://github.com/ribeirovillar/tc4-fiap-pedido-service) |
| **Pagamento Service** | [tc4-fiap-pagamento-service](https://github.com/ribeirovillar/tc4-fiap-pagamento-service) |

### Infraestrutura

| Componente | Porta | Descrição |
|------------|-------|-----------|
| **RabbitMQ** | 5672, 15672 | Message broker para comunicação assíncrona |
| **PostgreSQL** | 5432-5436 | Bancos de dados por microserviço |

---

## 🚀 Tecnologias

- **Java 21/24**
- **Spring Boot 3.5+** / **Quarkus 3.24+**
- **PostgreSQL 16**
- **RabbitMQ 3.13.7**
- **Docker & Docker Compose**
- **Clean Architecture**
- **MapStruct** (mapeamento DTO ↔ Domínio)
- **Flyway** (migrações de banco)
- **Jacoco** (cobertura de testes)

---

## 📋 APIs e Endpoints

### 🧑‍💼 Cliente Service (localhost:8080)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/customers` | Listar todos os clientes |
| GET | `/customers/{id}` | Buscar cliente por ID |
| POST | `/customers` | Criar novo cliente |
| PUT | `/customers/{id}` | Atualizar cliente |
| DELETE | `/customers/{id}` | Remover cliente |

### 🛍️ Produto Service (localhost:8081)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/products` | Listar todos os produtos |
| GET | `/products/{id}` | Buscar produto por ID |
| GET | `/products/skus` | Buscar produtos por lista de SKUs |
| POST | `/products` | Criar novo produto |
| PUT | `/products/{id}` | Atualizar produto |
| DELETE | `/products/{id}` | Remover produto |

### 📦 Estoque Service (localhost:8082)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/stocks` | Listar todos os estoques |
| GET | `/stocks/products/{productId}` | Buscar estoque por ID produto |
| POST | `/stocks` | Criar novo registro de estoque |
| PUT | `/stocks/products/{productId}` | Atualizar quantidade estoque |
| DELETE | `/stocks/products/{productId}` | Remover registro de estoque |
| POST | `/stocks/deduct` | Baixa de estoque em lote |
| POST | `/stocks/reverse` | Reverter baixa de estoque em lote |

### 📋 Pedido Receiver (localhost:8083)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | `/orders` | Receber e encaminhar pedido para fila |

### 📋 Pedido Service (localhost:8084)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/orders` | Listar todos os pedidos |
| GET | `/orders/{id}` | Buscar pedido por ID |
| POST | `/payments/{id}` | Processar notificação de pagamento |

### 💳 Pagamento Service (localhost:8085)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/payments` | Listar todos os pagamentos |
| POST | `/payments` | Processar novo pagamento |

---

## 🏃 Como Rodar a Plataforma

### Pré-requisitos

- Docker & Docker Compose
- Portas 8080-8085, 5432-5436, 5672, 15672 livres

### Iniciar toda a plataforma

```bash
# Clone o repositório
git clone <repo-url>
cd tc4-fiap-infra

# Suba todos os serviços
docker-compose up --build

# Para rodar em background
docker-compose up -d --build
```

### Verificar status dos serviços

```bash
# Ver logs de todos os serviços
docker-compose logs -f

# Ver logs de um serviço específico
docker-compose logs -f fiap-cliente-service

# Verificar se todos estão rodando
docker-compose ps
```

### Parar a plataforma

```bash
# Parar todos os serviços
docker-compose down

# Parar e remover volumes (CUIDADO: apaga dados)
docker-compose down -v
```

---

## 📊 Monitoramento e Gestão

### RabbitMQ Management

- **URL:** http://localhost:15672
- **Usuário:** guest
- **Senha:** guest

---

## 🧪 Testando a Plataforma

### Coleções Postman

Na pasta `postman/` você encontra coleções pré-configuradas para cada serviço:

- `fiap-cliente-service.postman_collection.json`
- `fiap-produto-service.postman_collection.json`
- `fiap-estoque-service.postman_collection.json`
- `fiap-pedido-receiver.postman_collection.json`
- `fiap-pedido-service.postman_collection.json`
- `fiap-pagamento-service.postman_collection.json`
- `fiap-regression-tests.postman_collection.json` ⭐ **Suite completa de testes de regressão**

### Suite de Testes de Regressão

A coleção `fiap-regression-tests.postman_collection.json` é uma suite completa que executa um fluxo end-to-end da plataforma:

#### 🔍 **Estrutura dos Testes:**

1. **Service Connectivity** - Verifica se todos os serviços estão acessíveis
2. **Cliente Service** - Cria e valida cliente
3. **Produto Service** - Cria produtos para o teste
4. **Estoque Service** - Configura estoque dos produtos
5. **Pedido Service** - Cria pedido completo
6. **Pagamento Service** - Processa pagamento do pedido
7. **Validações** - Verifica baixa de estoque e integrações
8. **Limpeza** - Remove dados criados durante o teste

#### ✅ **Recursos da Suite:**

- **Testes Automatizados**: Cada request tem validações automáticas
- **Variáveis Dinâmicas**: IDs são capturados e reutilizados automaticamente
- **Validações de Performance**: Tempo de resposta < 5 segundos
- **Fluxo Completo**: Simula um processo real de e-commerce
- **Cleanup**: Remove dados de teste ao final

#### 🚀 **Como Executar:**

1. Importe a coleção no Postman
2. Certifique-se que todos os serviços estão rodando
3. Execute a coleção completa com "Run Collection"
4. Acompanhe os resultados em tempo real

---

## 🗄️ Bancos de Dados

Cada microserviço possui seu próprio banco PostgreSQL:

| Serviço | Database | Porta Host | Container |
|---------|----------|------------|-----------|
| Cliente | `customerdb` | 5432 | `fiap-cliente-postgres` |
| Produto | `productdb` | 5433 | `fiap-produto-postgres` |
| Estoque | `stockdb` | 5434 | `fiap-estoque-postgres` |
| Pedido | `orderdb` | 5435 | `fiap-pedido-postgres` |
| Pagamento | `paymentdb` | 5436 | `fiap-pagamento-postgres` |

**Credenciais padrão:**
- Usuário: `postgres`
- Senha: `postgres`

---

## 🔄 Comunicação entre Serviços

### Síncrona (REST)

O Pedido Service se comunica via HTTP REST com:
- Cliente Service (validação de cliente)
- Produto Service (validação de produtos)
- Estoque Service (verificação e baixa de estoque)
- Pagamento Service (processamento de pagamento)

### Assíncrona (RabbitMQ)

- **Queue:** `order-queue`
- **Consumer:** Pedido Service
- **Producer:** Pedido Receiver

---

## 🛠️ Desenvolvimento

### Estrutura dos Projetos

Todos os microserviços seguem Clean Architecture:

```
src/main/java/
├── domain/          # Entidades e regras de negócio
├── usecase/         # Casos de uso da aplicação
├── gateway/         # Interfaces e implementações (DB, HTTP)
├── controller/      # REST Controllers
├── cinfiguration/   # Configurações Spring
├── exception/       # Tratamento de exceções
├── consumer/        # Consumidores RabbitMQ
└── mapper/          # Conversores DTO ↔ Domínio ↔ Persistência
```

### Executar Testes

```bash
# Em cada projeto individualmente
./mvnw clean verify

# Visualizar relatório de cobertura
open target/site/jacoco/index.html
```
