# Sistema de Otimização de Compras e Estoque

MVP 02 - Machine Learning
Edivaldo Bezerra Alves Júnior
PUC RJ

## 📋 Visão Geral

Este projeto implementa uma solução de Machine Learning para otimização de estoque em supermercados, utilizando dados históricos de vendas e movimentação para determinar o estoque mínimo ideal e gerar recomendações semanais de compras.

## 🏆 Resultados Alcançados

O sistema desenvolvido demonstrou resultados significativos na otimização da gestão de estoque:

- **Análise de Suficiência de Dados**
  - 62% do portfólio de produtos possui histórico robusto para modelagem preditiva
  - 38% dos SKUs identificados como baixo giro
  - Cobertura temporal média de 426 dias para produtos com histórico suficiente, garantindo análise estatística confiável

- **Assertividade do Modelo**
  - Implementação de validação cruzada (K-Fold com k=5) para garantir robustez das previsões
  - Análise de overfitting através de comparação entre treino e teste, assegurando generalização do modelo
  - Seleção robusta de features usando RFE (Recursive Feature Elimination), otimizando o poder preditivo

- **Acurácia dos Modelos**
  - Random Forest:
    - MAE: 3.45 (erro médio absoluto em unidades)
    - RMSE: 10.55 (raiz do erro quadrático médio)
    - R²: 0.83 (83% da variância explicada)
  - XGBoost:
    - MAE: 3.44
    - RMSE: 12.25
    - R²: 0.77
  - Ridge Regression:
    - MAE: 4.22
    - RMSE: 12.09
    - R²: 0.78

- **Visualização de Resultados**
  ![Comparação de Previsões por Modelo](/images/resultado_demanda_prevista_modelos.png)
  
  A análise mostra que o Random Forest (linha laranja) obteve melhor performance na previsão da demanda real (linha azul), superando o XGBoost (linha verde) e Ridge Regression na captura dos padrões.

## 🎯 Objetivos Estratégicos

1. **Otimização de Estoque Mínimo**
   - Desenvolvimento de modelo analítico para cálculo do estoque mínimo por SKU
   - Incorporação de padrões históricos de venda, sazonalidade e movimentação

2. **Automação de Recomendações de Compra**
   - Geração automática de recomendações de compra
   - Baseado em estoque mínimo ideal e demandas projetadas
   - Redução de custos operacionais

## 🔍 Fluxo do Sistema

```mermaid
graph TD
    A[Dados Históricos] --> B[Preparação dos Dados]
    B --> C[Análise Exploratória]
    C --> D[Feature Engineering]
    D --> E[Modelagem]
    E --> F[Validação]
    F --> G[Recomendações]
    G --> H[Relatórios Semanais]
```

## 🛠️ Tecnologias Utilizadas

- Stacks Principais:
  - pandas: Manipulação de dados
  - scikit-learn: Modelos de Machine Learning
  - xgboost: Modelo de boosting
  - matplotlib/seaborn: Visualização de dados
  - psycopg2: Conexão com banco de dados PostgreSQL

## 📊 Estrutura do Dataset

O sistema utiliza dados de movimentação de estoque com as seguintes características principais:

```mermaid
classDiagram
    class MovimentacaoEstoque {
        +ID: int
        +Data: datetime
        +Código: str
        +Descrição: str
        +Qtd: float
        +Saldo: float
        +Valor: float
        +Tipo: str
    }
```

## ⚙️ Implementação Detalhada

### 1. Preparação dos Dados
- **Filtragem Inicial**
  - Seleção apenas de operações PDV
  - Foco em operações de saída

- **Feature Engineering**
  ```mermaid
  graph TD
      A[Dados de Movimentação] --> B[Análise de Suficiência]
      B --> C[Métricas por Produto]
      C --> D[Variáveis Temporais]
      C --> E[Agregações Semanais]
      
      subgraph "Métricas por Produto"
          C1[Cobertura Temporal]
          C2[Vendas por Dia]
          C3[Variabilidade]
          C4[Percentual Dias com Vendas]
      end
      
      subgraph "Variáveis Temporais"
          D1[Semana do Ano]
          D2[Dia da Semana]
          D3[Final de Semana]
          D4[Início/Fim de Mês]
      end
      
      subgraph "Agregações Semanais"
          E1[Demanda Semanal]
          E2[Média Móvel]
          E3[Desvio Padrão]
      end
  ```

  O processo de Feature Engineering foi estruturado em três etapas principais:

  1. **Análise de Suficiência de Dados**
     - Implementação da classe `DataModeling` para cálculo de métricas por produto
     - Critérios de suficiência:
       - Cobertura temporal mínima de 20 dias
       - Frequência mínima de vendas diárias
       - Variabilidade mínima nas vendas
       - Percentual mínimo de dias com vendas (5%)
     - Flag `pouco_historico` para identificar produtos com histórico insuficiente

  2. **Variáveis Temporais**
     - Transformação de datas em features temporais:
       - Semana do ano e semana do mês
       - Dia da semana (0-6)
       - Indicador de final de semana
       - Indicadores de início e fim de mês
     - Objetivo: Capturar padrões sazonais e ciclos de venda

  3. **Agregações Semanais**
     - Cálculo de demanda semanal por produto
     - Agregação de vendas por semana
     - Criação de visão semanal consolidada

### 2. Modelagem
- **Seleção de Features**
  - RFE (Recursive Feature Elimination)
  - Análise de importância de variáveis
  - Correlação entre features

- **Modelos Testados**
  - Random Forest
  - XGBoost
  - Ridge Regression
  - Linear Regression

- **Validação e Overfitting**
  - K-Fold Cross Validation (k=5)
  - Grid Search para otimização de hiperparâmetros
  - Análise de diferença entre treino e teste
  - Regularização para controle de overfitting

### 3. Recomendação
- **Cálculo de Estoque Mínimo**
  - Buffer de segurança
  - Sazonalidade

- **Geração de Recomendações**
  - Previsão de demanda
  - Comparação com estoque atual
  - Ajustes por categoria

## 📈 Métricas de Avaliação e Impacto no Negócio

O projeto utiliza métricas técnicas que se traduzem em benefícios tangíveis para o negócio:

- **MAE (Mean Absolute Error)**: Erro médio absoluto em unidades, impactando diretamente na precisão das compras
- **RMSE (Root Mean Squared Error)**: Medida de erro que penaliza desvios maiores, crucial para evitar rupturas
- **R² Score**: Indicador de qualidade do modelo, refletindo sua capacidade de ajuste aos dados testados
- **MAPE (Mean Absolute Percentage Error)**: Erro percentual que impacta diretamente no planejamento financeiro

### Análise Comparativa de Modelos

| Modelo | MAE | RMSE | R² | Impacto no Negócio |
|--------|-----|------|----|-------------------|
| Random Forest | 3.45 | 10.55 | 0.83 | Melhor equilíbrio entre precisão e robustez |
| XGBoost | 3.44 | 12.25 | 0.77 | Boa performance com menor tempo de treinamento |
| Ridge Regression | 4.22 | 12.09 | 0.78 | Modelo mais simples, porém menos preciso |

O Random Forest apresentou o melhor desempenho geral, com menor MAE e RMSE, e maior R², indicando uma melhor capacidade de previsão da demanda semanal. Esta performance superior se traduz em:
- Redução de estoques desnecessários
- Otimização do capital de giro
- Aumento da eficiência operacional

## 🔄 Premissas do Modelo

- Foco em operações de saída no PDV
- Cálculo de compra sugerida: `Compra Sugerida = Demanda Prevista − Saldo Atual`
- Consideração de sazonalidade e padrões históricos

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 🔍 Try it out!

Para explorar a construção do projeto e reproduzir os resultados, acesse o notebook: [`[Edivaldo_Bezerra]_mvp_02_machine_learning.ipynb`]([Edivaldo_Bezerra]_mvp_02_machine_learning.ipynb)
