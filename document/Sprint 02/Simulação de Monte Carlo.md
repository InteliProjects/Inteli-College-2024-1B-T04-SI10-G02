## Sumário

[1. Introdução e Contexto](#c1)

[2. Setup (Bibliotecas e Importação de Dados)](#c2)

[3. Simulações de Monte Carlo](#c3)

<br>


# <a name="c1"></a>1. Introdução e Contexto

&emsp;&emsp; A simulação de Monte Carlo é uma técnica estatística que se baseia na geração de números aleatórios para modelar fenômenos complexos ou sistemas cujo comportamento não pode ser facilmente previsto por
métodos analíticos tradicionais. Ela foi originalmente desenvolvida pelo matemático Stanislaw Ulam, tendo aplicações no campo da física, engenharia, finanças e biologia. A essência dessa técnica é realizar uma grande quantidade de experimentos virtuais,onde parâmetros importantes são selecionados aleatoriamente dentro de intervalos conhecidos, permitindo assim a obtenção de estimativas probabilísticas de resultados. Isso proporciona insights valiosos sobre o comportamento de sistemas complexos e a probabilidade de ocorrência de eventos específicos. No atual projeto com a Wizard, o sistema complexo que se deseja analisar é o comportamento dos 
usuários na Landing Page da Wizard On. Os parâmetros utilizados são definidos a partir dos dados disponibilizados pela própria Wizard. Nesse documento será detalhado o desenvolvimento das simulações de Monte Carlo, incluindo a construção precisa dos modelos, a escolha dos parâmetros e a execução das iterações. Além disso, serão analisados os resultados gerados pela simulação e suas implicações para otimização da Landing Page. As simulações foram feitas em dois arquivos python (.ipynb), por conterem a mesma estrutura básica, eles serão detalhados de forma única nesse documento. 


# <a name="c2"></a>2. Setup (Bibliotecas e Importação de Dados)

- Bibliotecas
  
  ![image](https://github.com/joaomtm/Rascunho/assets/99208815/38dcb6c1-d6f5-475b-8c2f-66550a897159)

O Pandas é a principal biblioteca análise de dados em Python, oferecendo estruturas como o DataFrame, que simplificam a manipulação de dados tabulares.O Plotly Express é uma ferramenta de alto nível que facilita a criação rápida de gráficos interativos, tornando a visualização de dados complexos mais acessível. O NumPy, ele desempenha um papel fundamental na computação numérica, fornecendo suporte para arrays multidimensionais e operações matemáticas eficientes. O Matplotlib.pyplot é uma biblioteca para visualização estática de dados em Python, oferecendo uma variedade de gráficos. Essas bibliotecas servirão de apoio à Simulação de Monte Carlo, disponibilizando dados, ajudando na construção de modelos e na visualização de resultados.

- Importação de Dados
  
![image](https://github.com/joaomtm/Rascunho/assets/99208815/3665890c-d721-4b97-a9e5-3b73906619a2)

Os dados são importados pelo método pandas, lendo diretamente um csv armazenado no Google Drive.
O dado em questão é o hubspot de CRM disponibilizado pela Wizard, dando detalhes específicos sobre o comportamento do usuário na landing page, sua características e jornada até se tornar um lead. No total, a tabela contém 46 colunas e mais de 52 mil linhas.

Para a construção das simulações 4 colunas foram utilizadas: Became a Lead Date (registra a data em que um visitante se tornou um lead), Number of Sessions (indica quantas vezes um visitante acessou o site em um determinado período.), Number of Pageviews (contabiliza o total de visualizações de página durante as sessões dos visitantes) e Number of Form Submissions (registra quantas vezes os formulários do site foram preenchidos e enviados pelos visitantes).

Essas quatro colunas foram selecionadas entre as 42 disponíveis, levando em consideração o objetivo central do projeto: aumentar a taxa de conversão de leads. Assim, selecionamos as colunas que se referem diretamente às datas da conversão de lead (Became a Lead Date), o número de leads convertidos (Number of Form Submissions) e características de uso e comportamento desse lead (Number of Sessions e Number of Pageviews). Os dados dessas colunas são ideais para a definição  de variáreis na Simulação de Monte Carlo, sendo extremamente quantitativos os dados das três últimas colunas, o que permite desenvolver comparações minuciosas e análises temporais. 

As outras colunas não foram utilizadas para o desenvolvimento das simulações por serem voltadas  para uma área de marketing avançada, fugindo do escopo do projeto (ex: Google ad click id). Outras colunas também aumentavam demais a granularidade das informações e não contribuiam para a análise quantitativa dos dados (ex: IP City).

# <a name="c3"></a>3. Simulações de Monte Carlo
## Simulações Macro

As primeiras simulações realizadas são feitas em um escopo macro, não considerando um período de campanha específico, mas sim de todos os anos. O objetivo é analisar o comportamento geral dos usuários na Landing Page como um todo e identificar padrões ou tendências que possam influenciar a conversão de leads. Essas simulações ajudarão a entender melhor o contexto geral e a direcionar análises mais detalhadas. 

<a href="https://colab.research.google.com/drive/1qmxWdNjerV5V8BRmYN5VU90dRzYALPn0#scrollTo=NZTnLLy4Y_HT">Colab</a>


- Simulação 1: "Coluna "Number of Form Submissions"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/7bcff987-8549-4922-848e-7f675eec1158)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/81e650c5-6dc1-416c-9827-cb74c92d0221)

Essa primeira parte do código realiza pré-processamento e análise de um DataFrame df com dados de leads da Wizard. Primeiro, converte a coluna 'Became a Lead Date' para o formato datetime. Em seguida, divide os dados em duas partes com base na data '2024-04-18', data em que o botão do Whatssap foi incluído na Landing Page. Assim, calcula-se o maior e o menor número de formulários submetidos antes e depois dessa data. A partir desses dados, é possível observar que existem casos de envio de mais de um formulário, sendo que o primeiro período de tempo contém um grande outlier (401).

![image](https://github.com/joaomtm/Rascunho/assets/99208815/6864c368-decf-4d36-9ed0-566c91810969)

Esse código define uma função chamada remove_outliers que remove outliers de um conjunto de dados usando a técnica do intervalo interquartil. Primeiro, calcula os quartis Q1 e Q3 e, em seguida, calcula o IQR. Com base nesses valores, determina os limites inferior e superior para identificar outliers. Em seguida, filtra os dados para manter apenas aqueles que estão dentro desse intervalo. A função retorna os dados sem outliers.Depois, o código aplica essa função aos dados de formulários submetidos antes e depois de uma determinada data, armazenando os resultados em duas variáveis separadas. Em seguida, combina esses resultados em um único conjunto de dados, removendo outliers de todos os dados de formulários submetidos.


![image](https://github.com/joaomtm/Rascunho/assets/99208815/892792b1-f96a-4175-b625-5d9eebfa876c)


Esse código gera um histograma para visualizar a distribuição do número de formulários submetidos, excluindo outliers. Primeiro, ele calcula o histograma usando os dados sem outliers e especifica o número de bins desejado. Em seguida, calcula as porcentagens de cada bin em relação ao total de dados e cria um gráfico de barras com essas porcentagens em relação aos bins.

O gráfico resultante mostra a distribuição dos formulários submetidos, destacando a proporção de dados em cada intervalo. Isso permite uma visualização clara da concentração dos formulários submetidos em diferentes faixas de valores.

![image](https://github.com/joaomtm/Rascunho/assets/99208815/8779ce8a-dfb6-4e7e-b837-4556f1723a2a)

Este código realiza a simulação de Monte Carlo em si. Primeiro, define-se uma função simulacao(). Em seguida, cria-se um array das possibilidades e outra array das probabilidades, associadas a cada valor da array anterior. Depois, define-se o número de simulações desejadas (no caso, serão feitas 100 simulações)

Dentro de um loop, para cada simulação, uma amostra é retirada aleatoriamente do array com base nas probabilidades especificadas. A função simulacao() é então chamada com essa amostra como argumento, e o resultado é armazenado em uma lista de resultados de simulação.

Por fim, um histograma é criado com base nos resultados da simulação.

- Gráficos gerados: 

![image](https://github.com/joaomtm/Rascunho/assets/99208815/9907cee7-1cfc-478a-9c2a-5dcaef58f53c)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/030d1ea1-844d-4317-add3-6df120bd63b5)

A observação dos gráficos gerados revela que a distribuição aleatória produzida pela simulação segue o mesmo padrão observado no gráfico construído diretamente com os dados originais. Isso confirma a consistência do padrão e das proporções das distribuições. Analisando as colunas, é possível concluir que a maioria dos usuários enviam pelo menos dois formulários de inscrição. Pode-se supor então que os usuários não sentem uma mensagem de confirmação de inscrição ao preencherem o formulário, preenchendo-o mais de uma vez para talvez "garantirem que de fato se inscreveram". Esse fator pode ser um indício de que o campo do formulário não transmite uma sensação de confirmação ou compromisso, fazendo possivelmente potenciais leads saírem da página ao não identificarem ou sentirem que de fato começaram o processo de integração no curso. Outra hipótese é que de alguma forma um preenchimento de formulário está gerando um resultado duplicado, afetando análises como essa.

- Simulação 2: Coluna "Number of Pageviews"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/96cad9c7-b45e-49a0-80f7-7d30bd534e58)

Este trecho de código calcula o maior e o menor valor do número de visualizações de página. É possível perceber que no primeiro período (antes do botão do Whatssap ser aplicado na Landing Page), há outliers (2061).

![image](https://github.com/joaomtm/Rascunho/assets/99208815/cac9842d-d662-4ac0-bfe3-15d48cb1e92d)

O código remove outliers dos dados de visualizações de página (aplicado na simulação anterior)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/f6132cdb-211d-4459-9d05-b14a904ef428)

O código gera um histograma das visualizações de página, excluindo outliers. Ele calcula o histograma dos dados sem outliers e cria um gráfico de barras para mostrar a distribuição das visualizações de página em relação à porcentagem de ocorrência em cada intervalo.

![image](https://github.com/joaomtm/Rascunho/assets/99208815/f7e65a8f-8387-4906-a166-74c967a3d352)

O código realiza simulações de Monte Carlo com base em um conjunto de probabilidades associadas às probabilidades do número de pageviews, gerando um histograma. No total, 100 simulações são feitas.

- Gráficos gerados:

![image](https://github.com/joaomtm/Rascunho/assets/99208815/a8f1ca5f-23b9-426a-9961-08ca538feacf)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/fdf81446-9241-4d6e-9b37-1b883b1f6869)

Analisando e comparando os gráficos gerados, podemos observar que a distribuição aleatória produzida pela simulação segue o mesmo padrão observado no gráfico construído diretamente com os dados originais. Isso confirma a consistência do padrão e das proporções das distribuições. Fazendo uma análise, podemos concluir que o usuário valoriza na hora de se inscrever no curso (se tornar um lead) explorar a página em questão e consumir seu conteúdo, mas muito pouco, limitando seu pageview a dois. Assim, é possível concluir que para uma melhor conversão de leads, uma boa estratégia seria  desenvolver uma página não muito longa e com todas as informações sendo apresentadas de forma compilada, assim, o usuário ira ver todas as informações que considermaos essenciais para ele se tornar um lead.


- Simulação 3: Coluna "Number of Sessions"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/17868d7d-794c-4ef0-a98f-d47aa729b3a3)

Essa seção do código, assim como nas simulações anteriores, irá dividir a coluna referente a data em duas seções, calculando em cada uma delas o valor mínimo e valor máximo. Assim, é possível identificar a existência de outliers (no caso, o 289  em número de seções antes).

![image](https://github.com/joaomtm/Rascunho/assets/99208815/abe3ebdd-a8d7-4174-aae0-92c98669ab78)

Essa terceira célula da simulação ira construir uma função que calcula os outliers para que eles sejam retirados dos dados da simulação. O método é bem simples, fazendo o cálculo dos quartis pelo IQR para que os extremos sejam removidos.

![image](https://github.com/joaomtm/Rascunho/assets/99208815/fe9a4ca8-4633-4ae4-9020-ec72ff513fa8)

Esse código gera um histograma para visualizar a distribuição do número de sessões dos usuários, excluindo os outliers. 

O gráfico resultante mostra a distribuição dos número de seções, destacando a proporção de dados em cada intervalo.

![image](https://github.com/joaomtm/Rascunho/assets/99208815/26543dfd-3bf9-4cf2-8408-f896260d80fb)

O código realiza simulações de Monte Carlo com base em um conjunto de probabilidades (61%, 27% e 12%) . Ele gera um histograma dos resultados das simulações, mostrando a frequência de ocorrência de cada um dos três resultados. No total, 100 simulações são realizadas

- Gráficos gerados:

![image](https://github.com/joaomtm/Rascunho/assets/99208815/5b788b78-4050-4b6e-a0cf-35405603e0b6)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/022e8fb3-4c53-485e-82ab-7a48b76ad67b)

Analisando e comparando os gráficos gerados, podemos observar que a distribuição aleatória produzida pela simulação segue o mesmo padrão observado no gráfico construído diretamente com os dados originais. Isso confirma a consistência do padrão e das proporções das distribuições. Fazendo uma análise, podemos observar que um número relevante de usuários visitou o site mais de uma vez antes de se tornar um lead (quase 40%). Essa informação permite inferir que o usuário, antes de fazer sua inscrição, fica receoso ou pensativo, a ponto de sair do site para pesquisar outros cursos oferecidos pelo mercado ou até mesmo pensar mais um pouco sobre participar do curso. Nesse cenário, muitos leads podem ter sido perdidos ao não sentirem um senso de urgência ou escassez do serviço oferecido, um aspecto que poderá ser desenvolvido nas próximas telas B.


## Simulações por Período

As próximas simulações realizadas são feitas em um um período de tempo específico. A data que marca o período é o dia 24/07/2023 ("2023-07-24"), referente a uma das campanhas de marketing da Wizard On que oferecia 50% de desconto na matrícula. O objetivo é analisar o impacto que o formato da landing page teve na conversão de leads durante o período, compreendendo o comportamento dos usuários. Ao fazer essa análise, será possível desenvolver insights e propostas para uma nova Landing Page mais otimizada (teste B).

- Simulação 4: "Número de Seções Após Mudanças"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/5c44eb8a-18fd-4acf-bbe7-d8c0cb8ffd4b)
![image](https://github.com/joaomtm/Rascunho/assets/99208815/da6133f0-68f5-4f7d-90f7-e8205652e49a)


Este trecho de código começa filtrando os dados em dois grupos, antes e depois da data específica que marca mudanças na Landing Page. São calculadas médias e desvios padrão do número de sessões em cada período. Em seguida, uma função é definida para simular o impacto das mudanças nas sessões dos usuários. Essa função gera contribuições aleatórias para UI e velocidade, calcula um ajuste no número de sessões e simula novos números de sessões. Os resultados são ajustados para serem positivos e inteiros, e um histograma é criado para visualizar a distribuição dos números de sessões simuladas após as mudanças. Ele compara as médias antes e depois das mudanças, destacando qualquer impacto significativo. Ao total dez mil simulações são feitas.

- Gráfico gerado:
  
![image](https://github.com/joaomtm/Rascunho/assets/99208815/b7796ef9-4448-41bd-b503-523616b918a3)

Analisando o gráfico, podemos observar que uma curva normal foi gerada (no caso, apenas nos eixos positivos). Nesse histograma, duas linhas são determinadas: uma verde representando a média do número de seções depois da mudança (quando a campanha de marketing indicada entrou em vigor) e outra em vermelho indicando a média antes da mudança (quando a campanha de marketing ainda não tinha entrado em vigor). Comparando a duas médias distribuídas no histograma, podemos concluir que após a campanha, o número de sessões por usuário diminuiu. Essa informação pode trazer o insight de que os usuários começaram a visitar menos vezes a Landing Page até se tornarem um lead, ou seja, estão se convencendo em menos visitas a se inscrever no curso da Wizard On. Nossa suposição é que a oferta dessa campanha (de 50% de desconto) foi muito mais atrativa para o cliente, fazendo essa conversão em menos visitas ao site (visitas essas que aumentam quando o usuário está em dúvida e indecisão).


- Simulação 5: "Número de Pageviews Após Mudanças"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/c11a2943-5d36-49c7-b194-1fcfd95ccc2d)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/fc249d9f-57ad-48ad-ae05-914aa4eaa820)

Esse código começa filtrando os dados em dois grupos, antes e depois de uma data específica que marca mudanças na Landing Page. São calculadas médias e desvios padrão do número de visualizações de página em cada período. Em seguida, uma função é definida para simular o impacto das mudanças nas visualizações de página dos usuários. Essa função gera contribuições aleatórias para UI e velocidade, calcula um ajuste no número de visualizações de página e simula novos números. Os resultados são ajustados para serem positivos e inteiros, e um histograma é criado para visualizar a distribuição das visualizações de página simuladas após as mudanças. Ele compara as médias antes e depois das mudanças, destacando qualquer impacto significativo. Ao total, dez mil simulações foram feitas.

- Gráfico gerado:

![image](https://github.com/joaomtm/Rascunho/assets/99208815/a4f56bfd-dcae-485e-b63c-409ff86fd6a1)

Analisando o gráfico, podemos observar que uma curva normal seria gerada caso a distribuição no 0 não fosse tão concentrada. Nesse histograma, duas linhas são determinadas: uma verde representando a média do número de pageviews depois da mudança (quando a campanha de marketing indicada entrou em vigor) e outra em vermelho indicando a média antes da mudança (quando a campanha de marketing ainda não tinha entrado em vigor). Comparando a duas médias distribuídas no histograma, podemos concluir que após a campanha, o número de pageviews por usuário diminuiu. Essa informação pode trazer o insight de que os usuários começaram a precisar ver menos páginas  na Landing Page até se tornarem um lead. Nossa suposição é que o UX writing da nova página era mais efetivo e com um  maior call to action, dando a impressão que você conseguiria fazer um relevante curso de inglês pagando (por tempo limitado) apenas por 50% do valor de matrícula. Supomos que esse texto promocional tirou a incerteza de valor que o cliente tinha ( o que o levava antes a buscar informações que agregassem valor ao curso), dando a impressão de oportunidade.


- Simulação 6: "Número de Seções Após Mudanças (Sem Outliers)"
![image](https://github.com/joaomtm/Rascunho/assets/99208815/bd1027e8-f79a-4f3d-81d4-753f23a35206)

Essa sexta simulação segue a mesma estrutura da simulação 4. A única diferença de código é que ela retira os outliers calculados a partir do método IQR, identificador de quartis. 

- Gráfico gerado:
  
![image](https://github.com/joaomtm/Rascunho/assets/99208815/91e25a18-4bf1-4cbb-a269-9988c954ee81)

Proporcionalmente, a diferença da média antes e depois é semelhante daquela gerada na simulação 4. Assim, as conclusões e insights permanecem os mesmos, apenas reforçando as médias geradas pelo modelo.


- Simulação 7: "Número de Seções Após Mudanças (Sem Outliers e Histogramas Separados)"

![image](https://github.com/joaomtm/Rascunho/assets/99208815/e54cb2f0-0968-4a03-98ec-27594c7ef0f5)
![image](https://github.com/joaomtm/Rascunho/assets/99208815/0a6aa9e2-451f-4f53-bb2a-bdf3ba23b6fc)

O código dessa simulação se assemelha ao código da simulação 4 ( em sua construção geral) e 6 (aplicando o método IQR para retirar os outliers). A principal diferença é que dois histogramas estão sendo gerados, um para cada período (antes e depois da campanha de marketing selecionada).

- Gráficos gerados:

![image](https://github.com/joaomtm/Rascunho/assets/99208815/77e226a8-a3e3-4871-85e6-5266c6f6e217)

![image](https://github.com/joaomtm/Rascunho/assets/99208815/5d0ae622-4dbd-4278-9dac-618af740baed)

Visualizando os histogramas de forma separada e não mais por médias (simulação 4 e 6), podemos observar que o número de sessões maiores que 3 na Landing Page diminuíram drasticamente após o surgimento da campanha analisada, a ponto das seções maiores que 3 se tornarem outliers e não serem mais consideradas. Essa informação reforça o insight desenvolvido na simulação 4, de que os usuários estavam tendo menos dúvidas ou receios na hora de se inscrever no curso (reflexo da menor necessidade de acessar o site inúmeras vezes até se tornar um lead). Esse fator é atribuído à força da campanha de 50% de desconto, que proporcionou uma maior identificação do valor do produto.
















 

