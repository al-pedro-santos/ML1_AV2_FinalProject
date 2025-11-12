# MachineLearning_AV2_FinalProject

Repositório desenvolvido para o **projeto final da disciplina de Aprendizado de Máquina 1 (MD22)** do **IMPA Tech**.  
O objetivo foi aplicar diferentes métodos supervisionados e não supervisionados para previsão de inadimplência a partir de dados financeiros reais.

---

### Integrantes

- Bruno Pereira de Paula  
- Isabelli Cristhini da Silva  
- Marcos Abílio Esmeraldo Melo  
- Pedro Henrique Barbosa da Silva  
- Pedro Henrique dos Reis Porto  
- Pedro Miguel Rocha Santos  
- Pedro Rodrigues Alberti  
- Vinicius Glowacki Maciel  

---

### Fonte dos Dados

Os dados foram obtidos do **dataset _Give Me Some Credit_** disponível no [Kaggle](https://www.kaggle.com/competitions/GiveMeSomeCredit).  
A proposta original da competição era:

> **Algoritmos de _credit scoring_** estimam a probabilidade de inadimplência e são utilizados por bancos para decidir se um empréstimo deve ser concedido.  
> O desafio consiste em melhorar o desempenho dos modelos de _credit scoring_, prevendo a probabilidade de que uma pessoa enfrente **dificuldades financeiras nos dois anos seguintes**.

De acordo com o organizador do dataset, a melhor ferramenta comercial disponível à época alcançava um **AUC de 0.865**. No conjunto de teste fornecido pela competição há **probabilidades de baseline** para comparação.

---

### 🧾 Descrição das Variáveis

| Variável | Descrição | Tipo |
|-----------|------------|------|
| **SeriousDlqin2yrs** | Pessoa apresentou inadimplência grave (≥ 90 dias de atraso) dentro dos dois anos seguintes | 0 = não, 1 = sim (treino); probabilidade (teste). |
| **RevolvingUtilizationOfUnsecuredLines** | Saldo total em cartões de crédito e linhas de crédito pessoais (exceto dívidas imobiliárias) dividido pelo limite total de crédito | Percentual |
| **age** | Idade do tomador (em anos) | Inteiro |
| **NumberOfTime30-59DaysPastDueNotWorse** | Número de vezes que o cliente teve atraso de 30–59 dias, mas não pior, nos últimos 2 anos | Inteiro |
| **DebtRatio** | Despesas mensais (dívidas, pensões, custos de vida) divididas pela renda bruta mensal | Percentual |
| **MonthlyIncome** | Renda mensal | Real |
| **NumberOfOpenCreditLinesAndLoans** | Quantidade de empréstimos e linhas de crédito em aberto | Inteiro |
| **NumberOfTimes90DaysLate** | Número de vezes que o cliente teve atraso de 90 dias ou mais | Inteiro |
| **NumberRealEstateLoansOrLines** | Quantidade de empréstimos imobiliários e linhas de crédito associadas | Inteiro |
| **NumberOfTime60-89DaysPastDueNotWorse** | Número de vezes que o cliente teve atraso de 60–89 dias, mas não pior, nos últimos 2 anos | Inteiro |
| **NumberOfDependents** | Número de dependentes (cônjuge, filhos etc.) | Inteiro |

---

Segundo o autor do dataset:
 
- **Y (`SeriousDlqin2yrs`)** indica se o cliente **se tornou inadimplente nos dois anos seguintes** ao início do período de observação.  
- **`NumberOfTimes90DaysLate`** e demais variáveis de atraso representam **histórico passado**, ou seja, o número de vezes que o cliente **já havia** atrasado pagamentos nos **dois anos anteriores** ao início da observação.  

---

### Metodologia

Neste trabalho, implementamos modelos estudados em sala e outros pesquisados de forma independente, buscando prever a **probabilidade de inadimplência a médio prazo**.  
Os modelos foram comparados com as probabilidades de referência presentes nos dados de teste.

**A seleção  e  a otimização dos modelos** foram feitas por validação cruzada, e:
- a métrica **AUC** foi calculada no conjunto de **validação (treino com Y = 0/1)**,  
- enquanto **MAE**, **MSE** e **correlação** foram calculadas no **conjunto de teste**, que contém probabilidades.  

Ao final de cada notebook, os resultados dos modelos utilizados foram comparados e discutidos.

---

### Estrutura do Projeto

- [**Notebook 1**](https://colab.research.google.com/drive/10szDpCCD9Tiy5opm9mP-KCuLaI67ch7o?usp=sharing) : análise exploratória de dados, com visualizações de distribuições e correlações entre variáveis.
- [**Notebook 2**](): tratamento de valores ausentes - imputação com **GAM** para a variável de renda.  
- [**Notebook 3**](): implementação de dois modelos de **regressão probabilística**: **Logit** e **Probit**.  
- [**Notebook 4**](Notebook4_lda_qda_svm.ipynb): análise da variável *número de dependentes* e avaliação de padrões por meio de **LDA, QDA e SVM**.  
- [**Notebook 5**](https://github.com/al-pedro-santos/ML1_AV2_FinalProject/blob/ef3394e2debd4872ed1559fe5022209482188f3a/Notebook5_trees.ipynb): modelos baseados em **árvores de decisão**: *Histogram Gradient Boosting*, *Random Forest* e *Decision Tree* simples.  
- [**Notebook 6**](https://github.com/al-pedro-santos/ML1_AV2_FinalProject/blob/e150cc0bbc027caadb95a49c42883d6bf517a43b/Notebook6_unsupervised.ipynb): métodos **não supervisionados** - *Isolation Forest* (usando a medida de anomalia como proxy para probabilidade de inadimplência) e *Gaussian Mixture Model* (usando a representação latente como entrada para modelos Logit e Probit).

---

### Resultados

| Modelo | MAE (teste) | MSE (teste) | Corr (teste) |  AUC (validação) |
|:--|:--:|:--:|:--:|:--:|
| Logit | – | – | – |– |
| Logit GM(4) | 0.040390 | 0.004460 | 0.797694 | 0.7640 |
| Logit GM(7) | 0.038780 | 0.004156 | 0.813338 | 0.7646 |
| Probit | – | – | – |– |
| Probit GM(4) | 0.040390 | 0.004461 | 0.797665 | 0.7640 |
| Probit GM(7) | **0.038759** | **0.004155** | 0.813527 | 0.7648 |
| Isolation Forest | 0.094175| 0.015732 |0.727324 | 0.7860 |
| Decision Tree | 0.263811 | 0.103721 | 0.7999 | 0.8492 |
| Random Forest | 0.230091 | 0.076918 | **0.8604** | **0.8619** |
| Histogram GB | 0.039583 | 0.005881 | 0.8293 |0.8232 |

---

#### Resultados - clássificação dependentes

#### Resultados - clássificação dependentes

| Modelo | Acurácia | Precisão | Recall | F1-score |
|:--|:--:|:--:|:--:|:--:|
| LDA | **0.5909** | – | – | – |
| QDA | 0.5904 | – | – | – |
| SVM (Linear) | 0.1043 | 0.53 | 0.10 | 0.04 |
| SVM (Gaussiano) | 0.2688 | 0.47 | 0.27 | 0.29 |


---

### Comentário Final

O estudo permitiu comparar o desempenho de diferentes abordagens supervisionadas e não supervisionadas na previsão de inadimplência. 

Os métodos **não supervisionados** forneceram representações úteis e intuitivas para estimação de risco em contextos sem rótulos binários explícitos (isolation forest), além de fornecer insights sobre os diferentes perfis de clientes.  
Os métodos de **árvores** ...
Os modelos de **regressão** ... 

O modelo X obteve maior AUC quando avaliado em conjunto de validação, isso ... . O modelo X obteve menor MAE quando comparado com as probabilidades do conjunto de teste ... . Se quisessemos um modelo que tivesse um desempenho semelhante ao que gerou as probabilidades do conjunto de teste, usariamos ... . Por outro lado, se quisessemos minimizar empréstimos a indivíduos com grandes chances de se tornar inandimplentes, mesmo a custas de índividuos com baixo risco não  receberem empréstimos, usariamos ... . 

