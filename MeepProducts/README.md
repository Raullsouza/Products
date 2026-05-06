# Products API

API RESTful para gerenciamento de produtos, categorias, portais e locais. Construída com ASP.NET Core 6 e autenticação JWT.

## Tecnologias

- .NET 6 / ASP.NET Core Web API
- Entity Framework Core + SQL Server
- ASP.NET Identity (autenticação de usuários)
- JWT Bearer Token
- AutoMapper
- Swagger / OpenAPI

## Modelo de Dados

```
Local (Cidade/Estado)
  └── Portal (N portais por local)
        └── Categoria (N categorias por portal)  [requer autenticação]
              └── Produto (N produtos por categoria)
```

## Pré-requisitos

- [.NET 6 SDK](https://dotnet.microsoft.com/download/dotnet/6.0)
- SQL Server (ou SQL Server Express)

## Configuração

1. Clone o repositório e acesse a pasta do projeto:

```bash
git clone <url-do-repositorio>
cd MeepProducts
```

2. Atualize a string de conexão em `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=SEU_SERVIDOR;Initial Catalog=MeepProducts;Integrated Security=True;..."
  },
  "Jwt": {
    "Key": "sua-chave-secreta-aqui"
  },
  "TokenConfiguration": {
    "Audience": "sua-audience",
    "Issuer": "seu-issuer",
    "ExpireHours": 12
  }
}
```

3. Execute as migrations para criar o banco:

```bash
dotnet ef database update
```

4. (Opcional) Popule o banco com dados iniciais:

```bash
dotnet run seeddata
```

5. Inicie a aplicação:

```bash
dotnet run
```

A API estará disponível em `https://localhost:7xxx` e a documentação Swagger em `/swagger`.

## Autenticação

Algumas rotas exigem autenticação via Bearer Token (ex: `/api/Categoria`).

**Registrar usuário:**
```http
POST /api/Autoriza/register
Content-Type: application/json

{
  "email": "usuario@email.com",
  "password": "SuaSenha@123"
}
```

**Login:**
```http
POST /api/Autoriza/login
Content-Type: application/json

{
  "email": "usuario@email.com",
  "password": "SuaSenha@123"
}
```

Ambos retornam um token JWT. Use-o no header das requisições protegidas:

```
Authorization: Bearer <token>
```

## Endpoints

### Local
| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/Local` | Lista todos os locais |
| GET | `/api/Local/{id}` | Busca local por ID |
| GET | `/api/Local/cidade/{nome}` | Busca local por nome da cidade |
| GET | `/api/Local/portais/{localId}` | Lista portais de um local |
| POST | `/api/Local` | Cria novo local |
| PUT | `/api/Local/{localId}` | Atualiza local |
| DELETE | `/api/Local/LocalId` | Remove local |

### Portal
| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/Portal` | Lista todos os portais |
| GET | `/api/Portal/ById/{portalId}` | Busca portal por ID |
| GET | `/api/Portal/ByName/{portalName}` | Busca portal por nome |
| GET | `/api/Portal/CategoriaByPortalId/{portalId}` | Lista categorias de um portal |
| POST | `/api/Portal` | Cria novo portal |
| PUT | `/api/Portal/{portalId}` | Atualiza portal |
| DELETE | `/api/Portal/portalId` | Remove portal |

### Categoria `[Requer Autenticação]`
| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/Categoria` | Lista todas as categorias |
| GET | `/api/Categoria/ById/{categoryId}` | Busca categoria por ID |
| GET | `/api/Categoria/ByName/{nomeCategoria}` | Busca categoria por nome |
| GET | `/api/Categoria/produtoByCategoria/{categoriaId}` | Lista produtos de uma categoria |
| POST | `/api/Categoria` | Cria nova categoria |
| PUT | `/api/Categoria/{categoriaId}` | Atualiza categoria |
| DELETE | `/api/Categoria/categoriaId` | Remove categoria |

### Produto
| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/Produto` | Lista todos os produtos |
| GET | `/api/Produto/ById/{produtoId}` | Busca produto por ID |
| GET | `/api/Produto/ByName/{produtoName}` | Busca produto por nome |
| GET | `/api/Produto/GetProdutosByCategoria/{categoriaId}` | Lista produtos por categoria |
| POST | `/api/Produto` | Cria novo produto |
| PUT | `/api/Produto/{produtoId}` | Atualiza produto |
| DELETE | `/api/Produto/produtoId` | Remove produto |

## Estrutura do Projeto

```
MeepProducts/
├── Controllers/      # Endpoints da API
├── Data/             # DbContext (EF Core)
├── Dto/              # Data Transfer Objects
├── Helper/           # Configuração do AutoMapper
├── Interfaces/       # Contratos dos repositórios
├── Migrations/       # Migrations do banco de dados
├── Models/           # Entidades do domínio
├── Repository/       # Implementações dos repositórios
└── Seed.cs           # Script de população inicial do banco
```
