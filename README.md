# Audio Stream WeblookS

Encoder de transmissão para Windows, feito para emissoras que usam
**Playlist Digital**. Captura o áudio, codifica e envia para Icecast ou
Shoutcast — e publica sozinho o nome da música que o Playlist grava no
arquivo TXT.

**[⬇ Baixar a versão mais recente](https://github.com/bergpinheiro/audio-stream-weblooks-releases/releases/latest)**

Programa gratuito. Atualiza-se sozinho.

<!-- PRINT: Painel com ON AIR, música atual e VU -->

---

## O que ele faz

| | |
|---|---|
| **Codecs** | AAC+ (HE-AAC), MP3, Ogg Vorbis, Opus |
| **Servidores** | Icecast 2, Shoutcast v1 e v2 (DNAS), Liquidsoap harbor, AzuraCast |
| **Entradas** | microfone, Stereo Mix, **o que o computador está tocando** (loopback), interface USB |
| **Metadados** | lê o RDS.txt do Playlist Digital e publica automaticamente |
| **Processamento** | equalizador, compressor e limitador |
| **Automação** | troca de perfil por horário ou dia da semana |

---

## Instalação

1. Baixe o instalador na
   [página de versões](https://github.com/bergpinheiro/audio-stream-weblooks-releases/releases/latest)
2. Execute. O Windows vai avisar **"aplicativo não reconhecido"** —
   clique em *Mais informações* → *Executar assim mesmo*
3. Pronto

> **Sobre o aviso do Windows.** Ele aparece porque o programa não tem
> certificado de assinatura comprado, que custa algumas centenas de dólares
> por ano. Não é vírus nem defeito: é reputação. O aviso some sozinho
> conforme o programa acumula instalações.

Depois de instalado, ele verifica atualizações sozinho e avisa quando há
versão nova. Sua configuração é preservada.

---

## Primeira execução

Na primeira vez abre um assistente de sete passos que cobre o essencial.
Se preferir configurar na mão, ou refazer depois, é pelas telas abaixo.

<!-- PRINT: primeira tela do assistente -->

---

## Áudio — de onde vem o som

<!-- PRINT: tela Áudio, com VU e lista de dispositivos -->

Escolha o dispositivo e o som já aparece no medidor. Se o VU não se mexe,
não adianta seguir — nada vai ao ar.

### Qual dispositivo escolher

| Sua situação | Escolha |
|---|---|
| O áudio sai do próprio computador (Playlist Digital tocando) | a **saída** dele, em modo *loopback* |
| Mesa de som ligada por interface USB | a **interface USB** |
| Microfone direto no computador | o **microfone** |

O modo *loopback* captura exatamente o que o computador está tocando, sem
cabo de retorno. É o caso mais comum em estúdio com Playlist Digital.

> **Atenção com o loopback:** se nada estiver tocando, ele não entrega
> nenhum áudio — nem silêncio. O medidor fica parado e parece defeito. Ponha
> uma música para tocar e confira.

### Ganho

Ajuste para o pico bater na faixa amarela nos momentos mais altos, sem
entrar no vermelho. Vermelho é distorção, e no ar não tem conserto.

### Formato da captura

| | |
|---|---|
| **Taxa** | 48 kHz na dúvida. Só mude para 44,1 se sua placa for nativamente 44,1 |
| **Canais** | **Mono** soa melhor que estéreo no mesmo bitrate em programa só de fala — os bits não se dividem entre dois canais |

Fica salvo no perfil, então dá para ter um perfil de jornal em mono e um
musical em estéreo, trocando por horário.

Com a transmissão no ar esses campos ficam travados: o formato é combinado
com o servidor na hora de conectar, e mudá-lo por baixo derruba a conexão.

---

## Servidor — para onde vai o som

<!-- PRINT: tela Servidor (troque os dados pelos de exemplo antes) -->

Peça estes dados à sua hospedagem:

| Campo | O que é | Exemplo |
|---|---|---|
| **Tipo** | Icecast 2, Shoutcast v1 ou v2 | Icecast 2 |
| **Endereço** | o servidor, sem `http://` | `radio.exemplo.com.br` |
| **Porta** | a porta de transmissão | `8000` |
| **Ponto de montagem** | só Icecast; às vezes é só `/` | `/live` |
| **Usuário** | quase sempre `source` | `source` |
| **Senha** | a senha de transmissão | — |

> **Se a hospedagem só te deu uma senha no formato `usuario:senha`**, ponha
> tudo isso no campo de senha e deixe o usuário como `source`. É uma
> convenção antiga que várias hospedagens ainda usam.

**Use o botão Testar antes de ir ao ar.** Ele conecta de verdade e diz o
que houve, em vez de você descobrir com a emissora fora do ar.

A senha é guardada criptografada pelo Windows (DPAPI), nunca em texto.

### Codec e bitrate

| Codec | Quando usar |
|---|---|
| **AAC+** | melhor qualidade por banda. 64 kbps já soa bem — é a recomendação |
| **MP3** | o mais compatível. Precisa de 128 kbps para soar equivalente |
| **Opus** | excelente, mas nem todo servidor e player aceitam |
| **Ogg Vorbis** | alternativa livre ao MP3 |

Shoutcast **não** aceita Opus nem Vorbis; o programa impede a combinação.

---

## Playlist — o nome da música no ar

<!-- PRINT: tela Playlist com pré-visualização -->

Aponte para o arquivo que o Playlist Digital grava, normalmente:

```
C:\Playlist\pgm\RDS\RDS.txt
```

A tela mostra **ao vivo** o que está lendo e como vai ao ar. Confira com
uma música tocando antes de confiar.

### Ajustes que costumam ser necessários

| Ajuste | Para quê |
|---|---|
| **Ordem** | se aparecer "Música - Artista" invertido, um clique troca |
| **Remover número da faixa** | tira o `03. ` do começo |
| **Remover etiquetas** | tira coisas como ` - #Escolhas` do fim |
| **Lista de supressão** | evita que vinheta e institucional apareçam como nome de música |

A lista de supressão é a mais importante. Sem ela, quando entra a vinheta o
ouvinte vê o nome da emissora no lugar da música. Com ela, o programa
**mantém a última música válida** no ar.

Acentos são tratados automaticamente — "Coração" aparece certo, sem
configuração.

---

## Processamento — equalizador, compressor e limitador

<!-- PRINT: tela Processamento com os ajustes prontos -->

A ordem é fixa e não é opinião: **equalizador → compressor → limitador**. O
compressor precisa reagir ao som já equalizado, e o limitador é o teto
absoluto antes do encoder.

### Ajustes prontos

| Ajuste | Para |
|---|---|
| **Neutro** | não mexe em nada |
| **Voz** | programa falado — realça a inteligibilidade |
| **Música** | programação musical |
| **Alto** | mais presença e volume aparente |

Comece por um ajuste pronto e mexa só se precisar. Nas Estatísticas há um
contador de quantas amostras o limitador segurou — se ele estiver sempre
trabalhando, é sinal de realce ou compensação além do ponto.

Fica salvo no perfil.

---

## Automação — trocar de perfil por horário

<!-- PRINT: tela Automação com uma regra -->

Serve para o que muda de rotina: programação musical de madrugada em
bitrate menor, jornal em mono pela manhã, e assim por diante.

Crie um perfil para cada situação e uma regra dizendo quando cada um vale.
As regras podem ser **diárias** ou por **dia da semana**.

O programa não agenda: ele **confere** a cada meio minuto se o perfil certo
está no ar. A diferença importa — computador suspenso, relógio mudado ou
horário de verão não fazem a agenda se perder.

---

## Estatísticas

<!-- PRINT: tela Estatísticas -->

| O que mostra | Como ler |
|---|---|
| **Tempo no ar / conexão atual** | seis horas de sessão com dois minutos de conexão = internet caindo |
| **Bitrate real** | os dentes são normais; ruim é encostar no zero e ficar |
| **Buffers** | ocupação alta e persistente = alguém não está acompanhando |
| **Ouvintes** | quantos estão escutando, quando o servidor informa |
| **Consumo** | memória, handles e GDI subindo em linha reta denunciam problema |

### Ouvintes

Normalmente o programa descobre sozinho. Num servidor que hospeda **várias
emissoras** ele não tem como saber qual é a sua — aí informe o **link
público do seu stream**, o mesmo que você dá aos ouvintes:

```
https://seu-servidor/listen/sua-radio/radio.mp3
```

Desse link ele extrai o que precisa. Funciona com Icecast, Shoutcast e
AzuraCast, e o botão **Testar** diz o que encontrou antes de você salvar.

---

## Configurações

<!-- PRINT: tela Configurações -->

| Opção | O que faz |
|---|---|
| **Perfis** | configurações completas, para trocar de uma vez |
| **Entrar no ar sozinho** | volta ao ar depois de queda de energia, sem ninguém no estúdio |
| **Iniciar com o Windows** | sobe junto com o computador |
| **Minimizar para a bandeja** | some da barra de tarefas e fica só o ícone ao lado do relógio |

Com **Entrar no ar sozinho** e **Iniciar com o Windows** ligados, a
emissora volta sozinha depois de falta de energia. É a configuração
recomendada para estúdio sem operador de plantão.

Ao fim da tela aparece a **versão instalada** — é a primeira coisa que o
suporte pergunta.

---

## Quando algo dá errado

**Abra a tela Logs.** Ela registra tudo: conexões, quedas, trocas de música,
erros. Dá para exportar e enviar para o suporte.

Os arquivos ficam em:

```
%APPDATA%\AudioStreamWeblooks
```

### Problemas comuns

| Sintoma | Causa provável |
|---|---|
| VU parado | dispositivo errado, ou loopback sem nada tocando |
| Não conecta | endereço, porta ou senha — use o botão Testar |
| Nome da música não muda | caminho do TXT errado, ou o Playlist grava noutro lugar |
| Vinheta aparece como música | falta a lista de supressão |
| Áudio picotado | veja Buffers nas Estatísticas |
| Ouvintes não aparecem | informe o link público do stream |

O programa **reconecta sozinho** quando a internet cai, quando o servidor
reinicia e quando o dispositivo de áudio é desconectado e recolocado. Não
precisa ficar de olho.

---

## Requisitos

Windows 10 ou 11, 64 bits. Nada além disso.

---

<sub>O código-fonte é privado. Este repositório distribui os instaladores e
o arquivo de atualização automática.</sub>
