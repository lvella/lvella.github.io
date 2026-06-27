---
title: "Announcing libestream"
date: 2013-05-10
language: en
---

Block ciphers, like AES, are not the best thing around for secure communication, for they require an [mode of operation](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation) in order to be properly used — which adds complexity, thus is itself a source of problems, see, for instance, the [BEAST attack](http://en.wikipedia.org/wiki/Transport_Layer_Security#BEAST_attack). Also, block ciphers are designed with reversibility guarantees that makes their execution cost very high compared to specialized solutions for communication: the stream ciphers.

But the only stream cipher algorithm in widespread adoption by 2013, called RC4, is old and broken in many ways. Due to its weakness, WEP WiFi protection is broken. While many cryptosystems relies on it for security, RC4’s shortcomings are rendering these systems increasingly fragile, specially due to its recent surge of popularity when people could not count on AES on SSL anymore due to BEAST attack, exposing RC4 to even more cryptanalysis.

To offer an alternative to RC4, European Union’s [ECRYPT](http://www.ecrypt.eu.org/) launched the [eSTREAM](http://www.ecrypt.eu.org/stream/) project in create/find, analyze and select the next generation of stream ciphers suitable for widespread adoption. The project was concluded in 2008 and recommended 4 stream cipher algorithms suitable to be implemented efficiently in software: HC-128, Rabbit, Salsa20/12 and Sosemanuk.

Despite the time since initial publication of eSTREAM, their adoption goes at very slow pace, with very few implementations besides the reference one. In a modest attempt to encourage the adoption and facilitate the usage of these algorithms, I have developed [libestream](http://github.com/lvella/libestream). It is a [free](http://en.wikipedia.org/wiki/Free_software) pure C library featuring all the eSTREAM software profile algorithms written from ground up based on the specifications. It provides a clean interface directly to the algorithms output and a more general interface that buffers their outputs and apply sequentially to stream chunks of any size.

It also features, for sake of completeness, a partial implementation of [UMAC](http://en.wikipedia.org/wiki/UMAC), a [message authentication code (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code) algorithm, that together with any of the ciphers are sufficient to sign/authenticate the chunks of (or the whole of it) encrypted stream, considering that stream ciphered messages should not be transmitted without a secure authentication method.

*originally posted on the old blog*
