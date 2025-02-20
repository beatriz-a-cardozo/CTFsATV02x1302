# Eavesdrop

###### Solved by @beatriz-a-cardozo, @CupNudous

Este é um CTF sobre Forense, Análise de Tráfego e Criptografia.

## About the Challenge

O desafio Eavesdrop do PicoCTF nos fornece um arquivo .pcap, que, segundo a dica fornecida, contém uma captura de pacotes de uma conversa de chat entre duas pessoas e uma transferência de arquivo. Nosso objetivo é analisar a captura para recuperar a flag escondida no arquivo transmitido.

## Solution

Antes de iniciar a análise, pesquisamos sobre a extensão `.pcap` para entender como poderíamos utilizá-la. Descobrimos que arquivos `.pcap` (Packet Capture) são usados para armazenar capturas de tráfego de rede e podem ser analisados com ferramentas como o `Wireshark` para extrair informações valiosas.

Então decidimos analisar o arquivo .pcap recebido. Para isso, utilizamos o comando `strings` no terminal do Kali Linux para extrair possíveis mensagens em texto claro:

>`strings capture.flag.pcap`

A partir da saída do comando, encontramos o conteúdo da conversa de chat a seguir, que revelou informações valiosas sobre a transferência do arquivo:

[![Screenshot-2025-02-18-173435.png](https://i.postimg.cc/g0bxgj3M/Screenshot-2025-02-18-173435.png)](https://postimg.cc/jDvq5sgN)

[![Screenshot-2025-02-18-173446.png](https://i.postimg.cc/Dyt0z0p2/Screenshot-2025-02-18-173446.png)](https://postimg.cc/WhwTWNPK)

[![Screenshot-2025-02-18-173454.png](https://i.postimg.cc/3JyW7mBS/Screenshot-2025-02-18-173454.png)](https://postimg.cc/JGLR5ySZ)

>`openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123`

O comando acima encontrado nos revela que o arquivo foi encriptado com usando um método conhecido como `Triple Data Encryption Standard (3DES),` um algoritmo que aplica o DES (Data Encryption Standard) três vezes para aumentar a segurança. O `Salt` é um valor aleatório adicionado antes da criptografia para dificultar ataques de força bruta e dicionário, garantindo que a mesma senha produza saídas diferentes.

Além disso, a conversa menciona que o arquivo estaria sendo enviado na porta `9002`.

Com essas informações, abrimos o arquivo .pcap no Wireshark para investigar melhor a transferência do arquivo, identificando todos aqueles que estabeleciam algum tipo de conexão com a porta 9002:

[![Screenshot-2025-02-18-173403.png](https://i.postimg.cc/6pMsYRBh/Screenshot-2025-02-18-173403.png)](https://postimg.cc/gxLB0xXw)

Utilizamos então, a opção Follow TCP Stream para visualizar os dados transferidos. Essa funcionalidade do Wireshark permite reconstruir toda a comunicação entre cliente e servidor ao longo da conexão, facilitando a identificação de mensagens transmitidas. No caso deste desafio, isso nos permitiu extrair o conteúdo completo do arquivo transferido pela porta 9002, em que percebemos que os mesmos dados estavam sendo enviados em todas as conexões.

O conteúdo do arquivo estava originalmente codificado em `ASCII`, mas através do próprio Wireshark podíamos trocar para `Raw`:

[![Screenshot-2025-02-18-173415.png](https://i.postimg.cc/c4bDkcVT/Screenshot-2025-02-18-173415.png)](https://postimg.cc/2VnFVnjB)

Após isso, criamos então o arquivo `file.des3`, para que, usando o comando original mencionado na conversa do chat, pudemos desencriptar o conteúdo e recebê-lo em texto claro:

>`openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123`

Finalmente, ao abrir o arquivo flag.txt que foi gerado no diretório, encontramos a flag do desafio:

>`picoCTF{nc_73115_411_dd54ab67}`