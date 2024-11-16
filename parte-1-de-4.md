# Análise de documentos maliciosos - Parte 01 - Introdução e VMs

Introdução

Este (mini)curso destina-se aos entusiastas e profissionais de segurança digital (suporte técnico, facilitadores, socorristas, etc.) que desejam saber mais sobre documentos maliciosos e como identificá-los. Esses documentos podem ser anexos de e-mail, arquivos em pen drives ou downloads de sites específicos. Os principais objetivos são:

Aprender os conceitos básicos de como funcionam os formatos comuns de documentos e como eles podem ser transformados em armas, com ênfase especial em arquivos Portable Document Format (PDF) e documentos do Microsoft Office (pelo menos MS Word, Excel e PowerPoint).

Apresentar algumas ferramentas que podem ajudar a identificar sinais de documentos perigosos ou confirmar se é seguro abri-los.

Fornecer alguns conselhos de segurança e esclarecer dúvidas comuns sobre como lidar com arquivos suspeitos.

Este curso utiliza o formato de leituras curtas e questionários na maior parte do conteúdo abordado, sendo que, dependendo do material, será necessário executar algumas ferramentas. Isso será abordado na seção Ambiente de trabalho logo após a introdução, os requisitos gerais são:

Para concluir os exercícios propostos:

```
Capacidade de executar 1) uma máquina virtual no computador usando o Virtualbox ou software semelhante, ou 2) scripts python (somente para analisar os arquivos do curso, não para analisar amostras reais).
```

O tempo para cobrir o material (aproximadamente 2 horas)

Este curso utiliza materiais disponíveis em outras referências e usa apenas ferramentas disponíveis gratuitamente. A maior parte do conteúdo é inspirada no trabalho que Didier Stevens tem feito ao longo do tempo, especialmente para o SANS, bem como em outras referências:

https://blog.didierstevens.com/2011/05/25/malicious-pdf-analysis-workshop-screencasts/

https://github.com/filipi86/MalwareAnalysis-in-PDF

https://www.sentinelone.com/blog/malicious-pdfs-revealing-techniques-behind-attacks/

https://www.youtube.com/watch?v=opdVFQEBCNU

Estrutura

Isenções de responsabilidade

Algumas considerações sobre modelagem de ameaças

Para cada tipo de formato de arquivo (PDFs, MS Office)

```
Como eles são estruturados (de uma forma mais técnica)


Como eles podem ser transformados em armas


Como podemos fazer uma análise introdutória


Algumas conclusões/fatos sobre o formato de arquivo
```

Alguns conselhos gerais contra as ameaças relacionadas

O que vem a seguir

A seguir, uma série de isenções de responsabilidade úteis antes de começar a usar o material Isenções de responsabilidade

Dada a natureza da tarefa que realizaremos após dominar o conteúdo fornecido (análise de arquivos maliciosos e perigosos) e a complexidade do tópico (que consideramos uma introdução à análise de malware), recomendamos enfaticamente a leitura desta seção e a concordância com todos os itens antes de prosseguir.

Este curso é introdutório: ele foi criado para pessoas sem experiência anterior em análise de documentos suspeitos. Na seção Próximas etapas, incluímos uma lista de recursos para leitura e referência adicionais.

Este curso não aborda muitas técnicas avançadas: há muitas ameaças específicas cuja complexidade vai além do escopo deste material. Além disso, como em tudo relacionado à segurança da informação, pode haver ameaças esperando para serem descobertas que não serão abordadas neste curso. Recomendamos procurar ajuda caso suspeitemos que estamos vendo uma ameaça avançada ou desconhecida em um arquivo ou em qualquer outro artefato. Dito isso, este curso nos ajudará a entender melhor como um arquivo benigno geralmente se parece, em vez de como todo documento mal-intencionado é estruturado.

Tome suas precauções ao analisar arquivos reais: as amostras usadas neste curso são inofensivas, mas repetir os fluxos de trabalho apresentados em amostras reais sem as respectivas medidas de segurança provavelmente fará com que seu dispositivo seja infectado. Não execute nenhum arquivo suspeito em seu computador principal, use uma máquina virtual, um dispositivo dedicado ou um ambiente em que não seja possível executar o arquivo em sua máquina, mas apenas analisar suas propriedades.

Questionário de isenção de responsabilidade

Pergunta 1: Entendo os riscos de analisar arquivos suspeitos, as possíveis consequências de executar malware de propósito ou acidentalmente, li o conteúdo desta página/seção e entendo as estratégias mais comuns para lidar com essas possíveis ameaças.

Pergunta 2: Qual das opções a seguir descreve melhor o que precisamos fazer ao analisar um arquivo realmente suspeito?

1. Devemos iniciar uma máquina virtual (VM) ou um computador dedicado para analisar o arquivo e dar a ele o mínimo de acesso possível à nossa máquina host e ao restante da rede
2. Podemos analisar o arquivo em nosso próprio computador/ambiente, mas sem ter acesso à Internet.
3. Devemos analisar o arquivo em um computador ou máquina virtual (VM) com sistemas operacionais menos comuns, como Linux ou macOS.

Sobre modelos de ameaças

Ao procurar conselhos sobre como lidar com arquivos suspeitos, geralmente a abordagem proposta é evitar qualquer interação com os arquivos, por exemplo:

Não abra arquivos desconhecidos.

Não interaja com arquivos suspeitos.

Não faça contato visual com nenhum arquivo suspeito. Ou podemos encontrar outro tipo de conselho que, embora seja suficiente para a maioria das pessoas, pode ser enganoso para usuários sensíveis, como ativistas de direitos humanos ou jornalistas que trabalham em ambientes perigosos, ou simplesmente contraproducente, por exemplo:

O uso de um antivírus é suficiente para protegê-lo de arquivos maliciosos.

Somente os documentos do Microsoft Office com macros são perigosos, portanto, você pode tratar outros tipos de arquivos sem se preocupar muito. Exclua qualquer e-mail com anexos suspeitos. Isso é particularmente preocupante em alguns cenários porque, se excluirmos os e-mails e anexos de nossa caixa de entrada, perderemos evidências importantes que podem nos ajudar a avaliar se os artefatos são de fato mal-intencionados ou direcionados, o que pode ser uma informação inestimável.

Na prática, quando estamos trabalhando com comunidades visadas (especialmente jornalistas), não interagir com arquivos não é uma opção. Muitas organizações, grupos e indivíduos precisam abrir arquivos potencialmente perigosos como parte de seu trabalho, e eles o farão mesmo sabendo dos riscos:

Jornalistas recebem um convite para uma coletiva de imprensa. Ativistas recebem um documento de apoio como prova em um caso de violação de direitos humanos ou como um vazamento.

Uma instituição adversária envia um documento que deve ser analisado e tratado.

Um fator extra a ser considerado é que os atores da sociedade civil estão expostos a ameaças direcionadas desconhecidas pelos mecanismos antivírus. Este curso utiliza materiais disponíveis em outras referências e usa apenas ferramentas disponíveis gratuitamente.A maior parte do conteúdo é inspirada no trabalho que Didier Stevens tem feito ao longo do tempo, especialmente para o SANS, bem como em outras referências: https://blog.didierstevens.com/2011/05/25/malicious-pdf-analysis-workshop-screencasts/ https://github.com/filipi86/MalwareAnalysis-in-PDF

https://www.sentinelone.com/blog/malicious-pdfs-revealing-techniques-behind-attacks/ https://www.youtube.com/watch?v=opdVFQEBCNU Estrutura Isenções de responsabilidade

Algumas considerações sobre modelagem de ameaças Para cada tipo de formato de arquivo (PDFs, MS Office)

Como eles são estruturados (de uma forma mais técnica) Como eles podem ser transformados em armas

Como podemos fazer uma análise introdutória Algumas conclusões/fatos sobre o formato de arquivo

Alguns conselhos gerais contra as ameaças relacionadas O que vem a seguir A seguir, uma série de isenções de responsabilidade úteis antes de começar a usar o material

Isenções de responsabilidade Dada a natureza da tarefa que realizaremos após dominar o conteúdo fornecido (análise de arquivos maliciosos e perigosos) e a complexidade do tópico (que consideramos uma introdução à análise de malware), recomendamos enfaticamente a leitura desta seção e a concordância com todos os itens antes de prosseguir.

Este curso é introdutório: ele foi criado para pessoas sem experiência anterior em análise de documentos suspeitos.Na seção Próximas etapas, incluímos uma lista de recursos para leitura e referência adicionais.

Este curso não aborda muitas técnicas avançadas: há muitas ameaças específicas cuja complexidade vai além do escopo deste material. Além disso, como em tudo relacionado à segurança da informação, pode haver ameaças esperando para serem descobertas que não serão abordadas neste curso.Recomendamos procurar ajuda caso suspeitemos que estamos vendo uma ameaça avançada ou desconhecida em um arquivo ou em qualquer outro artefato.Dito isso, este curso nos ajudará a entender melhor como um arquivo benigno geralmente se parece, em vez de como todo documento mal-intencionado é estruturado.

Sobre modelos de ameaças

Ao procurar conselhos sobre como lidar com arquivos suspeitos, geralmente a abordagem proposta é evitar qualquer interação com os arquivos, por exemplo:

Não abra arquivos desconhecidos.

Não interaja com arquivos suspeitos.

Não faça contato visual com nenhum arquivo suspeito.

Ou podemos encontrar outro tipo de conselho que, embora seja suficiente para a maioria das pessoas, pode ser enganoso para usuários sensíveis, como ativistas de direitos humanos ou jornalistas que trabalham em ambientes perigosos, ou simplesmente contraproducente, por exemplo:

O uso de um antivírus é suficiente para protegê-lo de arquivos maliciosos.

Somente os documentos do Microsoft Office com macros são perigosos, portanto, você pode tratar outros tipos de arquivos sem se preocupar muito.

Exclua qualquer e-mail com anexos suspeitos. Isso é particularmente preocupante em alguns cenários porque, se excluirmos os e-mails e anexos de nossa caixa de entrada, perderemos evidências importantes que podem nos ajudar a avaliar se os artefatos são de fato mal-intencionados ou direcionados, o que pode ser uma informação inestimável.

Na prática, quando estamos trabalhando com comunidades visadas (especialmente jornalistas), não interagir com arquivos não é uma opção. Muitas organizações, grupos e indivíduos precisam abrir arquivos potencialmente perigosos como parte de seu trabalho, e eles o farão mesmo sabendo dos riscos:

Jornalistas recebem um convite para uma coletiva de imprensa.

Ativistas recebem um documento de apoio como prova em um caso de violação de direitos humanos ou como um vazamento.

Uma instituição adversária envia um documento que deve ser analisado e tratado.

Um fator extra a ser considerado é que os atores da sociedade civil estão expostos a ameaças direcionadas desconhecidas pelos mecanismos antivírus. Outro fator é que, dependendo do tipo de ataque, outros formatos de arquivo também podem ser usados como arma. Esses fatores são essenciais para serem considerados pelas pessoas que ajudam grupos vulneráveis a entender melhor como os documentos e outros formatos de arquivos comuns podem ser transformados em armas, para dar conselhos úteis, mas também para ajudá-los a analisar arquivos específicos para entender e avaliar se estão sendo vítimas de ataques direcionados.

Com tudo isso em mente, vamos nos concentrar em entender como os formatos de arquivo padrão são estruturados, como identificar os ataques mais comuns que os utilizam e algumas medidas defensivas atualizadas para evitar ser vítima desse tipo de ameaça. Questionário sobre o modelo de ameaça

Pergunta 1: Para uma organização altamente visada que recebe muitos documentos do Microsoft Office por e-mail, qual das opções a seguir é verdadeira? (Apenas uma está correta)

1. Mesmo que o software antivírus (AV) diga que o arquivo é seguro, ele ainda pode conter malware.
2. Eles precisam excluir imediatamente todos os anexos suspeitos, pois pode ser perigoso tê-los na caixa de entrada.
3. Eles não devem abrir nenhum anexo de fontes desconhecidas.

Ambiente: Considerações gerais

Para executar a maioria das tarefas deste curso, usaremos ferramentas básicas escritas na linguagem de programação Python. Dada a ampla compatibilidade do Python com todos os sistemas operacionais, há inúmeras maneiras de configurar um ambiente. Propomos uma especificamente, mas se você estiver familiarizado com Python, análise de malware e/ou virtualização, poderá configurar uma versão diferente que funcione para você. O único pedido importante seria ter um ambiente isolado para manipular artefatos perigosos (arquivos, neste caso). Há outras considerações, mas provavelmente essa é a mais importante. Ambiente isolado e outras boas práticas

As amostras usadas neste curso são inofensivas, apenas para demonstrar como os arquivos são estruturados e como identificar sinais de alerta; no entanto, se você pretende analisar arquivos reais, é provável que encontre um arquivo infectado que possa causar todos os tipos de problemas, como infectar o computador que você está usando, comprometer suas informações ou tornar seu dispositivo inutilizável, entre outros. Dito isso, é prática comum ter um ambiente exclusivamente para analisar e executar amostras suspeitas de forma controlada, de modo que, se algo der errado ao manipular a amostra, você não afetará seu dispositivo nem as informações contidas nele.

Outra vantagem de ter um ambiente dedicado é que, depois de manipular amostras de malware, você pode excluir tudo e começar de novo sem medo de perder arquivos não relacionados. Isso nos permite planejar maneiras práticas de “redefinir” nosso ambiente para um estado pronto para uso antes de cada análise.

Uma das estratégias mais usadas para garantir um ambiente isolado é o uso de máquinas virtuais (VMs), que basicamente emulam um computador completo dentro de outro computador, incluindo o sistema operacional (SO), discos rígidos, tela etc. As ferramentas comuns para configurar e usar VMs são o Virtualbox e o VMware Workstation Player, entre outras. O uso de hardware dedicado também é uma opção, desde que esteja protegido em caso de infecção.

Uma possível desvantagem pode ser o fato de alguns malwares incluírem código para verificar se são executados em ambientes isolados e não são executados, dificultando a análise; no entanto, o perigo inerente de executar malware em nossos ambientes cotidianos não vale a pena nem mesmo tentar; recomendamos procurar ajuda, concentrando-se em técnicas que não dependam da execução de arquivos suspeitos ou obtendo informações sobre como configurar um ambiente que se pareça com uma máquina real para uma amostra de malware. Para este recurso, isso não deve ser um problema, pois não executaremos nenhum código de documentos, mas se você quiser aprender e realizar análises dinâmicas em arquivos suspeitos, isso será útil. Outras considerações

Além da boa prática de ter um ambiente isolado, outras práticas comuns são:

Certifique-se de que o computador que está usando não esteja conectado à Internet ou à rede local: especialmente se estiver abrindo arquivos suspeitos, o motivo mais frequente para fazer isso é evitar o acionamento de sinais que alertem os operadores de malware de que o código está sendo executado ou testado de acordo com outros dados, como o endereço IP ou o tipo de dispositivo que está executando o malware. Além disso, alguns malwares tentarão se propagar para a rede local, tentando infectar outros dispositivos não intencionais, portanto, é uma prática comum isolar os dispositivos de teste em diferentes redes físicas ou virtuais (ou VLANs). Lembre-se de que, no caso de analisar uma amostra executando-a, é possível que o malware detecte que não tem acesso à Internet e não seja executado. Se for se conectar à Internet, use uma VPN ou algo semelhante: a ideia é ocultar sua localização real caso o malware que estamos analisando seja executado e sinalize seus operadores. Novamente, geralmente não é recomendável executar malware sem medidas para evitar qualquer possível comunicação com os operadores; no entanto, usar uma VPN pode ser uma boa medida em caso de execução acidental ou se outras configurações falharem em algum momento.

Organize um processo para redefinir seu ambiente para um estado “limpo”: Dependendo se você estiver usando uma máquina virtual ou um hardware dedicado, há algumas ferramentas e recursos que são úteis para redefinir o ambiente de modo que, toda vez que você analisar uma amostra, a máquina estará limpa. Para VMs, o uso de instantâneos é um bom exemplo, e há um software para reverter um computador físico para um estado anterior.

Atenha-se à análise estática: Em geral, podemos dividir a análise de malware de acordo com o fato de estarmos executando as amostras ou não. A análise estática tenta dissecar os arquivos e outros artefatos para obter o máximo de informações possível sem executá-los, enquanto a análise dinâmica executa as amostras para ver o que muda no ambiente de teste. Dependendo do tipo de malware, um tipo de análise pode ser mais útil do que o outro, mas, em geral, a análise dinâmica exigirá mais medidas para proteger o ambiente de teste e a rede para poder suportar a execução de malware real. Este curso mostra apenas técnicas de análise estática.

Outro fator é que, dependendo do tipo de ataque, outros formatos de arquivo também podem ser usados como arma.Esses fatores são essenciais para serem considerados pelas pessoas que ajudam grupos vulneráveis a entender melhor como os documentos e outros formatos de arquivos comuns podem ser transformados em armas, para dar conselhos úteis, mas também para ajudá-los a analisar arquivos específicos para entender e avaliar se estão sendo vítimas de ataques direcionados. Com tudo isso em mente, vamos nos concentrar em entender como os formatos de arquivo padrão são estruturados, como identificar os ataques mais comuns que os utilizam e algumas medidas defensivas atualizadas para evitar ser vítima desse tipo de ameaça. Questionário sobre o modelo de ameaça Pergunta 1: Para uma organização altamente visada que recebe muitos documentos do Microsoft Office por e-mail, qual das opções a seguir é verdadeira?

(Apenas uma está correta) 1.Mesmo que o software antivírus (AV) diga que o arquivo é seguro, ele ainda pode conter malware. 2.Eles precisam excluir imediatamente todos os anexos suspeitos, pois pode ser perigoso tê-los na caixa de entrada. 3.Eles não devem abrir nenhum anexo de fontes desconhecidas. Ambiente:Considerações gerais Para executar a maioria das tarefas deste curso, usaremos ferramentas básicas escritas na linguagem de programação Python. Dada a ampla compatibilidade do Python com todos os sistemas operacionais, há inúmeras maneiras de configurar um ambiente. Propomos uma especificamente, mas se você estiver familiarizado com Python, análise de malware e/ou virtualização, poderá configurar uma versão diferente que funcione para você.O único pedido importante seria ter um ambiente isolado para manipular artefatos perigosos (arquivos, neste caso). Há outras considerações, mas provavelmente essa é a mais importante. Ambiente isolado e outras boas práticas As amostras usadas neste curso são inofensivas, apenas para demonstrar como os arquivos são estruturados e como identificar sinais de alerta; no entanto, se você pretende analisar arquivos reais, é provável que encontre um arquivo infectado que possa causar todos os tipos de problemas, como infectar o computador que você está usando, comprometer suas informações ou tornar seu dispositivo inutilizável, entre outros.Dito isso, é prática comum ter um ambiente exclusivamente para analisar e executar amostras suspeitas de forma controlada, de modo que, se algo der errado ao manipular a amostra, você não afetará seu dispositivo nem as informações contidas nele. Em seguida, procure a seção com os pacotes por plataforma, conforme mostrado na imagem.

Captura de tela do site do virtualbox com a lista das diferentes plataformas para download da ferramenta

Aqui, clique em sua plataforma e siga as instruções. Depois disso, você pode executar o Virtualbox e ver uma janela como esta Captura de tela da janela do Virtualbox Você ainda não terá nada na área embaçada. A partir daqui, estamos prontos para baixar e instalar o Remnux Instalando o Remnux

Agora, acesse https://remnux.org/ e clique em “Download” na seção correspondente. É possível que você seja redirecionado para outra página solicitando que selecione se deseja fazer o download de um OVA geral ou de um OVA do Virtualbox; no nosso caso, o último será o correto.

Captura de tela do site do Remnux na seção de downloads Depois de fazer o download do arquivo, é recomendável verificar se o arquivo foi baixado corretamente. Para isso, precisamos verificar o hash associado ao arquivo.Hashing é um tópico denso que incentivamos a aprender e aplicar (também é muito usado na análise de malware), mas, por enquanto, podemos resumi-lo como um processo matemático que transforma um dado (como um texto ou um arquivo) em um código alfanumérico. Esse código deve ser exclusivo para os dados que você está analisando e, mesmo com pequenas alterações, o hash mudará muito, Portanto, verificar se o arquivo baixado tem o mesmo hash publicado no site do Remnux nos dirá que o arquivo foi baixado sem problemas; se o hash for diferente, será um sinal de que o arquivo foi corrompido por causa de um processo de download defeituoso ou de que, de alguma forma, não é o arquivo correto (talvez um erro de nossa parte ao selecionar a versão correta ou, em um cenário remoto, alguém alterou o arquivo para uma versão maliciosa, portanto, fique atento).Em geral, podemos dividir a análise de malware de acordo com o fato de estarmos executando as amostras ou não.

A análise estática tenta dissecar os arquivos e outros artefatos para obter o máximo de informações possível sem executá-los, enquanto a análise dinâmica executa as amostras para ver o que muda no ambiente de teste.

Dependendo do tipo de malware, um tipo de análise pode ser mais útil do que o outro, mas, em geral, a análise dinâmica exigirá mais medidas para proteger o ambiente de teste e a rede para poder suportar a execução de malware real.Este curso mostra apenas técnicas de análise estática. Tenha cuidado ao publicar amostras ou outras informações das amostras analisadas:Em geral, isso pode alertar os operadores de malware sobre o fato de estarmos analisando a campanha de malware, fazendo com que eles fechem a infraestrutura, limpem todos os rastros para dificultar a atribuição etc.Isso se aplica a qualquer plataforma pública, como mídias sociais e sites, incluindo algumas plataformas públicas em que podemos enviar arquivos para analisá-los na nuvem em busca de sinalizações de mecanismos antivírus e da comunidade de segurança da informação.

Para o último cenário, compartilharemos alguns exemplos e técnicas para verificar as informações de que precisamos sem alertar ninguém. Questionário sobre o ambiente Pergunta 1: Qual das seguintes afirmações é verdadeira? 1.A execução de amostras exigirá menos medidas de segurança do que tentar dissecar os artefatos para procurar insights úteis.

2.

Cortar o acesso à Internet tornará mais difícil para uma amostra de malware notificar os criadores de que foi executada.

3.

A maneira mais eficiente de analisar malware é usar máquinas virtuais porque, se a máquina for infectada, poderemos criá-la novamente do zero.

Exemplo de ambiente:

Remnux + Virtualbox Caso você queira um ambiente funcional pronto para uso, recomendamos o uso do Remnux, uma máquina virtual (VM) para download pré-configurada com algumas ferramentas úteis para análise de malware.Aqui, usaremos o Virtualbox para virtualizar a máquina Remnux. Se você estiver familiarizado com esse processo, fique à vontade para pular para a próxima seção do curso.

Instalação do Virtualbox

Primeiro, precisaremos de um programa para gerenciar nossas máquinas virtuais. Escolhemos o Virtualbox porque é a solução mais usada, compatível com as três principais plataformas (Windows, macOS e Linux) e é gratuita.

Para fazer o download do respectivo instalador, visite https://www.virtualbox.org/ e procure o grande botão azul.Em seguida, procure a seção com os pacotes por plataforma, conforme mostrado na imagem. Dependendo do uso que daremos à nossa máquina, para a configuração inicial, podemos manter o NAT ativado para acessar a Internet para fazer download de ferramentas etc., e antes de iniciar nossa análise, podemos alterá-lo para Não conectado, Rede interna ou desativar o adaptador.

Configurações extras no Virtualbox - Compartilhamento de informações com a máquina host

Como analisaremos arquivos potencialmente prejudiciais, não é aconselhável executar a máquina de forma que ela possa se comunicar com o restante da rede. A estratégia específica pode variar dependendo do estilo do analista, mas a configuração é feita principalmente na tela de interfaces da VM. Com nossa máquina Remnux desligada, clicamos no botão “Settings” (Configurações) na barra de ferramentas.

Captura de tela da ferramenta VirtualBox com a máquina virtual REMnux pronta para uso Em seguida, na seção “Network” (Rede), você terá uma série de opções, sendo as mais importantes: Captura de tela do aplicativo VirtualBox na janela de configurações de uma máquina virtual na seção Network (Rede) com um menu suspenso estendido destacando NAT no campo Attached to (Anexado a)

Enable Network Adapter (Ativar adaptador de rede): a desativação dessa opção eliminará qualquer conectividade entre nossa VM e outros dispositivos por meio da rede (inclusive a nossa, que será gerenciada por meio da interface gráfica) e emulará a ausência de hardware para se conectar a qualquer rede na VM.

Attached to - NAT: a configuração padrão emulará uma nova rede para a VM, o que permite que ela acesse a Internet, mas também outros dispositivos da nossa rede, o que não é recomendado para o tipo de uso que daremos à nossa VM.

Attached to - Bridged Adapter (conectado a um adaptador em ponte): Isso compartilhará o adaptador de rede do nosso computador host físico com a VM, colocando-a como qualquer outro dispositivo em nossa rede. Essa opção também não é recomendada para o nosso caso de uso.

Attached to - Host-only Adapter (Conectado a um adaptador somente de host): conecta a VM a uma rede que está conectada somente à nossa máquina host e a outras VMs com a mesma configuração; em alguns casos, isso pode ser útil, mas também pode expor nossa máquina a atividades mal-intencionadas.

Attached to - Internal Network (Conectado a uma rede interna): semelhante ao anterior, mas nossa máquina host não está acessível; é útil quando queremos ver como duas ou mais máquinas interagem entre si.

Attached to - Not attached (Conectado a - Não conectado): Isso emula um adaptador de rede sem um cabo conectado a ele Dependendo do uso que daremos à nossa máquina, para a configuração inicial, podemos manter o NAT ativado para acessar a Internet para fazer download de ferramentas etc., e antes de iniciar nossa análise, podemos alterá-lo para Não conectado, Rede interna ou desativar o adaptador. Configurações extras no Virtualbox - Compartilhamento de informações com a máquina host

É muito comum compartilhar arquivos e outros dados entre nosso computador e a VM. Novamente, há diferentes abordagens que podemos adotar:Pastas compartilhadas: semelhante a uma pasta compartilhada de rede, podemos sincronizar uma pasta entre nosso host e nosso sistema convidado (a máquina virtual).Isso nem sempre é recomendado para compartilhar amostras de malware, pois abrirá um espaço em nosso computador que é controlado por nossa VM, que pode ser infectada em instantes de nossa análise.Para configurar as pastas compartilhadas, há uma seção dedicada nas configurações. Captura de tela do aplicativo VirtualBox na janela de configurações de uma máquina virtual na seção Shared Folders (Pastas compartilhadas)

Área de transferência compartilhada e arrastar e soltar: isso nos permitirá compartilhar a área de transferência entre o nosso computador e a máquina virtual, que pode ser desativada, unidirecional ou bidirecional, conforme sugerido na imagem. Semelhante para arrastar e soltar arquivos entre o sistema host e o sistema guest. Para alguns, desabilitar o compartilhamento de pastas e habilitar o arrastar e soltar somente de “Host to Guest” é a opção mais segura para proteger nossos computadores físicos, de forma semelhante ao compartilhamento da área de transferência; no entanto, em alguns momentos, talvez seja necessário extrair informações da VM.

Captura de tela de uma máquina virtual no VirtualBox com o menu Devices (Dispositivos) e o submenu Shared Clipboard (Área de transferência compartilhada) abertos com a opção Bidirectional (Bidirecional) destacada Configurações extras no Virtualbox - Snapshots Um recurso muito útil do Virtualbox é salvar uma versão da VM para a qual podemos reverter a qualquer momento no futuro. Assim, por exemplo, se configurarmos a máquina Remnux para analisar malware, talvez queiramos salvar um instantâneo antes de iniciar a análise, de modo que, quando terminarmos, possamos reverter a VM para o instantâneo salvo para ter certeza de que a máquina não está infectada e que estamos prontos para continuar a análise. Para salvar um instantâneo, com a máquina no estado desejado, clique em “Machine” (Máquina) e depois em “Take Snapshot” (Tirar instantâneo) Captura de tela de uma máquina virtual no VirtualBox com o menu Machine aberto e a opção Take screenshot destacada

Em seguida, selecione um nome e clique em “OK”. Levará algum tempo para criar o instantâneo e, depois disso, ele estará disponível na seção Snapshots da tela principal do Virtualbox para a nossa VM.

Captura de tela da janela de snapshot do VirtualBox

Podemos usar o botão “Restore” (Restaurar) na respectiva tela

Captura de tela da janela do VirtualBox com o botão Restore destacado O que vem a seguir

Como podemos lidar com o básico com o VirtualBox, podemos aprender sobre o Remnux enquanto entendemos e analisamos nosso primeiro formato de arquivo: PDFs.