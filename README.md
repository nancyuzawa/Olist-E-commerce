# Olist-E-commerce
Previsão de demandas de itens por categoria de produto com base no histórico pedidos do e-commerce.


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
