# Projeto de Previsão de Demanda e Faturamento | E-commerce Olist

## Integrantes do Grupo
* [Luiz Henrique Nunes Santana](https://github.com/luizHenrique-Dev21) - CP3025608
* [Nancy Miyuki Yuzawa](https://github.com/nancyuzawa) - CP3025641
  
## O Problemático Analisado
Este projeto foca na **previsão de volume de vendas (demanda) do próximo mês** utilizando o histórico de pedidos de um e-commerce real. 

A capacidade de antecipar o volume de pedidos é altamente estratégica e fundamental para o setor de comércio eletrônico, pois impacta diretamente no planejamento e gestão de estoque, logística, dimensionamento de equipes de entrega, fluxo de caixa e no planejamento de campanhas de marketing. O objetivo é responder a questões como:
* Existe tendência temporal de crescimento nas vendas?
* Quais fatores mais influenciam o volume das vendas?
* Como os modelos preditivos se comportam diante da não-linearidade e sazonalidade dos dados?

Trata-se de um problema de **regressão supervisionada**, onde a variável-alvo (`qtd_pedidos_proximo_mes`) é contínua e construída por meio da agregação mensal dos pedidos e pelo deslocamento (*shift*) de um período para a frente, o que evita o vazamento de dados (*data leakage*) durante o treinamento do modelo.

## Dataset Utilizado
A base de dados foi obtida no [Kaggle - Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Trata-se de dados reais de pedidos realizados entre 2016 e 2018.

O conjunto de dados é relacional e composto por tabelas integradas através de IDs. No desenvolvimento do pipeline, foram carregadas e unificadas as tabelas de itens, pedidos, produtos, clientes, avaliações e pagamentos.

<img width="2486" height="1496" alt="HRhd2Y0" src="https://github.com/user-attachments/assets/ac6a3aa6-0912-4f44-a617-62fdd38664a8" />

*Fonte: Kaggle — Brazilian E-commerce (Olist)*

## Metodologia Adotada
O pipeline do projeto foi desenvolvido seguindo as seguintes etapas estritas:
1. **Limpeza Inicial e Filtros:** Filtragem para manter apenas pedidos com o status `delivered` (entregues), eliminando ruídos de cancelamentos e pendências (representando 97.0% da base total, ou seja, 96.478 de 99.441 pedidos). Remoção do primeiro e do último mês do período histórico (setembro/2016 e agosto/2018) por estarem incompletos.
2. **Engenharia de Atributos (Agregação Mensal):** Construção de um dataset mensal onde cada linha representa um mês e as colunas contêm atributos estruturados (métrica de tempo linear, mês do ano, trimestre, volume do mês anterior como *lag_1*, taxa de crescimento de pedidos, ticket médio e nota média de reviews).
3. **Validação Temporal (Treino/Teste):** Divisão dos dados respeitando a ordem cronológica para evitar que o modelo use informações do futuro de forma espúria.
   * **Treino:** 16 meses (Dezembro de 2016 a Março de 2018)
   * **Teste:** 3 meses (Abril de 2018 a Junho de 2018)
4. **Pipeline de Pré-processamento:** Uso do `Pipeline` do *Scikit-Learn* para encadear a imputação de valores ausentes (`SimpleImputer`) e a normalização/escala dos dados (`StandardScaler`), garantindo o ajuste exclusivo na base de treino.
5. **Modelagem:** Treinamento e ajuste de três modelos preditivos: Regressão Linear (como *baseline*), Random Forest Regressor e Gradient Boosting Regressor.

## Resumo dos Principais Achados da EDA (Análise Exploratória)
* **Fator Temporal e Tendência:** A série apresenta uma clara tendência de crescimento ao longo do período, com forte aceleração a partir de meados de 2017 e um pico visível e isolado no final do ano decorrente da **Black Friday**.
* **Volume vs Faturamento:** O faturamento total acompanha o crescimento do volume de pedidos, mas apresenta uma amplitude maior, sinalizando flutuações e aumento do ticket médio ao longo do tempo.
* **Comportamento do Ticket Médio:** A distribuição do ticket médio mensal é concentrada (mediana de R$ 121,76 e desvio padrão de R$ 25,33), sem outliers extremos após a limpeza dos meses incompletos.
* **Matriz de Correlação:** Identificou-se uma correlação extremamente alta (> 0.95) entre a quantidade de pedidos e o número de clientes únicos. O ticket médio apresentou correlação moderada negativa com o volume de pedidos, sugerindo que nos meses de maior pico há maior saída de produtos de menor valor (campanhas de massa e promoções). A satisfação do cliente (`review_score`) mostrou correlação fraca/moderada com o volume total futuro.

## Principais Resultados e Comparação de Modelos
A avaliação foi realizada utilizando as métricas MAE (Erro Absoluto Médio), RMSE (Raiz do Erro Quadrático Médio) e $R^2$ (Coeficiente de Determinação):

| Modelo | MAE (Pedidos) | RMSE (Pedidos) | $R^2$ |
| :--- | :---: | :---: | :---: |
| **Regressão Linear (Baseline)** | 1.943 pedidos | 2.004 pedidos | -45.6885 |
| 🥇 **Random Forest Regressor** | **262 pedidos** | **262 pedidos** | **+0.1993** |
| **Gradient Boosting Regressor** | 462 pedidos | 515 pedidos | -2.0890 |

### Análise Técnica dos Resultados:
* O **Random Forest Regressor** consagrou-se como o **melhor modelo**, apresentando o menor erro médio (errando apenas 262 pedidos por mês no período de teste) e o único com $R^2$ positivo, sendo capaz de explicar 19.9% da variância do alvo em uma série temporal.
* As variáveis mais importantes para o modelo Random Forest foram o `tempo_linear` (48.9% de importância) e o volume do mês anterior `pedidos_lag_1` (33.1% de importância).
* A **Regressão Linear** falhou gravemente (R² altamente negativo) por assumir uma relação estritamente linear que não condiz com a aceleração exponencial e as quebras de padrão sazonais encontradas no e-commerce brasileiro da Olist.

## Instruções para Execução do Notebook
Para reproduzir as análises e o modelo, siga os passos abaixo:

1. Pré-requisitos:
* Python 3.11 instalado
* Visual Studio Code (VS Code) instalado
* No VS Code, certifique-se de ter a extensão Jupyter e Python instalada.

2. Abrindo o Projeto:
Baixe ou clone o repositório:

```bash
git clone https://github.com/nancyuzawa/Olist-E-commerce.git
```

ou extraia o arquivo .zip.

3. Executando o Projeto:
Abra a pasta do projeto no VS Code (File -> Open Folder).

Abra o arquivo Olist_E_commerce.ipynb.

Selecione o Interpretador: No canto superior direito, clique em Select Kernel -> Python Environments e escolha a versão Python 3.11.

E por fim, execute o "Run All" para rodar todas as células.

# Créditos e Atribuição de Dados

Este projeto utiliza o conjunto de dados **Brazilian E-Commerce Public Dataset by Olist**, disponibilizado pela **Olist** no **Kaggle**.

* **Fonte original:** [Kaggle - Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* **Autor:** [Olist](https://www.olist.com/)
* **Licença:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

## Descrição do Trabalho Derivado
De acordo com os termos da licença **CC BY-NC-SA 4.0**, informo que os dados originais foram modificados para a realização desta análise técnica:

1. **Integração de Dados:** Foi realizado o cruzamento (*merge*) de múltiplas tabelas relacionais, incluindo pedidos, itens, produtos, clientes e avaliações.
2. **Tratamento e Limpeza:**
    * Conversão de colunas de texto para o formato cronológico (`datetime`).
    * Verificação e remoção de registros duplicados.
    * Tratamento de dados faltantes via remoção de valores nulos (*dropna*).
4. **Engenharia de Atributos:** Renomeação da variável `order_item_id` para `qtd_item` visando melhor interpretabilidade.
5. **Análise Exploratória (EDA):** Desenvolvimento de estudo estatístico e visual abrangendo volume de vendas, faturamento por categoria, correlação entre preço e demanda, além de análises de sazonalidade temporal e distribuição geográfica.

---
*Este trabalho é destinado estritamente a fins acadêmicos e está licenciado sob a mesma licença do material original ([ShareAlike](https://creativecommons.org/licenses/by-nc-sa/4.0/)).*
