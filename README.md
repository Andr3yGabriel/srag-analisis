# Análise de Dados SRAG (2019-2025)

## 👥 Autores
- **Andrey Gabriel Ferreira Gonçalves**
- **Julia Peghini Vilela Borges**
-**Jaqueline Nobre da Silva**

## 🎯 Objetivo
Este projeto realiza a consolidação e análise diagnóstica de dados de Síndrome Respiratória Aguda Grave (SRAG) no Brasil, abrangendo o período de **2019 a 2025**.

O foco central é avaliar a qualidade dos dados brutos (identificação de nulos, ignorados e viés de seleção) e preparar a base para modelos preditivos de gravidade, definindo a admissão em **UTI** como variável alvo.

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
*   **Otimização:** *Downcasting* de tipos numéricos para otimizar o uso de memória durante o processamento de grandes volumes.

### 3. Análises Realizadas
*   **Integridade dos Dados:** Quantificação de nulos e ignorados por variável.
*   **Análise Temporal:** Visualização do volume de casos e taxa de UTI ao longo do tempo, permitindo identificar ondas pandêmicas e possíveis colapsos no sistema (ex: queda na taxa de UTI durante picos de casos).
*   **Perfil de Risco:** Correlação entre idade/comorbidades e a chance de admissão em UTI.

## 🔍 Principais Insights
1.  **Fatores de Risco:**
    *   **Idade:** Forte correlação positiva com a gravidade.
    *   **Comorbidades:** *Obesidade* e *Doença Renal* destacaram-se com as maiores taxas proporcionais de conversão para UTI.
2.  **Limitações do Target:**
    *   Identificou-se o risco da **"Morte Invisível"**: pacientes que falecem na enfermaria ou emergência sem vaga de UTI são classificados como "não graves" (0) na lógica atual, o que pode enviesar modelos futuros.
3.  **Qualidade dos Dados:**
    *   As variáveis de comorbidade possuem baixa confiabilidade no preenchimento original, exigindo tratamento agressivo para serem utilizáveis.

## 🚀 Tecnologias Utilizadas
*   **Linguagem:** Python
*   **Bibliotecas:**
    *   `pandas` (Manipulação de dados)
    *   `numpy` (Cálculo numérico)
    *   `matplotlib` & `seaborn` (Visualização de dados)
    *   `os` (Manipulação de arquivos)

## 📂 Estrutura de Arquivos
O notebook espera que os arquivos CSV (`INFLUD19-*.csv`, etc.) estejam localizados em uma pasta `./dados`.

---
*Trabalho desenvolvido no contexto acadêmico para análise de dados de saúde pública.*
