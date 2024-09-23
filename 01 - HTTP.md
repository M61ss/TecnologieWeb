# 01 - HTTP <!-- omit from toc -->

- [Introduzione](#introduzione)
- [Terminologia](#terminologia)
- [Storia dell'HTTP](#storia-dellhttp)
  - [HTTP 1.0 (1996)](#http-10-1996)
  - [HTTP 1.1 (1997)](#http-11-1997)
  - [HTTP 1.1 con pipelining](#http-11-con-pipelining)
  - [HTTP/2 (2015)](#http2-2015)
  - [HTTP/3 (2022)](#http3-2022)
- [Latenza](#latenza)
- [Struttura dell messaggio HTTP "tradzionale"](#struttura-dell-messaggio-http-tradzionale)
  - [Header](#header)
  - [Comandi della richiesta HTTP](#comandi-della-richiesta-http)
- [Codici di stato](#codici-di-stato)
- [REST](#rest)
- [Modelli di esecuzione](#modelli-di-esecuzione)
- [Cookie](#cookie)
  - [Struttura dei cookie](#struttura-dei-cookie)
- [Autenticazione](#autenticazione)
  - [Riconoscimento dell'indirizzo IP](#riconoscimento-dellindirizzo-ip)
  - [HTTP Basic](#http-basic)
  - [Form](#form)
- [Sicurezza](#sicurezza)
  - [SSL/TLS](#ssltls)
- [Architetture distribuite](#architetture-distribuite)
- [Caching distribuito](#caching-distribuito)
  - [User Agent Cache](#user-agent-cache)
  - [Proxy Cache](#proxy-cache)
    - [Forward Proxy Cache](#forward-proxy-cache)
    - [Reverse Proxy Cache (server-side proxy cache)](#reverse-proxy-cache-server-side-proxy-cache)
  - [HTTP e cache](#http-e-cache)

> [**Return to full index**](00%20-%20Index.md)

## Introduzione

_**WWW = URL + HTTP + HTML**_

HTTP è l'acronimo di **HyperText Transfer Protocol**. Esso è il protocollo di livello applicativo utilizzato per _trasferire le risorse web_ (pagine o elementi di pagina) da server a client. Gestisce sia le richieste inviate al server (**url**) sia le risposte inviate al client (**pagine**).
\
HTTP è un protocollo **stateless**: né il server né il client mantengono, a livello di protocollo, informazioni relative ai messaggi precedentemente scambiati.

Versioni di rilevanza dell'HTTP:

- HTTP 1.0
- HTTP 1.1
- HTTP 1.1 con pipelining
- HTTP/2 + websocket
- HTTP/3

> [!IMPORTANT] HTTP si trova a livello application nello stack TCP/IP.
>
> ![stack_TCP-IP](resources\stack_TCP-IP.png)
>
> (vedi [03.1 - TCP-IP](03.1%20-%20TCP-IP.md))

## Terminologia

- **Client**: programma applicativo che stabilisce una connessione al fine di inviare delle richieste.
- **Server**: programma applicativo che accetta connessioni al fine di ricevere richieste ed inviare specifiche risposte con le risorse richieste.
- **Connessione**: circuito virtuale stabilito a livello di trasporto tra due applicazioni per fini di comunicazione.
- **Messaggio**: è l'unità base della comunicazione HTTP; è definita come una specifica sequenza di byte concettualmente atomica.
  - _Request_: messaggio HTTP di richiesta.
  - _Response_: messaggio HTTP di risposta.
- **Resource**: dato di tipo univocamente definito.
- **URI**: Uniform Resourcer Identifier (identificatore unico per una risorsa).
- **Entity**: rappresentazione di una risorsa; può essere incapsulata in un messaggio, tipicamente di risposta.

## Storia dell'HTTP

### HTTP 1.0 (1996)

Esso è un protocollo **request-response**, **stateless** e **one-shot** in cui ogni interazione tra client e server è limitata a una singola richiesta e riposta, senza che il server mantenga lo stato tra le interazioni, e ogni richiesta necessita una nuova connessione al server.
\
HTTP 1.0 è un protocollo basato su **TCP**: sia le richieste al server sia le risposte ai client sono trasmesse utilizzando stream TCP.

_Esempio_:

![request-response](resources\request-response.png)

### HTTP 1.1 (1997)

La differenza principale tra HTTP 1.0 e HTTP 1.1 è la possibilità di specificare una **serie multipla di richiesta/risposta** nella stessa connessione. Le connessioni 1.0 vennero dunque dette "non persistenti", mentre quelle 1.1 vennero definite "**persistenti**". Il server lascia aperta la connessione TCP dopo aver stabilito la risposta e può quindi ricevere le richieste successive sulla stessa connessione.
\
Il server HTTP chiude la connessione richiesta dal client solo quando viene specificato nell'header del messaggio oppure quando non viene utilizzata da un certo intervallo di tempo (**timeout**).

### HTTP 1.1 con pipelining

Il **pipelining** consiste nell'invio di molteplici richieste da parte del client prima di terminare la ricezione delle risposte. Queste ultime devono però essere date nello stesso ordine delle richieste perché non è specificato un metodo esplicito di associazione tra richiesta e risposta; TCP garantisce che i dati vengano consegnati in ordine sequenziale, anche se ciò può essere causa di ritardi se una delle risposte dovesse essere lenta.

> [!IMPORTANT] Confornto tra protocolli <!-- omit from toc -->
>
> ![connections](resources\connections.png)

### HTTP/2 (2015)

L'obiettivo fondamentale di HTTP/2 è il **miglioramento complessivo delle performance** con **backward compatibility** con HTTP 1.1.

In aggiunta rispetto ai precedenti protocolli si hanno le seguenti caratteristiche:

- **Request-response multiplexing**: consiste in un meccanismo di gestione simultanea di multiple richieste e riposte.
- **Header compression**.
- **Server push**: esso è un server che può inviare risorse aggiuntive senza che il client le richieda esplicitamente.

HTTP/2 è basato su SPDY, un protocollo cosidetto di open networking promosso da Google.

### HTTP/3 (2022)

L'obiettivo fondamentale di HTTP/3 è la **riduzione della latenza** e il complessivo **miglioramento della velocità**, specialmente su reti congestionate o inaffidabili, grazie all'uso del protocollo di trasporto QUIC al posto di TCP.

> [!NOTE] QUIC
>
> QUIC è un protocollo che richiede meno passaggi di TCP per mantenere la connessione tra server e client.

In aggiunta rispetto ai precedenti protocolli si hanno le seguenti caratteristiche:

- **Stream multiplexing**: gestione simultanea di più stream di richieste adottando una politica di non-blocking. La presenza di uno stream attivo non blocca la possibilità di mantenere connessioni con altri stream.
- **Controllo di flusso per-stream**.
- Realizzazione di **connessioni con bassissima latenza**.

> [!IMPORTANT] Protocollo stack
>
> ![protocol_stack](resources/protocol_stack.png)

## Latenza

La latenza è il **tempo di ritardo** che intercorre tra l'invio di un'azione e il momento in cui si riceve la risposta.

Alcuni dei fattori che influenzano la latenza sono:

- **Distanza fisica** tra client e server: più il server è lontano, maggiore sarà il tempo impiegato dai dati per andare e tornare.
- **Congestione della rete**: una rete sovraccarica può causare ritardi nei tempi di risposta.
- **Qualità della connessione**: le connessioni meno stabili o quelle mobili possono avere una latenza più elevata.
- **Tipo di protocollo**: TCP, ad esempio, può causare latenza maggiore rispetto a QUIC perché richiede più passaggi per stabilire e mantenere una connessione.

## Struttura dell messaggio HTTP "tradzionale"

Un messaggio HTTP è definito da due strutture:

- **Message header**: questo blocco contiene tutte le informazioni necessarie per identificare il messaggio, dunque le intestazioni di quest'ultimo.
- **Message body**: contiene i dati trasportati dal messaggio.

Esistono degli standard per formattare ogni tipo di messaggio.
\
I messaggi di response contengono i dati relativi alle risorse richieste e sono codificati secondo il formato specificato dall'header, solitamente in formato **MIME** (Multipurpose Internet Mail Extentions).

### Header

Gli header sono costituiti da insiemi di coppie `nome: valore` che specificano le caratteristiche del messaggio trasmesso o ricevuto:

- Header generali della **trasmissione**: data, codifica, versione, tipo di comunicazione, ecc...
- Header relativi all'**entità** trasmessa: content-type, content-length, data di scadenza, ecc...
- Header riguardo la **richiesta** effettuata: chi fa la richiesta, a chi viene fatta la richiesta, che tipo di caratteristiche il client è in grado di accettare, quale autorizzazione, ecc...
- Header della **risposta** generata: quale server dà la risposta, che tipo di autorizzazione è necessaria, ecc...

**I messaggio sono in formato ASCII**.

_Esempio_:

**Richiesta**:
\
![GET](resources\GET.png)

**Risposta**:
\
![risposta](resources\risposta.png)

> [!NOTE]
>
> - q= : tale attributo indica il grado di preferenza del valore specificato.
> - Keep-alive: è un valore espresso in secondi che indica per quanto la connessione deve essere mantenuta.
> - Refer: URL della pagina di origine da cui proviene la richiesta.

### Comandi della richiesta HTTP

- **GET**: serve per richiedere una risorsa ad un server. Tale è il metodo più frequente che viene attivato facendo un click su un link ipertestuale su un documento HTML o specificato un URL nell'apposito campo di un browser. La lunghezza di un URL è limitata ed è previsto il passaggio di parametri all'interno della parte `<query>` dell'URL.
- **POST**: progettato come il messaggio per richiedere una risorsa. A differenza di GET, i dettagli per l'identificazione e l'elaborazione della risorsa stessa non sono nell'URL, ma nel body. Si tratt di una trasmissione di informazioni da client a server che, però, non porta alla creazione di una risorsa sul server. Non ci sono limiti di lunghezza nei parametri di una richiesta. POST viene usato, per esempio, per sottomettere i dati di un form HTML ad un'applicazione CGI sul server.
- **PUT**: tramite il metodo PUT il client chiede la memorizzazione sul server di una risorsa all'URL specificato. A differenza del POST si ha la creazione di una risorsa, eventualmente la sua sostituzione in caso essa esista già. L'argomento del metodo PUT è la risorsa che ci si aspetta di ottenere facendo un GET con lo stesso nome di seguito.
- **DELETE**: richiede la cancellazione della risorsa riferita dall'URL specificato. Questo metodo è normalmente disabilitato sui server pubblici.
- **HEAD**: è simile al metodo GET, ma il server deve rispondere soltanto con gli header relativi senza body. HEAD viene usato per verificare la validità e l'accessibilità di un URL.
- **OPTIONS**: serve per richiedere informazioni sulle opzioni disponibili per la comunicazione.
- **TRACE**: è usato per invocare il loop-back remoto a livello applicativo del messaggio di richiesta. Consente al client di vedere che cosa è stato ricevuto dal server e viene usato nella diagnostica e testing dei serivizi web.

> [!NOTE] CGI - Common Gateway Interface
>
> CGI viene utilizzata dai web server per interfacciarsi con applicazioni esterne generando contenuti web dinamici.

## Codici di stato

Lo **status code** è un numero di tra cifre di cui la prima indica la classe della risposta e le altre due la specifica.
\
Le classi di risposta sono 5:

- **1xx: Informational**: essa è una risposta temporanea alla richiesta durante il suo svolgimento, **sconsigliata da HTTP 1.0**.
- **2xx: Successful**: indica che il server ha ricevuto, capito e accettato la richiesta.
- **3xx: Redirection**: signfica che il server ha ricevuto e capito la richiesta, ma sono necessarie altre azioni da parte del client per portare a termine la richiesta.
- **4xx: Client error**: indica che la richiesta del client non può essere soddisfatta per un errore sintattico da parte sua o che esso non abbia l'autorizzazione per accedere a tale risorsa.
- **5xx: Server error**: significa che la richiesta può essere corretta, ma il server non è in grado di soddisfare la richiesa per un problema interno o di applicazioni CGI.

_Esempi_:

- 100 Continue (DEPRECATED): il client non ha ancora mandato il body
- 200 Ok: GET success
- 201 Created: PUT succcess
- 301 Moved permanently: URL non valida, il server conosce la nuova posizione
- 400 Bad request: errore sintattico della richiesta
- 401 Unauthorized: manca l'autorizzazione
- 403 Forbidded: richiesta non autorizzabile
- 404 Not found: URL errato
- 500 Internal server error: solitamente CGI mal fatto
- 501 Not implemented: metodo non consentito dal server

## REST

REST è un'**architettura** per la progettazione di servizi web basata su un insieme di principi e vincoli per la comunicazione tra client e server attraverso protocolli, come HTTP.

REST sfrutta HTTP come protocollo di trasporto per inviare richieste e ricevere risposte. HTTP è perfetto per implementare REST perché fornisce metodi standard che mappano direttamente alle operazioni CRUD (Create, Read, Update, Delete) su risorse.

## Modelli di esecuzione

I server possono essere di 3 tipi:

- **Sequenziale**.
- **Concorrnete**.
- **Parallelo**.

La scelta del tipo di server da utilizzare dipende dai requisiti del servizio che lo dovrà sfruttare.

## Cookie

Parallelamente alle sequenze request/response, il protocollo prevede una struttura dati, detta "**cookie**", che si muove come un token dal client al server e viceversa.
\
I cookie possono essere generati sia dal client che dal server. Dopo la loro creazione vengono sempre passati ad ogni tramissione di request e response. Essi hanno come scopo quello di fornire un **supporto per il mantenimento di stato** in un protocollo come HTTP, che è essenzialmente stateless.

### Struttura dei cookie

I cookie sono una collezione di stringhe:

- **Key**: identifica univocamente un cookie all'interno di un dominio (path).
- **Value**: valore associato al cookie; è una stringa della lunghezza massima di 255 caratteri.
- **Path**: posizione nell'albero di un sito al quale è associato (default = /).
- **Domain**: dominio dove è stato generato.
- **Max-age** (opzionale): numero di secondi di vita della sessione.
- **Secure** (opzionale): poco usato, questi cookie vengono trasferiti se e solo se il protocollo è sicuro (https).
- **Version**: identifica la versione del protocollo di gestione dei cookie.

## Autenticazione

Esistono situazioni in cui si vuole restringere l'accesso alle risorse ai soli utenti abilitati.
\
Le tecniche comunemente utilizzate sono:

- Filtro su set di indirizzi IP.
- Form per la richiesta di username e password.
- HTTP Basic.
- HTTP Digest.

### Riconoscimento dell'indirizzo IP

Basare l'autenticazione sull'indirizzo IP del client è una soluzione che presenta vari svantaggi:

- Non funziona se l’indirizzo non è pubblico.
- Non funziona se l’indirizzo IP è assegnato dinamicamente.
- Esistono tecniche che consentono di presentarsi con un IP fasullo (spoofing).

HTTP Digest è caduta in disuso negli ultimi anni. Di norma si preferisce utilizzare HTTP Basic oppure Form.

### HTTP Basic

HTTP Basic consiste in due passaggi:

- Challenge: il server invia al client una richiesta di autenticazione, tipicamente espressa usando il codice 401.
- Request: il client invia le credenziali per accedere alla risorsa che aveva richiesto utilizzando l'header HTTP Authorization. Username e password sono uniti nella stringa `username:password` e il risultato è codificato con base64.

_Esempio_:

**Challenge**:

```HTML
HTTP/1.1 401 Authorization Required
WWW-Authenticate: Basic realm=“Secure Area"
```

**Request**:

```HTML
GET /private/index.html HTTP/1.1
Host: localhost
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
```

### Form

Il funzionamento è analogo a quello di HTTP Basic, ma al posto del metodo GET si utilizza POST.

## Sicurezza

Sono desiderabili alcune proprietà nella gestione della sicurezza. Le più importanti sono elencate di seguito:

- **Confidenzialità**.
- **Integrità**.
- **Autenticità**.
- **Non ripudio**: dopo l'invio del messaggio si riceve la conferma della ricezione di quest'ultimo dal destinatario cosicché esso non possa negare di aver ricevuto il messaggio.

Dove le prime tre vengono gestite da **SSL** (Secure Sockets Layer) e **TLS** (Transport Layer Security), che definiscono la sicurezza del canale di trasporto.

### SSL/TLS

TLS sostituisce SSL ed è alla base di HTTPS.

Il canale di trasporto viene posto a un livello che si occupa della gestione di confidenzialità, autenticità ed integrità della comunicazione tra HTTP e TCP.
\
Si basa su crittografia a chiave pubblica e privata ed è provvisto di certificato, che in genere viene utilizzato per autenticare il server che lo adotta.

## Architetture distribuite

- **Proxy**: programma applicativo in grado di agire sia come client sia come server al fine di effettuare richieste per conto di altri client. Le request vengono processate internamente oppure vengono ridirezionate al server. Un proxy deve interpretare e, se necessario, riscrivere le request prima di inoltrarle.
- **Gateway**: server che agisce da intermediario per altri servers. Al contrario dei proxy, il gateway riceve le request come se fosse il server originale e i client non sono in grado di identificare se la response proviene da un gateway o dai servers origiali. Per questa ragione viene detto anche "reverse proxy" o "server-side proxy".
- **Tunnel**: programma applicativo che agisce come “blind relay” tra due connessioni. Una volta attivo (in gergo “salito”) non partecipa alla comunicazione HTTP.

## Caching distribuito

L'idea di base consiste nel memorizzare **copie temporanee di documenti Web** al fine di ridurre l’uso della banda ed il carico sul server.
\
Una Web cache memorizza i documenti che la attraversano L’obiettivo è usare i documenti in cache per le successive richieste qualora alcune condizioni siano verificate.

**User Agent Cache** e **Proxy Cache** sono due esempi di Web cache.

### User Agent Cache

Lo user agent (tipicamente il browser) mantiene una cache delle pagine visitate dall’utente.
\
Questo modello di caching è ancora ora molto rilevante per i dispositivi mobili al fine di consentire agli utenti di lavorare con connettività intermittente ma anche per ridurre latenze dovute a caricamento di elementi statici, come icone, sfondi, ecc..

### Proxy Cache

#### Forward Proxy Cache

Servono per **ridurre le necessità di banda**. Il proxy intercetta il traffico e mette in cache le pagine cosicché successive richieste non provocano il download di ulteriori copie delle pagine al server.

#### Reverse Proxy Cache (server-side proxy cache)

Gateway cache che operano per conto del server e consentono di **ridurre il carico computazionale delle macchine**. I client non sono in grado di capire se le pagine arrivano dal server o dal gateway.
\
Si utilizza Internet Caching Protocol per il coordinamento fra diverse cache. Ciò è alla base per il content delivery network.

### HTTP e cache

HTTP definisce vari meccanismi che possono essere sfrtuttati per avere effetti collaterali positivi per la gestione "lazy" dell’aggiornamento cache:

- **Freshness**: controllata lato server dall'Expires response header e lato client e dalla direttiva `Cache-Control: max-age`.
- **Validation**: può essere usato per controllare se un elemento in cache è ancora corretto. Ad esempio, nel caso in cui sia in cache da molto tempo è possibile fare una validazione di quest'ultimo tramite richieste HEAD.
- **Invalidation**: è normalmente un effetto collaterale di altre request che hanno attraversato la cache. Se, per esempio, viene mandata una POST, una PUT o una DELETE a un URL, allora il contenuto della cache viene automaticamente invalidato.
