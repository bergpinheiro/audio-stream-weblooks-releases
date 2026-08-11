# Notas de versão

Escrito para quem opera a emissora, não para quem programa: o que mudou e
o que isso significa no ar.

O `tool/build_installer.dart --publish` lê deste arquivo a seção da versão
que está publicando e a usa como nota do release e na janela de
atualização. **Publicar sem a seção correspondente falha de propósito** —
uma versão sem nota é uma versão que ninguém sabe se deve instalar.

## 1.1.4

Seis correções e um recurso novo. Cinco das correções são o mesmo
assunto: **o que acontece quando a placa de som cai**.

### Correções

**A placa voltava e o programa não a encontrava mais.** Puxar o cabo USB
durante a transmissão parava a captura, como esperado. Religar não
adiantava: o programa dizia "aguardando" uma vez e nunca mais procurava.
Só reiniciando resolvia. Agora ele procura até achar — inclusive quando o
Windows devolve o aparelho com o nome mudado, o que acontece ao trocar de
porta.

**A codificação não voltava junto com a captura.** Quando a placa se
recuperava sozinha, o áudio voltava a ser capturado mas não a ser
codificado. A transmissão então conectava sem ter o que enviar, caía em
20 segundos e recomeçava. Numa madrugada isso deu 183 conexões em uma
hora e oitenta minutos fora do ar, sem uma linha de erro no log. Agora a
codificação volta junto — e continua parada se foi você quem a parou.

**O botão ON AIR liga a captura.** Se você parou a captura na tela de
Áudio e depois resolveu entrar no ar, o botão não fazia nada. Agora ele
liga no dispositivo que já está escolhido no perfil, e só avisa quando
esse dispositivo de fato não existe mais na máquina.

**Não dá mais para trocar de dispositivo no ar.** Era possível parar a
captura e escolher outra placa com a transmissão de pé — e o envio
parava sem explicação. A tela de Áudio agora fica travada durante a
transmissão, pelo mesmo motivo que codec e bitrate já eram.

**A automação por horário não rodava.** As trocas de perfil agendadas só
aconteciam se a tela de Automação tivesse sido aberta em algum momento
depois de o programa abrir. Reiniciando o computador, nenhuma acontecia.
Agora funciona com o programa na bandeja, e cada troca fica registrada no
log.

**A tela de Processamento avisa quando o ajuste não está salvo.** Os
ajustes valem no ar imediatamente, mas só entram no perfil quando você
salva em Configurações. Antes não havia nada dizendo isso, e um ajuste
demorado se perdia no fechamento sem aviso.

### Compressão por faixa de frequência

O compressor podia comprimir o sinal inteiro como um bloco só. Quando o
grave do surdo disparava, a voz abaixava junto — em pagode, samba e forró
isso é a batida engolindo o locutor.

Agora o compressor tem dois modos, e o novo separa grave, médio e agudo,
comprimindo cada um por conta própria. Medido: com uma faixa a voz cai
12,4 dB quando o grave entra; com três, 0,01 dB.

Duas predefinições novas usam o recurso:

- **Rádio ao vivo** — para quem fala por cima da música. Aproxima o
  volume do microfone do volume das faixas sem abafar uma para caber a
  outra.
- **Música popular** — para acervo percussivo, sem locução por cima.

**Atenção a quem usa "Música" ou "Alto":** essas duas predefinições
mudaram para usar o recurso novo. O seu perfil salvo continua exatamente
como está — nada muda sozinho. Mas se você clicar numa delas de novo,
vai ouvir o ajuste novo.

Quem não mexer em nada não percebe diferença: o modo de três faixas vem
desligado, e perfis salvos por versões anteriores abrem com ele
desligado.

## 1.1.3

**A janela de atualização oferecia a mesma versão para sempre.** Depois de
instalar a 1.1.2, o programa continuava avisando que havia uma
atualização — e era a 1.1.2 de novo.

A causa: o executável dizia `1.1.2+4`, com o número de compilação no fim,
e o arquivo de atualização dizia `1.1.2`. Como as duas cadeias diferem, e
um sufixo assim costuma indicar versão de teste, o programa concluía que a
instalada era mais antiga.

O número de compilação saiu da cadeia de versão. Ele continua registrado
no executável, num campo separado que não entra nessa comparação.

## 1.1.2

**Botão "verificar atualizações".** Fica em Configurações, ao lado da
versão. Até aqui a verificação acontecia sozinha uma vez por dia e não
havia como antecipar — reiniciar o programa não adianta, porque ele só
consulta quando as 24 horas vencem. Agora é um clique, e ele responde
mesmo quando não há novidade.

**O nome do programa aparece por extenso.** A janela de atualização e o
Gerenciador de Tarefas mostravam `audio_stream_weblooks`, o nome do
arquivo. Agora mostram "Audio Stream WeblookS".

**Reconexão mais rápida quando a internet volta.** O programa espera cada
vez mais entre as tentativas, até meio minuto — o que evita martelar o
servidor, mas custava tempo fora do ar quando a conexão voltava logo
depois. Agora ele percebe a volta e tenta na hora, sem esperar o relógio.

## 1.1.1

Correção de quatro defeitos que apareceram na emissora ao trocar uma placa
de captura USB de porta e reiniciar o computador.

**A placa mudou de porta e o programa não a reconheceu mais.** O Windows
dá identificador novo quando o aparelho entra por outra porta — o
identificador guarda a porta dentro dele. O programa já sabia cair para o
nome nesse caso, mas o Windows também mexe no nome: acrescenta `2- ` e
enche de espaços. `Linha (USB Sound Device)` virou
`Linha (2- USB Sound Device)` e deixou de casar. Agora casa.

**Entrava no ar sem ter captura.** Quando o dispositivo do perfil não é
encontrado, o programa tentava transmitir assim mesmo e falhava. Agora
recusa antes, e diz por quê.

**"Argumento inválido (código -4)" virou mensagem em português.** Era o que
aparecia quando faltava captura.

**Acentos nos nomes dos dispositivos.** "Mistura estéreo" aparecia
corrompido na lista. Corrigido.

E quando o dispositivo do perfil realmente não existe mais, o Painel passa
a dizer **qual** faltou, em vez de só pedir para escolher um. O programa
não escolhe outro sozinho de propósito: pôr uma fonte diferente no ar sem
alguém pedir é pior que ficar fora.

## 1.1.0

**Contagem de ouvintes.** Aparece no Painel, ao lado do botão, e detalhada
nas Estatísticas. O programa descobre sozinho na maioria dos servidores;
num que hospeda várias emissoras, informe o link público do seu stream —
aquele mesmo que você dá aos ouvintes — e ele identifica a sua. Funciona
com Icecast, Shoutcast, DNAS e AzuraCast.

**Taxa de amostragem e canais no perfil.** Dá para ter um perfil de jornal
em mono e um musical em estéreo, trocando por horário. Mono no mesmo
bitrate soa melhor em programa falado, porque os bits não se dividem entre
dois canais.

**A versão aparece na tela**, no fim das Configurações.

Três correções no caminho de rede, todas encontradas com injeção de falhas
e todas capazes de tirar a emissora do ar sem avisar:

- **Servidor travado deixava o programa "No ar" indefinidamente.** Escrever
  no socket era contado como entrega, mesmo sem ninguém do outro lado
  recebendo. Agora ele percebe em cerca de um minuto e reconecta.
- **Uma piscada de internet podia derrubar a transmissão de vez.** Ao
  reconectar, o servidor às vezes ainda segura o lugar da fonte anterior e
  responde "ponto de montagem ocupado" — que era tratado como erro
  definitivo. Agora o programa insiste até o lugar ser liberado.
- **A consulta de ouvintes podia baixar áudio sem parar.** Alguns servidores
  DNAS respondem qualquer endereço desconhecido servindo o stream. A
  consulta agora recusa resposta que não seja estatística.

## 1.0.0

Primeira versão.

Encoder de transmissão para Windows, para emissoras que usam Playlist
Digital.

- **Codecs:** AAC+ (HE-AAC), MP3, Ogg Vorbis e Opus
- **Servidores:** Icecast 2, Shoutcast v1 e v2, Liquidsoap harbor
- **Entradas:** microfone, Stereo Mix, interface USB e captura do que o
  computador está tocando, sem cabo de retorno
- **Playlist Digital:** lê o RDS.txt e publica o nome da música sozinho,
  com acentuação correta, sem deixar vinheta aparecer como música
- **Processamento:** equalizador, compressor e limitador, com ajustes
  prontos
- **Automação:** troca de perfil por horário ou dia da semana
- **Reconexão automática** quando a internet cai, quando o servidor
  reinicia e quando o dispositivo de áudio é desconectado
- **Atualização automática**
