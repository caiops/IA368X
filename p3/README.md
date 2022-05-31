# Projeto 3 - Reproduzindo o Experimento de um Artigo Científico

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [Ciência e Visualização de Dados em Saúde](https://ds4h.org/), oferecida no primeiro semestre de 2022, na Unicamp.

| Nome | RA | Especialização |
| --- | --- | --- |
| Caio Pinheiro Santana | 218653 | Elétrica |
| Bruno Rangel Balbino dos Santos | 218450 | Elétrica |

## Referência bibliográfica do artigo lido

<!-- Coloque aqui a referência bibliográfica do artigo lido, incluindo o link para o site. -->

ALEXANDER-BLOCH, Aaron F. et al. The architecture of co-morbidity networks of physical and mental health conditions in military veterans. Proceedings of the Royal Society A, v. 476, n. 2239, p. 20190790, 2020. Link: http://doi.org/10.1098/rspa.2019.0790.

## Resumo

<!-- Escreva um breve resumo do artigo (com as suas palavras, não deve ser copiado texto do artigo). -->

A co-ocorrência de condições médicas e/ou psiquiátricas tem um grande impacto nos custos atrelados à saúde, interferindo na eficácia de tratamentos e criando limitações aos sistemas de classificação diagnóstica, por exemplo. Dessa forma, a ideia do artigo é utilizar diversas condições de saúde como parte de uma rede de comorbidade, na tentativa de detectar e quantificar tendências gerais que não seriam possíveis de se observar utilizando outras abordagens.

Foram utilizados registros eletrônicos de saúde de quase 1 milhão de veteranos militares dos EUA - população escolhida devido ao grande número de registros disponíveis e à desproporcionalidade com a qual é impactada por condições psiquiátricas. Então, extraiu-se 95 condições médicas e psiquiátrias como os nós das redes. A partir de modelos de regressão logística, determinou-se as chances de uma condição afetar o diagnóstico de outra, de modo que as arestas das redes foram formadas por relações positivas (_log odds_ > 1) e significativas (p < 0.05) entre os pares de condições. Assim, cada aresta representa o aumento do _log odds_ (probabilidade logística??) de uma condição dada outra condição, gerando uma rede direcionada.

As forças internas e externas - soma dos pesos das arestas direcionadas para um determinado nó ou saindo dele, respetivamente - foram utilizadas para indicar a capacidade de outras condições em predizer o diagnóstico de uma determinada condição ou a capacidade da condição em questão de predizer o diagnóstico de outras, respectivamente.

se muitas condições podem prever o diagnóstico de uma determinada condição ou se a condição em questão pode predizer 

A força - propriedade da rede dada pela soma dos pesos de todas as arestas de um determinado nó - foi utilizada para medir a comorbidade de uma condição, dividida entre "força interna" - considerando apenas as arestas direcionadas para o nó - e "força externa" - considerando apenas as arestas saindo do nó.

## Breve descrição do experimento/análise do artigo que foi replicado

<!-- Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída. -->

Os dados foram disponibilizados em arquivos .txt no GitHub, mas a nomeclatura utilizada nessas tabelas era diferente da nomeclatura utilizada no artigo e nos nós da rede (abreviações).

A tabela 2 foi usada também pra separar as morbidades psicológicas das demais morbidades médicas, permitindo fazer as análises separadas da mesma forma que no artigo.

Pegamos a "tabela 2" do artigo, transformando-a em um arquivo do Excel (.xlsx). No entanto, notamos que essa tabela continha apenas 94 abreviações, enquanto as tabelas do GitHub possuíam 95 morbidades. Ainda, as figuras da rede apresentadas no artigo também continham 95 nós. Para facilitar a busca pela morbidade faltante, fizemos a contagem - através do próprio Excel - de quantas morbidades eram apresentadas em cada módulo da tabela 2 (coluna "community (7-module consensus)") e comparamos com o número de nós em cada um dos módulos apresentados na figura 3 do artigo (representados em diferentes cores). Vale notar que, durante esse processo, percebemos também que a morbidade de abreviação Uri (_urinary system disorder_) estava relacionada ao módulo HEM, enquanto as demais morbidades com mesma cor na figura 3 do artigo estavam atreladas ao módulo HEM/CIRC. Portanto, na tabela do Excel gerada, substituímos HEM por HEM/CIRC para a morbidade Uri.

Após realizar a contagem, descobrimos que o módulo RESP (em roxo na figura 3 do artigo) possuía apenas 14 morbidades na tabela, mas apresentava 15 nós na figura da rede. Após comparar as morbidades da tabela atreladas a esse módulo com os nós roxos da figura, descobrimos que o nó Lup não estava incluído na tabela. Dessa forma, ele foi incluído na tabela do Excel gerada, atrelado ao módulo RESP.

Por fim, para conseguir atrelar os nomes das morbidades nas tabelas disponibilizadas no GitHub com as abreviações utilizadas como nós da rede, pegamos a lista de nomes das tabelas do GitHub e, um a um, tentamos associar com as abreviações/condições apresentadas na tabela 2. De modo geral, a associação foi muito direta, de modo que os nomes disponibilizados pelo GitHub casavam quase perfeitamente com a condição apresentada na tabela 2. Em alguns casos essa associação não foi tão direta, mas utilizamos uma abordagem de associar aqueles mais obviamente relacionados e, por eliminação, acabou sendo relativamente fácil associar todas as abreviações aos respectivos nomes da tabela do GitHub.

### Dados usados como entrada

|      **Dataset**     |               **Endereço na Web**               |                **Resumo descritivo**                |
|:--------------------:|:-----------------------------------------------:|:---------------------------------------------------:|
| Comorbidity Networks | https://github.com/aaronab/comorbidity_networks | Breve resumo (duas ou três linhas) sobre o dataset. |

## Método

<!-- Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos. Etapas do processo reproduzido. -->

## Resultados

<!-- Apresente os resultados obtidos pela sua adaptação. Confronte os seus resultados com aqueles do artigo. Esta seção opcionalmente pode ser apresentada em conjunto com o método. -->
