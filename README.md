# Kairos PDI App

Kairos é uma aplicação moderna de Planejamento de Desenvolvimento Individual (PDI) que ajuda pessoas a converterem objetivos de crescimento pessoal e profissional em caminhos concretos, com ações executáveis e acompanhamento contínuo.

## Tecnologias

- **Frontend:** Angular 15+ com TypeScript
- **Backend:** .NET 8 (ASP.NET Core Web API)
- **Banco de Dados:** MySQL 8.0
- **Containerização:** Docker + Docker Compose

## Instalação e Execução

### Pré-requisitos

- Docker (v20.10+) e Docker Compose
- Git

### Inicialização Rápida

```bash
# 1. Clonar o repositório
git clone https://github.com/seu-usuario/kairos-app.git
cd kairos-app

# 2. Configurar variáveis de ambiente
cp .env.example .env
# Edite o arquivo .env com suas configurações

# 3. Iniciar os serviços
docker-compose up -d
```

### Acessos

- **Frontend:** http://localhost:4200
- **Backend API:** http://localhost:5000/swagger
- **MySQL:** localhost:3306

## Documentação

Para mais detalhes sobre o projeto:

- [Product Requirements Document (PRD)](docs/product-requirements.md)
- [Architectural Decision Records (ADR)](docs/architectural-decision-records.md)

## Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.