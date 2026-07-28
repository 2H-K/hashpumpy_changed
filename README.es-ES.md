# HashPump

Una herramienta para explotar el ataque de extensión de longitud de hash (hash length extension attack) en varios algoritmos de hashing.

Algoritmos soportados actualmente: MD5, SHA1, SHA256, SHA512.

## Menú de Ayuda

```bash
$ hashpump -h
HashPump [-h help] [-t test] [-s signature] [-d data] [-a additional] [-k keylength]
    HashPump genera cadenas para explotar firmas vulnerables al ataque de extensión de longitud de hash.
    -h --help          Muestra este mensaje.
    -t --test          Ejecuta pruebas para verificar que cada algoritmo funcione correctamente.
    -s --signature     La firma del mensaje conocido.
    -d --data          Los datos del mensaje conocido.
    -a --additional    La información que desea agregar al mensaje conocido.
    -k --keylength     La longitud en bytes de la clave utilizada para firmar el mensaje original.
    Version 1.2.0 with CRC32, MD5, SHA1, SHA256 and SHA512 support.
    <Developed by bwall(@botnet_hunter)>
```

## Ejemplo de Salida

```bash
$ hashpump -s '6d5f807e23db210bc254a28be2d6759a0f5f5d99' --data 'count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo' -a '&waffle=liege' -k 14
0e41270260895979317fff3898ab85668953aaa2
count=10&lat=37.351&user_id=1&long=-119.827&waffle=eggo\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02(&waffle=liege
```

## Compilación e Instalación

```bash
$ git clone https://github.com/bwall/HashPump.git
$ apt-get install g++ libssl-dev
$ cd HashPump
$ make
$ make install
```

`apt-get` y `make install` requieren privilegios de root para ejecutarse correctamente. El requisito real es `-lcrypto`, por lo que, dependiendo de su sistema operativo, sus dependencias pueden variar.

En OS X, HashPump también puede instalarse usando [Homebrew](http://brew.sh/):

```bash
$ brew install hashpump
```

## Menciones

HashPump ha sido mencionado en varios write-ups. Si se pregunta cómo puede utilizar HashPump, estos son algunos excelentes ejemplos.

* http://ctfcrew.org/writeup/54
* http://d.hatena.ne.jp/kusano_k/20140310/1394471922 (JP)
* http://conceptofproof.wordpress.com/2014/04/13/plaidctf-2014-web-150-mtgox-writeup/
* http://achatz.me/plaid-ctf-mt-pox/
* http://herkules.oulu.fi/thesis/nbnfioulu-201401141005.pdf
* https://github.com/ctfs/write-ups/tree/master/plaid-ctf-2014/mtpox

## Bindings de Python

Los amantes de Python estarán complacidos con esta adición. Para evitarme escribir una implementación de todos estos algoritmos de hash con la capacidad de modificar estados en Python, se han añadido bindings de Python en forma de hashpumpy. Esta adición proviene de [zachriggle](https://github.com/zachriggle).

### Instalación
Estos bindings de Python están disponibles en [PyPI](https://pypi.python.org/pypi/hashpumpy/1.0) y pueden instalarse vía pip.
  pip install hashpumpy
  
### Uso
    >>> import hashpumpy
    >>> help(hashpumpy.hashpump)
    Help on built-in function hashpump in module hashpumpy:
    
    hashpump(...)
        hashpump(hexdigest, original_data, data_to_add, key_length) -> (digest, message)
    
        Arguments:
            hexdigest(str):      Hex-encoded result of hashing key + original_data.
            original_data(str):  Known data used to get the hash result hexdigest.
            data_to_add(str):    Data to append
            key_length(int):     Length of unknown data prepended to the hash
    
        Returns:
            A tuple containing the new hex digest and the new message.
    >>> hashpumpy.hashpump('ffffffff', 'original_data', 'data_to_add', len('KEYKEYKEY'))
    ('e3c4a05f', 'original_datadata_to_add')

### Nota sobre Python 3
hashpumpy es compatible con Python 3. A diferencia de la versión de Python 2, el segundo valor (el nuevo mensaje) en la tupla devuelta por `hashpumpy.hashpump` es un objeto de tipo bytes en lugar de una cadena (string).
