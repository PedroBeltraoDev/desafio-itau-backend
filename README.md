# 🏆 Desafio Técnico Backend Júnior Itaú - API de Transações e Estatísticas

Este repositório apresenta a solução implementada para o desafio técnico de vaga Backend Júnior do Itaú. O objetivo foi construir uma **API RESTful** que gerencie o registro de transações financeiras e forneça estatísticas em tempo real sobre os dados, seguindo estritas restrições de arquitetura.

## 🌟 Visão Geral da Solução

O principal desafio foi implementar um sistema que lida com dados em tempo real sem utilizar qualquer banco de dados ou camada de persistência.

A solução utiliza **Java** e **Spring Boot** para:

1.  Receber e validar transações via um endpoint `POST`.
2.  Armazenar as transações **exclusivamente na memória** da aplicação.
3.  Calcular estatísticas agregadas (soma, média, min, max, count) sobre transações ocorridas nos **últimos 60 segundos**.
4.  Garantir a *thread-safety* e o alto desempenho no armazenamento e recuperação dos dados.

## 🛠️ Tecnologias Utilizadas

| Categoria | Tecnologia | Detalhes |
| :---: | :--- | :--- |
| **Linguagem** | Java | Versão 17+ |
| **Framework** | Spring Boot | Utilizado para criar a aplicação e configurar a API REST. |
| **Dependências** | Spring Web, Validation | Para endpoints REST e validação de DTOs. |
| **Ferramenta** | Maven | Gerenciador de dependências e build. |
| **Estrutura de Dados** | `ConcurrentLinkedQueue` | Estrutura *thread-safe* escolhida para armazenamento de transações em memória. |

## 🧭 Endpoints da API

A API está exposta na porta `8080` (configuração padrão do Spring Boot). O path base da aplicação é `http://localhost:8080`.

| Método | Endpoint | Descrição | Status de Retorno |
| :---: | :--- | :--- | :---: |
| `POST` | `/transacoes` | Registra uma nova transação com valor e `dataHora`. | `201 Created`, `400 Bad Request`, ou `422 Unprocessable Entity` |
| `GET` | `/estatisticas` | Retorna as estatísticas agregadas (sum, avg, max, min, count) de transações nos **últimos 60 segundos**. | `200 OK` |
| `DELETE`| `/transacoes` | Limpa completamente todas as transações armazenadas na memória. | `200 OK` |

---

### Detalhe do Endpoint: `/transacoes` (`POST`)

**Corpo (JSON de Exemplo):**

```json
{
  "valor": 123.45,
  "dataHora": "2025-11-05T20:55:00.000Z" 
}
