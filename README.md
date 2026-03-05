# 📊 Pipeline de Dados com DuckDB (Arquitetura Bronze, Silver e Gold)

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como prática durante estudos em **Engenharia de Dados**.

O objetivo é construir um **pipeline de dados simples**, utilizando arquivos CSV como fonte de dados e organizando o processamento em três camadas:

* **Bronze** → ingestão de dados brutos
* **Silver** → limpeza e transformação
* **Gold** → modelagem para consumo analítico

O projeto simula um fluxo básico de engenharia de dados utilizando **Python, Pandas e DuckDB**.

---

# 🧰 Tecnologias Utilizadas

* Python
* DuckDB
* Pandas
* Jupyter Notebook
* Git e GitHub
* VSCode

---

# 📂 Estrutura do Projeto

```
projeto_dados
│
├── landing
│   ├── z0019_1.csv
│   └── z0019_2.csv
│
├── scripts
│   ├── ingestao.ipynb
│   ├── enriquecimento.ipynb
│   └── refinamento.ipynb
│
├── dados_duckdb.db
│
└── README.md
```

---

# 🥉 Camada Bronze — Ingestão de Dados

Nesta etapa os dados são carregados dos arquivos **CSV** para o banco **DuckDB**, mantendo os dados o mais próximo possível da fonte original.

Processos realizados:

* leitura dos arquivos CSV
* inclusão da coluna **nome_arquivo**
* registro da **data de ingestão**
* armazenamento na tabela **bronze_produtos**

Exemplo de ingestão:

```python
df = pd.read_csv('../landing/z0019_2.csv', sep=';')

df['nome_arquivo'] = arquivo
df['data_ingestao'] = datetime.now()

con.execute("INSERT INTO bronze_produtos SELECT * FROM df")
```

---

# 🥈 Camada Silver — Tratamento de Dados

Na camada **Silver** os dados passam por limpeza e padronização.

Transformações realizadas:

* remoção de duplicidades
* seleção do registro mais recente por produto
* remoção de colunas técnicas
* padronização de nomes de colunas

Exemplo de transformação:

```sql
SELECT *,
ROW_NUMBER() OVER (
PARTITION BY NATBR
ORDER BY data_ingestao DESC
) AS row
```

Após isso são mantidos apenas os registros mais recentes.

---

# 🥇 Camada Gold — Modelagem

A camada **Gold** contém dados prontos para análise.

Foi criada uma dimensão de produtos:

```sql
CREATE TABLE dim_produtos(
    id_produto BIGINT,
    nm_produto TEXT,
    vl_produto FLOAT
)
```

Essa tabela contém os dados já tratados e prontos para consumo analítico.

---

# 🚀 Como Executar o Projeto

1️⃣ Clonar o repositório

```bash
git clone URL_DO_REPOSITORIO
```

2️⃣ Abrir o projeto no **VSCode**

3️⃣ Executar os notebooks na seguinte ordem:

1. `ingestao.ipynb`
2. `enriquecimento.ipynb`
3. `refinamento.ipynb`

---

# 📊 Fluxo do Pipeline

```
CSV (Landing)
     ↓
Bronze (Ingestão)
     ↓
Silver (Limpeza e Padronização)
     ↓
Gold (Modelagem Analítica)
```

---

# 📚 Aprendizados

Durante o desenvolvimento deste projeto foram aplicados conceitos importantes de engenharia de dados:

* ingestão de dados
* arquitetura em camadas
* transformação de dados
* uso de banco analítico (DuckDB)
* versionamento de código com Git

---

# 👨‍💻 Autor

Projeto desenvolvido por **Marcos Vinicius** como parte de estudos em **Engenharia de Dados**.
