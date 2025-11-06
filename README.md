# 🏆 Desafio Técnico Backend Itaú - API de Transações e Estatísticas

Este repositório contém a solução implementada para o desafio técnico de Backend do Itaú, focado na criação de uma API para gerenciamento de transações e cálculo de estatísticas em tempo real, seguindo estritas restrições arquiteturais.

A implementação é construída utilizando **Go (Golang)**, focando em concorrência, performance e código limpo.

## 🌟 Visão Geral e Requisitos Atendidos

O objetivo principal é fornecer uma API que:

1.  **Não utilize persistência de dados:** O armazenamento das transações é feito **exclusivamente na memória** da aplicação.
2.  **Calcule estatísticas em tempo real:** A agregação de dados (soma, média, min, max, count) deve ser feita apenas sobre transações que ocorreram nos **últimos 60 segundos**.
3.  **Use formato ISO 8601:** A data/hora das transações deve ser tratada neste padrão.

### ⚠️ Requisito de Arquitetura Chave: Armazenamento em Memória

Para lidar com a concorrência e o requisito de armazenamento em memória, a solução utiliza estruturas de dados nativas do Go, como **slices protegidos por `sync.RWMutex`** ou canais, garantindo *thread-safety* e acesso rápido aos dados sem a sobrecarga de um banco de dados.

## 🛠️ Tecnologias e Ferramentas

| Categoria | Tecnologia | Detalhes |
| :---: | :--- | :--- |
| **Linguagem** | Go | Versão 1.21+ (ou superior) |
| **Framework Web**| Gorilla Mux (Geralmente utilizado em projetos Go para rotas) | Gerenciamento eficiente de rotas da API. |
| **Dependências** | `net/http` | Para servir a API REST. |
| **Concorrência** | `sync.RWMutex` | Para proteger a estrutura de dados em memória contra condições de corrida. |
| **Formato Data** | `time` package | Utilizado para *parsing* e validação do formato ISO 8601 e cálculo do intervalo de 60s. |

## 🧭 Endpoints da API

A API é configurada para rodar em `http://localhost:8080`.

| Método | Endpoint | Descrição | Status de Sucesso |
| :---: | :--- | :--- | :---: |
| `POST` | `/transacoes` | Registra uma nova transação (`valor`, `dataHora`). | `201 Created` |
| `GET` | `/estatisticas` | Retorna as estatísticas agregadas (sum, avg, max, min, count) de transações nos **últimos 60 segundos**. | `200 OK` |
| `DELETE`| `/transacoes` | Limpa completamente todas as transações armazenadas na memória (reset do estado). | `200 OK` |

---

### Detalhe: Validações do Endpoint `/transacoes` (`POST`)

| Causa da Rejeição | Status HTTP Retornado |
| :--- | :---: |
| JSON Mal-formado ou Campos Ausentes | `400 Bad Request` |
| Data no Futuro | `422 Unprocessable Entity` |
| Valor Negativo ou Zero | `422 Unprocessable Entity` |

## 📐 Estrutura do Projeto (Padrão Go)

O projeto segue uma estrutura modular e limpa, típica de aplicações Go:

* `cmd/`: Ponto de entrada da aplicação (`main.go`).
* `internal/`: Contém o código de domínio e lógica de negócio.
    * `api/`: Handlers HTTP (`controllers`).
    * `domain/`: Entidades e regras de negócio (`services`).
    * `infra/`: Implementações de armazenamento em memória (repositório).
* `pkg/`: Pacotes reutilizáveis (se houver).

## ⚙️ Como Executar e Testar

### 1. Pré-requisitos

* **Go (Golang)** Versão 1.21+ instalada.

### 2. Instalação e Build

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/PedroBeltraoDev/desafio-itau-backend](https://github.com/PedroBeltraoDev/desafio-itau-backend)
    cd desafio-itau-backend
    ```
2.  **Baixe as dependências:**
    ```bash
    go mod download
    ```

### 3. Execução da Aplicação

Execute o arquivo principal:

```bash
go run cmd/main.go
# Ou, se houver um arquivo binário já compilado:
# ./desafio-itau-backend
