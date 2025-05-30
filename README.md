# Formação Aprendendo a fazer ETL G8-One ⚛️

# 📊 Desafio: Análise de Evasão de Clientes (Churn) na Telecom X

### 🔍 Introdução

A evasão de clientes é um problema crítico para empresas de telecomunicações. Manter os clientes satisfeitos e engajados é economicamente mais viável do que buscar constantemente novos assinantes. Diante disso, este projeto tem como objetivo analisar os dados dos clientes da Telecom X , identificar padrões relacionados ao cancelamento de serviços (também conhecido como *Churn* ) e extrair insights úteis para estratégias de retenção.

### Objetivo:
Entender o comportamento dos clientes que cancelam o serviço.
Identificar fatores associados à evasão.
Oferecer recomendações baseadas em dados para reduzir a taxa de Churn.

### 🧹 1. Importação, Limpeza e Tratamento de Dados
⬇️ Fonte dos dados

Os dados foram obtidos diretamente de uma API pública fornecida no desafio:

https://raw.githubusercontent.com/ingridcristh/challenge2-data-science/refs/heads/main/TelecomX_Data.json 

O dataset contém informações sobre clientes da Telecom X, incluindo dados demográficos, tipo de serviço contratado e status de cancelamento (Churn).

### Estrutura dos dados
O JSON recebido era aninhado, ou seja, continha dicionários dentro de dicionários. Para facilitar a análise, utilizamos pandas.json_normalize para transformar os dados em um DataFrame plano.

### Limpeza de dados
Realizei as seguintes etapas de limpeza:

1. Padronização de categorias : Valores como "Yes", "No", "yes", "1" e "0" foram padronizados para "Yes" e "No".
2. Tratamento de valores ausentes : Usei a moda para variáveis categóricas e a mediana para numéricas.
3. Remoção de duplicatas : Verifiquei e removi registros duplicados, se houvesse.
4. Correção de tipos de dados : Garanti que colunas numéricas estivessem no formato correto (int ou float).
5. Criação de nova coluna : Gerei a coluna Valor_Diario, calculando o valor médio diário pago pelo cliente (Valor_Mensal / 30).
7. Renomeação de colunas : Adotei nomes mais claros e compreensíveis para facilitar a interpretação por stakeholders não técnicos.

### 🔍 2. Análise Exploratória de Dados (EDA)

### 📊 Distribuição do *Churn*
Comecei analisando a distribuição geral do *Churn*:
> Não	73.46%, > Sim	26.54%

**➡️ Quase 1 em cada 4 clientes cancelou o serviço , indicando um problema relevante**.

Gráfico:
python

>sns.countplot(data=df, x='Cancelamento', palette='Set2')
plt.title('Distribuição de Cancelamento (Churn)')
plt.xlabel('Cancelamento')
plt.ylabel('Quantidade de Clientes')
plt.show()

![image](https://github.com/user-attachments/assets/75571cf0-6ae9-4f54-b8bc-a8306bac358e)

### 📈 Análise por Variáveis Categóricas
Explorei como o Churn se distribui entre diferentes categorias:

**Clientes com contrato mês a mês têm maior taxa de cancelamento** .

pd.crosstab(df['Tipo_Contrato'], df['Cancelamento'], normalize='index').plot(kind='bar', stacked=True)
plt.title('Proporção de Cancelamento por Tipo de Contrato')
plt.xlabel('Tipo de Contrato')
plt.ylabel('Proporção')
plt.xticks(rotation=0)
plt.legend(title='Cancelamento')
plt.show()

![image](https://github.com/user-attachments/assets/b2f66dd6-4037-4932-8cf2-39929d59e29f)


### 📉 Análise por Variáveis Numéricas

Também explorei como o cancelamento se relaciona com variáveis numéricas, como:

1. Tempo de contrato (Meses_de_Contrato)
2. Valor mensal (Valor_Mensal)
3. Valor total (Valor_Total)
4. Valor diário (Valor_Diario)
5. Estatísticas por grupo:


**Clientes que cancelaram tendem a ter menos tempo de contrato e pagar mais por mês.**

> sns.boxplot(data=df, x='Cancelamento', y='Valor_Mensal')
plt.title('Valor Mensal por Status de Cancelamento')
plt.show()

![image](https://github.com/user-attachments/assets/923c5382-5e51-406c-94ca-97c6a1265be6)


### 💡 3. Conclusões e Insights

## Principais Achados:
1 - Taxa de *Churn* alta : Cerca de 26,5% dos clientes cancelaram o serviço.
2 - Contratos curtos aumentam risco : Clientes com contrato mês a mês têm muito maior probabilidade de cancelar.
3 - Preço influencia no cancelamento : Clientes que pagam mais têm maior taxa de churn.
4 - Tempo de contrato protege contra cancelamento : Clientes com mais tempo de serviço tendem a permanecer.
5 - Serviços adicionais ajudam na retenção : Clientes com suporte técnico e backup online têm menor taxa de cancelamento.
6 - Forma de pagamento importa : Clientes que usam cheques como forma de pagamento têm maior taxa de evasão.


### 📌 4. Recomendações
Com base nos insights obtidos, apresento algumas recomendações estratégicas:

### ✅ Para Reduzir o *Churn*:

1. Incentivar contratos fixos : Ofereça benefícios ou descontos para clientes que optarem por contratos anuais ou bienais.
2. Melhorar oferta de serviços adicionais : Promova pacotes com suporte técnico e backup online como diferenciais.
3. Revisar preços para evitar sobrecarga financeira : Avalie se clientes que pagam acima da média estão satisfeitos com o custo-benefício.
4. Monitorar clientes com menos de 12 meses : Esse grupo tem maior risco de cancelamento; ações personalizadas podem melhorar a retenção.
5. Automatizar monitoramento de churn : Use modelos preditivos para identificar clientes em risco antes que cancelem.
6. Oferecer alternativas de pagamento : Reduzir o uso de formas de pagamento com alta taxa de evasão, como cheques.
