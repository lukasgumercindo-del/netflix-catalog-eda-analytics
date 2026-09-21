# 🎬 Netflix Catalog Analytics: Análise Exploratória & Diagnóstico Estratégico (H1 a H7)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Data%20Viz-11557c?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/Seaborn-Data%20Viz-3776ab?style=for-the-badge)

Análise Exploratória de Dados (EDA) rigorosa desenvolvida sobre o histórico do catálogo da Netflix. O projeto aborda **7 hipóteses de negócio (H1 a H7)**, combinando auditoria de dados, tratamento de vieses estatísticos, padronização visual e storytelling focado em tomadores de decisão.

---

## 📌 1. Cenário de Negócio & Objetivos

### O Cenário

A equipe de inteligência de conteúdo (representada ficcionalmente pelos stakeholders *Mariana* e *Carlos*) precisava entender as dinâmicas históricas do catálogo da Netflix para responder a perguntas estratégicas:

- A plataforma está focando mais em séries ou filmes?
- A expansão internacional continua acelerada?
- A defasagem entre o lançamento de um título e sua entrada na plataforma mudou?

### O Desafio

O dataset bruto continha inconsistências como:

- Datas formatadas como texto;
- Múltiplos países e gêneros agrupados na mesma célula;
- Registros com *lag* negativo (ano de adição anterior ao lançamento);
- Um **forte viés de amostragem nos anos iniciais (2008–2015)**, onde o baixo volume de adições distorcia severamente as médias e proporções históricas.

---

## 🛠️ 2. Arquitetura do Projeto & Estrutura do Repositório

O projeto foi dividido em notebooks sequenciais e modulares, garantindo **rastreabilidade e reprodutibilidade**:

```text
├── data/
│   ├── raw/                              # Dataset bruto original (netflix_titles.csv)
│   └── processed/                        # Datasets limpos e desnormalizados (exploded)
├── notebooks/
│   ├── 01_data_understanding.ipynb      # Inspeção inicial e identificação de nulos/anomalias
│   ├── 02_data_cleaning.ipynb            # Sanitização, parsing de datas e exportação de tabelas desnormalizadas
│   ├── 03_eda_catalogo.ipynb             # Análise de H1 (Mix), H3 (Rating/Adulto) e H6 (Duração)
│   ├── 04_eda_regionalizacao.ipynb       # Análise de H2 (Internacionalização) e H5 (Gêneros)
│   ├── 05_eda_diretores_atores.ipynb     # Análise de H4 (Elenco e Concentração)
│   ├── 06_eda_defasagem.ipynb            # Análise de H7 (Lag de Aquisição)
│   └── 07_insights_e_recomendacoes.ipynb # Storytelling final e síntese executiva

```

---

## 🔬 3. Rigor Metodológico & Decisões Técnicas

Para garantir a confiabilidade das conclusões, foram aplicadas as seguintes premissas metodológicas:

### Filtro de Robustez Temporal (`MIN_YEARLY_VOLUME >= 100`)

**Problema:**  
Entre 2008 e 2015, a plataforma adicionava volumes baixíssimos (1 a 82 títulos/ano), gerando volatilidade artificial — por exemplo, variações de **0% a 100%** de internacionalização de um ano para o outro.

**Solução:**  
As séries temporais foram restritas ao período considerado confiável (**2016–2020**).

### Tratamento do `Unknown`

Registros com país não mapeado foram excluídos do numerador e denominador do cálculo de internacionalização (H2), evitando subestimar a presença global.

### Desnormalização (`Explode`)

Separação de listas separadas por vírgula em gêneros (`listed_in`) e países (`country`) em arquivos `.csv` dedicados, evitando dupla contagem incorreta.

### Tratamento de *Lags*

Exclusão de **14 registros (0,16%)** com *lag* negativo (`year_added < release_year`), causados por atualizações de metadados de novas temporadas de séries antigas.

---

## 📊 4. Síntese dos Achados por Hipótese (H1–H7)

| Hipótese | Pergunta de Negócio | Métrica Operacional | Resultado Principal & Diagnóstico |
|---|---|---|---|
| **H1** | O mix do catálogo mudou? | Volume de Filmes vs. Séries por `year_added` | **Confirmada parcialmente:** Filmes mantêm a liderança em volume absoluto, mas as Séries apresentaram taxa de crescimento proporcional mais acelerada. |
| **H2** | O catálogo se internacionalizou? | % de aparições fora dos EUA (excl. `Unknown`) | **Refutada como tendência contínua:** A participação internacional estabilizou-se em patamar elevado (>60%) desde 2016, sem alteração de patamar até 2020. |
| **H3** | Houve migração para conteúdo adulto? | % de títulos TV-MA, R, NC-17 por ano | **Confirmada:** Conteúdo adulto representa a maioria expressiva das adições anuais (~45% a 55%), mantendo dominância constante. |
| **H4** | Há concentração em atores/diretores? | Top diretores e elenco por volume | **Concentração baixa:** O catálogo é amplamente pulverizado; mesmo os diretores mais frequentes possuem participações percentuais reduzidas do total. |
| **H5** | A diversidade de gêneros aumentou? | Gêneros únicos (H5a) e % Top 5 (H5b) | **Cobertura madura:** Todos os 42 gêneros do universo já estavam presentes em 2016. O Top 5 histórico responde por ~46%–51% do catálogo. |
| **H6** | A duração/formato dos títulos mudou? | Duração média (Filmes) e Mediana de temporadas (Séries) | **Estabilidade:** Filmes mantiveram média de ~95–100 min. Séries mantiveram mediana de 1 temporada (foco em produções curtas ou estreias). |
| **H7** | A Netflix acelerou a aquisição de lançamentos? | `year_added - release_year` (Média vs. Mediana) | **Confirmada com ressalva de assimetria:** A mediana do *lag* caiu para 2 anos, mostrando foco em conteúdos recentes. Contudo, a presença de clássicos assimétrica puxa a média para ~4 anos. |

---

## 🎨 5. Padrão Visual (Design System)

Todos os gráficos foram construídos utilizando uma **paleta Dark Netflix customizada**, otimizada para apresentações executivas.

| Elemento | Especificação |
|---|---|
| **Background** | `#141414` — Dark background |
| **Cor Primária** | `#E50914` — Netflix Red |
| **Cor Secundária** | `#B3B3B3` — Silver/Gray |
| **Elementos Visuais** | Remoção de bordas desnecessárias (*spines* superiores e direitas), substituição de eixos Y por rótulos diretos nos pontos e adição de subtítulos explicativos em itálico. |

---

## 🚀 6. Como Rodar o Projeto

### 1. Clone o repositório

```bash
git clone https://github.com/SEU-USUARIO/netflix-catalog-eda-analytics.git
cd netflix-catalog-eda-analytics
```

### 2. Crie um ambiente virtual e instale as dependências

```bash
python -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows
venv\Scripts\activate

pip install -r requirements.txt
```

### 3. Execute os Notebooks em ordem

Execute os notebooks presentes na pasta `notebooks/`, seguindo a sequência de **01 a 07**, para reproduzir o pipeline completo:

```text
01 → Data Understanding
02 → Data Cleaning
03 → EDA Catálogo
04 → EDA Regionalização
05 → EDA Diretores & Atores
06 → EDA Defasagem
07 → Insights & Recomendações
```

O fluxo permite reproduzir o projeto desde o **entendimento e tratamento dos dados até a síntese executiva dos principais achados**.

---

## 👤 Autor

**Lucas Gumercindo**

Estudante de Ciência de Dados / Análise de Dados
