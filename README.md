# 🧠 Projeto: Enriquecimento de Base Analítica com Web Scraping e API Financeira

## 🎯 Contexto

Uma **fintech de investimentos** precisa enriquecer sua base analítica com informações externas do mercado para apoiar decisões estratégicas.
Como Engenheira de Dados, foi desenvolvido um **pipeline de dados** que coleta informações públicas de **notícias** e **séries financeiras**, armazena localmente em um **banco DuckDB**, e permite posterior exploração via SQL e dashboards.

---

## 🧩 Objetivo

Construir um pipeline completo de **coleta, transformação e carga (ETL)** que una:

* **Web Scraping** de notícias econômicas e geopolíticas (BBC News);
* **API Pública** de dados financeiros (FRED e CoinGecko);
* **Integração analítica** em banco local **DuckDB**.

---

## ⚙️ Stack Utilizada

| Etapa         | Tecnologia                        | Descrição                                          |
| ------------- | --------------------------------- | -------------------------------------------------- |
| Coleta Web    | `Playwright` + `asyncio`          | Scraping assíncrono de páginas de notícias da BBC  |
| Coleta API    | `requests`, `pandas`              | Consumo de APIs FRED (Federal Reserve) e CoinGecko |
| Armazenamento | `DuckDB`                          | Banco analítico local com três tabelas             |
| Ambiente      | `Python 3.9+`, `Jupyter Notebook` | Execução e análise                                 |
| Persistência  | `.duckdb`, `.parquet`, `.csv`     | Formatos intermediários                            |

---

## 🌐 Fontes de Dados

### 🔹 Notícias (Web Scraping – BBC News)

* Fonte: [BBC News – US-Canada](https://www.bbc.com/news/us-canada)
* Coletadas **100 notícias** contendo título, resumo, link e data de coleta, no período de 01/08 até 01/11.
* Campos armazenados:

  ```
  [`title`, `url`, `summary`, `collected_at`, `published_at`, `published_text`]
  ```
* Objetivo: capturar contexto geopolítico e eventos com impacto em mercados.

### 🔹 Séries Financeiras (APIs Públicas)

| Fonte     | Série          | Descrição                                   | Período  |
| --------- | -------------- | ------------------------------------------- | -------- |
| FRED      | `DCOILBRENTEU` | Preço diário do petróleo Brent (USD/barril) | 6+ meses |
| FRED      | `DEXUSUK`      | Taxa USD/GBP (invertida para GBP/USD)       | 6+ meses |
| CoinGecko | `BTC/USD`      | Cotação diária do Bitcoin                   | 6+ meses |

---

## 🗄️ Modelagem de Dados no DuckDB

### Tabelas criadas:

| Tabela          | Descrição                       | Principais Campos                          |
| --------------- | ------------------------------- | ------------------------------------------ |
| **prices**      | Séries históricas dos ativos    | `instr`, `date`, `close` |
| **news_bbc**    | Notícias coletadas via scraping | `title`, `url`, `summary`, `collected_at`, `published_at`, `published_text`  |
| **instruments** | Metadados dos instrumentos      | `instr_id`, `symbol`, `name`, `class`      |

```sql
-- Exemplo de schema no DuckDB
DESCRIBE prices;
DESCRIBE news_bbc;
DESCRIBE instruments;
```

---

## 📊 Resultados

* **100 notícias** coletadas da BBC News.
* **3 instrumentos** (Brent, GBP/USD, BTC/USD) com **211 dias** de dados cada.
* **3 tabelas analíticas** armazenadas no DuckDB (`prices`, `news_bbc`, `instruments`).
* Pipeline totalmente reprodutível e modular, pronto para expansão com novos tópicos ou ativos.

---

## 🧾 Estrutura Final

```
📂 projeto_etl_fintech/
│
├── market.duckdb                 # Banco analítico local
├── prices.parquet                # Dados de preços
├── bbc_us_canada_latest_updates.csv       # Notícias coletadas
├── projeto_final_web_scraping.ipynb            # Notebook principal
└── requirements.txt              # Dependências fixas
```

---

## ✅ Conclusão

O projeto integra dados não estruturados (notícias) e estruturados (séries econômicas), simulando um fluxo real de engenharia de dados.
Com as tabelas organizadas no DuckDB, é possível executar consultas SQL rápidas e realizar análises temporais sobre o impacto de eventos geopolíticos nos ativos financeiros.