# Análise de Dados SRAG (2019-2025)

## 👥 Autores
- **Andrey Gabriel Ferreira Gonçalves**
- **Julia Peghini Vilela Borges**
- **Jaqueline Nobre da Silva**

## Vídeo de Apreseentação

O vídeo gravado para apresentar os resultados encontrados durante a pesquisa se encontra no link do Google Drive abaixo:

https://drive.google.com/file/d/1Bzasmp0SL-HskVjKe0eCN4Q8okll3Y5t/view

## 📂 Estrutura de Arquivos
O notebook espera que os arquivos CSV (`INFLUD19-*.csv`, etc.) estejam localizados em uma pasta `./dados` na raiz do projeto, conforme estrutura abaixo:

```
/
├── .gitignore
├── Documentação Projeto ICD.pdf
├── main.ipynb
├── README.md
└── dados/
    ├── INFLUD19-*.csv
    ├── ...
    └── INFLUD25-*.csv
```

## 🎯 Objetivo
Este projeto realiza a consolidação, análise diagnóstica e modelagem preditiva de dados de Síndrome Respiratória Aguda Grave (SRAG) no Brasil, abrangendo o período de **2019 a 2025**.

O foco central é avaliar a qualidade dos dados brutos, preparar a base para modelos preditivos de gravidade (definindo a admissão em **UTI** como variável alvo) e testar algoritmos de classificação.

## 📊 Dados e Escopo
- **Fonte:** Arquivos anuais de notificação (`INFLUD19` a `INFLUD25`).
- **Volume Inicial:** Aproximadamente 4.4 milhões de notificações.
- **Filtro de Escopo:** A análise restringiu-se a pacientes **hospitalizados** (~95% da base original).

## 🛠️ Metodologia e Pipeline

### 1. Definição de Variáveis
*   **Target (Alvo):** `ALVO_GRAVIDADE`
    *   `1`: Admitido em UTI.
    *   `0`: Não admitido em UTI (ou dado ignorado/ausente).
*   **Variáveis Explicativas (Comorbidades):**
    *   Cardiopatia, Diabetes, Obesidade, Doença Renal, Asma, Imunodepressão, Síndrome de Down, entre outras.
*   **Variável de Controle:** Idade (`NU_IDADE_N`).

### 2. Tratamento de Dados
*   **Limpeza de Ruído:** Devido à alta taxa de dados ausentes nas comorbidades (55% a 65%), adotou-se a premissa conservadora: **ausência de marcação "Sim" = "Não"**.
*   **Idade:** Padronização para anos e remoção de *outliers* (idades negativas ou > 120 anos).
*   **Otimização:** *Downcasting* de tipos numéricos para otimizar o uso de memória.

### 3. Análises Realizadas
*   **Integridade dos Dados:** Quantificação de nulos e ignorados por variável.
*   **Análise Temporal:** Visualização do volume de casos e taxa de UTI ao longo do tempo, identificando ondas pandêmicas.
*   **Perfil de Risco:** Correlação entre idade/comorbidades e a chance de admissão em UTI.

### 4. Modelagem Preditiva
*   **Baseline (Regressão Logística):** O modelo inicial apresentou acurácia de ~78%, porém sofreu com o desbalanceamento das classes, tendo baixo *recall* para casos graves (UTI).
*   **Estratégias de Melhoria:**
    *   **Undersampling:** Aplicação de balanceamento manual (50/50) entre as classes.
    *   **PCA (Análise de Componentes Principais):** Redução de dimensionalidade mantendo 95% da variância explicada.
    *   **Árvore de Decisão:** Teste de um modelo não linear na base balanceada, resultando em métricas mais equilibradas entre as classes.

## 🔍 Principais Insights
1.  **Fatores de Risco:**
    *   **Idade:** Forte correlação positiva com a gravidade.
    *   **Comorbidades:** *Obesidade* e *Doença Renal* destacaram-se com as maiores taxas proporcionais de conversão para UTI.
2.  **Desafios de Modelagem:**
    *   O forte desbalanceamento dos dados (maioria dos pacientes não vai para UTI) enviesa modelos estatísticos simples. Técnicas de reamostragem são essenciais para que o modelo aprenda a identificar os casos graves.
3.  **Qualidade dos Dados:**
    *   As variáveis de comorbidade possuem baixa confiabilidade no preenchimento original, exigindo tratamento agressivo para serem utilizáveis.

## 🚀 Tecnologias Utilizadas
*   **Linguagem:** Python
*   **Bibliotecas:**
    *   `pandas` & `numpy` (Manipulação de dados)
    *   `matplotlib` & `seaborn` (Visualização)
    *   `sklearn` (Machine Learning: Regressão Logística, Árvore de Decisão, PCA, Métricas)
    *   `os` (Manipulação de arquivos)

---
*Trabalho desenvolvido no contexto acadêmico para análise de dados de saúde pública.*
