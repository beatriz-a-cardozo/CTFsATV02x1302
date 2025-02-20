# Matryoshka Doll

###### Solved by @beatriz-a-cardozo & @CupNudous

## About the Challenge

O desafio Matryoshka Doll é distribuido pela plataforma PicoCTF e desenvolvido por Susie/Pandu, a descrição nos diz que 'Matryoshka doll (boneca russa) é um conjunto de bonecas de madeira de tamanhos cada vez menores colocadas uma dentro da outra. Qual é a última?', também temos uma dica que diz que podemos guardar imagens dentro de arquivos junto com um arquivo bara baixarmos.

[![dolls.png](https://i.postimg.cc/3Njmgp2s/dolls.png)](https://postimg.cc/rRmDTDnj)

## Solution

O arquivo 'dolls.jpg' que baixamos é a foto de uma boneca russa e com o que foi falado na descrição sabemos que tem um arquivo dentro da imagem, mas para confirmar, usamos o `strings dolls.jpg` e dentro da imagem tem:

[![imagem-2025-02-19-202954933.png](https://i.postimg.cc/0yPZH5QG/imagem-2025-02-19-202954933.png)](https://postimg.cc/ZCsrWS60)
> base_images/2_c.jpgUT

Para extrairmos o arquivo usamos o software [binwalk](https://www.kali.org/tools/binwalk/) que foi passado na reunião do dia 19/02, com o comando `binwalk -e dolls.jpg` podemos extrair todos os arquivos que tem nessa imagem.

[![imagem-2025-02-19-203654010.png](https://i.postimg.cc/bvdxKZjn/imagem-2025-02-19-203654010.png)](https://postimg.cc/yJ43RYx6)

Aqui nos diz que foi retirado dois arquivos, um .zip e uma imagem, que é exatamente o que estamos procurando. O software criou a pasta `_dolls.jpg_extracted`e dentro delas temos o arquivo zipado (que não vai importar para esse exercício) e um diretório chamado 'base_images', lá se encontra nossa segunda boneca, a mesma imagem só que menor agora, mas nada da flag.

Então, repetimos os passos de: 

1. Extrair os arquivos da imagem com `binwalk -e <imagem_que_queremos_extrair>.jpg`
2. Navegar até o diretório criado que desejamos `cd <imagem_extraida>_extracted/base_images`
3. Voltar para o passo 1

Até que encontramos o arquivo `flag.txt`, ao extrair os arquivos da 4° imagem.

[![imagem-2025-02-19-205221111.png](https://i.postimg.cc/J4ZM5xDD/imagem-2025-02-19-205221111.png)](https://postimg.cc/Czh3Fj4S)

[![imagem-2025-02-19-205620984.png](https://i.postimg.cc/hGyBt3ww/imagem-2025-02-19-205620984.png)](https://postimg.cc/Y4WVX8tf)


> picoCTF{4f11048e83ffc7d342a15bd2309b47de} 
