# Medidas defensivas, próximas etapas e encerramento

Até agora, abordamos uma [introdução](https://greaterinternetfreedom.org/course/part01-intro-and-vms/) à modelagem de ameaças, o uso de máquinas virtuais como ambientes para analisar malware e como começar a analisar PDFs e documentos do Microsoft Office nas partes anteriores. Todos esses tópicos nos ajudarão a oferecer melhor suporte a outras pessoas como a primeira linha de defesa contra documentos suspeitos.

Ainda assim, se oferecermos suporte a agentes vulneráveis (o principal público-alvo desta série de postagens), sabemos que isso é apenas metade da história. Também precisamos garantir que possamos fornecer boas orientações proativas aos nossos beneficiários, para que eles estejam bem protegidos contra documentos mal-intencionados antes que eles cheguem aos seus discos rígidos. Esperamos que, com todo o conteúdo que analisamos, possamos não apenas entender melhor as recomendações clássicas que damos aos usuários finais, mas também propor algumas medidas adicionais que possam ser úteis para agentes vulneráveis com um nível de risco maior.

A primeira ideia que queremos reforçar é que dizer a ativistas, jornalistas e à mídia, que eles não devem abrir arquivos de fontes desconhecidas, não é um conselho sustentável. Para realizar suas atividades, muitas pessoas dessas categorias precisam abrir arquivos enviados a elas que podem conter ameaças, como convites para conferências de imprensa, documentos vazados, agendas de eventos etc. Portanto, o conselho mais pertinente que podemos dar a elas é conhecer os riscos e criar processos que lhes permitam abrir os arquivos da maneira mais segura possível.

Dito isso, algumas medidas de defesa contra documentos maliciosos incluem&#x20;

### Em geral&#x20;

#### Soluções antivírus

Muitos documentos mal-intencionados distribuídos fazem parte de operações maciças que são documentadas com sucesso e integradas aos bancos de dados de detecção de antivírus. Lembre-se de que essa recomendação não garante a detecção de arquivos mal-intencionados que são feitos sob medida para alvos específicos. Ainda assim, ela fornecerá uma camada de segurança que vale a pena ter, especialmente se estiver operando com muitos arquivos não confiáveis. Lembre-se de escolher um fornecedor de boa reputação, ativar a detecção em tempo real, se disponível, e manter o banco de dados atualizado.&#x20;

#### Mantenha o software legalizado e atualizado

Todos os anos, centenas de novas vulnerabilidades são descobertas e divulgadas para muitos programas de uso diário, incluindo o Microsoft Office e leitores de PDF, e essas vulnerabilidades são "corrigidas" por meio de atualizações de software. Portanto, ter todos os softwares atualizados reduzirá drasticamente as chances de alguém usar vulnerabilidades conhecidas e documentadas para atacar um alvo e ser bem-sucedido. Ter um software pirata afetará sua capacidade de detectar e aplicar atualizações de software, tornando-o um problema de segurança. É por isso que é recomendável ter um software original do ponto de vista da segurança, mesmo antes de abordar as considerações legais.&#x20;

#### Analise a extensão de arquivos suspeitos

Algumas [campanhas ](#user-content-fn-1)[^1]induzem os usuários a abrir arquivos nocivos disfarçados de documentos, mas que, na verdade, são outros tipos de arquivos, como aplicativos executáveis, arquivos .zip ou outros tipos de arquivos de contêiner, como .iso, entre outros. Geralmente, esses arquivos incluem até mesmo ícones personalizados para se parecerem com documentos do MS Word, PDFs, etc. Verificar cuidadosamente os arquivos que baixamos e abrimos nos dará outra camada de proteção ao detectar quando um arquivo não se enquadra em um tipo de arquivo comum ou esperado.&#x20;

#### Use leitores "emprestados"

Uma estratégia comum é abrir documentos suspeitos em um ambiente fora do seu computador que esteja mais bem equipado para detectar e conter qualquer ameaça em potencial. Um exemplo clássico é abrir arquivos usando o Google Drive (isso inclui visualizações no Gmail); em seguida, o arquivo é realmente aberto nos servidores do Google e renderizado em nossos computadores, fazendo com que o Google (ou qualquer outra plataforma com recursos semelhantes) seja o agente que precisa se preocupar com ameaças específicas contidas nos documentos abertos. Uma desvantagem dessa abordagem é que perdemos alguma visibilidade e capacidade de análise, já que não estamos baixando os arquivos, mas isso será útil para o uso diário.&#x20;

#### Use ferramentas específicas projetadas para esse caso de uso

Existem ferramentas que adotam o mesmo princípio de usar um ambiente seguro para abrir arquivos, mas simplificam o processo para o usuário e geram cópias seguras contendo apenas os elementos visíveis (semelhante à impressão do documento e à digitalização do resultado em um arquivo final). Uma dessas ferramentas é o Dangerzone, um programa que recebe um arquivo suspeito e gera uma cópia que é segura para abertura. A única desvantagem notável é que a ferramenta exige o download de dependências de alguns GB de tamanho, portanto, se o espaço em disco rígido e/ou a velocidade, e a estabilidade do download forem um problema, essa ferramenta pode ser mais difícil de configurar. Outra ferramenta para atingir esse objetivo é o CIRCLean USB sanitizer da Circ.lu, que usa um computador separado (eles propõem um Raspberry Pi) e duas unidades USB. Na primeira unidade, são salvas as versões suspeitas dos arquivos, e o software gera as cópias seguras e as salva na segunda unidade USB. Os desafios mais notáveis dessa abordagem são o uso de hardware dedicado para operar os arquivos e a adição de etapas físicas extras para mover os arquivos entre as unidades USB.&#x20;

#### Verificação de _hashes_ de arquivos em plataformas de detecção

Outra estratégia comum diante de arquivos suspeitos é verificar em plataformas como o VirusTotal se o arquivo é conhecido como malicioso. Isso nos ajudará a economizar tempo, caso o arquivo seja uma ameaça conhecida, e ainda nos fornecerá informações mais valiosas, como o tipo de malware que ele tenta executar e as mensagens de membros da comunidade associadas ao arquivo. Uma observação muito importante é saber que o upload do arquivo para ferramentas como o VirusTotal colocará o arquivo à disposição da comunidade, divulgando o conteúdo do documento (que pode conter informações confidenciais) e, possivelmente, alertando os criadores do documento se eles estiverem monitorando o arquivo específico. Uma solução alternativa para isso é não carregar o arquivo e, em vez disso, verificar seu _hash_. Na primeira parte desta série, há orientações sobre como verificar _hashes_.

\[imagem - Captura de tela do site VirusTotal destacando a barra de pesquisa com um texto de hash]

Exemplo de pesquisa do _hash_ de um arquivo, agosto de 2022

\[imagem - Captura de tela do site VirusTotal mostrando os resultados de uma verificação de arquivo em que ele foi detectado como malicioso por 29 de 63 mecanismos antivírus]

Exemplo de resultados no VirusTotal, agosto de 2022&#x20;

### Específico para PDFs: Desativar a execução de Javascript no leitor (e outras configurações de segurança)

Dependendo do seu software leitor de PDF, a execução do código JavaScript pode já estar desativada; no entanto, é aconselhável verificar novamente caso você espere abrir arquivos suspeitos. Além disso, dependendo do seu leitor, pode haver outros recursos de segurança que podem ser configurados.

\[imagem - Captura de tela do Adobe PDF Reader destacando as opções para ativar ou desativar o JavaScript]

Exemplo para o Acrobat Reader (agosto de 2022, Menu Editar -> Preferências -> Javascript)

\[imagem - Captura de tela do Foxit PDF reader destacando as opções para ativar ou desativar o JavaScript]

Exemplo para o Foxit Reader (agosto de 2022, Menu File->Preferences->Javascript)&#x20;

### [Específico para Office](#user-content-fn-2)[^2]: Minimizar o uso de macros em atividades legítimas

Mesmo que existam muitos casos de uso aceitáveis e inofensivos para macros, usar documentos com elas com frequência e estar acostumado a permiti-las pode abrir a porta para receber um documento malicioso e ativar suas macros acidentalmente. Por isso, especialmente as organizações devem estar atentas a essa situação e planejar adequadamente, seja para remover o uso de macros, seja para preparar processos para conviver com elas, considerando como lidar com arquivos não confiáveis.&#x20;

\[PAREI AQUI, TERMINAR E REVISAR LINKS DE TUDO]

### [Específico para Office](#user-content-fn-3)[^3]: Desative as macros e verifique a Central de Confiança

Recentemente, o Microsoft Office alterou várias vezes sua política em relação às macros, portanto, dependendo de quando você estiver lendo este material, o Office poderá ter as macros ativadas ou desativadas por padrão; além disso, poderá haver regras dependendo da origem dos arquivos, etc. Uma maneira de ter mais visibilidade e controle é verificar a Central de Confiança para ver e configurar diretamente o comportamento em relação às macros.

Captura de tela da janela de configuração do Microsoft Office destacando as opções para ativar ou desativar as macros

Trust Center para MS Office (agosto de 2022, Menu File->Options->Trust Center->Trust Center Settings->Macro Settings)

Outro recurso interessante do MS Office é o Protected View, que, considerando a origem de um arquivo, abre-o em um ambiente de área restrita com pouco privilégio ou acesso para interferir no computador, oferecendo outra camada de confiança. Um problema com esse modo é que, de forma semelhante ao bloqueio de macros, há um botão em uma faixa de opções acima do documento em que o usuário pode desativar o recurso Modo de Exibição Protegido, permitindo novamente ataques em que o criador do documento engana o usuário para desativar essa proteção.

Captura de tela da janela de configuração do Microsoft Office mostrando as opções para ativar ou desativar o Protected View

Configurações do Protected View (agosto de 2022, Menu File->Options->Trust Center->Trust Center Settings->Protected View)

Captura de tela do Microsoft Word mostrando o aviso de Protected View

Modo de exibição protegido em um documento do MS Word (agosto de 2022) O que vem a seguir

Em primeiro lugar, ao receber um arquivo suspeito, você terá a capacidade de executar uma primeira análise para verificar se há problemas óbvios de segurança; os possíveis cenários podem ser abstraídos para isso:

```
Se o arquivo não parecer conter nada prejudicial, podemos diminuir o nível de suspeita do arquivo.
    Se, por acaso, o alvo tiver um nível de alto risco, talvez seja necessário verificar novamente com outros colegas ou grupos mais especializados.
Se o arquivo parecer mal-intencionado, podemos tentar pesquisar seu hash em plataformas como o VirusTotal e, se for uma amostra de malware conhecida, encontraremos muitas informações mais detalhadas que serão úteis para entender a natureza da ameaça, a dimensão da campanha etc.
Se as ameaças contidas no arquivo forem fáceis de detectar e entender, mas não forem conhecidas pela comunidade, poderemos fornecer alguns insights sobre os detalhes ao pesquisar mais ou procurar ajuda.
Se os elementos contidos no arquivo parecerem avançados, difíceis de entender ou até mesmo difíceis de categorizar como ameaças, e o hash do arquivo for desconhecido pelas plataformas públicas, vale a pena entrar em contato com organizações mais especializadas que possam examinar melhor os arquivos em busca de ameaças não tão óbvias.
```

Além disso, aconselhamos em geral a:

Evite executar qualquer código suspeito (a menos que você compreenda os riscos e tome as respectivas precauções, que não são abordadas neste material) Pesquise mais sobre qualquer coisa específica que você encontrar e sobre a qual não tenha conhecimento. Há muitos comandos e maneiras diferentes de realizar coisas com o código, e é insustentável conhecer todos eles; portanto, é normal e esperado analisar a documentação em busca de instruções ou recursos específicos para entender melhor a funcionalidade de uma macro ou de outros objetos desconhecidos.

Lembre-se de que a análise de documentos maliciosos (e de malware em geral) é uma carreira completa que geralmente requer anos de experiência para lidar com casos mais avançados. Dito isso, reforçamos que este material é uma introdução a uma parte específica da análise de malware que, esperamos, incentive o leitor a aprender mais e a adquirir mais habilidades nesse campo. No entanto, dado o risco associado à operação de artefatos mal-intencionados sem os devidos processos e considerações de segurança, desencorajamos os leitores a adicionar mais processos e ferramentas de análise aos fluxos de trabalho apresentados sem o devido conhecimento desses processos e ferramentas, especialmente aqueles que envolvem a execução de malware (ou análise dinâmica).

Este é um material em constante revisão. Em caso de dúvidas ou comentários, entre em contato com cguerra@internews.org

[^1]: será que tem uma palavra melhor? campanha é meio enigmático

[^2]: "office" está em minúscula no original, mas acho que se refere ao programa MS OFFICE, checar

[^3]: "office" está em minúscula no original, mas acho que se refere ao programa MS OFFICE, checar
