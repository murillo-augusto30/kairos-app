# 📋 Sobre o Projeto

**Kairos** é uma aplicação moderna de **Planejamento de Desenvolvimento Individual (PDI)** que ajuda pessoas a converterem objetivos de crescimento pessoal e profissional em caminhos concretos, com ações executáveis e acompanhamento contínuo.

## Stack Completa

- **Frontend:** Angular 15+ com TypeScript, Bootstrap 5, RxJS  
- **Backend:** .NET 8 (ASP.NET Core Web API), Entity Framework Core  
- **Banco de Dados:** MySQL 8.0  
- **Proxy / Web Server:** Nginx  
- **Containerização:** Docker + Docker Compose  

# 📁 Estrutura do Projeto

```text
kairos-app/
├── 📁 src/                          # Código fonte principal
│   ├── 📁 frontend/                 # Aplicação Angular
│   │   ├── 📁 src/app/
│   │   │   ├── 📁 core/             # Serviços essenciais, interceptors
│   │   │   ├── 📁 modules/          # Módulos funcionais (PDI, objetivos, etc.)
│   │   │   ├── 📁 shared/           # Componentes/diretivas/pipes reutilizáveis
│   │   │   └── 📁 layouts/          # Layouts da aplicação
│   │   ├── angular.json
│   │   ├── package.json
│   │   └── Dockerfile
│   │
│   └── 📁 backend/                  # API .NET
│       ├── 📁 src/
│       │   ├── 📁 Kairos.API/         # Controllers, Filters, Middlewares
│       │   ├── 📁 Kairos.Application/ # Application Services, DTOs, Mappers
│       │   ├── 📁 Kairos.Domain/      # Entities, Value Objects, Domain Services, Repository Interfaces
│       │   └── 📁 Kairos.Infrastructure/ # EF Core, Repositories, Email, Storage
│       ├── Kairos.sln
│       └── Dockerfile
│
├── 📁 docker/                       # Configurações Docker
│   ├── docker-compose.yml           # Compose principal
│   ├── docker-compose.dev.yml       # Configurações desenvolvimento
│   ├── docker-compose.prod.yml      # Configurações produção
│   ├── 📁 mysql/                    # Configurações MySQL
│   │   ├── init.sql                 # Script inicialização BD
│   │   └── my.cnf                   # Configurações personalizadas
│   └── 📁 nginx/                    # Configurações Nginx
│       ├── nginx.conf               # Config principal
│       ├── default.conf             # Config virtual host
│       └── ssl/                     # Certificados SSL (produção)
│
├── 📁 docs/                         # Documentação
│   ├── 📁 architecture/             # Diagramas de arquitetura
│   ├── 📁 api/                      # Documentação API (Swagger/OpenAPI)
│   └── 📁 database/                 # Modelos ER, migrations
│
├── 📁 scripts/                      # Scripts utilitários
│   ├── init-db.sh                   # Inicialização banco de dados
│   ├── seed-data.sh                 # Dados iniciais
│   └── deploy-local.sh              # Deploy local
│
├── 📁 .github/                      # Configurações GitHub
│   └── 📁 workflows/                # CI/CD pipelines
│
├── .env.example                     # Variáveis de ambiente exemplo
├── .gitignore
├── docker-compose.yml               # Link simbólico para docker/docker-compose.yml
├── Makefile                         # Comandos úteis
└── README.md                        # Este arquivo
```

## 🚀 Como Executar o Projeto

### Pré-requisitos

- Docker (v20.10+) e Docker Compose
- Git
- (Opcional) Make

### Inicialização Rápida (Desenvolvimento)

```bash
# 1. Clonar o repositório
git clone https://github.com/seu-usuario/kairos-app.git
cd kairos-app

# 2. Configurar variáveis de ambiente
cp .env.example .env
# Edite o arquivo .env

# 3. Iniciar os serviços
docker-compose -f docker/docker-compose.yml -f docker/docker-compose.dev.yml up -d
```

### Acessos

- **Frontend:** http://localhost:4200
- **Backend API:** http://localhost:5000/swagger
- **MySQL:** localhost:3306

### Usando Makefile (Recomendado)

```bash
make help
make dev-up
make down
make rebuild
```

## 🔄 Fluxo de Comunicação

```
Usuário → Nginx (porta 80) → Frontend Angular
                    ↓
              Backend .NET (API)
                    ↓
               MySQL Database
```

## 🔧 Configuração dos Serviços

### Nginx

O Nginx atua como:
- Proxy reverso para a API .NET
- Servidor web para o Angular
- Balanceador de carga (produção)
- Terminação SSL

Exemplo (docker/nginx/default.conf):

```nginx
server {
    listen 80;
    server_name kairos.local;

    location / {
        root /usr/share/nginx/html;
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://backend:8080/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /ws/ {
        proxy_pass http://backend:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

### MySQL

- **Porta:** 3306
- **Volume persistente:** mysql_data
- **Script inicial:** docker/mysql/init.sql
- **Usuário padrão:** kairos_user

## ⚙️ Variáveis de Ambiente Principais

```bash
# Banco de Dados
DB_NAME=kairos_db
DB_USER=kairos_user
DB_PASSWORD=strong_password
DB_ROOT_PASSWORD=root_password

# Backend .NET
ASPNETCORE_ENVIRONMENT=Development
ConnectionStrings__DefaultConnection=Server=mysql;Database=${DB_NAME};User=${DB_USER};Password=${DB_PASSWORD}

# Frontend Angular
API_URL=http://localhost/api
```

## 📦 Build e Deploy

### Build Local

```bash
docker-compose build
docker-compose build frontend
docker-compose build backend
```

### Produção

```bash
docker-compose -f docker/docker-compose.yml -f docker/docker-compose.prod.yml up -d
```

Com SSL:

```bash
docker-compose -f docker/docker-compose.yml \
               -f docker/docker-compose.prod.yml \
               -f docker/docker-compose.ssl.yml up -d
```

## 🛠️ Comandos Úteis

```bash
docker-compose logs -f
docker-compose exec backend dotnet ef migrations add Initial
docker-compose exec mysql mysql -u kairos_user -p kairos_db
docker-compose down -v
docker system prune -a
```

## 🔍 Monitoramento e Debug

### Portas

- **4200** — Frontend Angular
- **5000** — Backend API
- **3306** — MySQL
- **8080** — Nginx
- **9229** — Debug Node.js
- **5001** — Debug .NET

### Ferramentas

- **Swagger UI:** http://localhost:5000/swagger
- **PHPMyAdmin:** porta 8081 (opcional)
- **PgAdmin:** porta 5050 (opcional)

## 🧪 Testes

```bash
# Backend
docker-compose exec backend dotnet test

# Frontend
docker-compose exec frontend npm test
docker-compose exec frontend npm run e2e

# End-to-End
npm run e2e
```