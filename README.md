# Projeto de Previsão de Demanda e Faturamento | E-commerce Olist

## Integrantes do Grupo
* Luiz Henrique Nunes Santana - CP3025608
* Nancy Miyuki Yuzawa - CP3025641
  
## O Problema Analisado
Este projeto foca na **previsão de volume de vendas (demanda) e faturamento** por categoria de produto, utilizando o histórico de pedidos de um e-commerce real. 

A previsão é fundamental para o setor de comércio eletrônico, pois impacta diretamente no planejamento de estoque, logística, distribuição e na tomada de decisões estratégicas de receita. O objetivo é responder a questões como:
* Existe tendência temporal de crescimento nas vendas?
* Quais fatores (região, categoria, preço) mais influenciam o valor das vendas?
* Como a sazonalidade e a distribuição geográfica impactam o faturamento total?

## Dataset Utilizado
A base de dados foi obtida no [Kaggle - Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Trata-se de dados reais de pedidos realizados entre 2016 e 2018.

O conjunto de dados é relacional e composto por tabelas integradas através de IDs. Como pode ser observado no esquema de dados abaixo:

<img width="2486" height="1496" alt="HRhd2Y0" src="https://github.com/user-attachments/assets/ac6a3aa6-0912-4f44-a617-62fdd38664a8" />

*Fonte: Kaggle — Brazilian E-commerce (Olist)*

## Resumo dos Principais Achados da EDA (Análise Exploratória)
Com base na análise exploratória realizada, destacam-se os seguintes pontos:
* **Concentração de Vendas:** Identificou-se uma alta concentração de pedidos em poucas categorias e regiões específicas do Brasil, o que indica a necessidade de atenção a possíveis vieses no modelo.
* **Não-Linearidade:** A relação entre variáveis como quantidade, preço e avaliação do cliente não é totalmente linear, sugerindo que modelos de regressão simples podem ter limitações.
* **Outliers e Distribuição:** Foram detectadas distribuições assimétricas e a presença de *outliers* nos preços, tornando recomendável a normalização dos dados.
* **Fator Temporal:** A variação temporal é um forte indicador de vendas, confirmando que a sazonalidade é um componente essencial para a previsão de demanda.
* **Satisfação do Cliente:** A influência do `review_score` no volume de vendas existe, mas é moderada em comparação ao preço e categoria.

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
