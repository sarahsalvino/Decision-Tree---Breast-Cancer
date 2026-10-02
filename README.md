# Breast Cancer Classificação com Árvore de Decisão

Modelo de classificação para identificar tumores de mama como **Benignos** ou **Malignos** com base em 30 características médicas extraídas de imagens de células, utilizando uma Árvore de Decisão.

---

## Objetivo

Classificar tumores de mama como Benigno (B) ou Maligno (M) a partir de características computadas de imagens digitalizadas de biópsias, contribuindo para o apoio ao diagnóstico médico.

---

## Estrutura do Projeto

```
 projeto
 ┣Breast_Cancer.ipynb        # Notebook principal
 ┣breast-cancer.csv          # Dataset
 ┗ README.md
```

---

## Dataset

- **Fonte:** [Kaggle — Breast Cancer Dataset](https://www.kaggle.com/datasets/yasserh/breast-cancer-dataset)
- **569 registros** | **32 colunas originais** | **30 features utilizadas**
- **Sem valores nulos** | **Sem duplicados**
- **Coluna removida:** `id` identificador único do paciente, sem valor preditivo

### Distribuição dos diagnósticos

| Diagnóstico | Quantidade |
|-------------|------------|
| Benigno (B) | 357 (62.7%) |
| Maligno (M) | 212 (37.3%) |

### Entendendo as colunas

O dataset possui 30 features numéricas divididas em 3 grupos, cada um com 10 características calculadas sobre as células do tumor:

**`_mean`** — média dos valores de todas as células da imagem  
**`_se`** — erro padrão (variação entre as células)  
**`_worst`** — média dos 3 piores valores (células mais atípicas)

As 10 características medidas em cada grupo são:

| Característica | O que representa |
|----------------|-----------------|
| `radius` | Raio médio das células (distância do centro à borda) |
| `texture` | Textura — desvio padrão dos valores de escala de cinza |
| `perimeter` | Perímetro das células |
| `area` | Área das células |
| `smoothness` | Suavidade — variação local nos comprimentos dos raios |
| `compactness` | Compacidade — medida de quão circular é a célula |
| `concavity` | Concavidade — severidade das partes côncavas do contorno |
| `concave points` | Pontos côncavos — número de partes côncavas no contorno |
| `symmetry` | Simetria das células |
| `fractal_dimension` | Dimensão fractal — complexidade do contorno da célula |

**Exemplo de leitura:** `radius_mean` é o raio médio de todas as células; `radius_worst` é a média dos 3 maiores raios encontrados na imagem os casos mais extremos.

---

## Modelo — Árvore de Decisão

### Por que Árvore de Decisão?
A Árvore de Decisão cria uma sequência de perguntas sobre os atributos do tumor tipo *"o raio médio é maior que X? Se sim, vai por esse caminho..."* até chegar a uma classificação. É um modelo interpretável e eficiente para problemas de classificação binária.

### Pipeline utilizado
```python
Pipeline(steps=[
    ('preprocessor', ColumnTransformer([
        ('num', SimpleImputer(strategy='mean'), numeric_cols)
    ])),
    ('classifier', DecisionTreeClassifier(random_state=42))
])
```

O `SimpleImputer` foi incluído como proteção preventiva para eventuais valores nulos, e o `random_state=42` garante reprodutibilidade dos resultados.

### Divisão dos dados
- **80% treino** — 455 amostras
- **20% teste** — 114 amostras

---

## Resultados

### Métricas de avaliação

| Métrica | Benigno | Maligno |
|---------|---------|---------|
| Precision | **0.96** | **0.93** |
| Recall | **0.96** | **0.93** |
| F1-score | **0.96** | **0.93** |
| **Acurácia geral** | | **0.95 (95%)** |

### Entendendo as métricas

- **Precision** — de tudo que o modelo classificou como Maligno, 93% realmente eram malignos
- **Recall** — de todos os tumores realmente Malignos, o modelo identificou 93% corretamente
- **F1-score** — equilíbrio entre Precision e Recall

### Ponto de atenção — Recall Maligno

O **Recall de 0.93 para Maligno** é a métrica mais crítica nesse contexto. Significa que o modelo deixou de identificar **7% dos casos malignos**, classificando-os como benignos (falsos negativos). Em aplicações médicas reais, esse tipo de erro é o mais grave um tumor maligno não detectado pode atrasar o diagnóstico e o tratamento do paciente.

---

## Conclusão

O modelo atingiu **95% de acurácia** na classificação de tumores, com desempenho ligeiramente superior para tumores benignos (96%) em relação aos malignos (93%). O resultado é muito bom considerando a simplicidade do algoritmo utilizado.


<img width="496" height="387" alt="image" src="https://github.com/user-attachments/assets/a276b657-eb98-4e7b-9b18-aa1ac68cd3b8" />


<img width="524" height="389" alt="image" src="https://github.com/user-attachments/assets/20384c31-0153-4fd4-8a43-1b360626f404" />

- Como próximos passos, seria interessante testar modelos mais robustos como **Random Forest** ou **SVM**, buscando especialmente aumentar o Recall para a classe Maligno e reduzir os falsos negativos.
---

## Tecnologias Utilizadas

- Python 3
- Pandas
- Scikit-learn
- Seaborn
- Matplotlib

---

## Como Executar

1. Clone o repositório
```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
```

2. Instale as dependências
```bash
pip install pandas scikit-learn seaborn matplotlib
```

3. Abra o notebook
```bash
jupyter notebook Breast_Cancer.ipynb
```

> Certifique-se de que o arquivo `breast-cancer.csv` está na mesma pasta do notebook antes de rodar.
