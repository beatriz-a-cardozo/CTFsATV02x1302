# m00nwalk 2

###### Solved by @beatriz-a-cardozo, @CupNudous

Este é um CTF sobre Forense, Análise de áudio e Esteganografia.

## About the Challenge

O desafio `m00nwalk 2` do picoCTF fornece quatro arquivos de áudio: `message.wav`,
`clue1.wav`, `clue2.wav` e `clue3.wav`, e uma dica que diz que devemos usar os áudios com
nome "clue" para extrair a verdadeira flag. Ao ouvi-los, percebe-se um som semelhante
ao de uma conexão de internet discada, seguido de uma sequência de bipes agudos e
aparentemente aleatórios. O objetivo do desafio é interpretar esses sons para
encontrar a flag escondida.

## Solution

Antes de iniciar a análise, pesquisamos sobre como esconder arquivos dentro de áudio,
o que de início não parecia possível, então analisamos o espectograma dos áudios
usando a ferramenta `Spek`:

>[![Screenshot-2025-02-20-151341.png](https://i.postimg.cc/DwqN4jNw/Screenshot-2025-02-20-151341.png)](https://postimg.cc/sMgmtcyk)

>[![Screenshot-2025-02-20-151353.png](https://i.postimg.cc/J0vNp8sC/Screenshot-2025-02-20-151353.png)](https://postimg.cc/5Q5QtZ5s)

>[![Screenshot-2025-02-20-151413.png](https://i.postimg.cc/zX38sgfk/Screenshot-2025-02-20-151413.png)](https://postimg.cc/XpMRFXdZ)

>[![Screenshot-2025-02-20-151429.png](https://i.postimg.cc/MTN0DB3p/Screenshot-2025-02-20-151429.png)](https://postimg.cc/5Y5F9XMh)

Através da análise desses espectogramas, com a ajuda de IA, chegamos à conclusão que
os arquivos wav escondem sinais `SSTV (Slow Scan Television)`,  um método de
transmissão de imagens usando sinais de áudio modulados em diferentes frequências.
Basicamente, cada linha da imagem é convertida em um tom de frequência específico, que
varia de acordo com a intensidade da cor (normalmente, brilho). O espectrograma de 
sinal SSTV mostrará essas variações de frequência ao longo do tempo.

Pesquisando mais a fundo, encontramos sobre o software QSSTV, um software para
decodificação de imagens transmitidas via SSTV, então, após entender o funcionamento
desses sinais e dos softwares necessários na Kali, decodificamos a primeira imagem
oculta no arquivo `message.wav`, gerando a seguinte imagem com uma flag:

>[![Screenshot-2025-02-19-203711.png](https://i.postimg.cc/CKkmmvCZ/Screenshot-2025-02-19-203711.png)](https://postimg.cc/SXQ6Kfvq)

>`picoCTF{beep-boop-im-in-space}`

Ao tentar colocar a flag obtida como resposta da questão, o sistema retorna que a flag
é inválida, não resolvendo a questão.

No entanto, ainda existem 3 áudios a serem analisados, além da dica dada pelo próprio
picoCTF, que diz para usarmos esses áudios para encontrar a flag real, então, vamos
analisar as imagens contidas dentro deles também.

O áudio `clue1.wav` esconde a seguinte imagem:

>[![Screenshot-2025-02-19-203946.png](https://i.postimg.cc/0yjQrMnt/Screenshot-2025-02-19-203946.png)](https://postimg.cc/878D0CqW)

A imagem apresenta a silhueta de um estegossauro, junto da frase`"Password:hidden_stegosaurus`
indicando que a senha de algo seria `hidden_stegousaurus`, talvez uma chave para
descriptografar um arquivo, ou a senha para abrir algo, possivelmente fazendo um
pequeno trocadilho com a palavra "esteganografia", tema central dessa questão, ambas tendo o mesmo prefixo "esteg" começando a palavra.

o áudio `clue2.wav` esconde outra imagem: 

>[![Screenshot-2025-02-19-204231.png](https://i.postimg.cc/Fzp7Yw6c/Screenshot-2025-02-19-204231.png)](https://postimg.cc/hQJSwZcP)

Apesar do ruído na parte superior da imagem, ainda é possível entender o seu conteúdo,
o desenho de duas orelhas com a frase `The quieter you are the more you can HEAR`
escrita, que faz uma referência em forma de metáfora, a resolução desse próprio exercício, sugerindo que há algo oculto nos sons e que, ao "escutar com mais atenção" (ou seja, analisar o espectrograma e decodificar os sinais), você descobriria as imagens escondidas. Isso reforça a ideia de que a solução não estava apenas em ouvir os áudios, mas sim em interpretar os sinais digitais transmitidos dentro deles.

O terceiro e último áudio, `clue3.wav` devolve a seguinte imagem ao ser decodificado:

>[![Screenshot-2025-02-19-204742.png](https://i.postimg.cc/Hkt9TTX2/Screenshot-2025-02-19-204742.png)](https://postimg.cc/XrpC8S2G)

Essa imagem apresenta os dizeres `"Alan Eliasen` e `"the FutureBoy`, que de início,
pode parecer não significar nada, mas pesquisando esses termos juntos, encontramos o
website https://futureboy.us mostrado a seguir:

>[![Screenshot-2025-02-20-162421.png](https://i.postimg.cc/G2R8M5X5/Screenshot-2025-02-20-162421.png)](https://postimg.cc/CZvx5mTG)

O site parece um portfólio de um programador, com as diversas ferramentes e jogos que
ele criou ao longo dos anos, incluindo uma que chama mais atenção, chamada
`Steganography` ou, estaganografia, nome dado a arte de esconder informações dentro de
arquivos ou até mesmo objetos.

Ao acessar a ferramenta de esteganografia, mais especificamente o seu decodificador,
podemos adicionar algum arquivo nosso para descriptografar, junto de uma senha, tendo
então o arquivo `message.wav` e a senha `hidden_stegosaurus` em mãos, podemos então
decodificar o arquivo e encontrar a verdadeira flag:

>[![imagem-2025-02-20-163227583.png](https://i.postimg.cc/x8ML0jYj/imagem-2025-02-20-163227583.png)](https://postimg.cc/YL2vRHhT)

[![Screenshot-2025-02-20-163327.png](https://i.postimg.cc/htCt6FxX/Screenshot-2025-02-20-163327.png)](https://postimg.cc/WdkVqH5V)

>`picoCTF{the_answer_lies_hidden_in_plain_sight}`