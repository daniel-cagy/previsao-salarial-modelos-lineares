# Previsão de salários com modelos de regressão linear

Projeto de ciência de dados desenvolvido para comparar modelos lineares na previsão da variável `wage`. Foram avaliadas regressões lineares simples, regressão linear múltipla e regressão Lasso, utilizando o Erro Absoluto Médio (MAE) como métrica principal.

## Objetivo

O trabalho busca responder à seguinte pergunta:

> Qual das abordagens consideradas — regressão linear simples, regressão linear múltipla ou Lasso — apresenta o menor Erro Absoluto Médio na previsão da variável `wage` a partir das características disponíveis?

O estudo tem finalidade preditiva. Os resultados e coeficientes não devem ser interpretados como relações causais entre as características pessoais e o salário.

## Metodologia

O conjunto utilizado possui 3.000 observações, oito variáveis preditoras e a variável-alvo `wage`. O procedimento experimental inclui:

- separação aleatória em 80% para treinamento e 20% para teste, com semente 42;
- normalização Min-Max das variáveis `year` e `age`;
- codificação *one-hot* das variáveis categóricas;
- validação cruzada repetida com cinco divisões e três repetições;
- comparação de oito regressões simples, uma regressão múltipla e 200 configurações do Lasso;
- seleção pelo menor MAE médio de validação;
- avaliação final no conjunto de teste, mantido isolado durante a seleção;
- rastreamento dos experimentos com MLflow.

Foram realizados 3.135 ajustes dos modelos candidatos e 15 avaliações adicionais do baseline baseado na mediana, totalizando 3.150 avaliações individuais.

## Principais resultados

| Modelo | MAE de teste |
|---|---:|
| Regressão linear múltipla | **22,14** |
| Lasso | 22,21 |
| Regressão simples — educação | 24,13 |
| Baseline da mediana | 28,32 |

A regressão linear múltipla apresentou o menor MAE no teste, com redução aproximada de 21,8% em relação ao baseline. O Lasso obteve desempenho muito próximo e selecionou `alpha = 0,063682`, zerando apenas um dos 16 coeficientes.

## Estrutura do repositório

```text
.
├── data/                       # Dados locais (não versionados)
├── notebooks/
│   └── main.ipynb             # Análise, treinamento e exportação dos resultados
├── report/
│   ├── figures/               # Figuras utilizadas no relatório
│   ├── sections/              # Seções do relatório em LaTeX
│   ├── main.tex               # Documento principal
│   └── main.pdf               # Relatório final compilado
├── results/
│   └── rodadas_mlflow.xlsx    # Avaliações individuais dos experimentos
└── requirements.txt           # Dependências do projeto
```

## Como executar

É recomendado utilizar Python 3.12 em um ambiente virtual.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

O conjunto de dados não é armazenado no repositório. Antes da execução, coloque o arquivo na seguinte localização:

```text
data/wage.csv
```

Depois, inicie o JupyterLab e execute todas as células de `notebooks/main.ipynb` na ordem apresentada:

```bash
jupyter lab
```

O notebook cria localmente o banco `mlflow.db`, os artefatos do MLflow, as figuras do relatório e a planilha `results/rodadas_mlflow.xlsx`.

## Visualização dos experimentos

Após executar o notebook, a interface do MLflow pode ser iniciada na raiz do projeto com:

```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db
```

A interface ficará disponível, por padrão, em `http://127.0.0.1:5000`.

## Relatório e resultados

- [Relatório final](report/main.pdf)
- [Notebook principal](notebooks/main.ipynb)
- [Planilha das rodadas](results/rodadas_mlflow.xlsx)

## Tecnologias utilizadas

Python, pandas, NumPy, scikit-learn, Matplotlib, Seaborn, MLflow, JupyterLab e LaTeX.
