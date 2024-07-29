# Projeto de análise de Dados usando Python e SQL

Esse projeto é uma demonstração da análise de uma base de dados usando linguagem SQL para consultas inseridas dentro dos códigos Python. Essa base de dados está disponível no site **Kaggle¹** com o nome de **E-commerce dataset by Olist (SQLite)**

Nesse projeto, busquei utilizar consultas SQL dentro do código Python para simular o uso de consultas avançadas, para auxiliar na análise de uma base de dados real, ao invés de usar comandos Python usando a biblioteca pandas, como normalmente é feito. As bibliotecas Seaborn e Matplotlib foram utilizadas para gerar os gráficos, já a biblioteca Folium foi usada para gerar o mapa de calor, contendo uma análise visual da distribuição geográfica dos dados da base de dados.

## Tecnologias Utilizadas

Esse projeto foi executado utilizando Python e linguagem SQL (análise de dados). Foram utilizados o Git (Versionamento de código), o DBeaver para testes, o SQLite como tecnologia de banco de dados e o Debian Linux como Sistema Operacional.

**Links relacionados:**

* [SQLite](https://www.sqlite.org/)
* [DBeaver](https://dbeaver.io/)
* [Git](https://git-scm.com/)
* [Debian Linux](https://www.debian.org/index.pt.html)

### Dependências e Versões Necessárias

Os softwares e bibliotecas utilizados no projetos tinham a seguintes versões:

* DBeaver- Versão: 23.2.5
* Debian Linux - Versão: 12
* Visual Studio Code - Versão: 1.91.1

**Obs:** Para descrição completa ou replicação do ambiente de desenvolvimento, favor usar o aquivo **"requirements.txt"**

## Como rodar o projeto (Linux)

Para rodar o projeto o seguinte método pode ser utilizados:

**Passo 1:**

Na pagina principal do projeto [https://github.com/thaleswillreis/Analise-Dados-E-commerce.git](https://github.com/thaleswillreis/Analise-Dados-E-commerce.git), busque pelo botão verde contendo o nome "<> Code".

**Passo 2:**

Clique em "Download ZIP"

**Passo 3:**

Descompacte a pasta que você terminou de baixar.

**Passo 4:**

Abra o arquivo com extenção .ipynb para visualizar o código completo no seu editor de código.

**IMPORTANTE:** Antes de acessar o arquivo de código, certifique-se que seu editor de código consegue editar arquivos Jupyter Notebook.


**ATENÇÃO:**

Caso tenha dificuldades de rodar o projeto devido a falta das dependências, inicie um ambiente virtual a partir da pasta do projeto e instale as dependências a partir do aquivo **"requirements.txt"** usando a seguinte sequência de comandos:

***Criação de um ambiente virtual:***

Abra um terminal a partir da pasta do projeto:

```python
virtualenv .venv
```

```python
source .venv/bin/activate
```

```python
pip install -r requirements.txt
```

Ao finalizar o uso do código do projeto, desative o ambiente:

```python
deactivate
```

## Considerações Finais

Caso esse conteúdo seja útil para você, tenha alguma dúvida ou queira contribuir com alguma melhoria, deixe seu comentário ou contribua com o projeto.

1 https://www.kaggle.com/datasets/terencicp/e-commerce-dataset-by-olist-as-an-sqlite-database/datahttps://www.kaggle.com/datasets/terencicp/e-commerce-dataset-by-olist-as-an-sqlite-database/data
