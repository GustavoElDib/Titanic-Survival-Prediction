# Titanic Survival Prediction | Machine Learning

## Descrição do Projeto

Este projeto apresenta a construção de um modelo de Machine Learning para prever a sobrevivência de passageiros no desastre do Titanic.  

O trabalho foi desenvolvido como projeto de aprendizagem em uma competição do Kaggle, com o objetivo de aplicar na prática técnicas de pré-processamento de dados, modelagem e otimização de hiperparâmetros.

O problema é tratado como uma classificação binária, em que o modelo estima a probabilidade de sobrevivência de um passageiro com base em características socioeconômicas e demográficas.

- 0 → Não sobreviveu  
- 1 → Sobreviveu  

A partir dessas informações, o modelo aprende padrões nos dados históricos para prever o desfecho de novos passageiros.

---

# Técnicas Utilizadas

O desenvolvimento do modelo seguiu um pipeline clássico de Machine Learning supervisionado, incluindo etapas de pré-processamento de dados, modelagem, otimização de hiperparâmetros e avaliação de desempenho.

---

# Pré-processamento dos dados

Antes da modelagem foi necessário preparar os dados para garantir que o modelo pudesse aprender padrões de forma eficiente.

## Tratamento de valores ausentes

A base apresenta valores faltantes em algumas variáveis, especialmente na variável Age.

Para lidar com esse problema foram utilizadas estratégias de imputação, evitando a remoção de observações e preservando a maior quantidade possível de informação da base. Para isso, foi utilizado o método de Regressão do KNN que é capaz de imputar valores mais realistas tratando a ausência de valores através da semelhança e proximidade do conjunto de dados, além disso tem resultados mais satisfatórios que média e mediana para o modelo, uma vez que não fixa um único valor para a conjunto inteiro, dado que aproximadamente 20% da feature Age era nulo


---

## Conversão de variáveis categóricas

Algumas variáveis da base são categóricas, como:

- `Sex`
- `Embarked`

Essas variáveis foram transformadas em formato numérico utilizando técnicas de codificação de variáveis categóricas, permitindo que os algoritmos de Machine Learning possam processá-las corretamente.

---

## Seleção de features

Algumas variáveis da base possuem baixa relevância preditiva ou alta cardinalidade, podendo introduzir ruído no modelo.

Variáveis como:

- `Name`
- `Ticket`
- `Cabin`

foram removidas do conjunto de dados por apresentarem grande quantidade de valores únicos ou pouca estrutura para utilização direta no modelo.

---

# Modelo de Machine Learning

O algoritmo escolhido para a modelagem foi o XGBoost Classifier, um método baseado em Gradient Boosting com árvores de decisão.

Esse tipo de algoritmo funciona construindo árvores sequencialmente, onde cada nova árvore tenta corrigir os erros cometidos pelas anteriores.

O processo de treinamento consiste em minimizar iterativamente uma função de perda/custo, ajustando os pesos do modelo a cada nova árvore adicionada.

Entre as principais vantagens do XGBoost estão:

- Alta capacidade de capturar relações não lineares
- Uso de regularização para redução de overfitting
- Excelente desempenho dos modelos

---

# Otimização de Hiperparâmetros

Para melhorar o desempenho do modelo foi utilizado o GridSearchCV, um método de busca exaustiva que testa diversas combinações de hiperparâmetros.

O GridSearchCV utiliza validação cruzada, permitindo avaliar o desempenho médio do modelo em diferentes subconjuntos da base de treinamento.

Os hiperparâmetros avaliados foram:

param_grid = {
    'n_estimators': [100, 150, 200, 300],
    'max_depth': [3, 4, 5, 7],
    'learning_rate': [0.01, 0.1, 0.2, 0.3], 
    'reg_alpha': [0, 0.1, 0.5, 1]
}
Significado dos hiperparâmetros:

n_estimators
Número de árvores utilizadas no modelo de boosting.

max_depth
Profundidade máxima de cada árvore, controlando a complexidade do modelo.

learning_rate
Taxa de aprendizado que controla o impacto de cada nova árvore adicionada ao modelo.

reg_alpha
Parâmetro de regularização L1, utilizado para reduzir a complexidade do modelo e evitar overfitting.

**Melhores Hiperparâmetros**: {'learning_rate': 0.2, 'max_depth': 3, 'n_estimators': 100, 'reg_alpha': 0.5}

## Análise de Importância das Variáveis (Feature Importance)

Modelos baseados em árvores, como o **XGBoost**, permitem avaliar a contribuição relativa de cada variável na construção das previsões. Essa análise ajuda a entender quais características tiveram maior influência na decisão do modelo.

As variáveis utilizadas no modelo e suas respectivas importâncias foram:

| Feature | Importância |
|-------|-------------|
| Sex_male | 0.6317 |
| Pclass | 0.1946 |
| SibSp | 0.0683 |
| Fare | 0.0375 |
| Age | 0.0372 |
| Parch | 0.0307 |

### Principais insights

A variável **Sex_male** foi a mais importante no modelo, representando aproximadamente **63% da importância total**. Esse resultado indica que o sexo do passageiro foi o fator mais determinante para prever a sobrevivência. Isso está alinhado com o contexto histórico do desastre, em que a política de evacuação priorizava **mulheres e crianças**. Essa hipótese é corroborada de uma análise inicial de sobreviventes da base de treino, uma vez que 74,2% das mulheres sobreviveram e apenas 19%, aproximadamente, dos homens sobreviveram. 

A segunda variável mais relevante foi **Pclass (classe do passageiro)**, com aproximadamente **19% de importância**. Passageiros da primeira classe possuíam maior acesso aos botes salva-vidas e estavam em posições mais privilegiadas no navio, o que aumentava suas chances de sobrevivência.

Variáveis como **SibSp** (número de irmãos ou cônjuges a bordo) também apresentaram alguma relevância, sugerindo que a estrutura familiar pode influenciar a probabilidade de sobrevivência, possivelmente devido a decisões tomadas em grupo durante a evacuação.

Já variáveis como **Age**, **Fare** e **Parch** apresentaram menor contribuição individual no modelo, mas ainda ajudam a capturar padrões adicionais que melhoram a capacidade preditiva.

### Interpretação geral

A análise de importância das variáveis mostra que o modelo conseguiu capturar padrões coerentes com o contexto real do desastre do Titanic. Fatores **sociais e estruturais**, como sexo e classe do passageiro, foram significativamente mais relevantes do que características individuais isoladas.


# Resultados

Após o processo de treinamento e otimização dos hiperparâmetros, o modelo apresentou o seguinte desempenho:

**Acurácia na base de teste: 78,8%**

O modelo foi utilizado para gerar previsões submetidas na competição do Kaggle.

# Resultado obtido na competição:

**Posição: 1638º lugar entre 11.400 participantes**
