# Analysis of malicious documents – Part 02 – PDF documents.

Após a visão geral do gerenciamento de máquinas virtuais e da instalação do Remnux como um ambiente para analisar artefatos suspeitos, exploraremos os arquivos PDF em termos de formato e maneiras como eles podem ser usados para prejudicar os usuários.
O que é um PDF?

Quando abrimos um arquivo PDF, usamos um software específico que renderiza seu conteúdo de forma legível; no entanto, como muitos outros tipos de arquivo, os PDFs são criados usando uma combinação de texto simples regular e dados binários (para imagens e outros elementos que possam exigir isso); por exemplo, o texto a seguir renderiza o arquivo PDF mostrado depois:

```txt
%PDF-1.4
1 0 obj
<<
	/Length 51
>>
stream
1 0 0 RG
5 w
36 144 m
180 144 l
180 36 l
36 36 l
s
endstream
endobj
2 0 obj
<<
	/Type /Catalog
	/Pages 3 0 R
>>
endobj
3 0 obj
<<
	/Type /Pages
	/Kids [4 0 R ]
	/Count 1
>>
endobj
4 0 obj
<<
	/Type /Page
	/Parent 3 0 R
	/MediaBox [0 0 612 792]
	/Contents 1 0 R
>>
endobj


xref
0 4
0000000000 65535 f
0000000010 00000 n
0000000113 00000 n
0000000165 00000 n
0000000227 00000 n
trailer
<<
	/Size 4
	/Root 2 0 R
>>
startxref
344
%%EOF
```
Captura de tela de um arquivo PDF em branco com um quadrado vermelho no canto inferior esquerdo da página

De "How to create a simple PDF file" (Como criar um arquivo PDF simples) da Callas Software
Estrutura do arquivo

A partir desse exemplo, podemos ver a estrutura padrão de qualquer arquivo PDF:

Captura de tela com o texto que compõe um arquivo PDF com divisões marcando as diferentes seções: Cabeçalho, Corpo, Tabela de referência cruzada e Trailer

Cabeçalho: Contém a versão do protocolo com o qual o arquivo foi criado, para instruir o programa leitor sobre como ler o restante da estrutura e renderizar todos os seus elementos.

Corpo: Aqui estarão todos os objetos que compõem o arquivo PDF, páginas, imagens, texto, fontes etc. Até mesmo códigos e ações automatizadas, se o arquivo os tiver.

Tabela de referência cruzada: Aqui encontraremos uma lista de todos os objetos do documento para fácil acesso e seus respectivos locais dentro do arquivo. Isso é semelhante a um "índice", mas para o software leitor de PDF ler. Se, a qualquer momento, o leitor precisar renderizar um objeto específico (por exemplo, se rolarmos para uma página aleatória em um documento grande), o software leitor verá qual página deve ser renderizada e procurará nessa tabela para localizar os respectivos elementos no corpo e carregá-los na tela.


Trailer: Aqui encontraremos o local no documento da tabela de referência cruzada e outras informações úteis, como o número de objetos na tabela de referência cruzada (para verificar se o arquivo não está corrompido, por exemplo), o objeto raiz do documento e informações de criptografia, se aplicável. Por padrão, os leitores de PDFs começam a ler os documentos a partir do final, onde podem encontrar rapidamente onde está o objeto raiz e a tabela de referência cruzada para começar a renderizar o conteúdo.
Entendendo os objetos do PDF

Considerando a forma como os PDFs são estruturados, a seção mais interessante para se aprofundar é o corpo do documento, pois lá se encontra tudo o que podemos ver e interagir (texto, páginas, imagens, formulários, código etc.). Todos esses elementos são chamados de objetos, e há uma grande variedade deles de acordo com as especificações, por exemplo, no exemplo acima, o objeto


```txt
4 0 obj
<<
	/Type /Page
	/Parent 3 0 R
	/MediaBox [0 0 612 792]
	/Contents 1 0 R
>>
endobj
```

é uma página com o id 4, tem um elemento pai com o id 3 e contém como filho o elemento com o id 1 (o retângulo vermelho). Mostrar a maneira exata como cada objeto é escrito está fora do escopo deste material; no entanto, há alguns tipos de elementos que podem ser usados para fins maliciosos, e queremos abordá-los um pouco mais.

/OpenAction: Esse objeto faz referência a um conjunto de ações a serem executadas quando o arquivo PDF é aberto, o que pode ser usado para iniciar um site para dar suporte ao conteúdo ou para fins de rastreamento, executar código JavaScript etc. Isso pode ser usado para induzir os usuários a executar ações extras que podem ser prejudiciais ou, dependendo do ambiente do computador, esse objeto pode executar malware diretamente.

/AA: esse objeto também inclui uma série de ações que são acionadas em circunstâncias variadas, como visualizar uma página, passar o ponteiro do mouse sobre objetos específicos, preencher campos de formulários etc. Os riscos associados são semelhantes aos do objeto /OpenAction, mas com muito mais cenários de acionamento.

/JS ou /Javascript:  Contém código JavaScript a ser executado depois que uma ação é acionada; esse código pode incluir funções exclusivas para PDFs.

/Launch: Tenta iniciar um aplicativo externo no dispositivo depois que uma ação é acionada, o que pode ser usado, por exemplo, para abrir outros documentos ou executar comandos específicos, que podem ser prejudiciais.


/EmbeddedFile: permite a inclusão de arquivos arbitrários, de documentos a executáveis, dentro do arquivo PDF. Há antecedentes de arquivos PDF benignos contendo outros arquivos nocivos incorporados, como executáveis de malware ou documentos do Microsoft Office com macros maliciosas.

/ObjStm: Contém informações arbitrárias que serão processadas de acordo com a forma como é chamado. O principal uso desse objeto é para agrupar muitos objetos e compactá-los, resultando em um arquivo menor. No entanto, ele também pode ser usado para compactar código mal-intencionado como uma forma de ofuscá-lo e evitar a detecção de antivírus. Considerando os muitos casos de uso diferentes para esse tipo de objeto, presumir sua presença como mal-intencionada nos levará a muitos falsos positivos.

Em ataques mais elaborados, uma combinação desses objetos pode ser usada; por exemplo, um arquivo PDF pode acionar uma ação que abre um arquivo incorporado no mesmo arquivo criptografado em um /ObjStm.

Considerando tudo isso, queremos saber se um arquivo tem algum desses tipos de objetos como primeira etapa para verificar se um arquivo PDF é mal-intencionado ou, pelo menos, para ter certeza de que não é.

Digite `pdfid`

Uma vez que sabemos por onde começar a procurar sinais de alerta em arquivos PDF que podemos considerar suspeitos, podemos começar a usar a ferramenta pdfid como a primeira etapa para ver quais tipos de objetos estão contidos em nosso arquivo. O pdfid faz parte de um conjunto de ferramentas desenvolvidas por Didier Stevens para simplificar alguns processos de análise em arquivos PDF. Essas ferramentas são executadas usando a linha de comando, por isso são conhecidas como aplicativos CLI (Command Line Interface). Explicaremos como usá-las usando a máquina virtual Remnux que configuramos na parte anterior deste curso.

Para usar o pdfid, precisamos abrir um aplicativo de Terminal em nossa máquina virtual. Quando iniciamos nossa máquina virtual, essa janela já deve estar aberta; no entanto, sempre podemos clicar no menu Activities (Atividades) no canto superior esquerdo e, em seguida, no ícone Terminal no painel esquerdo, conforme mostrado na imagem,

Captura de tela da barra de navegação no REMnux destacando o ícone do terminal

Captura de tela da máquina virtual REMnux em seu status inicial

Uma vez na janela do Terminal, podemos começar a explorar o uso do comando pdfid por meio de sua ajuda, basta digitar

`pdfid.py -h`

-h significa "ajuda".

Dica: sempre podemos usar a tecla Tab para autocompletar alguns comandos

Captura de tela de uma janela de terminal com o comando de ajuda da ferramenta pdfid.py

Aqui podemos ver várias opções que podemos empregar ao usar o pdfid, o que pode parecer desafiador para quem está começando a usar o terminal; no entanto, geralmente nos limitamos a algumas dessas opções e, com um pouco de prática, o processo se torna mais rápido e fácil.

Para analisar nosso primeiro arquivo, precisamos estar cientes de que o comando que executamos na linha de comando está sendo executado "de uma pasta/diretório", portanto, precisamos saber de onde estamos executando o comando e onde está localizado o arquivo que queremos analisar. Para contextualizar, toda vez que abrimos o aplicativo Terminal no Remnux, estamos abrindo um terminal no diretório Home, o mesmo local que vemos quando abrimos o aplicativo Files

Captura de tela da barra de navegação no REMnux destacando o ícone de arquivos

Captura de tela do aplicativo explorador de arquivos no REMnux

Para facilitar as coisas por enquanto, podemos colocar nossos PDFs nessa pasta, de modo que o comando do terminal seja executado a partir do mesmo diretório do nosso arquivo PDF.

Podemos pegar nosso arquivo de exemplo acima e salvá-lo como um arquivo PDF com a ajuda de um editor de texto em nosso computador host e arrastar e soltar o arquivo no diretório inicial do Remnux.

Captura de tela do aplicativo explorador de arquivos dentro do REMnux com o arquivo test.pdf destacado

Com isso, podemos executar o seguinte comando em nosso Terminal:

`pdfid.py test.pdf`

Para receber esta resposta:

Captura de tela de uma janela de terminal com a saída da ferramenta pdfid.py para a análise do arquivo test.pdf

Captura de tela de uma janela de terminal com a saída da ferramenta pdfid.py para a análise do arquivo test.pdf

Como podemos verificar, todos os objetos vistos pelo pdfid correspondem aos que conhecemos da fonte do arquivo PDF, e nenhum deles parece estar na lista de objetos suspeitos que descrevemos acima.
Como dissemos antes, há técnicas que os agentes mal-intencionados empregam para evitar a fácil detecção de tipos específicos de objetos. O pdfid tenta mostrar até mesmo objetos ofuscados; no entanto, em alguns casos menos comuns, pode haver objetos ocultos que exigem um mergulho um pouco mais profundo para serem descobertos.
Digite pdf-parser, exemplo 1

Agora, o que acontece quando encontramos um arquivo PDF com um objeto suspeito? Imagine que temos um arquivo ex005.pdf que nos dá uma saída como esta no pdfid:

Captura de tela de uma janela de terminal com a saída da ferramenta pdfid.py para a análise do arquivo ex005.pdf, destacando os objetos dos tipos JS, JavasScript e OpenAction

A partir daqui, e da orientação acima desse recurso, sabemos que há 3 objetos dos tipos /JS, /Javascript e /OpenAction que podem ser interessantes de analisar, especialmente porque sugerem que o arquivo está tentando executar alguma ação quando abrimos o arquivo. Aqui, podemos processá-lo com o pdf-parser para obter qual tipo de objeto é cada objeto e ver o conteúdo desses objetos. Para nosso arquivo de exemplo, executaremos o seguinte comando:

`pdf-parser.py -a ex005.pdf`

Usamos o argumento -a para ver as estatísticas do arquivo; para uma melhor referência, podemos sempre usar pdf-parser.py -help para ver uma lista de opções na tela.

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex005.pdf, destacando os objetos dos tipos JS, JavasScript e OpenAction

Aqui, podemos ver que de fato temos esses três objetos problemáticos, mas também algumas informações adicionais, podemos ver o id dos objetos para cada tipo de objeto. Agora sabemos que os objetos /JS e /Javascript estão no objeto com id 7, e o objeto /OpenAction está no objeto com id 1. Em seguida, podemos ver o conteúdo do objeto /OpenAction para ver o que o documento está tentando fazer ao abri-lo. Para isso, usamos o comando

`pdf-parser.py -o 1 ex005.pdf`

Aqui, o argumento -o é usado para fornecer à ferramenta a identificação do objeto cujo conteúdo queremos ver na tela:

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex005.pdf e seu objeto 1, destacando um tipo de objeto OpenAction

Aqui, podemos ver a linha "/OpenAction 7 0 R", o que significa que o conteúdo real do objeto /OpenAction está no objeto com id 7 e, quando abrirmos o arquivo, chamaremos ou faremos referência a esse objeto. Repetindo o processo para ver o conteúdo do objeto com id 7, obtemos:

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex005.pdf e seu objeto 7, destacando um tipo de objeto Javascript

Onde podemos ver que o documento está tentando mostrar um alerta ou pop-up com a mensagem descrita no terminal, se abrirmos o arquivo, ele terá a seguinte aparência:

Captura de tela de uma janela pop-up dizendo Hello from PDF JavaScript sobre um documento PDF dizendo JavaScript example

Exemplo 2

Como mencionamos anteriormente, pode haver arquivos em que o conteúdo suspeito não seja visível em texto simples. Pode haver vários motivos para fazer isso em casos legítimos, como compactar informações longas para reduzir o tamanho do arquivo, entre outros; no entanto, os arquivos mal-intencionados empregam essas técnicas com o objetivo de ofuscação para ajudar a evitar a detecção pelo software antivírus e outras soluções de segurança. Por exemplo, se repetirmos o fluxo de trabalho anterior para o arquivo ex006.pdf, veremos que a saída do comando pdfid é a seguinte:

Captura de tela de uma janela de terminal com a saída da ferramenta pdfid.py para a análise do arquivo ex006.pdf destacando um objeto JavasScript

Aqui podemos ver na linha /JavaScript "1(1)", isso significa que o pdfid detectou um objeto desse tipo, mas ofuscado, repetindo o mesmo fluxo de trabalho que já conhecemos, analisamos o objeto com id 8 (onde reside o código JavaScript) para ver o seguinte:


Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex006.pdf e seu objeto 8, destacando um tipo de objeto Javascript com um filtro FlateDecode
Aqui não podemos ver o código real como no último exemplo, em vez disso, vemos, entre outras coisas, a linha "/Filter /FlateDecode". A opção /Filter executa uma operação no conteúdo final de um fluxo para decodificá-lo e, em seguida, /FlateDecode indica a codificação associada que deve ser considerada ao decodificar o conteúdo. Para ter uma noção melhor disso, se abrirmos o arquivo com um editor de texto e procurarmos manualmente por esse elemento, veremos algo assim:

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex006.pdf e seu objeto 8, destacando um tipo de objeto Javascript com um filtro FlateDecode e o conteúdo do fluxo, que não pode ser entendido por causa de caracteres ininteligíveis
Onde o conteúdo dentro do quadrado vermelho é o conteúdo codificado real, nesse caso, o pdf-parser pode tentar decodificar o conteúdo real, para isso usamos o argumento -f, então acabamos usando o comando

`pdf-parser.py -o 8 -f ex006.pdf`

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo ex006.pdf e seu objeto 8, destacando um tipo de objeto Javascript com um filtro FlateDecode e o conteúdo do fluxo. e o conteúdo do fluxo, a função incluiu uma opção para decodificar o fluxo e revela uma função JavaScript

Onde podemos ver o conteúdo real do objeto a ser renderizado pelo leitor de PDF.

Agora que já sabemos o básico sobre como analisar arquivos PDF em busca de objetos suspeitos, há alguns desafios para você.
Exemplo 3

Outra coisa que o software criador de PDF faz normalmente para criar novos arquivos é criar objetos dentro de fluxos que são codificados para tornar os arquivos resultantes menores. Isso é desejável em geral, mas também cria uma maneira de ofuscar ainda mais o código malicioso. Analisando o arquivo example3.pdf, vemos alguns /ObjStm (Object Streams) que podem conter (e de fato contêm) outros objetos que podem ser interessantes.
Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo example3.pdf, mostrando vários tipos de objetos diferentes
Para esse tipo de cenário, é aconselhável usar a opção -O (como em o maiúsculo) do pdf-parser. Essa opção tentará analisar qualquer fluxo que contenha um objeto e tratá-los como objetos regulares do arquivo, por exemplo, usando essa opção assim.

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo example3.pdf, mostrando vários tipos de objetos diferentes

Revela que o arquivo tem "novos" objetos e que um deles é um /AA, o que é interessante para procurar comportamento malicioso, observando o respectivo objeto que temos

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo example3.pdf e seu objeto 10, mostrando um tipo de objeto AA

Isso nos diz que a ação está vinculada a um objeto de página; nesse caso, o /O está indicando que a ação é acionada quando abrimos a página, e a ação real é armazenada no objeto 37.

Captura de tela de uma janela de terminal com a saída da ferramenta pdf-parser.py para a análise do arquivo example3.pdf e seu objeto 37, mostrando um tipo de objeto desconhecido

Após algumas pesquisas, podemos concluir que esse objeto tenta abrir a caixa de diálogo de propriedades do leitor de PDF da seguinte forma (nada especialmente perigoso - exemplo em um computador configurado em espanhol)

Captura de tela de uma janela de propriedades de arquivo do leitor de PDF
Conclusão: tente usar o parâmetro -O caso haja outros objetos escondidos nos fluxos
Desafios

Pergunta: analisando o arquivo challenge1.pdf (md5: 3b20821cb817e40e088d9583e8699938), que tipo de objeto interessante está escondido atrás de um fluxo?

1. OpenAction




2. AA




3. JS




4. JavaScript



Pergunta: Analisando o arquivo challenge2.pdf (md5: 30373b268d516845751c10dc2b579c97), podemos ver uma ação que tenta abrir uma URL. Qual código a URL inclui como código de rastreamento? (dica: 6 números)

Quero ver a resposta


Agora que já sabemos mais sobre PDFs, vamos abordar na próxima parte outro tipo de arquivo fortemente armado: Documentos do Office.
