# Urea Pricing Analysis

Este repositório contém notebooks e scripts para coletar, processar e analisar dados de comércio e preço de ureia (HS 310210). Abaixo está o **mapa de diretórios** e a localização exata onde cada arquivo de dados deve estar para que os notebooks executem corretamente.

---

## Estrutura de diretórios (arquivos) 📁

- `data/` : pasta principal de dados
  - `comtrade/` : arquivos CSV mensais da UN Comtrade (ex.: `156_comtrade_199501.csv`)
  - `raw/` : dados brutos originais baixados / snapshots
  - `eda_outputs/` : saídas do EDA (figuras, tabelas, etc.)
  - `urea_trade_features.csv` / `urea_trade_features.parquet` : features mensais geradas pelo pipeline
  - `merged_data.csv` : dataset combinado usado em análises
  - `india_urea_hs6_by_partner_wits.csv` : exportações/importações obtidas via WITS
- `output/figures/` : figuras e gráficos gerados pelos notebooks
- Notebooks principais: `get_comtrade_data.ipynb`, `get_wits_data_webscrapping.ipynb`, `exploratory_data_analysis.ipynb`, `imports.ipynb`

---

## Onde colocar cada arquivo de dados

- `data/comtrade/` (obrigatório para `get_comtrade_data.ipynb`)
  - Arquivos mensais com padrão `{reporter}_comtrade_YYYYMM.csv` (ex.: `156_comtrade_199501.csv`). O notebook reutiliza esses arquivos se presentes; caso contrário, tentará baixá-los (requer `COMTRADE_SUBSCRIPTION_KEY`).

- `data/`
  - `india_urea_hs6_by_partner_wits.csv` - produzido/consumido por `get_wits_data_webscrapping.ipynb`.
  - `urea_trade_features.csv` / `.parquet` - gerados por `get_comtrade_data.ipynb` e usados em `exploratory_data_analysis.ipynb`.
  - `merged_data.csv` - saída combinada usada em análises e modelagem.

- `data/raw/` (recomendado)
  - Armazene cópias dos dados originais baixados (JSON/CSV) para auditoria/reprocessamento.

- `data/eda_outputs/` & `output/figures/`
  - Armazenam resultados do EDA e figuras geradas pelos notebooks.

---

## Requisitos e configuração

Este projeto foi cencebido utilizando a biblioteca Poetry para gerenciamento de pacotes Python, portanto recomenda-se fortemente a utilização desta biblioteca para a devida utilização deste repositório.

- Use o `pyproject.toml` / `poetry` para instalar dependências do projeto.
- Variáveis de ambiente:
  - `COMTRADE_SUBSCRIPTION_KEY` (ou `COMTRADE_PRIMARY`, `COMTRADE_KEY`) - necessária para baixar da UN Comtrade.
  - O repositório usa `python-dotenv` (`load_dotenv()`), então um arquivo `.env` na raiz com `COMTRADE_SUBSCRIPTION_KEY=...` funciona bem.

Exemplo: Instalando o ambiente virtual (PowerShell):
```powershell
# Caso ainda não tenha o Poetry instalado:
pip install poetry

# Instalar o .venv no projeto atual
poetry install --no-root
```

Exemplo (PowerShell):

```powershell
# Definir temporariamente na sessão
$env:COMTRADE_SUBSCRIPTION_KEY = "SUA_CHAVE_AQUI"
```

---

## Observações práticas

- `get_comtrade_data.ipynb` salva downloads em `data/comtrade/` e gera `data/urea_trade_features.csv` / `.parquet`.
- `get_wits_data_webscrapping.ipynb` gera `data/india_urea_hs6_by_partner_wits.csv`.
- `exploratory_data_analysis.ipynb` espera que os arquivos acima estejam disponíveis para reproduzir análises e exportar figuras para `output/figures/`.
- Funções de merge esperam que o dataframe de preços tenha `date` (timestamp mensal) e a coluna de preço (ex.: `urea_price_usd_t`).

---

## Solução de problemas rápida

- Erro de download (Comtrade): verifique `COMTRADE_SUBSCRIPTION_KEY`, limites de taxa e logs.
- Falha ao mesclar preços: cheque se `date` está em `YYYY-MM` ou timestamp mensal.

