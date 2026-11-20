# 📅 API de Agendamentos - Laravel 11

[![Laravel](https://img.shields.io/badge/Laravel-11.42.1-red.svg)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.3.11-blue.svg)](https://php.net)
[![Tests](https://img.shields.io/badge/Tests-18%2F19%20Passing-green.svg)](https://phpunit.de)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

API robusta para gerenciamento de agendamentos com controle de acesso baseado em roles, desenvolvida com arquitetura limpa e modernas práticas do Laravel 11.

## ✨ Funcionalidades

- ✅ **CRUD completo de agendamentos** com validações robustas
- ✅ **Controle de acesso por roles** (admin, gestor, professor)
- ✅ **Autenticação JWT** com Laravel Sanctum
- ✅ **Gestão de suspensões** de usuários e locais
- ✅ **Amistosos** apenas na 1ª e 3ª semana do mês
- ✅ **Validação de horários** (sem finais de semana, conflitos)
- ✅ **Arquitetura limpa** com Services e Request classes
- ✅ **Testes automatizados** (94,7% de cobertura)
- ✅ **Documentação Swagger** interativa
- ✅ **Middleware de segurança** customizado

## 🏗️ Arquitetura

Este projeto segue princípios de **Clean Architecture** e **SOLID**:

```
app/
├── Http/
│   ├── Controllers/          # Controllers enxutos
│   ├── Requests/            # Validação de dados
│   └── Middleware/          # Controle de acesso
├── Services/                # Lógica de negócio
├── Models/                  # Eloquent models com relationships
└── Console/Commands/        # Comandos Artisan customizados
```

### Principais Melhorias Implementadas

- **Services Layer**: Lógica de negócio separada dos controllers
- **Request Classes**: Validação centralizada e reutilizável
- **Middleware Moderno**: Compatível com Laravel 11
- **Models Limpos**: Constantes, scopes e métodos helper
- **Exception Handling**: Tratamento consistente de erros

## 🚀 Tecnologias

- **PHP** 8.3.11
- **Laravel** 11.42.1
- **Laravel Sanctum** (Autenticação JWT)
- **PostgreSQL** (Banco de dados)
- **PHPUnit** (Testes automatizados)
- **Swagger/OpenAPI** (Documentação)
- **Composer** (Gerenciador de dependências)

## 📦 Instalação

### Pré-requisitos

- PHP 8.3.11 ou superior
- Composer 2.x
- PostgreSQL ou MySQL
- Git

### Passos

```bash
# 1. Clonar o repositório
git clone https://github.com/RobertoTheDev/api-agendamento.git
cd api-agendamento

# 2. Instalar dependências
composer install

# 3. Configurar ambiente
cp .env.example .env
php artisan key:generate

# 4. Configurar banco de dados no .env
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=api_agendamento
DB_USERNAME=seu_usuario
DB_PASSWORD=sua_senha

# 5. Executar migrações e seeders
php artisan migrate --seed

# 6. Gerar documentação Swagger
php artisan l5-swagger:generate

# 7. Iniciar servidor
php artisan serve
```

## 🧪 Testes

```bash
# Executar todos os testes
php artisan test

# Executar testes específicos
php artisan test --filter=AuthTest

# Executar com relatório de cobertura
php artisan test --coverage
```

**Status dos Testes**: 18 de 19 testes passando (94,7%)

## 📡 Endpoints da API

A API utiliza **autenticação JWT** via Laravel Sanctum. Endpoints marcados com 🔒 exigem token no header:

```
Authorization: Bearer {TOKEN}
```

### 🔑 Autenticação (`/api/auth`)

| Método | Endpoint | Auth | Descrição |
|--------|----------|------|-----------|
| POST | `/api/auth/register` | ❌ | Registrar novo usuário |
| POST | `/api/auth/login` | ❌ | Login e retorno do token |
| POST | `/api/auth/logout` | 🔒 | Logout do usuário |
| GET | `/api/auth/user` | 🔒 | Dados do usuário autenticado |

### 📅 Agendamentos (`/api/bookings`)

| Método | Endpoint | Auth | Descrição |
|--------|----------|------|-----------|
| GET | `/api/bookings/my-bookings` | 🔒 | Agendamentos do usuário logado |
| POST | `/api/bookings` | 🔒 | Criar agendamento regular |
| DELETE | `/api/bookings/{id}` | 🔒 | Cancelar agendamento |
| POST | `/api/bookings/friendly-match` | 🔒 | Criar amistoso (1ª e 3ª semana) |

### 🛠 Gestão (`/api/management`) - Apenas Gestores/Admins

| Método | Endpoint | Auth | Descrição |
|--------|----------|------|-----------|
| GET | `/api/management/suspensions` | 🔒 | Listar suspensões |
| POST | `/api/management/suspensions` | 🔒 | Criar suspensão |
| GET | `/api/management/suspensions/{id}` | 🔒 | Exibir suspensão |
| DELETE | `/api/management/suspensions/{id}` | 🔒 | Remover suspensão |

### 👤 Usuários (`/api/users`) - Apenas Gestores/Admins

| Método | Endpoint | Auth | Descrição |
|--------|----------|------|-----------|
| GET | `/api/users` | 🔒 | Listar usuários com filtros |
| POST | `/api/users` | 🔒 | Criar usuário |
| GET | `/api/users/{id}` | 🔒 | Exibir usuário |
| PUT | `/api/users/{id}` | 🔒 | Atualizar usuário |
| DELETE | `/api/users/{id}` | 🔒 | Remover usuário |

## 🔍 Documentação Interativa

Acesse a documentação Swagger completa em:

**http://localhost:8000/api/documentation**

A documentação inclui:
- Interface interativa para testar endpoints
- Esquemas de dados detalhados
- Exemplos de request/response
- Códigos de erro documentados
- Autenticação Bearer JWT integrada

## 💡 Exemplos de Uso

### Registro e Login

```bash
# Registrar usuário
curl -X POST http://localhost:8000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva",
    "email": "joao@example.com",
    "password": "senha123456",
    "password_confirmation": "senha123456",
    "role": "professor"
  }'

# Fazer login
curl -X POST http://localhost:8000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "professor@booking.com",
    "password": "password123"
  }'
```

### Criar Agendamento

```bash
curl -X POST http://localhost:8000/api/bookings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer SEU_TOKEN_AQUI" \
  -d '{
    "location": "Quadra 1",
    "lesson_period": 3,
    "start_time": "2024-12-25 14:00:00",
    "end_time": "2024-12-25 15:00:00",
    "is_evaluation_period": false,
    "notes": "Aula de tênis"
  }'
```

### Criar Amistoso

```bash
curl -X POST http://localhost:8000/api/bookings/friendly-match \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer SEU_TOKEN_AQUI" \
  -d '{
    "location": "Quadra 1",
    "lesson_period": 4,
    "start_time": "2024-12-07 16:00:00",
    "end_time": "2024-12-07 17:00:00",
    "is_evaluation_period": false,
    "notes": "Amistoso de sábado"
  }'
```

## ⚙️ Comandos Artisan

### Verificar Suspensões Expiradas

```bash
# Executar verificação (dry-run)
php artisan suspensions:check-expired --dry-run

# Executar e aplicar mudanças
php artisan suspensions:check-expired
```

### Estatísticas de Agendamentos

```bash
# Estatísticas do mês atual
php artisan bookings:stats --period=month

# Estatísticas por local
php artisan bookings:stats --location="Quadra 1"
```

## 🔐 Segurança

### Controle de Acesso

- **Professores**: Podem criar e cancelar seus próprios agendamentos
- **Gestores**: Podem gerenciar suspensões e visualizar todos os agendamentos
- **Admins**: Acesso completo ao sistema

### Validações Implementadas

- Agendamentos apenas em dias úteis
- Amistosos restritos à 1ª e 3ª semana do mês
- Verificação de disponibilidade de locais
- Controle de suspensões ativas
- Tokens JWT com expiração

## 📊 Estrutura do Banco

### Principais Tabelas

- **users**: Usuários com roles e status de suspensão
- **bookings**: Agendamentos com tipos e status
- **suspensions**: Suspensões de usuários/locais
- **personal_access_tokens**: Tokens JWT do Sanctum

## 🤝 Contribuição

1. Faça fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Changelog

### v2.0.0 - Refatoração Completa
- ✅ Arquitetura limpa com Services e Request classes
- ✅ Migração para Laravel 11 com middleware moderno
- ✅ Testes automatizados (94,7% de cobertura)
- ✅ Documentação Swagger interativa
- ✅ Correção de bugs de validação
- ✅ Melhoria na segurança e controle de acesso

### v1.0.0 - Versão Inicial
- ✅ CRUD básico de agendamentos
- ✅ Autenticação com Sanctum
- ✅ Controle de roles básico

## 🐛 Problemas Conhecidos

- ExampleTest falha (teste padrão do Laravel - irrelevante para API)
- Para reportar bugs, abra uma [issue](https://github.com/RobertoTheDev/api-agendamento/issues)

## 📞 Suporte

- **Email**: robertofilholopesg202@gmail.com
- **GitHub Issues**: [Reportar problema](https://github.com/RobertoTheDev/api-agendamento/issues)
- **Documentação**: http://localhost:8000/api/documentation

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

**Desenvolvido com ❤️ por [Roberto Developer](https://github.com/RobertoTheDev)**

*API de Agendamentos - Transformando a gestão de reservas com tecnologia moderna*
