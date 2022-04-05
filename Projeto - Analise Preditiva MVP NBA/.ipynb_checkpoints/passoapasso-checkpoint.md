# PARTE 1 - SCRAPING

- Site que será retirado os dados https://www.basketball-reference.com/
- Criar uma variavel com o range de anos que será extraido os dados
- Criar uma url com o site, nessa url iremos substituir os anos pelas chaves, pois as mesmas irão variar
- Criar um for para substituir as chaves da url inicial, pelos anos da lista de anos criada
- Salvar na pasta mvp os dados, criando um arquivo, como o nome de cada ano, e habilitando o mesmo com a opção de sobrescrever caso o ano ja exista e escrever em cada um desses arquivos o conteudo de cada ano
- Extrair de cada tabela de cada arquivo html, os dados de cada ano

- utilizar o beatful soup para parsear os arquivos html
- utilizar o metodo find e decompose, para localizar e excluir elementos do html que nao será necessário
- colocar toda a sequencia criada para um ano, num for para todos os anos
- e colocar todos as tebalas numa lista de dataframe
- criar uma coluna com o ano para melhor identificacao das tabelas
- concatenar todos os dataframes em apenas 1
- filtrar as primeiras linhas e as ultimas linhas para analise rapida
- converter o dataframe em arquivo csv

## Scraping para pegar a estatistica de todos os jogadores

- url: https://www.basketball-reference.com/leagues/NBA_2021_per_game.html
- criar uma pasta para armazenar os arquivos das estiticas anuais dos jogadores
- observar que a pagina não carrega totalmente, pois o site utiliza javascripit para renderizar a pagina, conforme utilizamos o scroll
- por isso deveremos utilizar o selenium para habilitar uma utomatização desse scroll no site
- com o cromedriver importado executar o codigo para executar o script na pagina web e pegar os dados
- fazer um loop para executar com todos os anos
- novamente criar um for para pegar todos os arquivos, retirar a linha que nao precisa, criar os dataframes e concatenar os mesmos
- converter em csv


## realizar o scraping com o desempenho dos times por ano
- url: https://www.basketball-reference.com/leagues/NBA_2021.html
- criar uma pasta para armazenar os arquivos das estiticas anuais dos jogadores
- verificar se a pagina carrega corretamente em especial a tabela que sera utilizada
- fazer um loop para executar com todos os anos
- novamente criar um for para pegar todos os arquivos, retirar a linha que nao precisa, criar os dataframes e concatenar os mesmos
- converter em csv



# PARTE 2 - DATA CLEANING
- Iremos unir as tabelas em uma unica, para isso demos que fazer algumas analises:
 1. Na tabela de mvp temos os votos que cada jogador recebeu para mvp em determinado ano
 2. Só a informação dos votos para mvp não é sufuciente, precisamos da estatitisca de todos os jogadores da liga, para analisar o porque determinado jogador recebeu mais ou menos votos para mvp
 3. Sendo assim iremos na tabela de jogadores, incluir uma coluna com a quantidade de votos que cada jogador recebeu para mvp em determinado ano
 4. iremos tbm utilizar a tabela de times, pois um time com bom desempenho contribui para a escolha de um mvp, ou seja, é mais provavel que um mvp saia de um time com desempenho positivo do que de um time com desempenho negativo, o que faz sentido.
 
- No data frame de mvps, iremos nos livrar de algumas colunas, repetidas por exemplo, com dados que ja existem na tabela de players, deicaremos apenas as colunas referentes ao ano e a quantidade de votos para mvp.

- iremos combinar a tabela de mvps e players por meio das colunas de nome do jogador e ano, porem pode-se ser veriifcdo que alguns nomes de jogadores da tabela Player tem um asterisco, na frente do nome, iremos retirar isso, utilizando o str.replace

- Outro problema identificado é que há linhas com jogadores repetidas, pois eles passaram por diferentes times durante o ano, neste caso iremos pegar o desempenho total desses jogadores. Iremos fazer um grouby, para agrupar os jogadores com mesmo nome e ano. Iremos criar uma funcao para iterar sobre cada um desses grupos e garantir que cada um desses grupos tenha apenas 1 linha.

- a funcao será: se tiver apenas uma linha nesse grupo, retorne essa unica linha, ou seja n faca nada. Agora se tiver mais de uma linha era ira retornar a linha que contem a statisstica total "TOT", por sua vez como "TOT", não é um time válido iremos substituir pelo ultimo time desse jogador, na temporada.

- Essa funcao ira criar duas novas coluna de indices que iremos excluir, pois nao sera necessário

- iremos realizar o merge entre a coluna players com mvps, como queremos manter os jogadores, mesmo que nao tenham recebido votos para mvp iremos utilizar o outer merge.

- criando tres novas colunas na tabela players, que guardamos numa nova variavel chamada combined, os jogadores que nao estavam na tabela de mvp, estarao como NaN ao inves dos votos.

- iremos substituir as linhas com valores NaN por 0 .


- Combinar agora com o data frame de times.

- no data frame de teams iremos excluir as linhas que contem a palavra Division, se trata de uma sujeira na tratada no processo de scraping

- Iremos excluir os asteriscos da coluna de times, deixando o nome correto.

- No dataframe combinado os nomes de times estao abreviados, na tabela de times esta completo, iremos utilizar uma tabela auxiliar com os nicknames nome abreviado e completo para corrigir e unificar as informações

- verificar se o merge deu certo verificando a qntidade de linhas de ambos os data frames, limpar a data frame stats, excluindo a coluna unnamed

- verificar se os tipos de dados estao corretos e corrigir aqueles que deveriam ser numericos e estao como objetos
- converter o df recem criado, para csv
- plotar alguns graficos para analise