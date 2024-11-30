# Documentos do Microsoft Office

Depois de verificar como configurar máquinas virtuais como ambientes seguros e apresentar um fluxo de trabalho introdutório para analisar documentos PDF suspeitos, estamos prontos para continuar com os formatos de arquivo do Microsoft Office.&#x20;

### Problemas de segurança com documentos do Microsoft Office

Em geral, os documentos do Office não são perigosos por si só, se contiverem apenas as informações para as quais foram projetados: páginas com texto e outros elementos imprimíveis para o MS Word, células com valores e fórmulas para o MS Excel, slides com elementos observáveis no MS PowerPoint etc.

No entanto, entre os vários recursos disponíveis no ecossistema do MS Office que acrescentam funcionalidades aos documentos, um é especificamente interessante do ponto de vista da segurança digital: a possibilidade de incorporar objetos nos documentos. Os tipos de objetos que podemos incorporar são muitos, como notação matemática, multimídia, outros documentos etc. E, entre todos eles, há um que é especialmente poderoso porque permite a execução de código personalizado, que pode ser usado como arma para prejudicar o usuário: a macro.&#x20;

### Macros

O uso inicial das macros, nos documentos do MS, permite executar tarefas repetitivas com facilidade, "gravando-as" uma vez e "reproduzindo-as" repetidamente depois. Pode-se criar macros sem ter nenhum conhecimento de programação, apenas gravando cliques em botões e atalhos de teclado e, em seguida, o MS Office traduzirá a gravação em uma série de comandos que serão executados como um "pequeno programa" que vive dentro dos nossos documentos.

Quando começamos a nos aprofundar no funcionamento das macros, começamos a ver como elas podem ser transformadas em armas e por que são tão populares em ataques de _phishing_ para infectar computadores, em comparação com outras técnicas. Primeiro, as macros são armazenadas como código escrito na linguagem de programação Visual Basic for Applications (VBA), que é bem documentada, simples de escrever e poderosa. O Visual Basic, em geral, também é usado para escrever programas autônomos inteiros, portanto, tem a capacidade de fazer coisas fora do escopo do documento, como baixar e executar arquivos e alterar as configurações do sistema, por exemplo. Os comandos associados a essas tarefas também estão disponíveis, em sua maioria, nas macros do MS Office, portanto, podemos escrever macros que aproveitem os comandos avançados disponíveis e usá-los para executar tarefas prejudiciais, como baixar e executar malware mais avançado, excluir arquivos etc.

Em segundo lugar, a execução de macros a partir de documentos é muito fácil. Os criadores de documentos podem configurá-las para serem executadas automaticamente ao abrir o arquivo, ao clicar em um botão, link ou qualquer elemento específico, entre outros gatilhos. Então, para arquivos desconhecidos, o MS Office nos avisará que suas macros podem ser perigosas e as bloqueará, mas geralmente estamos a um ou dois cliques de desativar essa proteção e executar as macros de qualquer maneira. Essa situação torna muito atraente para os agentes mal-intencionados usar macros e nos convencer de que é seguro executá-las por meio de argumentos convincentes próprios de cada campanha de _phishing._

Ao comparar PDFs com documentos do MS Office com macros para atividades maliciosas, observa-se que os documentos do MS Office oferecem mais possibilidades de comandos a serem executados em dispositivos que os abrem, o que os torna mais poderosos e também mais populares do que os PDFs. [Além disso, essa flexibilidade é o motivo pelo qual estamos nos concentrando em transformar macros permitidas em documentos maliciosos em armas, em vez de focar em outras formas de transformar documentos do MS Office em armas. ](#user-content-fn-1)[^1]Se você estiver interessado em maneiras diferentes de usar esses arquivos para vulnerar usuários de versões desatualizadas do Office, forneceremos links para outras referências ao final.&#x20;

#### Vulnerabilidades do Office&#x20;

Há muitas maneiras documentadas para explorar macros ou documentos do Office em geral; no entanto, muitas das mais criativas não são possíveis de serem exploradas em instâncias totalmente atualizadas do MS Office.&#x20;

Como qualquer outro programa, o MS Office pode ter vulnerabilidades conhecidas ou desconhecidas que podem permitir o comprometimento de um dispositivo, mesmo sem o uso de macros.&#x20;

### Os "antigos" e os "novos" formatos de documentos do MS Office

Desde 2003, o Microsoft Office mudou a forma como os documentos são criados por padrão, incluindo novas extensões de arquivo, de modo que os novos documentos do MS Word são armazenados com a extensão ".docx" em vez de ".doc" e assim por diante. Mesmo com estruturas internas diferentes, as macros são armazenadas de maneira semelhante, portanto, a orientação fornecida neste material se aplica tanto aos formatos de arquivo antigos, quanto aos novos. Se estiver interessada em saber mais sobre as convenções para armazenar macros e muitos outros tipos de objetos, você pode pesquisar mais sobre Object Linking and Embedding (ou OLE), que ainda é usado e adaptado a novos formatos de arquivo, incluindo documentos do MS Office.

### &#x20;Análise de documentos do MS Office

Para começar a explorar maneiras de detectar quando as macros estão incluídas em documentos do MS Office, usaremos oledump.py, uma ferramenta Python desenvolvida por Didier Stevens, o mesmo autor das ferramentas propostas anteriormente, com foco em PDFs. Também usaremos uma série de arquivos para exemplificar como os documentos do MS Office funcionam e como as macros podem ser detectadas e analisadas, também a partir de materiais de treinamento de Didier Stevens.

O fluxo de trabalho para iniciar a análise de documentos do MS Office é muito semelhante ao que usamos em PDFs. Primeiro, listamos os diferentes elementos presentes no arquivo, identificamos objetos interessantes em termos de segurança e, em seguida, tentamos obter o conteúdo real desses elementos para ver se há algo prejudicial.

#### `oledump.py`

O principal uso dessa ferramenta é listar todos os objetos OLE incluídos em um arquivo específico e mostrar o conteúdo de qualquer um deles. O uso básico da ferramenta é o seguinte:

`oledump.py ex001.doc`

Onde ex001.doc é o nome do documento que queremos analisar

\[imagem - Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex001.doc]

Aqui, podemos ver todos os elementos de um arquivo sem macros ou outros objetos incomuns incorporados em um tipo de arquivo "antigo" do MS Office. Fazendo o mesmo experimento com um arquivo com o "novo" formato (após o MS Office 2003), obteremos algo parecido com isto.

\[imagem - Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex005.docx]

Aqui, podemos ver que mais elementos no exemplo .doc são armazenados como objetos OLE, enquanto no exemplo .docx temos um documento funcional sem usar objetos OLE. Isso ocorre porque os documentos .docx são empacotados como um arquivo .zip com a maioria dos arquivos .xml dentro (você pode até tentar alterar um arquivo .docx|.xlsx|.pptx seguro para a extensão .zip e abri-lo) e usar dados formatados em OLE somente quando necessário.

Quando abrirmos um arquivo com macros, o resultado incluirá novos elementos:

\[imagem - Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex008.doc.zip]

Se olharmos atentamente para esse exemplo, veremos que [o arquivo que passamos para o comando](#user-content-fn-2)[^2] é um arquivo .zip que contém um documento do MS Word. Além disso, o arquivo .zip está protegido por senha com a senha "infected" (infectado). Essa é uma prática comum na comunidade de análise de malware, e o oledump.py a considera como uma entrada válida e gerencia toda a descompressão, passando o documento para análise automaticamente.

\[PAREI AQUI]

Nessa saída, vemos dois objetos que são diferentes e são identificados com a letra "m" ou "M". Isso significa que esses objetos específicos contêm macros. Vamos usar o comando -s no oledump.py para ver o conteúdo desses fluxos, começando com o objeto (ou fluxo 8)

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex008.doc.zip para o objeto com id 8

Usamos o comando -s para selecionar o objeto identificado com o número 8 e o comando -v para descompactar o conteúdo porque o VBA pode compactar o código por padrão, portanto, é uma prática segura incluir esse comando ao solicitar objetos com macros.

gora, observando o conteúdo, vemos algumas declarações de atributo, que são feitas por padrão pelo VBA e nem mesmo são visíveis para o criador do documento, portanto, esse código não é considerado personalizado ou prejudicial aos nossos efeitos. Dito isso, o código conhecido como inofensivo pela ferramenta é identificado com um "m" minúsculo, assumindo-o como seguro e menos interessante para análise posterior. Vamos analisar o outro objeto que contém uma macro.

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex008.doc.zip do objeto com id 7

Para dar algum contexto, essa macro usa o comando MsgBox que abre uma janela de diálogo com a mensagem "Hello world" nesse caso. Além disso, AutoOpen() informa ao programa que essa macro deve ser executada ao abrir o arquivo automaticamente. Na prática, um documento desconhecido que tente executar uma macro AutoOpen() acionará um alerta de segurança; no entanto, dependendo do contexto, o usuário pode ser enganado para ignorar o aviso e executar a macro mesmo assim.

Para explorar a maneira como isso pode ser transformado em uma arma, vamos verificar o seguinte arquivo

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex013.doc.zip

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex013.doc.zip para o objeto com id 7, incluindo uma macro

Aqui, a macro de interesse é um pouco mais complexa do que uma janela de diálogo com uma mensagem de texto. Podemos ver que, novamente, o recurso AutoOpen() é usado para executar o código abaixo ao abrir o documento. Observando o código, com uma pequena ajuda para verificar os comandos usados, podemos inferir que a macro tenta fazer o download do conteúdo de um URL em um arquivo no diretório temporal de nossa máquina e executar o arquivo que foi baixado.

Nesse caso, parece que o arquivo está preenchendo um arquivo de texto, que deve ser inofensivo, mas com o URL certo e o tipo de arquivo certo usado para baixar o conteúdo, uma macro como essa pode escrever e executar programas ou outros artefatos prejudiciais sem muita interação ou mesmo conhecimento do usuário. Além disso, vale a pena mencionar que essa macro é uma versão simplificada de ameaças mais reais, que geralmente ofuscam seu código para evitar a detecção pelo software antivírus e podem executar ações mais elaboradas, como adicionar o malware baixado a programas de inicialização ou tarefas agendadas, para que o malware seja persistente ao longo do tempo, entre outras.

Muitas vezes, a análise de macros mais complexas exigirá habilidades não abordadas neste material, como descriptografar o código e descobrir como, por meio do uso de diferentes comandos e estruturas de dados, podemos obter o código final executado para que possamos entender o que ele faz (desofuscação). Um exemplo ainda muito simples que mostra uma técnica comum para ofuscar o conteúdo pode ser encontrado na verificação do próximo arquivo.

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para o arquivo ex015.doc.zip

Captura de tela da janela do terminal do REMnux com a saída da ferramenta oledump.py para um objeto mostrando uma variável chamada strPayload com uma cadeia de caracteres codificada em Base64

Aqui, em vez de usar texto simples, o criador da macro usou o esquema de codificação base64, o que dificulta a leitura da carga útil que está tentando executar. Para este exemplo, há muitas ferramentas que podem nos ajudar a decodificar essa variável, uma delas é o CyberChef, um aplicativo da Web em que podemos inserir alguns dados e executar algumas operações para obter um resultado, neste caso, temos:

Captura de tela da ferramenta CyberChef mostrando a string de texto decodificada dizendo Hello world

Com esses exemplos e referências, devemos ser capazes de saber se um arquivo tem macros incorporadas usando oledump.py, se um arquivo é interessante para ser analisado mais detalhadamente e, caso seu código seja simples o suficiente, o que a macro está tentando fazer. No caso de encontrar documentos com macros muito complexas, o conselho é procurar ajuda para analisar o arquivo mais detalhadamente e nunca tentar executar o arquivo em nossos ambientes, pois isso pode ter efeitos terríveis em nossos dispositivos, caso eles sejam infectados.

Agora que aprendemos alguns fluxos de trabalho introdutórios para analisar PDFs e arquivos do MS Office, estamos prontos para analisar algumas estratégias defensivas para nos protegermos de documentos maliciosos na próxima e última parte.

Desafios

Pergunta: Aplicando o mesmo fluxo de trabalho ao arquivo ex006.doc.zip, vemos esta saída. Qual das seguintes hipóteses se confirma com as informações que obtivemos até agora?

Captura de tela de um terminal mostrando a saída da ferramenta oledump.py para o arquivo ex006.doc.zip, mostrando dois objetos com macros descritas pela letra m minúscula

1. O arquivo não tem nenhuma macro
2. O arquivo tem macros personalizadas criadas como qualquer um dos outros exemplos abordados
3. De alguma forma, o arquivo tem macros inofensivas ao sistema, mas não tem macros criadas de forma personalizada.

Pergunta: aplicando o mesmo fluxo de trabalho ao arquivo ex016.doc.zip, vemos uma macro semelhante à última abordada acima, em que a macro decodifica algo em base64. O que está sendo passado para a função Decode64? (dica: aaaaaaaaaaaaaaaaaa.aaaaaaa.aaaa)

Janela da interface de linha de comando mostrando a saída de uma macro de documento com uma cadeia de texto ofuscada

Quero ver a resposta

O que vem a seguir?

Depois de ter uma ideia melhor de como o PDF e o MS Office podem ser avaliados quanto a códigos mal-intencionados, podemos entender melhor como propor medidas defensivas e também podemos revisar as dicas finais sobre como conduzir esse tipo de avaliação inicial. Bônus: leitura adicional sobre segurança do MS Office

Oletools: outra ferramenta famosa para analisar arquivos do MS Office Lista de vulnerabilidades conhecidas do Microsoft Office (a maioria não envolve macros) CVE-2022-30190 ou codinome "Follina": uma vulnerabilidade recente que está na moda, abusando de documentos para interagir com os recursos de solução de problemas do Windows. "Uncompromised: Unpacking a malicious Excel macro", um caso interessante que explora um arquivo malicioso passo a passo. Analyzing Malicious Documents Cheat Sheet, uma orientação rápida de Lenny Zeltser para analisar documentos suspeitos.

[^1]: essa frase não faz sentido pra mim



[^2]: nào entendi essa parte da frase
