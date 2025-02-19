# endianness-v2

###### Solved by @beatriz-a-cardozo & @CupNudous

> Este é um CTF que envolve Forensics.

## Sobre o Desafio

O desafio é distribuído pela PicoCTF e criado por Junias Bonou. Classificado como nível médio, a descrição apenas diz: 'Aqui está um arquivo que foi recuperado de um sistema 32-bits que organizava os bytes de uma maneira estranha. Não temos certeza de que tipo de arquivo é.' Logo abaixo, encontra-se o arquivo para download.

## Solução

O arquivo baixado, chamado 'challengefile', não possui uma extensão que possa nos indicar que tipo de arquivo é. Portanto, para uma melhor análise, usamos o comando `exiftool challengefile` no terminal para obter mais informações.

[![Captura-de-tela-2025-02-19-112831.png](https://i.postimg.cc/C5sRBwLF/Captura-de-tela-2025-02-19-112831.png)](https://postimg.cc/N2LsZqHV)

Obtivemos algo importante com isso: o arquivo se assemelha a um **JPEG**, o que nos dá um caminho para seguir. 

Para vermos o que há dentro do arquivo, usamos `cat challengefile`, que nos mostra apenas uma bagunça de caracteres que não conseguimos identificar muita coisa, exceto que no começo do arquivo parece haver um cabeçalho separado do resto. Isso pode ser os **magicbytes** (assinaturas de arquivo que servem para identificar/verificar o tipo de arquivo).

Com isso, procuramos pelos magicbytes de um arquivo JPEG na tabela [File Magic Numbers](https://gist.github.com/leommoore/f9e57ba2aa4bf197ebc5) e encontramos os identificadores `ff d8 ff e0`. É importante ressaltar que esses números mágicos estão em hexadecimal. Portanto, antes de compará-los com o conteúdo do arquivo, precisamos converter o challengefile para HEX. Para isso, usaremos o site [CyberChef](https://gchq.github.io/CyberChef).

[![Captura-de-tela-2025-02-19-114958.png](https://i.postimg.cc/257MKJ61/Captura-de-tela-2025-02-19-114958.png)](https://postimg.cc/KK40K0tb)

A receita que usamos está na imagem, e com a saída vemos que o começo do arquivo é `e0 ff d8 ff`, que são os componentes do magicbyte de um JPEG, mas estão invertidos. Se conseguirmos colocá-los na ordem correta, possivelmente poderemos abrir esse arquivo como uma imagem.

No começo, tentamos fazer um código que trocasse a ordem dos bytes para que batessem com o magicbyte, mas deu completamente errado e nunca trocava da maneira correta. Depois, tentamos usar o [CyberChef](https://gchq.github.io/CyberChef) para reverter de maneira que os bytes ficassem do jeito que queríamos, mas novamente, não funcionou.

Procurando pelo [CyberChef](https://gchq.github.io/CyberChef) algo que funcionasse, encontramos uma função chamada `swap endianness`, cujo nome bate com o nome do exercício. Mas não é só isso. **Endianness** é a ordem em que os dados são alocados em bytes na memória. Abaixo, temos uma imagem retirada do slide de Arquitetura de Computadores do Professor Yvo do Inatel, onde vemos os dois tipos de ordenação: **Big-endian** e **Little-endian**. A diferença entre eles é que, no Big-endian, os bytes são guardados em ordem decrescente de seu peso numérico em endereços sucessivos de memória, e no Little-endian, são guardados em ordem crescente.

[![Captura-de-tela-2025-02-19-175013.png](https://i.postimg.cc/GmYvbFsk/Captura-de-tela-2025-02-19-175013.png)](https://postimg.cc/DSvSgb6Z)

O tipo de endianness de uma máquina é definido no hardware. O sistema de onde esse arquivo foi retirado é Little-endian, e precisamos passar para Big-endian para que funcione em nossos computadores. Então, usando o `swap endianness` do [CyberChef](https://gchq.github.io/CyberChef), temos:

[![imagem-2025-02-19-183115640.png](https://i.postimg.cc/zGb1dkcY/imagem-2025-02-19-183115640.png)](https://postimg.cc/zyrMvTNx)

Finalmente, conseguimos os magicbytes na ordem correta!

Porém, ao salvar como um arquivo .jpg, ainda não conseguimos abrir a imagem. Então, resolvemos reverter e tirar do formato hex o arquivo que obtivemos com o swap endianness, usando a função `From_Hex` do [CyberChef](https://gchq.github.io/CyberChef). Após baixar o resultado e tentar abrir, temos:

[![swapped-challengefile.jpg](https://i.postimg.cc/TY5kFHf0/swapped-challengefile.jpg)](https://postimg.cc/Whj6J8Dq)

> picoCTF{cert!f1Ed_iNd!4n_s0rrY_3nDian_004850bf}
