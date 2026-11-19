# manutencao-preditiva-ia
# Pipeline Preditivo de Manutenção Industrial (Indústria 4.0)

Projeto de Ciência de Dados e Aprendizado de Máquina Supervisionado voltado à previsão de falhas em equipamentos fabris a partir de telemetria de sensores (temperatura, rotação, torque e desgaste de ferramenta).

## Estrutura do Repositório

```text
manutencao-preditiva-ia/
├── data/
│   └── manutencao_preditiva.csv    # Base histórica de sensores (10.000 registros)
├── projeto_preditivo.ipynb         # Notebook principal com código, gráficos e análises
├── requirements.txt                # Dependências exatas do ambiente Python 3.12
└── README.md                       # Documentação do projeto
```

## Como Executar o Projeto

1. **Clone o repositório:**
   ```bash
   git clone <URL_DO_SEU_REPOSITORIO>
   cd manutencao-preditiva-ia
   ```

2. **Instale as dependências:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Execute o Notebook:**
   Abra o arquivo `projeto_preditivo.ipynb` no Jupyter Notebook ou VS Code e execute todas as células sequencialmente (`Run All`).

## Metodologia e Principais Resultados

* **Análise Exploratória (EDA) e Data Prep:** Identificação de severo desbalanceamento na variável alvo (`96,61%` normal vs. `3,39%` falhas) e imputação de `300` registros ausentes utilizando a **mediana** (evitando viés causado por *outliers* operacionais).
* **Feature Engineering:** Criação da variável física `potencia` (`velocidade_rotacao_rpm * torque_nm`) e codificação *One-Hot* da variável categórica `tipo`.
* **Prevenção de Data Leakage:** Remoção de colunas identificadoras (`udi`, `id_produto`) e variáveis pós-falha (`falha_twf`, `falha_hdf`, `falha_pwf`, `falha_osf`, `falha_rnf`). Aplicação de **SMOTE** e **`StandardScaler`** exclusivamente sobre o conjunto de treino (`80%`).
* **Comparação de Modelos (Conjunto de Teste Inédito - 2.000 registros):**
  * **Árvore de Decisão (`max_depth=6`):** **91,80%** de acurácia no teste (detectou 50 de 68 falhas reais, com apenas 18 falsos negativos).
  * **KNN (`K=5`, dados escalonados):** **91,35%** de acurácia no teste (detectou 48 de 68 falhas reais, com 20 falsos negativos).
* **Veredito:** A **Árvore de Decisão podada (`max_depth=6`)** foi selecionada como o melhor modelo tanto pela acurácia superior quanto pela interpretabilidade das regras para a equipe de manutenção industrial.
