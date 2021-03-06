# Toteutusdokumentti

### Ohjelman yleisrakenne

Käyttäjän syöte tulee komentoriviparametrien kautta. Ensin Luokka **ArgsParser** validoi että ne on syötetty oikeassa muodossa, eli että ensimmäinen parametri on 'c' tai 'd' (compress tai decompress), ja toinen parametri on polku tiedostoon, joka on olemassa, ja ei ole tyhä tai kansio.

Mikäli validointi onnistuu, käytetään pakkaamiseen/purkamiseen **FileConverter**-luokkaa, jonka tehtävänä on korvata tiedostoja uusilla tiedostoilla, joiden data määrittyy sen mukaan mitä **DataConverter**-luokka tekee vanhan tiedoston datalle (pakkaa tai purkaa).

**DataConverter**-rajapinnan toteuttavat luokat **Encoder** ja **Decoder**. Nämä luokat käyttävät Huffmanin koodausta pakattujen/purettujen datojen luomiseen.

Huffman-koodien luomista varten pitää ensin luoda huffmanin puu (luokka **HuffmanTree**). **Encoder** käyttää **HuffmanTreeMaker**-rajapinnan takaa löytyvää **HuffmanTreeBuilder**iä ja **Decoder** saman rajapinnan takaa löytyvää **HuffmanTreeRebuilder**iä.

**HuffmanTreeBuilder** käyttää [wikipedian artikkelista](https://github.com/user/repo/blob/branch/other_file.md) löytyvää algoritmia huffmanin puun luomiseen, käyttäen luokkia **Node** ja **PrioirtyQueue**.

Kun **HuffmanTreeMaker** on luonut **Encoder**ille **HuffmanTree**n, se voi alkaa kirjoittamaan pakattua dataa. Sitä varten se tarvitsee **BitOutputStream**iä joka käyttää javan ByteArrayOutputStreamiä. Javassa ei ole valmiiksi bittitason streamejä. Ensin kirjoitetaan [tästä postauksesta](http://stackoverflow.com/a/759766) löytyvän algoritmin mukaisesti puun bittitasoesitys ja lopuksi kirjoitetaan **HuffmanTree**-luokasta löytyviä koodeja käyttäen loput viestistä uudessa esitysmuodossaan.

Purkamisessa käytettävä **HuffmanTreeRebuilder** käyttää **BitInputStream**iä luodakseen puun edellisestä linkistä löytyvän purkamisalgoritmin avulla. **Decoder** voi sitten tämän avulla kääntää viestin.

### Saavutetut aika- ja tilavaativuudet

_n_ on uniikkien merkkien määrä viestissä, joka on maksimissaan 256, eli merkkien määrä extended ascii:ssa.

Aikavaativuus: O(_n_ log _n_). huffmanin puuta luodessa pitää lisätä n määrä alkioita heappiin, mikä kestää log _n_ per operaatio, mutta alkioita voi maksimissaan olla 256, jolloin jokainen extended ascii merkistön merkki on mukana.

Tilavaativuus: O(_n_), sillä heapin maksimikoko on 2n-1.

### Työn mahdolliset puutteet ja parannusehdotukset

Kanoninen koodaus ja viestin pituuden ilmaiseminen loppumerkin sijaan pakkaisivat tiedoston pienempään tilaan.

### Lähteet

[http://stackoverflow.com/a/759766](http://stackoverflow.com/a/759766)  
[http://en.wikipedia.org/wiki/Huffman_coding](http://en.wikipedia.org/wiki/Huffman_coding)  
