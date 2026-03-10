# 📚 Biblioteca Digital — José Baptista

> Sistema de gestão de biblioteca construído com microserviços (Spring Boot + Flask).  
> **Projeto Lab Alternativo** — Microservices + DevOps + Cloud Academy 2026.

---

## 👤 Informações do Aluno

| Campo | Valor |
| :--- | :--- |
| **Nome** | José Baptista |
| **Turma** | Microservices Academy 2026 |
| **Lab** | Lab Alternativo — Dia 1 (Tarde) |
| **Data de entrega** | 09 de Março de 2026 |
| **Email** | josecbaptista2001@gmail.com |

---

## 🏗️ Arquitetura do Sistema

O sistema utiliza o padrão de **Poliglot Persistence**, separando dados relacionais (PostgreSQL) de documentos não-estruturados (MongoDB) para máxima eficiência.

| Serviço | Tecnologia | Porta | Responsabilidade |
| :--- | :--- | :--- | :--- |
| **web-ui** | Python 3.11 Flask | `5000` | Interface Web (Frontend) |
| **book-service** | Spring Boot 3.x | `8080` | API REST e Regras de Negócio |
| **postgresql** | PostgreSQL 15 | `5432` | Catálogo de Livros (SQL) |
| **mongodb** | MongoDB 7 | `27017` | Avaliações/Reviews (NoSQL) |

---

## ⚡ Execução Rápida

### 1. Pré-requisitos

- **Docker Desktop** em execução.
- Terminal aberto na pasta raiz do projeto (`book-service`).

### 2. Build do Backend (Java)

Como o Dockerfile utiliza o artefacto já compilado, é necessário gerar o JAR localmente primeiro:

```powershell
./mvnw clean package -DskipTests
```

### 3. Iniciar os Microserviços

```powershell
docker compose up --build -d
```

### 4. Verificar Status

```powershell
docker compose ps
```

Aceda à interface em: **http://localhost:5000**

---

## 📡 Endpoints da API (Exemplos)

| Método | Endpoint | Descrição |
| :--- | :--- | :--- |
| `GET` | `/api/books` | Lista todos os livros (inseridos pelo Seeder) |
| `GET` | `/api/books/search?query=Dune` | Pesquisa livros por título |
| `POST` | `/api/books/{id}/reviews` | Adiciona uma nova avaliação ao MongoDB |

---

## 📁 Estrutura do Projeto

```plaintext
book-service/ (RAIZ)
├── src/main/java/           # Backend Spring Boot
│   └── pt/openup/book_service/
│       ├── controller/      # Endpoints REST
│       ├── model/           # Entidades (Postgres) e Documentos (Mongo)
│       ├── repository/      # Repositórios Spring Data
│       └── service/         # Lógica de Negócio e DataSeeder
├── web-ui/                  # Frontend Flask
│   ├── templates/           # Páginas HTML (Jinja2)
│   ├── app.py               # Servidor Web Python
│   └── Dockerfile           # Imagem Docker Python
├── target/                  # Binários compilados (JAR)
├── docker-compose.yml       # Orquestração dos 4 containers
├── Dockerfile               # Imagem Docker Java (JRE 17)
├── pom.xml                  # Gestão de dependências Maven
└── README.md                # Documentação do Projeto
```

---

## 🛑 Parar e Limpar

Para encerrar os serviços e libertar memória:

```powershell
docker compose down
```

Para remover também os volumes (limpar as bases de dados):

```powershell
docker compose down -v
```

---

## 🐛 Troubleshooting

**Erro `lstat /target`:**
Este erro ocorre se o comando `./mvnw package` não for executado antes do build do Docker. O Dockerfile precisa do ficheiro `.jar` dentro da pasta `target`.

**Ficheiro em uso (Windows):**
Se o Maven falhar ao apagar a pasta `target`, feche o IntelliJ ou execute o seguinte comando no PowerShell para libertar o processo:

```powershell
stop-process -name java -force
```

**Porta 5000 (macOS):**
Se a porta estiver ocupada pelo AirPlay, altere o mapeamento no ficheiro `docker-compose.yml` para `5001:5000`.
