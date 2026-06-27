---
title: "Anunciando a libestream"
date: 2013-05-07
language: pt
---

O algoritmo de criptografia mais importante da atualidade, o AES (*Advanced Encryption Standard*), também conhecido como Rijndael, faz parte de uma categoria chamada de cifradores de bloco. Eles têm esse nome porque operam sobre conjuntos de dados de tamanho fixo: os blocos. Cada bloco — de 128 bits cada, no caso do AES — é cifrado/decifrado individualmente com uma chave secreta. Cada 128 bits gerados pelo processo de cifragem pode ser revertido a 128 bits do texto original usando-se a mesma chave no processo de decifragem.

Da natureza dos cifradores de bloco deriva-se um problema imediato: e se as mensagens forem maiores que o tamanho do bloco? Com frequência, o que deve ser cifrado é maior que 128 bits, então viu-se a necessidade de se definir os modos de operação, como o CBC (*cipher-block chaining*) ou o CTR (*counter*). Ao contrário do que pode parecer a princípio, simplesmente dividir a mensagem em pedaços de 128 bits e cifrar cada um separadamente (modo de operação ECB, *electronic codebook*) não é uma boa ideia: toda a vez que um mesmo pedaço da mensagem é cifrado com a mesma chave, o resultado é o mesmo padrão de bits, o que pode revelar a ocorrência de padrões repetidos dentro da mensagem original.

Os outros modos de operação resolvem esse problema, ou encadeando de alguma forma os blocos, como faz o CBC, propagando a entropia dos blocos anteriores e ocultando qualquer tipo padrão, ou o modo CTR, que transforma o cifrador de blocos efetivamente em um cifrador de fluxo, onde este é utilizado para gerar uma sequência de bits arbitrariamente grande, aparentemente aleatória, de distribuição estatística normal, que quando aplicado com uma função reversível à mensagem (como a KGB fazia com os *one-time pad*) a oculta até que a operação inversa seja feita.

Acontece que muita gente usa o AES no modo CTR, um algoritmo grande e complexo, feito para operar em blocos e com garantia de reversibilidade, simplesmente para imitar um cifrador de fluxo, que são algoritmos muito mais simples e rápidos de se computar, especificamente desenvolvidos para operar desta maneira.

O mais conhecido dos cifradores de fluxo é o RC4, um algoritmo extremamente simples, porém antigo e com várias falhas de segurança (falhas estas que permite que redes WiFi protegidas por WEB sejam facilmente invadidas, por exemplo), mas que em muitas situações acaba sendo utilizado por falta de alternativas. Isso acontece na própria Web, nas conexões seguras HTTPS, seja por questões de performance (cifradores de bloco são mais lentos e complexos) ou por causa da falha que encontraram no modo de operação do SSL/TLS (que afeta o AES, por ser um cifrador de bloco, tornando-o inseguro), muitos sites ainda utilizam o RC4, inclusive os maiores (Goolgle, Facebook, WordPress e Wikipedia incluídos, bem como muitos bancos), já que ele é o único cifrador de fluxo disponível nesse tipo de conexão.

Mas o RC4 não é o único desta família de algoritmos de criptografia. A exemplo do que foi a competição do AES, que selecionou o Rijndael e o tornou o mais usado algoritmo de cifragem de blocos do mundo, uma outra competição, chamada [eSTREAM](http://www.ecrypt.eu.org/stream/ "eSTREAM"), realizado de 2006 a 2008 por um órgão da União Européia, teve por objetivo avaliar, selecionar e recomendar para o mundo os melhores cifradores de fluxo disponíveis. Essa competição teve quatro vencedores na categoria software (isto é, excelente performance quando implementados em software): Rabbit, HC-128, Salsa20/12 e Sosemanuk.

Numa modesta tentativa de divulgar, incentivar e facilitar o uso destes algoritmos em detrimento do RC4, que é fraco, e, onde cabível, do AES, que é lento e complexo — e complexidade deve ser levada em consideração: a necessidade de se usar um modo de operação custou a segurança dos cifradores de bloco no SSL e na Web — eu desenvolvi a biblioteca [libestream](http://github.com/lvella/libestream "libestream").

Libestream é uma pequena biblioteca de software que implementa, sob domínio público (e portanto, software livre), em C portável, os quatro algoritmos selecionados pelo eSTREAM na categoria software, além do algoritmo [UMAC](http://tools.ietf.org/html/rfc4418 "UMAC") para autenticação de mensagem, necessário a qualquer sistema de criptografia para garantir a integridade da mensagem, mas especialmente importante no caso de cifragem de fluxo, que é particularmente suscetível a falsificações.

Com isso, espero prover um conjunto mínimo e auto-contido de ferramentas suficientes para se desenvolver sistemas criptográficos baseados em cifragem de stream. Mas se isso for trabalho demais e você só quiser se comunicar com segurança via sockets TCP, a biblioteca ainda oferece um protocolo simples, pronto para usar, que cuida de cifrar, assinar, decifrar e verificar as mensagens para você.

*postado originalmente no blog antigo*
