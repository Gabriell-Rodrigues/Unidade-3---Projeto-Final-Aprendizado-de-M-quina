# Previsão de Pontuação na Fórmula 1 (Top 10)

Projeto final da disciplina de Aprendizado de Máquina. A partir dos dados históricos da
Fórmula 1 (1950 a 2024), o objetivo é prever se um piloto termina uma corrida entre os
10 primeiros, ou seja, se pontua.

## Objetivo

Construir e comparar modelos de classificação que, a partir de informações conhecidas
antes ou no início da corrida (por exemplo, a posição de largada, a equipe e o circuito),
prevejam se o piloto vai pontuar. A aplicação percorre todas as etapas de um projeto de
aprendizado de máquina: compreensão dos dados, análise exploratória, pré-processamento,
treinamento, comparação de modelos, avaliação e discussão crítica.

## Integrantes

Preencher com nome completo e usuário do GitHub de cada integrante.

| Papel no trabalho | Nome completo | GitHub |
|---|---|---|
| Pessoa A | Gabriel Rodrigues dos Santos | @Gabriell-Rodrigues |
| Pessoa B | Ysrael de Jesus Sacramento | @ysrael12 |
| Pessoa C | Evilásio Cavalcante de Melo Neto | @EvilasioNtt |

## Fonte dos dados

Formula 1 World Championship (1950 a 2024), disponível no Kaggle:
https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020

O conjunto reúne corridas, pilotos, construtores, classificação, tempos de volta, paradas
nos boxes e campeonatos. Para este trabalho são usados os arquivos `results.csv`,
`races.csv`, `drivers.csv`, `constructors.csv`, `qualifying.csv`, `status.csv` e
`circuits.csv`.

Os arquivos usados ficam versionados na pasta `data/` deste repositório e são carregados
diretamente pelas URLs públicas do GitHub. O notebook não depende de nenhum arquivo que
exista apenas no computador dos integrantes.

## Tipo da tarefa

Classificação binária.

## Atributo-alvo e atributos preditivos

Atributo-alvo: `pontuou`, variável binária construída no notebook.
Vale 1 se o piloto terminou entre os 10 primeiros e 0 caso contrário. A regra de pontuação
da F1 mudou ao longo das temporadas, então o alvo é definido pela posição final (top 10),
que é um critério estável em todo o período.

Atributos preditivos candidatos:

- `grid`: posição de largada
- construtor (equipe)
- piloto
- circuito
- ano e etapa da temporada

Cuidado com vazamento: não podem ser usados como preditores atributos que só existem depois
da corrida, como posição final, pontos, número de voltas, tempo, status e volta mais rápida.
Esses campos são usados apenas para montar o alvo, nunca como entrada do modelo.

## Organização dos arquivos

```
.
├── README.md              # este arquivo
├── notebook.ipynb         # notebook principal, roda de ponta a ponta no Colab
└── data/                  # arquivos .csv usados, carregados por URL pública
    ├── results.csv
    ├── races.csv
    ├── drivers.csv
    ├── constructors.csv
    ├── qualifying.csv
    ├── status.csv
    └── circuits.csv
```

## Como abrir e executar no Colab

1. Abra o notebook no Google Colab por um destes caminhos:
   - clique no selo abaixo, ou
   - no Colab, use `Arquivo > Abrir notebook > GitHub` e cole a URL do repositório.
2. No menu, use `Ambiente de execução > Executar tudo`.
3. Não é preciso enviar nenhum arquivo. Os dados são baixados automaticamente das URLs
   públicas do GitHub.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Gabriell-Rodrigues/Unidade-3---Projeto-Final-Aprendizado-de-M-quina/blob/main/notebook.ipynb)

Exemplo de carregamento dos dados usado no notebook:

```python
import pandas as pd

base = "https://raw.githubusercontent.com/Gabriell-Rodrigues/Unidade-3---Projeto-Final-Aprendizado-de-M-quina/main/data/"

# os arquivos da F1 marcam ausência com \N, então convertemos esse marcador para NaN na leitura
results = pd.read_csv(base + "results.csv", na_values=r"\N")
races = pd.read_csv(base + "races.csv", na_values=r"\N")
drivers = pd.read_csv(base + "drivers.csv", na_values=r"\N")
constructors = pd.read_csv(base + "constructors.csv", na_values=r"\N")
qualifying = pd.read_csv(base + "qualifying.csv", na_values=r"\N")
status = pd.read_csv(base + "status.csv", na_values=r"\N")
circuits = pd.read_csv(base + "circuits.csv", na_values=r"\N")
```

## Modelos utilizados

Modelos mínimos exigidos para classificação:

- `SGDClassifier`
- `RandomForestClassifier`

Além disso, é usado um baseline para servir de referência (por exemplo, `DummyClassifier`
prevendo sempre a classe majoritária). A escolha do modelo final é justificada pela
comparação das métricas com validação cruzada.

## Principais resultados

Preencher após executar o notebook. Métricas medidas no conjunto de teste.

| Modelo | Acurácia | Precisão | Revocação | F1 |
|---|---|---|---|---|
| Baseline | 0.577 | 0.000 | 0.000 | 0.000 |
| SGDClassifier | 0.702 | 0.656 | 0.621 | 0.638 |
| RandomForestClassifier | 0.689 | 0.643 | 0.597 | 0.619 |

O modelo escolhido foi o SGDClassifier pois, além de superar amplamente o baseline, apresentou o melhor equilíbrio geral de métricas, especialmente uma melhor capacidade de identificar a classe minoritária (Revocação) em relação ao Random Forest, resultando no melhor F1-Score.

## Divisão das contribuições

A divisão foi feita por blocos do notebook, equilibrada pelos pesos da nota, para que
nenhum integrante fique sobrecarregado. Cada pessoa também apresenta sua parte no vídeo,
justifica ao menos uma decisão técnica e interpreta ao menos um resultado.

| Pessoa | Responsabilidades |
|---|---|
| A | Definição do problema (seção 1) e compreensão dos dados (seção 2). Monta o repositório, sobe os `.csv` em `data/`, cria o esqueleto do notebook e garante a execução no Colab. No README, cuida da organização dos arquivos, das instruções do Colab e da declaração de uso de IA. |
| B | Análise exploratória (seção 3), pré-processamento (seção 4) e separação treino/teste (seção 5). |
| C | Modelagem, baseline e comparação (seção 6) e avaliação com discussão de erros e limitações (seção 7). Preenche a tabela de principais resultados no README. |

Detalhamento por pessoa:

Pessoa A
- Seção 1: título, integrantes, fonte dos dados, objetivo, atributo-alvo, atributos
  preditivos e tipo da tarefa.
- Seção 2: número de registros e atributos, tipos das variáveis, valores ausentes,
  duplicações, inconsistências, distribuição do alvo e desbalanceamento, tudo com
  interpretação, não apenas `head`, `info` ou `describe`.
- Repositório: criar o projeto no GitHub, subir os arquivos de `data/`, montar o notebook
  e conferir que roda no Colab carregando os dados por URL pública.
- Vídeo: apresenta a definição do problema e a compreensão dos dados.

Pessoa B
- Seção 3: histogramas, boxplots, dispersão, tabelas de frequência, correlações e a relação
  entre preditores e alvo. Cada figura ou tabela vem com explicação.
- Seção 4: tratar valores ausentes, variáveis categóricas, escalonamento, valores extremos,
  atributos irrelevantes, duplicações e inconsistências. Usar `Pipeline` e
  `ColumnTransformer` para evitar vazamento entre treino e teste. Para cada tratamento,
  explicar o problema encontrado, o que foi aplicado e por quê.
- Seção 5: separar treino e teste com `train_test_split` estratificado pelo alvo, justificar
  a proporção (por exemplo 80/20), reservar o teste para o fim e usar validação cruzada na
  comparação dos modelos.
- Vídeo: apresenta a análise exploratória e o pré-processamento.

Pessoa C
- Seção 6: baseline, treino do `SGDClassifier` e do `RandomForestClassifier`, principais
  parâmetros, comparação com validação cruzada e escolha justificada do modelo final.
- Seção 7: matriz de confusão, acurácia, precisão, revocação e F1 no conjunto de teste.
  Discutir qual modelo foi melhor, por que foi escolhido, quais erros aparecem, quais são as
  limitações e o que poderia melhorar.
- README: preencher a tabela de principais resultados.
- Vídeo: apresenta os modelos, as métricas e a discussão.

## Link do vídeo

https://drive.google.com/file/d/1emKHBq_K_vQyDW1jVdIFfdmbIGv-rgYG/view?usp=drive_link

## Declaração de uso de ferramentas de IA

| Item | Descrição |
|---|---|
| Ferramenta | Claude (assistente de IA da Anthropic). |
| Finalidade | Dividir as tarefas entre os integrantes, montar a estrutura do README e apoiar a redação dos textos das seções. |
| Parte do trabalho | Planejamento, organização dos arquivos e redação dos textos. |
