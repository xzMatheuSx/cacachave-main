# Cacachave

Ferramenta para procurar chaves privadas para criptomoedas que utilizam a curva elíptica secp256k1

Postagem: https://bitcointalk.org/index.php?topic=5322040.0

Funciona para Bitcoin
- endereços comprimidos ou não comprimidos
- hashes rmd160 comprimidos ou não comprimidos
- chaves públicas comprimidas ou não comprimidas

Funciona para Ethereum
- endereços

# TL:DR

- Baixar e compilar
- Executar contra o puzzle 66 (modo endereço)

```
./cacachave -m address -f tests/66.txt -b 66 -l compress -R -q -s 10
```

Você precisa adicionar `-t numberThreads` para obter uma melhor velocidade

- Executar contra o Puzzle 125 (modo bsgs)

```
./cacachave -m bsgs -f tests/125.txt -b 125 -q -s 10 -R
```

Você precisa adicionar `-t numberThreads` e `-k factor` para obter uma melhor velocidade

## Código Livre

Este código é gratuito, veja a licença para mais detalhes. https://github.com/albertobsd/keyhunt/blob/main/LICENSE

Embora este projeto seja um hobby para mim, ainda envolve uma quantidade considerável de trabalho.
Se você deseja apoiar este projeto, considere fazer uma doação em https://github.com/albertobsd/keyhunt#donations.


# Aviso Legal

Eu fiz esta ferramenta como uma ferramenta genérica para os Puzzles.
Eu recomendo a todos que permaneçam nos puzzles.

Vários usuários me solicitaram para adicionar suporte ao Ethereum e minikeys, eu o fiz.
Mas, novamente, recomendo usar este programa apenas para puzzles.

## Para usuários regulares

Por favor, leia o CHANGELOG.md para ver as novas alterações.

# Baixar e compilar

Este programa foi feito em um ambiente Linux.
Se você é um usuário de Windows, recomendo fortemente usar o ambiente WSL no Windows.
Ele está disponível na Microsoft Store.

Por favor, instale no seu sistema:

- git
- build-essential

Para a versão legada, você também vai precisar de:

- libssl-dev
- libgmp-dev

Em sistemas baseados em Debian, execute estes comandos para atualizar seu ambiente atual
e instalar as ferramentas necessárias para compilar

```
apt update && apt upgrade
apt install git -y
apt install build-essential -y
apt install libssl-dev -y
apt install libgmp-dev -y
```

Para clonar o repositório:

```
git clone https://github.com/lmajowka/cacachave.git
```

não se esqueça de mudar para o diretório cacachave (Mas eu não estou aqui para ensinar comandos Linux)

```
cd cacachave
```

Primeira compilação:

```
make
```

se você tiver problemas ao compilar a versão `main`, você pode compilar a versão `legacy`

```
make legacy
```

e então executar com `-h` para ver a ajuda

```
./cacachave -h
```

## ¡Beta!

Esta versão ainda é uma versão **beta**, há muitas coisas que podem falhar ou melhorar.
Esta versão também pode ter alguns bugs. por favor, relate-os.

# Modos

cacachave pode trabalhar de diferentes maneiras em diferentes velocidades.

Os modos disponíveis atualmente são:
- address
- rmd160
- xpoint
- bsgs

## Modos experimentais

- minikeys
- pub2rmd

## modo endereço

Esta é a abordagem mais básica para trabalhar, neste modo seu arquivo de texto precisa ter uma lista dos endereços públicos a serem procurados.

Exemplo de endereços de puzzles resolvidos, este arquivo já está no repositório `tests/1to32.txt`

```
1BgGZ9tcN4rm9KBzDn7KprQz87SZ26SAMH
1CUNEBjYrCn2y1SdiUMohaKUi4wpP326Lb
...
```

Para direcionar esse arquivo, precisamos executar o cacachave com esta linha

`./cacachave -m address -f tests/1to32.txt -r 1:FFFFFFFF`

saída:
```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode address
[+] Setting search for btc address
[+] N = 0x100000000
[+] Range
[+] -- from : 0x1
[+] -- to   : 0xffffffff
[+] Allocating memory for 32 elements: 0.00 MB
[+] Bloom filter for 32 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 32 values were loaded and sorted
Base key: 1
Hit! Private Key: 1
pubkey: 0279be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798
Address 1BgGZ9tcN4rm9KBzDn7KprQz87SZ26SAMH
rmd160 751e76e8199196d454941c45d1b3a323f1433bd6

Hit! Private Key: 3
pubkey: 02f9308a019258c31049344f85f89d5229b531c845836f99b08601f113bce036f9
Address 1CUNEBjYrCn2y1SdiUMohaKUi4wpP326Lb
rmd160 7dd65592d0ab2fe0d0257d571abf032cd9db93dc

Hit! Private Key: 7
pubkey: 025cbdf0646e5db4eaa398f365f2ea7a0e3d419b7e0330e39ce92bddedcac4f9bc
Address 19ZewH8Kk1PDbSNdJ97FP4EiCjTRaZMZQA
rmd160 5dedfbf9ea599dd4e3ca6a80b333c472fd0b3f69
(Output omitted)
```

Neste modo, você pode especificar para buscar apenas endereços comprimidos ou não comprimidos com `-l compress` ou `-l uncompress`.

Tente sua sorte com o parâmetro aleatório `-R` contra o puzzle #66

```
./cacachave -m address -f tests/66.txt -b 66 -l compress -R -q -s 10
```

Observe a mudança de `-r 1:FFFFFFFF` para `-b 66`, com -b você pode especificar o intervalo de bits.

saída:
```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode address
[+] Search compress only
[+] Random mode
[+] Quiet thread output
[+] Stats output every 10 seconds
[+] Setting search for btc address
[+] N = 0x100000000
[+] Bit Range 66
[+] -- from : 0x20000000000000000
[+] -- to   : 0x40000000000000000
[+] Allocating memory for 1 elements: 0.00 MB
[+] Bloom filter for 1 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 1 values were loaded and sorted
^C] Total 47634432 keys in 10 seconds: ~4 Mkeys/s (4763443 keys/s)
```

### busca de vanity.

Para buscar apenas um endereço de vanity como `1Good1` ou `1MyKey`, use o seguinte comando:

comando completo

```
./cacachave -m vanity -l compress -R -b 256 -v 1Good1 -v 1MyKey
```

saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode vanity
[+] Search compress only
[+] Random mode
[+] Added Vanity search : 1Good1
[+] Added Vanity search : 1MyKey
[+] N = 0x100000000
[+] Bit Range 256
[+] -- from : 0x8000000000000000000000000000000000000000000000000000000000000000
[+] -- to   : 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141
[+] Bloom filter for 4 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
Base key: a5247120867e8d57b8908b0d962df84a924cba7f684903e2c942574353520a03
Vanity Private Key: 5adb8edf798172a8476f74f269d207b42862226746ff9c58f69007497c0d7516
pubkey: 0273267f9764b022bb462b359a12917dbb3568e4e6cd3aa2e846b8c1d9cae0363a
Address 1Good1mjxXjNqb8TucvKjyCuZfihMZgFcc


rmd160 ad63f02cb68254ce12982e5e312bd51e8a239a84
```

comando para buscar múltiplos endereços de vanity a partir de um arquivo `-f filename.txt`.

```
./cacachave -m vanity -f ~/main/keyhunt/vanitytargets.txt -l compress -R -b 256 -e -s 10 -q 
```

Saída:
```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode vanity
[+] Search compress only
[+] Random mode
[+] Endomorphism enabled
[+] Stats output every 10 seconds
[+] Quiet thread output
[+] N = 0x100000000
[+] Bit Range 256
[+] -- from : 0x8000000000000000000000000000000000000000000000000000000000000000
[+] -- to   : 0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141
[+] Bloom filter for 225 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
^C] Total 58202112 keys in 10 seconds: ~5 Mkeys/s (5820211 keys/s)
```

Todos os endereços de vanity e suas chaves privadas serão salvos no arquivo `VANITYKEYFOUND.txt` do seu diretório atual.


## modo rmd160

rmd significa RIPE Message Digest (veja https://en.wikipedia.org/wiki/RIPEMD)

o modo rmd160 funciona da mesma forma que o endereço, mas a diferença é que o arquivo precisa ter hashes rmd160 em vez de endereços.


exemplo de arquivo `tests/1to32.rmd`:

```
751e76e8199196d454941c45d1b3a323f1433bd6
7dd65592d0ab2fe0d0257d571abf032cd9db93dc
5dedfbf9ea599dd4e3ca6a80b333c472fd0b3f69
9652d86bedf43ad264362e6e6eba6eb764508127
...
```

para direcionar esse arquivo você precisa executar a próxima linha:

```
./cacachave -m rmd160 -f tests/1to32.rmd -r 1:FFFFFFFF -l compress -s 5
```

saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode rmd160
[+] Search compress only
[+] N = 0x100000000
[+] Range
[+] -- from : 0x1
[+] -- to   : 0xffffffff
[+] Allocating memory for 32 elements: 0.00 MB
[+] Bloom filter for 32 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 32 values were loaded and sorted
Base key: 1
Hit! Private Key: 1
pubkey: 0279be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798
Address 1BgGZ9tcN4rm9KBzDn7KprQz87SZ26SAMH
rmd160 751e76e8199196d454941c45d1b3a323f1433bd6

Hit! Private Key: 3
pubkey: 02f9308a019258c31049344f85f89d5229b531c845836f99b08601f113bce036f9
Address 1CUNEBjYrCn2y1SdiUMohaKUi4wpP326Lb
rmd160 7dd65592d0ab2fe0d0257d571abf032cd9db93dc
(Output omitted)
```

tente sua sorte com o próximo arquivo para o puzzle #66


```
./cacachave -m rmd160 -f tests/66.rmd -b 66 -l compress -R -q
```

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode rmd160
[+] Search compress only
[+] Random mode
[+] Quiet thread output
[+] Stats output every 5 seconds
[+] N = 0x100000000
[+] Bit Range 66
[+] -- from : 0x20000000000000000
[+] -- to   : 0x40000000000000000
[+] Allocating memory for 1 elements: 0.00 MB
[+] Bloom filter for 1 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 1 values were loaded and sorted
^C] Total 70844416 keys in 15 seconds: ~4 Mkeys/s (4722961 keys/s)
```

## modo xpoint

Este método pode direcionar o valor X da chave pública da mesma forma que a ferramenta procura endereços ou hash rmd160, esta ferramenta pode procurar pelos valores X.

A velocidade deste método é melhor do que a velocidade para endereços ou rmd160.

O arquivo de entrada pode ter uma chave pública por linha, comprimida ou não comprimida:

- Chave pública comprimida (66 caracteres hexadecimais)
- Chave pública não comprimida (130 caracteres hexadecimais)

Exemplo de arquivo de entrada:

Alguns valores subtraídos do puzzle *40*

```
034eee474fe724cb631d19f24934e88016e4ef2aee80d086621d87d7f6066ff860 # - 453856235784
0274241b684e7c31e7933510b510aa14de9ac88ec3635bdd35a3bcf1d16da210be # + 453856235784
03abc6aff092b9a64bf69e00f4ec7a8b7ca51cfc6656732cbbc9f5674925b88609 # - 529328067324
034f4fe33b02c202b732d278f90eedc635af6f3be8a93c8d1cb0a01f6399aab2a4 # + 529328067324
03716ff57705e6446ac3e217c8c8bd9e9c8e58547457a6fe93ac254c37fd48afcb # - 14711740067
02ffa0769b0459c64b41f59f93495063ae031de0b846180bee37f921f20e141f60 # + 14711740067
03de1df5d801bbd5e7d86577bf14950f732fd41e586945d06d19e0fdea41a37d62 # - 549755814000
038d3711fd681e26c05b2f0cd423fa596e15054024e40add24a93bfa0c630531f1 # + 549755814000
03a2efa402fd5268400c77c20e574ba86409ededee7c4020e4b9f0edbee53de0d4 # target
```


Agora você pode usar cacachave contra alguns milhares de valores do puzzle 40:

```./cacachave -m xpoint -f tests/substracted40.txt -n 65536 -t 4 -b 40```

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode xpoint
[+] Threads : 4
[+] N = 0x10000
[+] Bit Range 40
[+] -- from : 0x8000000000
[+] -- to   : 0x10000000000
[+] Allocating memory for 6003 elements: 0.11 MB
[+] Bloom filter for 6003 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 6003 values were loaded and sorted
Base key: 80025b0000
Hit! Private Key: 800258a2ce
pubkey: 0474241b684e7c31e7933510b510aa14de9ac88ec3635bdd35a3bcf1d16da210be7ad946c9b185433fff3a7824ee140b15789d5f12d60cd2814154b0f8f1a4308e
Address 1CMg4mukBGVvid4ocTx5x5LEuCatKoHQRB
rmd160 7c92500fa9d2ecbca5bdd61bb6a14a249669bae4
```

Depois do acerto, precisamos procurar o índice subtraído e fazer uma operação matemática simples para obter a chave privada real:

```
0274241b684e7c31e7933510b510aa14de9ac88ec3635bdd35a3bcf1d16da

210be # + 453856235784
```
A operação é `800258a2ce` hex (+/-) neste caso + `453856235784` decimal que é igual a `E9AE4933D6`.

Este é um exemplo fácil, eu tentei o puzzle 120 com mais de 500 milhões de chaves subtraídas e sem sorte.

Teste sua sorte com o puzzle 120 com xpoint:

```./cacachave -m xpoint -f tests/120.txt -t 4 -b 125 -R -q```

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode xpoint
[+] Threads : 4
[+] Random mode
[+] Quiet thread output
[+] N = 0x100000000
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] Allocating memory for 1 elements: 0.00 MB
[+] Bloom filter for 1 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 1 values were loaded and sorted
^C] Total 462885888 keys in 30 seconds: ~15 Mkeys/s (15429529 keys/s)
```

## Endomorfismo

Para habilitar o endomorfismo use `-e`.

O endomorfismo é habilitado apenas para `address`, `rdm160`, `vanity` e `xpoint`.

Mas o que diabos é `Endomorfismo`?

Em poucas palavras, para curvas elípticas, um endomorfismo é uma função que mapeia pontos na curva para outros pontos na mesma curva.

Um tipo de Endomorfismo é a negação do ponto, por exemplo, a chave privada do puzzle 64

000000000000000000000000000000000000000000000000f7051f27b09112d4 chave pública 03100611c54dfef604163b8358f7b7fac13ce478e02cb224ae16d45526b25d9d4d
se negarmos essa chave privada ou chave pública, obtemos:

fffffffffffffffffffffffffffffffebaaedce6af48a03ac8cd3f651fa52e6d chave pública 02100611c54dfef604163b8358f7b7fac13ce478e02cb224ae16d45526b25d9d4d

Mas se negarmos este último valor, obtemos novamente o primeiro valor.

Existem alguns valores especiais lambda e beta

```
lambda = 0x5363ad4cc05c30e0a5261c028812645a122e22ea20816678df02967c1b23bd72
beta = 0x7ae96a2b657c07106e64479eac3434e99cf0497512f58995c1396c28719501ee
```

Para cada chave privada K e seu Ponto Q onde Q = GK

Q = (x,y)

Podemos multiplicar Q por lambda, exemplo:

```
Q * lambda = (x * beta mod p , y)
Q*lambda é uma Multiplicação Escalar
x*beta é apenas uma Multiplicação (Muito rápido)
```

p é 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEFFFFFC2F

Exemplo do puzzle 64

0x000000000000000000000000000000000000000000000000f7051f27b09112d4 chave pública 03100611c54dfef604163b8358f7b7fac13ce478e02cb224ae16d45526b25d9d4d
0x2924e3e5ac18fd894504878d4fd1820e71bd63cd9b15d69511926e5f05d99d3a chave pública 03792bfa55bf659967951b21060c05c250cd261ec3ea02704815bfb1c5ccc800fd
0xd6db1c1a53e70276bafb7872b02e7df048f179191432c9a5b73ad10619cb9133 chave pública 0376cdf3e4f29b709454a95ba0fc4242edf5f5685be94b6b09d36bf91280da5de5

prova

```
~/ecctools/modmath 0xf7051f27b09112d4 x 0x5363ad4cc05c30e0a5261c028812645a122e22ea20816678df02967c1b23bd72
Result: 0x2924e3e5ac18fd894504878d4fd1820e71bd63cd9b15d69511926e5f05d99d3a

~/ecctools/modmath 0x2924e3e5ac18fd894504878d4fd1820e71bd63cd9b15d69511926e5f05d99d3a x 0x5363ad4cc05c30e0a5261c028812645a122e22ea20816678df02967c1b23bd72
Result: 0xd6db1c1a53e70276bafb7872b02e7df048f179191432c9a5b73ad10619cb9133
```

mas se multiplicarmos 0xd6db1c1a53e70276bafb7872b02e7df048f179191432c9a5b73ad10619cb9133 novamente por lambda

```
~/ecctools/modmath 0xd6db1c1a53e70276bafb7872b02e7df048f179191432c9a5b73ad10619cb9133 x 0x5363ad4cc05c30e0a5261c028812645a122e22ea20816678df02967c1b23bd72
Result: 0xf7051f27b09112d4
```

O que diabos?? Voltamos à primeira chave, uau!!

Portanto, para cada chave que calculamos, podemos obter facilmente 6 valores mais. Esses valores são:

```
Ponto original, Ponto original negado
Ponto original * lambda, Ponto original * lambda negado
Ponto original * lambda^2, Ponto original * lambda^2 negado
```

Obviamente, precisamos fazer as operações com o valor X multiplicado por *beta* porque é mais rápido e vamos obter o mesmo resultado.

*Endomorfismo não funciona para puzzles porque 5 dessas 6 chaves estão fora do intervalo*.

Eu adicionei endomorfismo para acelerar o processo de busca de vanity, mas o estendi para todos os outros métodos mencionados antes.


## modo pub2rmd

Este método foi feito para tentar obter a chave pública dos puzzles, funciona um pouco mais rápido porque pula as operações EC.

O arquivo de entrada precisa ter o hash RMD160 do endereço sem chave pública vazada:

```
3ee4133d991f52fdf6a25c9834e0745ac74248a4
20d45a6a762535700ce9e0b216e31994335db8a5
739437bb3dd6d1983e66629c5f08c70e52769371
e0b8a2baee1b77fc703455f39d51477451fc8cfc
61eb8a50c86b0584bb727dd65bed8d2400d6d5aa
f6f5431d25bbf7b12e8add9af5e3475c44a0a5b8
bf7413e8df4e7a34ce9dc13e2f2648783ec54adb
105b7f253f0ebd7843adaebbd805c944bfb863e4
9f1adb20baeacc38b3f49f3df6906a0e48f2df3d
86f9fea5cdecf033161dd2f8f8560768ae0a6d14
783c138ac81f6a52398564bb17455576e8525b29
35003c3ef8759c92092f8488fca59a042859018c
67671d5490c272e3ab7ddd34030d587738df33da
351e605fac813965951ba433b7c2956bf8ad95ce
20d28d4e87543947c7e4913bcdceaa16e2f8f061
24cef184714bbd030833904f5265c9c3e12a95a2
7c99ce73e19f9fbfcce4825ae88261e2b0b0b040
c60111ed3d63b49665747b0e31eb382da5193535
fbc708d671c03e26661b9c

08f77598a529858b5e
38a968fdfb457654c51bcfc4f9174d6ee487bb41
5c3862203d1e44ab3af441503e22db97b1c5097e
9978f61b92d16c5f1a463a0995df70da1f7a7d2a
6534b31208fe6e100d29f9c9c75aac8bf06fbb38
463013cd41279f2fd0c31d0a16db3972bfffac8d
c6927a00970d0165327d0a6db7950f05720c295c
2da63cbd251d23c7b633cb287c09e6cf888b3fe4
578d94dc6f40fff35f91f6fba9b71c46b361dff2
7eefddd979a1d6bb6f29757a1f463579770ba566
c01bf430a97cbcdaedddba87ef4ea21c456cebdb
```

Para direcionar esse arquivo, você precisa fazer:

```./cacachave -m pub2rmd -f tests/puzzleswopublickey.txt -t 6 -q```

Saída:

```
[+] Version 0.2.211007 Chocolate ¡Beta!
[+] Mode pub2rmd
[+] Threads : 6
[+] Quiet thread output
[+] Opening file tests/puzzleswopublickey.txt
[+] Allocating memory for 29 elements: 0.00 MB
[+] Bloom filter for 29 elements.
[+] Loading data to the bloomfilter total: 0.00 MB
[+] Bloomfilter completed
[+] Sorting data ... done! 29 values were loaded and sorted
[+] Total 207618048 keys in 60 seconds: ~3 Mkeys/s (3460300 keys/s)
```

Você pode deixá-lo rodar por um tempo junto com outros scripts, se você conseguir uma dessas chaves públicas agora, você pode direcioná-la com um método melhor, como bsgs ou outras ferramentas, como kangaroo.


## modo bsgs (baby step giant step)

cacachave implementa o algoritmo BSGS para buscar chaves privadas para uma chave pública conhecida.

O arquivo de entrada precisa ter uma lista de chaves públicas, comprimidas ou não comprimidas, essas chaves públicas podem ser misturadas no mesmo arquivo, uma chave pública por linha e qualquer outra palavra seguida de um espaço é ignorada. Exemplo do arquivo:

```
043ffa1cc011a8d23dec502c7656fb3f93dbe4c61f91fd443ba444b4ec2dd8e6f0406c36edf3d8a0dfaa7b8f309b8f1276a5c04131762c23594f130a023742bdde # 0000000000000000000000000000000000800000000000000000100000000000
046534b9e9d56624f5850198f6ac462f482fec8a60262728ee79a91cac1d60f8d6a92d5131a20f78e26726a63d212158b20b14c3025ebb9968c890c4bab90bfc69 # 0000000000000000000000000000000000800000000000000000200000000000
```

Este exemplo contém 2 chaves públicas seguidas de sua chave privada apenas para testar o comportamento correto da aplicação.

*Não carregue mais de 100 ou 1000 chaves públicas* se você carregar mais do que isso, levará muito tempo para atualizar o contador de velocidade e a velocidade será muito baixa.

Aliás, qualquer palavra seguida de um espaço após a chave pública é ignorada, o arquivo pode ser apenas as chaves públicas:

```
043ffa1cc011a8d23dec502c7656fb3f93dbe4c61f91fd443ba444b4ec2dd8e6f0406c36edf3d8a0dfaa7b8f309b8f1276a5c04131762c23594f130a023742bdde
046534b9e9d56624f5850198f6ac462f482fec8a60262728ee79a91cac1d60f8d6a92d5131a20f78e26726a63d212158b20b14c3025ebb9968c890c4bab90bfc69
```

### Criação de Arquivo

o modo bsgs `-m bsgs` agora pode criar automaticamente os arquivos necessários para acelerar o processo de carregamento inicial do cacachave, isso inclui a criação de filtros bloom e da tabela bp.

Para solicitar ao cacachave a criação desses arquivos automaticamente, use `-S` (S maiúsculo) para SALVAR e LER arquivos.
Os 3 arquivos necessários para o cacachave podem variar de tamanho dependendo dos seus valores de `-n` e `-k`, então faça seu teste e fique com uma combinação de valores (n, k) ou você pode acabar com centenas de arquivos desnecessários.

O tamanho dos 3 arquivos é igual à quantidade de memória usada em tempo de execução.

Os arquivos são criados se não existirem quando você executar o programa pela primeira vez.

exemplo de criação de arquivo:

```
./cacachave -m bsgs -f tests/125.txt -R -b 125 -q -S -s 10
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Random mode
[+] Quiet thread output
[+] Stats output every 10 seconds
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 4194304 elements : 14.38 MB
[+] Bloom filter for 131072 elements : 0.88 MB
[+] Bloom filter for 4096 elements : 0.88 MB
[+] Allocating 0.00 MB for 4096 bP Points
[+] processing 4194304/4194304 bP points : 100%
[+] Making checkums .. ... done
[+] Sorting 4096 elements... Done!
[+] Writing bloom filter to file cacachave_bsgs_4_4194304.blm .... Done!
[+] Writing bloom filter to file keyhunt_bsgs_6_131072.blm .... Done!
[+] Writing bP Table to file keyhunt_bsgs_2_4096.tbl .. Done!
[+] Writing bloom filter to file keyhunt_bsgs_7_4096.blm .... Done!
^C] Total 457396837154816 keys in 30 seconds: ~15 Tkeys/s (15246561238493 keys/s)
```

Quando executamos o programa pela segunda vez, os arquivos agora são lidos e o processamento dos pontos bP é omitido:

```
./cacachave -m bsgs -f tests/125.txt -R -b 125 -q -S -s 10
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Random mode
[+] Quiet thread output
[+] Stats output every 10 seconds
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 4194304 elements : 14.38 MB
[+] Bloom filter for 131072 elements : 0.88 MB
[+] Bloom filter for 4096 elements : 0.88 MB
[+] Allocating 0.00 MB for 4096 bP Points
[+] Reading bloom filter from file keyhunt_bsgs_4_4194304.blm .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_6_131072.blm .... Done!
[+] Reading bP Table from file keyhunt_bsgs_2_4096.tbl .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_7_4096.blm .... Done!
^C
```

Todos os próximos exemplos foram feitos com a opção `-S`. Eu apenas omito essa parte da saída para evitar confusões. Use `-S` se desejar, mas lembre-se, com um grande `-n` devem vir grandes arquivos.

### Exemplos

Para tentar encontrar essas chaves privadas, esta é a linha de execução:

```
time ./cacachave -m bsgs -f tests/test120.txt -b 120 -S
```

Saída:

```
time ./cacachave -m bsgs -f tests/test120.txt -b 120 -S
[+] Version 0

.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode BSGS secuential
[+] Opening file tests/test120.txt
[+] Added 2 points from file
[+] Bit Range 120
[+] -- from : 0x800000000000000000000000000000
[+] -- to   : 0x1000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 4194304 elements : 14.38 MB
[+] Bloom filter for 131072 elements : 0.88 MB
[+] Bloom filter for 4096 elements : 0.88 MB
[+] Allocating 0.00 MB for 4096 bP Points
[+] Reading bloom filter from file keyhunt_bsgs_4_4194304.blm .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_6_131072.blm .... Done!
[+] Reading bP Table from file keyhunt_bsgs_2_4096.tbl .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_7_4096.blm .... Done!
[+] Thread Key found privkey 800000000000000000100000000000
[+] Publickey 043ffa1cc011a8d23dec502c7656fb3f93dbe4c61f91fd443ba444b4ec2dd8e6f0406c36edf3d8a0dfaa7b8f309b8f1276a5c04131762c23594f130a023742bdde
[+] Thread Key found privkey 800000000000000000200000000000
[+] Publickey 046534b9e9d56624f5850198f6ac462f482fec8a60262728ee79a91cac1d60f8d6a92d5131a20f78e26726a63d212158b20b14c3025ebb9968c890c4bab90bfc69
All points were found

real    0m3.632s
user    0m3.619s
sys     0m0.000s
```

Teste o puzzle 120 com a próxima chave pública:

```
0233709eb11e0d4439a729f21c2c443dedb727528229713f0065721ba8fa46f00e
```

Linha de execução em modo aleatório `-R` ou -B random

```./cacachave -m bsgs -f tests/125.txt -b 125 -q -s 10 -R```

```./cacachave -m bsgs -f tests/125.txt -b 125 -q -s 10 -B random```


Exemplo de Saída:

```
[+] Version 0.2.230507 Satoshi Quest, developed by AlbertoBSD
[+] Quiet thread output
[+] Stats output every 10 seconds
[+] Random mode
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 4194304 elements : 14.38 MB
[+] Bloom filter for 131072 elements : 0.88 MB
[+] Bloom filter for 4096 elements : 0.88 MB
[+] Allocating 0.00 MB for 4096 bP Points
[+] processing 4194304/4194304 bP points : 100%
[+] Making checkums .. ... done
[+] Sorting 4096 elements... Done!
[+] Total 158329674399744 keys in 10 seconds: ~15 Tkeys/s (15832967439974 keys/s)
```

Boa velocidade, não? 15 Terakeys/s para um único thread.

**^C] Total 158329674399744 keys in 10 seconds: ~15 Tkeys/s (15832967439974 keys/s)**

Podemos acelerar nosso processo selecionando um valor K maior `-k valor`, btw o valor n é o comprimento total do item testado no intervalo aleatório, um valor k maior significa mais RAM para ser usada:

Exemplo:
```
./cacachave -m bsgs -f tests/125.txt -b 125 -R -k 20 -S
```

Saída:

```
./cacachave -m bsgs -f tests/125.txt -b 125 -R -k 20 -S
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Random mode
[+] K factor 20
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0xfffff000000
[+] Bloom filter for 83886080 elements : 287.55 MB
[+] Bloom filter for 2621440 elements : 8.99 MB
[+] Bloom filter for 81920 elements : 0.88 MB
[+] Allocating 1.00 MB for 81920 bP Points
[+] processing 83886080/83886080 bP points : 100%
[+] Making checkums .. ... done
[+] Sorting 81920 elements... Done!
[+] Writing bloom filter to file keyhunt_bsgs_4_83886080.blm .... Done!
[+] Writing bloom filter to file keyhunt_bsgs_6_2621440.blm .... Done!
[+] Writing bP Table to file keyhunt_bsgs_2_81920.tbl .. Done!
[+] Writing bloom filter to file keyhunt_bsgs_7_81920.blm .... Done!
^C] Thread 0x1bbb290563ffcf38724482a45f2bed04  ~256 Tkeys/s (256259265658880 keys/s)
```

**~256 Terakeys/s para um único thread**

Observe o valor de N `0xfffff000000` com k = 20, isso significa que o valor N é menor que o valor padrão `0x100000000000`, isso porque k não é um número 2^X.

se você quiser mais velocidade, use um valor k maior, como 128, isso usará cerca de 2 GB de RAM.


```
./cacachave -m bsgs -f tests/125.txt -b 125 -R -k 128 -S
```

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Random mode
[+] K factor 128
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] -- from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 536870912 elements : 1840.33 MB
[+] Bloom filter for 16777216 elements : 57.51 MB
[+] Bloom filter for 524288 elements : 1.80 MB
[+] Allocating 8.00 MB for 524288 bP Points
[+] Reading bloom filter from file keyhunt_bsgs_4_536870912.blm .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_6_16777216.blm .... Done!
[+] Reading bP Table from file keyhunt_bsgs_2_524288.tbl .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_7_524288.blm .... Done!
^C] Thread 0x1d0e05e7aaf9eca861fe0b2245579241   ~1 Pkeys/s (1292439268063095 keys/s)
```

**~1.2 Pkeys/s para um único thread**

OK, neste ponto, talvez você queira usar TODA a sua memória RAM para resolver o puzzle 125, apenas um valor k maior.

Eu já testei com cerca de **8 GB** usados com `-k 512` e obtive **~46 Petakeys/s por thread**.

com **8** threads

`./cacachave -m bsgs -f tests/125.txt -b 125 -R -k 512 -q -t 8 -s 10 -S`

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Random mode
[+] K factor 512
[+] Quiet thread output
[+] Threads : 8
[+] Stats output every 10 seconds
[+] Mode BSGS random
[+] Opening file tests/125.txt
[+] Added 1 points from file
[+] Bit Range 125
[+] --

 from : 0x10000000000000000000000000000000
[+] -- to   : 0x20000000000000000000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 2147483648 elements : 7361.33 MB
[+] Bloom filter for 67108864 elements : 230.04 MB
[+] Bloom filter for 2097152 elements : 7.19 MB
[+] Allocating 32.00 MB for 2097152 bP Points
[+] Reading bloom filter from file keyhunt_bsgs_4_2147483648.blm .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_6_67108864.blm .... Done!
[+] Reading bP Table from file keyhunt_bsgs_2_2097152.tbl .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_7_2097152.blm .... Done!
^C] Total 2126103644397895680 keys in 110 seconds: ~19 Pkeys/s (19328214949071778 keys/s)
```
Eu consegui ~19 Petakeys/s no total.

Aviso: o valor n padrão tem um valor máximo de K de `4096`, se esse valor for excedido, o programa pode ter um comportamento desconhecido ou velocidade subótima.
Se você quiser usar um K maior, recomendo usar um valor N maior `-n 0x400000000000` e diminuir pela metade o seu valor K.

Apenas para comparação com o programa BSGS de JLP
Mesmas chaves públicas e intervalo usados pelo seu exemplo:

chaves públicas:
```
0459A3BFDAD718C9D3FAC7C187F1139F0815AC5D923910D516E186AFDA28B221DC994327554CED887AAE5D211A2407CDD025CFC3779ECB9C9D7F2F1A1DDF3E9FF8
04A50FBBB20757CC0E9C41C49DD9DF261646EE7936272F3F68C740C9DA50D42BCD3E48440249D6BC78BC928AA52B1921E9690EBA823CBC7F3AF54B3707E6A73F34
0404A49211C0FE07C9F7C94695996F8826E09545375A3CF9677F2D780A3EB70DE3BD05357CAF8340CB041B1D46C5BB6B88CD9859A083B0804EF63D498B29D31DD1
040B39E3F26AF294502A5BE708BB87AEDD9F895868011E60C1D2ABFCA202CD7A4D1D18283AF49556CF33E1EA71A16B2D0E31EE7179D88BE7F6AA0A7C5498E5D97F
04837A31977A73A630C436E680915934A58B8C76EB9B57A42C3C717689BE8C0493E46726DE04352832790FD1C99D9DDC2EE8A96E50CAD4DCC3AF1BFB82D51F2494
040ECDB6359D41D2FD37628C718DDA9BE30E65801A88A00C3C5BDF36E7EE6ADBBAD71A2A535FCB54D56913E7F37D8103BA33ED6441D019D0922AC363FCC792C29A
0422DD52FCFA3A4384F0AFF199D019E481D335923D8C00BADAD42FFFC80AF8FCF038F139D652842243FC841E7C5B3E477D901F88C5AB0B88EE13D80080E413F2ED
04DB4F1B249406B8BD662F78CBA46F5E90E20FE27FC69D0FBAA2F06E6E50E536695DF83B68FD0F396BB9BFCF6D4FE312F32A43CF3FA1FE0F81DF70C877593B64E0
043BD0330D7381917F8860F1949ACBCCFDC7863422EEE2B6DB7EDD551850196687528B6D2BC0AA7A5855D168B26C6BAF9DDCD04B585D42C7B9913F60421716D37A
04332A02CA42C481EAADB7ADB97DF89033B23EA291FDA809BEA3CE5C3B73B20C49C410D1AD42A9247EB8FF217935C9E28411A08B325FBF28CC2AF8182CE2B5CE38
04513981849DE1A1327DEF34B51F5011C5070603CA22E6D868263CB7C908525F0C19EBA6BD2A8DCF651E4342512EDEACB6EA22DA323A194E25C6A1614ABD259BC0
04D4E6FA664BD75A508C0FF0ED6F2C52DA2ADD7C3F954D9C346D24318DBD2ECFC6805511F46262E10A25F252FD525AF1CBCC46016B6CD0A7705037364309198DA1
0456B468963752924DBF56112633DC57F07C512E3671A16CD7375C58469164599D1E04011D3E9004466C814B144A9BCB7E47D5BACA1B90DA0C4752603781BF5873
04D5BE7C653773CEE06A238020E953CFCD0F22BE2D045C6E5B4388A3F11B4586CBB4B177DFFD111F6A15A453009B568E95798B0227B60D8BEAC98AF671F31B0E2B
04B1985389D8AB680DEDD67BBA7CA781D1A9E6E5974AAD2E70518125BAD5783EB5355F46E927A030DB14CF8D3940C1BED7FB80624B32B349AB5A05226AF15A2228
0455B95BEF84A6045A505D015EF15E136E0A31CC2AA00FA4BCA62E5DF215EE981B3B4D6BCE33718DC6CF59F28B550648D7E8B2796AC36F25FF0C01F8BC42A16FD9
```

definir intervalo

```
-r 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e0000000000000000:49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5effffffffffffffff
```

o valor n para obter a mesma tabela de baby step:


```
-n 1152921504606846976
```

número de threads

```
-t 6
```

Ocultando a velocidade:

```
-s 0
```

comando:

```
time ./cacachave -m bsgs -t 6 -f tests/in.txt -r 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e0000000000000000:49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5effffffffffffffff -n 0x1000000000000000 -M -s 0
```

Saída:
```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Threads : 6
[+] Matrix screen
[+] Turn off stats output
[+] Mode BSGS secuential
[+] Opening file tests/in.txt
[+] Added 16 points from file
[+] Range
[+] -- from : 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e0000000000000000
[+] -- to   : 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5effffffffffffffff
[+] N = 0x1000000000000000
[+] Bloom filter for 1073741824 elements : 3680.00 MB
[+] Bloom filter for 53687092 elements : 184.03 MB
[+] Allocating 819.00 MB for 53687092 bP Points


[+] processing 1073741824/1073741824 bP points : 100%
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e0000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e2000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e1000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e3000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e4000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e5000000000000000
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e5698aaab6cac52b3
[+] Publickey 0404a49211c0fe07c9f7c94695996f8826e09545375a3cf9677f2d780a3eb70de3bd05357caf8340cb041b1d46c5bb6b88cd9859a083b0804ef63d498b29d31dd1
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e59c839258c2ad7a0
[+] Publickey 040b39e3f26af294502a5be708bb87aedd9f895868011e60c1d2abfca202cd7a4d1d18283af49556cf33e1ea71a16b2d0e31ee7179d88be7f6aa0a7c5498e5d97f
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e38160da9ebeaecd7
[+] Publickey 04db4f1b249406b8bd662f78cba46f5e90e20fe27fc69d0fbaa2f06e6e50e536695df83b68fd0f396bb9bfcf6d4fe312f32a43cf3fa1fe0f81df70c877593b64e0
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e54cad3cfbc2a9c2b
[+] Publickey 04332a02ca42c481eaadb7adb97df89033b23ea291fda809bea3ce5c3b73b20c49c410d1ad42a9247eb8ff217935c9e28411a08b325fbf28cc2af8182ce2b5ce38
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e0d5eccc38d0230e6
[+] Publickey 04513981849de1a1327def34b51f5011c5070603ca22e6d868263cb7c908525f0c19eba6bd2a8dcf651e4342512edeacb6ea22da323a194e25c6a1614abd259bc0
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e2452dd26bc983cd5
[+] Publickey 04b1985389d8ab680dedd67bba7ca781d1a9e6e5974aad2e70518125bad5783eb5355f46e927a030db14cf8d3940c1bed7fb80624b32b349ab5a05226af15a2228
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e6000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e7000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e8000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e9000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ea000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5eb000000000000000
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ebb3ef3883c1866d4
[+] Publickey 0459a3bfdad718c9d3fac7c187f1139f0815ac5d923910d516e186afda28b221dc994327554ced887aae5d211a2407cdd025cfc3779ecb9c9d7f2f1a1ddf3e9ff8
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5eb5abc43bebad3207
[+] Publickey 04a50fbbb20757cc0e9c41c49dd9df261646ee7936272f3f68c740c9da50d42bcd3e48440249d6bc78bc928aa52b1921e9690eba823cbc7f3af54b3707e6a73f34
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e765fb411e63b92b9
[+] Publickey 04837a31977a73a630c436e680915934a58b8c76eb9b57a42c3c717689be8c0493e46726de04352832790fd1c99d9ddc2ee8a96e50cad4dcc3af1bfb82d51f2494
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e7d0e6081c7e0e865
[+] Publickey 040ecdb6359d41d2fd37628c718dda9be30e65801a88a00c3c5bdf36e7ee6adbbad71a2a535fcb54d56913e7f37d8103ba33ed6441d019d0922ac363fcc792c29a
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e79d808cab1decf8d
[+] Publickey 043bd0330d7381917f8860f1949acbccfdc7863422eee2b6db7edd551850196687528b6d2bc0aa7a5855d168b26c6baf9ddcd04b585d42c7b9913f60421716d37a
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e7c43b8e079ae7278
[+] Publickey 0456b468963752924dbf56112633dc57f07c512e3671a16cd7375c58469164599d1e04011d3e9004466c814b144a9bcb7e47d5baca1b90da0c4752603781

bf5873
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e8d63ef128ef66b42
[+] Publickey 04d5be7c653773cee06a238020e953cfcd0f22be2d045c6e5b4388a3f11b4586cbb4b177dffd111f6a15a453009b568e95798b0227b60d8beac98af671f31b0e2b
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5e7ad38337c7f173c7
[+] Publickey 0455b95bef84a6045a505d015ef15e136e0a31cc2aa00fa4bca62e5df215ee981b3b4d6bce33718dc6cf59f28b550648d7e8b2796ac36f25ff0c01f8bc42a16fd9
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ec000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ed000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ee000000000000000
[+] Thread 0x49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ef000000000000000
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ec737344ca673ce28
[+] Publickey 0422dd52fcfa3a4384f0aff199d019e481d335923d8c00badad42fffc80af8fcf038f139d652842243fc841e7c5b3e477d901f88c5ab0b88ee13d80080e413f2ed
[+] Thread Key found privkey 49dccfd96dc5df56487436f5a1b18c4f5d34f65ddb48cb5ee3579364de939b0c
[+] Publickey 04d4e6fa664bd75a508c0ff0ed6f2c52da2add7c3f954d9c346d24318dbd2ecfc6805511f46262e10a25f252fd525af1cbcc46016b6cd0a7705037364309198da1
All points were found

real    164m32.904s
user    973m8.387s
sys     0m26.803s
```

Quantidade de RAM usada ~4,5 GB, tempo para resolver as dezesseis chaves públicas no intervalo de chave de 64 bits: 164 min (~2,7 hrs) usando 6 threads.

se executarmos o mesmo comando com `-n 0x1000000000000000 -k 4 -t 6`, ele usa ~18 GB de RAM e resolve as mesmas chaves em 60 minutos.

```
All points were found

real    59m50.533s
user    329m29.836s
sys     0m22.752s
```

Existem várias variações para brincar com os valores `-n` e `-k`, mas existem alguns valores mínimos necessários, n não pode ser menor que 1048576 (2^20).

Para obter um desempenho ideal, os valores de k precisam ser valores base 2^x, isso é 1,2,4,8,16,32...

### Valores válidos de n e valores máximos de k para específico

```
+------+----------------------+-------------+
| bits |  n em hexadecimal    | valor max k |
+------+----------------------+-------------+
|   20 |             0x100000 | 1 (padrão)  |
|   22 |             0x400000 | 2           |
|   24 |            0x1000000 | 4           |
|   26 |            0x4000000 | 8           |
|   28 |           0x10000000 | 16          |
|   30 |           0x40000000 | 32          |
|   32 |          0x100000000 | 64          |
|   34 |          0x400000000 | 128         |
|   36 |         0x1000000000 | 256         |
|   38 |         0x4000000000 | 512         |
|   40 |        0x10000000000 | 1024        |
|   42 |        0x40000000000 | 2048        |
|   44 |       0x100000000000 | 4096        |
|   46 |       0x400000000000 | 8192        |
|   48 |      0x1000000000000 | 16384       |
|   50 |      0x4000000000000 | 32768       |
|   52 |     0x10000000000000 | 65536       |
|   54 |     0x40000000000000 | 131072      |
|   56 |    0x100000000000000 | 262144      |
|   58 |    0x400000000000000 | 524288      |
|   60 |   0x1000000000000000 | 1048576     |
|   62 |   0x4000000000000000 | 2097152     |
|   64 |  0x10000000000000000 | 4194304     |
+------+----------------------+-------------+
```

**Se você exceder o valor máximo de K, o programa pode ter um comportamento desconhecido, o programa pode ter um desempenho subótimo ou, nos casos errados, você pode perder alguns acertos e ter uma VELOCIDADE incorreta.**

Nota para o usuário que deseja usá-lo com memória SWAP. NÃO FUNCIONA com memória Swap, foi feito para pequenos pedaços de memória, além de ser lento.

### Quais valores usar de acordo com minha RAM atual:

2 G
-k 128

4 G
-k 256

8 GB
-k 512

16 GB
-k 1024

32 GB
-k 2048

64 GB
-n 0x100000000000 -k 4096

128 GB
-n 0x400000000000 -k 4096

256 GB
-n 0x400000000000 -k 8192

512 GB
-n 0x1000000000000 -k 8192

1 TB
-n 0x1000000000000 -k 16384

2 TB
-n 0x4000000000000 -k 16384

4 TB
-n 0x4000000000000 -k 32768

8 TB
-n 0x10000000000000 -k 32768


### Testando o puzzle 63 bits

Chave pública:

```
0365ec2994b8cc0a20d40dd69edfe55ca32a54bcbbaa6b0ddcff36049301a54579
```

Comando
```
time ./cacachave -m bsgs -t 8 -f tests/63.pub -k 512 -s 0 -S -b 63
```

saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Threads : 8
[+] K factor 512
[+] Turn off stats output
[+] Mode BSGS secuential
[+] Opening file tests/63.pub
[+] Added 1 points from file
[+] Bit Range 63
[+] -- from : 0x4000000000000000
[+] -- to   : 0x8000000000000000
[+] N = 0x100000000000
[+] Bloom filter for 2147483648 elements : 7361.33 MB
[+] Bloom filter for 67108864 elements : 230.04 MB
[+] Bloom filter for 2097152 elements : 7.19 MB
[+] Allocating 32.00 MB for 2097152 bP Points
[+] Reading bloom filter from file keyhunt_bsgs_4_2147483648.blm .... Done!
[+] Reading bloom filter from file keyhunt_bsgs_6_67108864.blm .... Done!
[+] Reading bP Table from file keyhunt_bsgs_2_2097152.tbl .... Done!
[+] Reading bloom filter from

 file keyhunt_bsgs_7_2097152.blm .... Done!
[+] Thread Key found privkey 7cce5efdaccf6808
[+] Publickey 0365ec2994b8cc0a20d40dd69edfe55ca32a54bcbbaa6b0ddcff36049301a54579
All points were found00000000
[+] Thread 0x7cf4d00000000000
real    4m11.358s
user    26m23.474s
sys     0m20.061s

```

## Minha velocidade é real?

Como esta ainda é uma versão beta, podemos ter algumas dúvidas sobre a velocidade mostrada no modo bsgs.

Para verificar isso, preparei um conjunto de chaves públicas de teste para serem encontradas em algum momento específico de acordo com sua velocidade.

Com 1 Petakeys/s, a chave pública será encontrada em 2 minutos:
Chave privada: 8000000000000001aa535d3d0c0000
Chave pública: 02af4535880d694d660031a161c53a6889c45d2de513454858e94739f9c790768b

Com 10 Petakeys/s, a chave pública será encontrada em 2 minutos:
Chave privada: 8000000000000010a741a462780000
Chave pública: 025deee1657cd5d363cff23ec1b14781e504cbb6292c273e515d73f98065131d40

Com 50 Petakeys/s, a chave pública será encontrada em 2 minutos:
Chave privada: 8000000000000053444835ec580000
Chave pública: 03c13e9c6e5cbe2ac06817e4d8fd0a3e836f1a121aab91bb67ef44747b25c7d791

Com 1 Exakey/s, a chave pública será encontrada em 2 minutos:
Chave privada: 800000000000068155a43676e00000
Chave pública: 022b6a74badcc4c3d8fab7d01ddc1854b9d8f262172789b2aa1bb7fd42cc1b2817

Com 5 Exakeys/s, a chave pública será encontrada em 2 minutos:
Chave privada: 8000000000002086ac351052600000
Chave pública: 024cf9e44f808e7b0bbb12a57ff63e3a8407cba1816f5e31e815d33d70e4a95a7f

Com 10 Exakeys/s, a chave pública será encontrada em 2 minutos:
Chave privada: 800000000000410d586a20a4c00000
Chave pública: 02ee0cf78d13b4aae9c8777a0f93dff7f5be3855bd2c0f85370f861c69bb5b533a

Selecione uma chave pública que se ajuste à sua velocidade atual, salve-a em um arquivo `testpublickey.txt` e teste com:

```
./cacachave -m bsgs -f testpublickey.txt -b 120 -q
```

Altere os valores de k, n e t.


As chaves públicas devem ser encontradas em cerca de 2 minutos após o carregamento dos arquivos.

Altere seus valores de n ou k de acordo com sua memória atual e lembre-se de não exceder o valor de k de cada N, por favor, verifique a tabela https://github.com/albertobsd/keyhunt#valid-n-and-k-values


## Modo minikeys

Este modo é experimental.

Por enquanto, apenas Minikeys de 22 caracteres estão disponíveis.

O minikey é gerado a partir de um buffer de 16 bytes usando a função de codificação base58, utilizando a string bitcoin `123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz`. Qualquer sugestão, por favor, me avise.

O arquivo de entrada pode ser uma lista de endereços ou hashes rmd das chaves de destino.


Exemplo de comando:

```
./cacachave -m minikeys -f tests/minikeys.txt -C SG64GZqySYwBm9KxE1wJ28 -n 0x10000
```

Saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode minikeys
[+] N = 0x10000
[+] Base Minikey : SG64GZqySYwBm9KxE1wJ28
[+] Allocating memory for 1 elements: 0.00 MB
[+] Bloom filter for 1 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 1 values were loaded and sorted
[+] Base minikey: SG64GZqySYwBm9KxE3QGrg?
HIT!! Private Key: d1a4fc1f83b2f3b31dcd999acd8288ff346f7df46401596d53964e0c69d5b4d
pubkey: 048722093a2b5dd05a84c28a18b2a6601320c9eaab9db99e76b850f9574cd3d5c987bf0c9c9ed3bd0f52124a57d9ef292b529536b225b90f8760d9c67cc3aa1c32
minikey: SG64GZqySYwBm9KxE3wJ29
address: 15azScMmHvFPAQfQafrKr48E9MqRRXSnVv
^C
```

comando minikeys aleatório

```
./cacachave -m minikeys -f tests/minikeys.txt -n 0x10000 -q -R
```

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Mode minikeys
[+] Quiet thread output
[+] Random mode
[+] N = 0x10000
[+] Allocating memory for 1 elements: 0.00 MB
[+] Bloom filter for 1 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 1 values were loaded and sorted
^C] Total 830464 keys in 30 seconds: 27682 keys/s

```


# Ethereum

Finalmente, endereços ethereum são suportados, para ethereum não há modos, exceto para endereço.

se você tiver chaves públicas para ethereum, pode usar o modo xpoint ou bsgs.

para testar a funcionalidade do ethereum, você pode usar o arquivo de amostra `tests/1to32.eth`

comando: 

```
./cacachave -c eth -f tests/1to32.eth -r 1:100000000 -M
```

saída:

```
[+] Version 0.2.230430 Satoshi Quest, developed by AlbertoBSD
[+] Setting search for ETH adddress.
[+] Matrix screen
[+] N = 0x100000000
[+] Range
[+] -- from : 0x1
[+] -- to   : 0x100000000
[+] Allocating memory for 32 elements: 0.00 MB
[+] Bloom filter for 32 elements.
[+] Loading data to the bloomfilter total: 0.03 MB
[+] Sorting data ... done! 32 values were loaded and sorted
Base key: 1 thread 0

 Hit!!!! Private Key: 1
address: 0x7e5f4552091a69125d5dfcb7b8c2659029395bdf

 Hit!!!! Private Key: 3
address: 0x6813eb9362372eef6200f3b1dbc3f819671cba69

 Hit!!!! Private Key: 7
address: 0xd41c057fd1c78805aac12b0a94a405c0461a6fbb

 Hit!!!! Private Key: 8
address: 0xf1f6619b38a98d6de0800f1defc0a6399eb6d30c
....
```

## Velocidades

Eu já expliquei a velocidade para BSGS.

Mas como há novas atualizações para outros modos, quero esclarecer.

Para os modos `address`, `rmd160`, `xpoint` e `vanity`.

Cada thread trabalha em grupos de 1024 chaves, então, toda vez que um ciclo interno de cada thread é concluído, o código atualiza seu próprio contador em 1.

```
steps[thread_number]++;
```

Portanto, cada passo representa 1024 chaves escaneadas.

se você habilitar endomorfismo, os passos totais são multiplicados por 6 para os modos `address`, `rmd160` e `vanity`.
Porque com endomorfismo estamos verificando efetivamente 6 chaves diferentes a cada passo.

Para o modo `xpoint` mais endomorfismo, o número é multiplicado por 3 apenas porque só nos import

amos com o valor X e não precisamos dos valores negados (espelho do eixo Y).

Caso especial para a busca `compress` dos modos `address`, `rmd160` e `vanity` SEM endomorfismo habilitado, para essas condições a velocidade é multiplicada por 2, porque estamos verificando efetivamente 2 chaves. O programa calcula um valor X e está verificando ambos os prefixos `02 + valor X` e `03 + valor X`, isso NÃO é opcional.
Tentei fazer sem esse comportamento, mas nesse caso a velocidade é pior.

Isso é importante porque, se você estiver direcionando um intervalo específico com `compress` e SEM endomorfismo, por exemplo, puzzles, a velocidade efetiva é metade da velocidade mostrada pelo programa.
Mas se você estiver direcionando toda a curva, então a velocidade mostrada está correta.

## FAQ

- Onde as chaves privadas serão salvas?
R: Em um arquivo chamado `KEYFOUNDKEYFOUND.txt`

- Posso salvar o bloomfilter e a tabela para acelerar o processo?
R: Sim, use apenas `-S` sempre que você executar o programa, funciona para:
`bsgs`, `address`, `rmd160`, `minikeys`, `xpoint`, não funciona para `vanity`.
Os arquivos serão gerados automaticamente no diretório atual.

- Por que a velocidade para bsgs diz 0 keys/s?
R: Isso foi perguntado aqui https://github.com/albertobsd/keyhunt/issues/69 e 
aqui https://github.com/albertobsd/keyhunt/issues/108 e também outros no telegram.

Por favor, veja o vídeo que fiz para responder isso https://youtu.be/MVby8mYNxbI

- Está disponível para Windows?
R: Pode ser compilado com mingw, mas recomendo fortemente o uso do WSL com Ubuntu para Windows 10.

Atualizado: 
Sim, graças a @kanhavishva.
Disponível em: https://github.com/kanhavishva/keyhunt.

Também, graças a @WanderingPhilosopher.
Disponível em: https://github.com/WanderingPhilosopher/keyhunt.

Também, graças a @XopMC.
Disponível em: https://github.com/XopMC/keyhunt-win.


## Agradecimentos

Este programa foi possível graças a:
- IceLand
- kanhavishva
- XopMC
- WanderingPhilosopher
- Malboro Man
- NetSec
- Jean Luc Pons
- Todo o grupo de CryptoHunters que tornou este programa possível
- Todos os usuários que testaram, relataram bugs, solicitaram melhorias e compartilharam seus conhecimentos.


## Doações

- BTC: 1Coffee1jV4gB5gaXfHgSHDz9xx9QSECVW
- ETH: 0x6222978c984C22d21b11b5b6b0Dd839C75821069
- DOGE: DKAG4g2HwVFCLzs7YWdgtcsK6v5jym1ErV

Todas as doações serão usadas apenas para duas coisas:

- Versão nativa do Windows sem dependências externas.
- Obter um computador desktop acessível com GPU decente, não de ponta, apenas para iniciar a versão GPU.

## Testnet

Também preciso fazer alguns testes na rede testnet. Se você tiver algum saldo na Testnet, pode me ajudar com doações para meu endereço de testnet:

Endereço: msKcxhizYWVvxCACFEG4GCSK1xYrEkib5A

Obrigado.
