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

O artigo foi localizado através do [índice recomendado](https://icon.colorado.edu/#!/networks) na descrição do projeto, filtrando os artigos pelo subdomínio _disease_.

## Resumo

<!-- Escreva um breve resumo do artigo (com as suas palavras, não deve ser copiado texto do artigo). -->

A co-ocorrência de condições médicas e/ou psiquiátricas tem um grande impacto nos custos atrelados à saúde, interferindo na eficácia de tratamentos e criando limitações aos sistemas de classificação diagnóstica, por exemplo. Dessa forma, a ideia do artigo é utilizar diversas condições de saúde como parte de uma rede de comorbidade, na tentativa de detectar e quantificar tendências gerais que não seriam possíveis de se observar utilizando outras abordagens.

Foram utilizados registros eletrônicos de saúde de quase 1 milhão de veteranos militares dos EUA - população escolhida devido ao grande número de registros disponíveis e à desproporcionalidade com a qual é impactada por condições psiquiátricas. Então, extraiu-se 95 condições médicas e psiquiátrias como os nós da rede. A partir de modelos de regressão logística, determinou-se as chances de uma condição afetar o diagnóstico de outra, de modo que as arestas das redes foram formadas por relações positivas e significativas (p < 0.05) entre os pares de condições. Assim, cada aresta representa o aumento do _log odds_ de uma condição dada outra condição, gerando uma rede direcionada.

As forças internas e externas - soma dos pesos das arestas direcionadas para um determinado nó ou saindo dele, respectivamente - foram utilizadas para indicar a capacidade de outras condições em predizer o diagnóstico de uma determinada condição ou a capacidade da condição em questão de predizer o diagnóstico de outras, respectivamente.

Uma detecção de comunidades foi aplicada utilizando o algoritmo de Louvain generalizado e testando diferentes valores para o parâmetro de resolução - que direciona o algoritmo para um número maior ou menor de comunidades. Ainda, para quantificar a comorbidade entre os módulos encontrados, utilizou-se a medida de comorbidade intermódulo normalizada (NIC, do inglês _normalized inter-module co-morbidity_) - definida como a soma dos pesos das arestas de um módulo A para um módulo B, dividida pelo produto do número de nós em cada módulo.

Análises foram realizadas considerando redes de condições psiquiátricas e médicas separadamente. Em ambos os casos, observou-se altos níveis de comorbidade e uma forte estrutura de comunidade hierárquica. Para as condições psiquiátricas, as comunidades se mostraram relativamente estáveis e representativas com 3 e 6 módulos. A partição em 3 módulos incluiu um módulo de condições do desenvolvimento e outros dois mais heterogêneos. Já a partição em 6 módulos, que apresentou subdivisões dos módulos da outra partição, incluiu módulos de uso de substância, depressão/ansiedade e psicose. Os resultados encontrados indicaram que uma relação preditiva da condição A para a B não necessariamente é recíproca (de B para A), de modo que as condições depressão menor, transtorno do estresse pós-traumático e demência se destacaram como preditoras (alta força externa), enquanto o transtorno de uso de alcóol e o transtorno do déficit de atenção com hiperatividade se destacaram pela sua previsibilidade a partir de outras condições (alta força interna).

Já na análise das condições médicas, a partição em 7 módulos se destacou, mapeando os módulos em fenótipos clinicamente relevantes. Essa partição foi formada pelos módulos gastrointestinal, musculoesquelético, sistema hematológico/circulatório, respiratório, doenças infecciosas, neurológico e endócrino/metabólico.

Ao considerar a rede completa com as condições psiquiátricas e médicas em conjunto, observou-se uma clara tendência das condições psiquiátricas como indicadoras de morbidades médicas futuras. Note que, nessa rede, as condições psiquiátricas foram consideradas como um módulo e as condições médicas foram divididas de acordo com a partição em 7 módulos. O módulo neurológico apresentou a evidência mais forte de comorbidade com as condições psiquiátricas, seguido pelos módulos de doenças infecciosas e muscoloesquelético. Arestas específicas que reprentavam uma comorbidade significativa entre condições psiquiátricas e médicas também foram investigadas. As condições psiquiátricas transtorno do estresse pós-traumático, transtorno de uso de substâncias, esquizofrenia, transtornos cognitivos e depressão menor foram significativamente preditivas do maior número de condições médicas. Ainda, as seguintes condições médicas apresentaram forte comorbidade com as condições psiquiátricas: paralisia, lesão intracraniana/lesão cerebral, dor de cabeça, problemas de ouvido, epilepsia/convulsões e doença cerebrovascular.

O estudo também investigou a associação entre distúrbios da dor e as comunidades encontradas, se o sexo dos pacientes está relacionado com um aumento de comorbidade e quantificou a mortalidade de cada condição.

A análise de mortalidade indicou uma relação entre o risco de mortalidade de uma condição e sua centralidade na rede de comorbidade, de modo que condições mais centrais tenderam a apresentar maior risco de morte para os pacientes, especialmente em relação à força interna da condição. Já a análise relativa ao sexo dos pacientes mostrou um aumento de relações de comorbidade específicas de cada sexo, principalmente para os homens. Em especial, o papel do transtorno do estressse pós-traumático como um nó preditor foi aumentado para os homens e esse mesmo papel para os transtornos depressivos foi aumentado para as mulheres.

Por fim, para avaliar uma potencial utilidade clínica da abordagem, utilizou-se uma regressão logística para a predição de esquizofrenia e comportamentos suicidas/autolesivos. A utilização da comorbidade entre todas as outras condições (diagnosticadas antes da condição de interesse) e a condição de interesse aumentou muito a precisão dos modelos em comparação com previsões baseadas em informações demográficas. Para ambas as condições consideradas, a acurácia, sensibilidade e especificidade ficaram em torno de 90%.

O artigo discute que, ao invés de excluídos, indivíduos com múltiplas comorbidades poderiam se tornar o foco de pesquisas futuras. Informações sobre condições médicas e psiquiátricas existentes poderiam ser utilizadas para aumentar a precisão de diagnósticos e levar ao desenvolvimento de tratamentos mais direcionados.

## Breve descrição do experimento/análise do artigo que foi replicado

<!-- Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída. -->

Dentre os diversos experimentos e análises realizadas no artigo, optamos por tentar replicar:

- __As detecções de comunidades nas redes psiquiátricas e médicas separadamente__. Nessa análise, as matrizes 95x95 de _log odds_ entre os pares de comorbidades e os respectivos p-valores obtidos pela regressão logística foram filtrados para considerar apenas as relações positivas e significativas (p < 0.05), além de subdivididas entre as condições psiquiátricas e médicas, gerando duas matrizes de adjacência utilizadas como entradas para gerar as redes. Desse modo, cada elemento da matriz de adjacências (A<sub>ij</sub>) continha o aumento do _log odds_ da condição i dada a condição j. Então, aplicou-se o algoritmo de Louvain generalizado, obtendo como saída as comunidades (ou módulos) para cada uma das redes (psiquiátrica ou médica);
- __A análise das condições psiquiátricas como preditoras de outras condições psiquiátricas futuras ou como previsíveis com base em condições psiquiátricas previamente diagnosticadas__. Nesse caso, a rede de condições psiquiátricas foi utilizada como entrada. Então, calculou-se os valores de força interna e externa de cada condição (ou cada nó). Como saída, temos as condições com maior força externa (condições preditoras) ou maior força interna (condições previsíveis);
- __A quantificação de comorbidade entre os módulos__. Como entradas, utilizou-se a matriz de adjacências completa (sem a separação entre condições psiquiátricas e médicas), mas considerando o módulo ao qual cada condição médica pertencia (obtido na primeira análise descrita) e atrelando as condições psiquiátricas a um único módulo diferente desses. Então, calculou-se a NIC entre cada par de módulos. Tais medidas foram utilizadas como saída para quantificar a comorbidade entre os módulos.
- __A investigação de arestas específicas que representem uma comorbidade significativa entre condições psiquiátricas e médicas__. Nesse caso, as arestas entre condições médicas e psiquiátricas (não necessariamente nessa ordem) foram utilizadas como entradas. Tais arestas foram filtradas, de modo a considerar apenas aquelas estatisticamente significativas e com _odds ratio_ acima de 1.5. Como saídas, temos a quantidade de arestas após a filtragem e sua proporção em relação ao número de arestas possíveis, separando entre condições médicas predizendo condições psiquiátricas e vice-versa, além das condições que apresentaram maior comorbidade considerando tal filtro. 

Vale notar que consideramos replicar também as análises com respeito ao sexo dos pacientes, que, em tese, teriam seus dados disponibilizados no GitHub atrelado ao artigo. No entanto, ao analisar as tabelas relacionadas ao efeito do sexo nas comorbidades, notamos que elas continham apenas valores iguais a zero, o que impossibilitou a replicação da análise.

### Dados usados como entrada

|      **Dataset**     |               **Endereço na Web**               |                **Resumo descritivo**                |
|:--------------------:|:-----------------------------------------------:|:---------------------------------------------------:|
| Comorbidity Networks | https://github.com/aaronab/comorbidity_networks |<!-- Breve resumo (duas ou três linhas) sobre o dataset. --> O dataset é composto por 4 arquivos .csv na forma de matrizes 95x95, contendo: o _log odds_ de comorbidade da condição i dada a condição j; os respectivos p-valores obtidos pela regressão logística; efeitos de interação do sexo dos pacientes; e seus respectivos p-valores (no entanto, esses dois últimos arquivos possuíam apenas valores iguais a zero). Ainda, foi disponibilizado um arquivo de código em R para uma rápida visualização da rede de comorbidades.|

## Método

<!-- Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos. Etapas do processo reproduzido. -->

### Transformação e processamento dos dados

Os arquivos "comorbidity_odds_matrix.csv" (_log odds_) e "comorbidity_pmat_matrix.csv" (p-valores) - disponíveis no GitHub atrelado ao artigo - foram utilizados como base para gerar as redes. No entanto, ambos os arquivos estavam na forma de matrizes 95x95, sendo necessário "achatá-los" para o formato aceito pelo Cytoscape (a ferramenta de processamento de redes utilizada neste projeto), além de descartar os valores da diagonal principal (que relaciona uma condição com ela mesma). Após esse processo, obtivemos duas matrizes: uma com colunas i (relativa às condições das linhas da matriz 95x95), j (relativa às condições das colunas da matriz 95x95) e _odds_ (_log odds_ de comorbidade da condição i dada a condição j); outra com colunas i, j e os respectivos p-valores.

Essas duas tabelas foram fundidas em uma única e a matriz obtida foi filtrada para considerar apenas as linhas com relações positivas (_Log odds_ > 0, o equivalente a _odds ratio_ > 1) e estatisticamente significativas (p-valor < 0.05), gerando uma matriz com as comorbidades de interesse. COMENTAR MELHOR SOBRE ESSA QUESTÃO DO ODDS RATIO?

Notamos que a nomeclatura utilizada para as condições era diferente da nomeclatura adotada no artigo para os nós das redes - que consistia em abreviações - além de ser necessário indicar quais condições foram consideradas como psiquiátricas e quais foram consideradas como médicas. No entanto, notamos também que o arquivo de código em R disponível no GitHub (sample_code.R) continha uma lista com as abreviações, bem como uma lista com os índices das condições psiquiátricas. Assim, utilizamos essas duas listas para gerar uma tabela de referência, atrelando o nome da condição no GitHub com sua respectiva abreviação e subgrupo (psiquiátrico ou médico).

Então, substituímos os nomes das condições na matriz de comorbidades de interesse pelas suas respectivas abreviações e criamos mais duas colunas, contendo o subgrupo relacionado à condição i e à condição j. Para obter a tabela relativa à rede de condições psiquiátricas (network_psych.csv), filtramos a matriz de comorbidade de interesse de modo a considerar apenas as linhas nas quais ambas as condições pertenciam ao subgrupo psiquiátrico. Da mesma forma, para obter a matriz relativa à rede de condições médicas (network_medic.csv), consideramos apenas as linhas nas quais ambas as condições pertenciam ao subgrupo médico. Por fim, a tabela relativa à rede completa (network_all.csv) foi gerada considerando todas as linhas da matriz de comorbidades de interesse.

Em todos esses casos, geramos os arquivos utilizando apenas as colunas das condições (i e j) e do _log odds_. Os arquivos podem ser encontrados no diretório /data/processed. Ainda, geramos o arquivo "ref_sub.csv" para ser utilizado em processamentos posteriores. Ele contém a lista de abreviações e seus respectivos subgrupos e pode ser encontrado no diretório /data/interim.

Todas as transformações e processamentos descritos nesta subseção foram realizados por meio do notebook "Network_data.ipynb", disponível no diretório \notebooks.

### Análises da rede de condições psiquiátricas



## Resultados

<!-- Apresente os resultados obtidos pela sua adaptação. Confronte os seus resultados com aqueles do artigo. Esta seção opcionalmente pode ser apresentada em conjunto com o método. -->


LEMBRAR DE DISCUTIR A QUESTÃO DO ODDS E LOG ODDS E QUE O ARTIGO NÃO DEIXOU SUPER CLARO O QUE FOI CONSIDERADO COMO RELAÇÕES POSITIVAS E TAL...
