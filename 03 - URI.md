# 03 - URI <!-- omit from toc -->

- [URI](#uri)
- [Uniformità](#uniformità)
- [Sintassi](#sintassi)
- [Specializzazioni di URI: URN e URL](#specializzazioni-di-uri-urn-e-url)
  - [URN](#urn)
  - [URL](#url)
  - [Componenti di un URL (HTTP-like)](#componenti-di-un-url-http-like)
- [Altri esempi di URI](#altri-esempi-di-uri)
- [URI opache e URI gerarchiche](#uri-opache-e-uri-gerarchiche)
  - [Operazioni sulle URI gerarchiche](#operazioni-sulle-uri-gerarchiche)

> [**Return to full index**](00%20-%20Index.md)

## URI

Gli URI (**Uniform Resource Identifier**) forniscono un meccanismo semplice ed estensibile per identificare una risorsa.
\
Con il termine **risorsa** intendiamo qualunque entità abbia un'identità: un documento, un’immagine, un servizio, una collezione di altre risorse.

URI è un concetto generale: non fa riferimento necessariamente a risorse accessibili tramite HTTP o a entità disponibili in rete. Esso è un **mapping** concettuale ad una entità: non si riferisce necessariamente ad una particolare versione dell’entità esistente in un dato momento.
Il mapping può rimanere inalterato anche se cambia il contenuto della risorsa.

## Uniformità

Gli URI rispettano una sintassi standard, semplice e regolare. Ciò porta con sé una serie di vantaggi:
- **Convenzioni sintattiche** comuni.
- **Comune semantica** per l’interpretazione.
- **Indipendenza dal protocollo**: possibilità di usare nello stesso contesto differenti tipologie di identificatori anche con meccanismi (protocolli) di accesso diversi.
- **Estensibilità**: facilità nell’introduzione di nuovi tipi di
identificatori.

## Sintassi

Un identificatore è un frammento di informazione che fa
riferimento ad una entità dotata di un’identità, ovvero ad una risorsa.

Nel caso degli URI gli identificatori sono stringhe con una sintassi definita,dipendente dallo schema, che può essere espressa nella forma più generale in questo modo:

```HTML
<scheme>:<scheme-specific-part>
```

Per la componente `<scheme-specific-part>` non esiste una struttura o una semantica comune a tutti gli URI. Esiste però un sottoinsieme di URI che condivide una sintassi comune per rappresentare relazioni gerarchiche in uno spazio di nomi: 

```HTML
<scheme>://<authority><path>?<query>
```

A parte `<scheme>`, le altre parti possono talora essere omesse,come nei casi in cui non è inclusa la componente `<authority>` o non è inclusa la componente `<query>`.

## Specializzazioni di URI: URN e URL

![URI](resources\URI.png)

### URN

**Uniform Resource Name** (URN): identifica una risorsa per mezzo di un riferimento che deve essere globalmente unico nel suo dominio di appartenenza, detto "namespace", e restare valido, anche se la risorsa diventa non disponibile o cessa di esistere.
\
Consente di riferirsi a una risorsa prescindendo dalla sua ubicazione e dalle modalità con cui è possibile accedervi.

Un esempio molto noto è il codice ISBN (International Standard Book Number) che identifica a livello internazionale in modo univoco e duraturo un libro o una edizione di un libro di un determinato editore. Non ci dice nulla su come e dove procurarci il libro.

### URL

**Uniform Resource Locator** (URL): identifica una risorsa per mezzo del suo meccanismo di accesso primario, ad esempio locazione nella rete, piuttosto che sulla base del suo nome o dei suoi attributi. 
\
Un URL dunque tiene conto anche della modalità per accedere alla risorsa e specifica il protocollo necessario per il trasferimento della risorsa stessa. Tipicamente il nome dello schema corrisponde al protocollo utilizzato. La parte rimanente del nome dipende dal protocollo.

Nella sua forma più comune (schema HTTP-like) la sintassi è: 

```HTML
<protocol>://[<username>:<password>@]
<host>[:<port>][/<path>[?<query>][#fragment]]
```

Questa forma vale per diversi protocolli di uso comune, quali HTTP, HTTPS, FTP,WAP, ..., ma non, ad esempio, per la posta elettronica.

### Componenti di un URL (HTTP-like)

- `<protocol>`: Descrive il protocollo da utilizzare per l'accesso al server (HTTP, HTTPS, FTP, MMS, ...).
- `<username>:<password>@`: credenziali per l'autenticazione.
- `<host>`: indirizzo server su cui risiede la risorsa. Può essere un indirizzo IP logico o fisico.
- `<port>`: definisce la porta da utilizzare (TCP come protocollo di trasporto per HTTP, che vedremo essere a livello applicativo). Se non viene indicata, si usa la porta standard per il protocollo specificato; per HTTP essa è la 80.
- `<path>`: percorso (pathname) che identifica la risorsa nel filesystem del server. Se manca, tipicamente si accede alla risorsa predefinita, come, ad esempio, alla homepage.
- `<query>`: una stringa di caratteri che consente di passare al server uno o più parametri. Di solito ha questo formato:
  ```HTML
  parametro1=valore&parametro2=valore2...
  ```

_Esempio_:

![URL_schema_HTTP](resources\URL_schema_HTTP.png)

## Altri esempi di URI

- Schema per servizi FTP: ftp://ftp.FreeBSD.org/pub/FreeBSD/.
- Schema per news group e articoli Use net: news:comp.infosystems.www.servers.unix.
- Schema per servizi Telnet: telnet://melvyl.ucop.edu.
- Schema per IRC: irc://irc.freenode.net/wikipedia-it.
- Schema per indirizzi di posta elettronica: mailto:laura.po@unimore.it.

## URI opache e URI gerarchiche

Le URI possono essere anche classificate come opache o gerarchiche:
- **URI opaca**: non è soggetta a ulteriori operazioni di parsing; ad esempio, una URI opaca è: mailto:paolo.rossi@disi.unibo.it.
- **URI gerarchica**: è soggetta a ulteriori operazioni di parsing,per esempio per separare l’indirizzo del server dal percorso all’interno filesystem. Alcuni esempi sono:
  - http://informatica.unibo.it/
  - docs/guide/collections/designfaq.html#28
  - ../../../lab/examples/ant/build.xml
  - file:///~/calendar

### Operazioni sulle URI gerarchiche

- **Normalizzazione**: processo di rimozione dei segmenti "." e ".."(e altri caratteri speciali) dal path di una URI gerarchica. La normalizzazione non ha alcun effetto sulle URI opache.
- **Risoluzione**: è il processo che a partire da una URI originaria porta all’ottenimento di una URI risultante. La URI originaria viene risolta basandosi su na terza URI, detta "base URI".
- **Relativizzazione**: è il processo inverso alla risoluzione.

_Esempi_:

- URI originaria: docs/guide/collections/designfaq.html#28
- Base URI: http://disi.unibo.it/
- Risultato: http://disi.unibo.it/docs/guide/collections/designfaq.html#28