# 02 - HTML <!-- omit from toc -->

- [Introduzione](#introduzione)
- [Codifica](#codifica)
  - [Linguaggio di codifica del testo](#linguaggio-di-codifica-del-testo)
  - [Sistemi di codifica caratteri](#sistemi-di-codifica-caratteri)
- [Linguaggi a marcatori](#linguaggi-a-marcatori)
  - [Classificazione dei linguaggi di mark-up](#classificazione-dei-linguaggi-di-mark-up)
  - [Linguaggi dichiarativi](#linguaggi-dichiarativi)
  - [SGML (1986)](#sgml-1986)
  - [HTML](#html)
- [Sintassi](#sintassi)
  - [Tag](#tag)
  - [Grammatica](#grammatica)
  - [Entity](#entity)
  - [Attributi](#attributi)
  - [Tipi MIME](#tipi-mime)
  - [Commenti](#commenti)
- [Struttura base di un documento HTML](#struttura-base-di-un-documento-html)
  - [DTD](#dtd)
  - [Header](#header)
  - [Body](#body)

> [**Return to full index**](00%20-%20Index.md)

## Introduzione

HTML è l’acronimo di **HyperText Markup Language**. Esso è il linguaggio utilizzato per descrivere le pagine che costituiscono i nodi dell’ipertesto ed è un linguaggio di **codifica del testo del tipo a marcatori** (markup).

## Codifica

### Linguaggio di codifica del testo

Un linguaggio di codifica del testo è un formalismo con cui rappresentare un documento su supporto digitale in modo che sia trattabile dall’elaboratore in quanto testo.

### Sistemi di codifica caratteri

I formalismi più elementari per la codifica informatica del testo sono i sistemi di codifica dei caratteri. In generale, ogni documento elettronico è costituito da una stringa di caratteri. Essi, come qualsiasi altro tipo di dato, vengono rappresentati all’interno di un elaboratore mediante codifica binaria.
\
Per codificare i caratteri si stabilisce una corrispondenza biunivoca tra elementi di una collezione ordinata di caratteri e un insieme di codici numerici. Si ottiene così un **coded character set** che di solito si rappresenta in forma di tabella, detta "code page" o "code table".

> [!IMPORTANT] Coded Character Set
>
> Un coded character set (insieme di caratteri codificati) è un sistema che associa a ciascun carattere un valore numerico univoco, chiamato codice. Questo insieme di caratteri è spesso rappresentato in forma di code page o code table, ovvero una tabella che elenca tutti i caratteri e i loro corrispondenti valori numerici. La tabella ASCII ne è un esempio.

Per ciascun coded character set si definisce un metodo di codifica dei caratteri, detto "**character encoding**". La codifica mappa una o più sequenze di byte a un numero intero che rappresenta un carattere in un determinato coded character set.
\
Il numero di caratteri rappresentabili in un certo coded charachter set è determinato dal numero di bit utilizzati per codificare ogni singolo carattere.

> [!IMPORTANT] Unicode
>
> Unicode ha molti vantaggi, tra cui:
>
> - **Universalità**.
> - **Codifica a più bit**: UTF-8, UTF-16, ecc..
> - **Retrocompatibilità**.
> - **Punti di codice**.
> - **Gestione del testo multilingua**.
> - **Interoperabilità globale**: consente la creazione di software e documenti che possono essere usati in tutto il mondo senza preoccuparsi di problemi di codifica dei caratteri.
> - **Ampia adozione**: Unicode è supportato da quasi tutte le piattaforme moderne (sistemi operativi, browser, database) ed è lo standard per la rappresentazione del testo su Internet.
> - **Espansibilità**: Unicode continua ad essere aggiornato per includere nuovi caratteri, simboli e emoji, consentendo una rappresentazione accurata di nuove esigenze di scrittura e comunicazione.

Un testo è un oggetto complesso caratterizzato da molteplici livelli strutturali che non si limitano alla sequenza di simboli del sistema di scrittura. Si parla propriamente di linguaggio di codifica testuale solo in riferimento ai linguaggi che consentono la rappresentazione o il controllo di uno o più livelli strutturali di un documento testuale. Tali linguaggi vengono correntemente denominati ""linguaggi a marcatori"", ovvero "**mark-up language**".

## Linguaggi a marcatori

Un linguaggio di mark-up è composto da:

- **Insieme di istruzioni**, dette "tag" o "mark-up" (marcatori), che rappresentano le caratteristiche del documento testuale.
- **Grammatica** che regola l’uso del mark-up.
- **Semantica** che definisce il dominio di applicazione e la funzione del mark-up.

I marcatori vengono inseriti direttamente all’interno del testo a cui viene applicato.
\
Ogni tag a sua volta è costituito da sequenza di caratteri, preceduta da caratteri speciali che la delimitano e che permettono di distinguere facilmente il testo dai marcatori.

### Classificazione dei linguaggi di mark-up

- **Linguaggi procedurali o imperativi**: il mark-up specifica quali operazioni un dato programma deve compiere su un documento elettronico per ottenere una determinata presentazione (Tex, LateX).
- **Linguaggi dichiarativi o descrittivi**: il mark-up descrive la struttura di un documento testuale identificandone i componenti (SGML, HTML, XML).

### Linguaggi dichiarativi

In particolare viene descritta la struttura editoriale costituita da componenti("**content object**") organizzati in modo gerarchico; ad esempio, frontespizio, introduzione, corpo, capotoli, titoli, paragrafi, enfasi, ecc..

### SGML (1986)

**Standard Generalized Markup Language**, è uno standard ISO. Esso è un meccanismo flessibile e portabile per rappresentare documenti elettronici.
\
Un documento SGML comprende oggetti di varie classi chiamati "**elementi**": capitoli, titoli, riferimenti, oggetti grafici, ecc.. SGML identifica gli estremi degli elementi tramite tag iniziali e tag finali, ma non contiene sequenze di istruzioni di formattazione. Gli elementi sono strutturati in una gerarchia: un capitolo contiene un titolo ed una o più sezioni che a loro volta contengono altri elementi, ecc..

### HTML

HTML è un’applicazione/semplificazione di SGML, ovvero un linguaggio per la rappresentazione di un tipo di documento SGML.
\
Tramite HTML è possibile realizzare documenti con una struttura semplice contenenti testo,immagini e contenuti multimediali, oggetti interattivi e
connessioni ipertestuali ad altri documenti.

Oltre a descrivere il contenuto, HTML associa anche significati grafici agli elementi che definisce, ovvero istruzioni più o meno precise su come rendere graficamente gli elementi che definisce.

Standard HTML Living: https://html.spec.whatwg.org/

In questo corso ci riferiremo principalmente a HTML 4.01, il quale è standard ISO dal 2000.
\
Esso prevede tre varianti:

- **Strict**: in cui gli elementi deprecati sono vietati.
- **Transational**: in cui gli elementi deprecati sono ammessi.
- **Frameset**: in cui sono ammessi anche i frame e gli elementi collegati.

## Sintassi

### Tag

I **tag** HTML sono usati per definire il mark-up di elementi HTML. Sono preceduti e seguiti rispettivamente dalle parentesi angolari "<" e ">" e normalmente accoppiati in **start tag** ed **end tag**. Il testo tra i tag è il **contenuto dell'elemento**.

_Esempio_:

```HTML
<p>Contenuto</p>
```

### Grammatica

HTML a differenza di SGML è meno rigoroso:

- Ammette elementi senza chiusura come `<br>`.
- I tag non sono case sensitive.
- L’apertura e chiusura di tag annidati può essere “incrociata”
  ```HTML
  <b><i>Testo corsivo grassetto</b></i>
  ```

Tuttavia, conviene seguire degli standard che diventano un obbligo in una versione più rigorosa del linguaggio, **XHTML**:

- Chiudere sempre anche i tag singoli:
  ```HTML
  <br></br>
  oppure in forma sintetica<br/>
  ```
- Scrivere i tag in minuscolo.
- Aprire e chiudere i tag senza incrociarli.

### Entity

HTML definisce un certo numero di entità (**entity**) per rappresentare i caratteri speciali senza incorrere in problemi di codifica. Ad esempio:

```HTML
- &amp; &
- &quot; “
- &lt; <
- &gt; <
- &reg; ®
- &nbsp; (non-breakingspace)
- &Aelig; Æ
- &Aacute; Á
- &Agrave; À
- &Auml; Ä
- &aelig; æ
- &aacute; á
- &agrave; à
- &auml; ä
- &ccedil; ç
- &ntilde; ñ
```

### Attributi

Un elemento può essere dettagliato mediante attributi. Essi sono coppie“nome = valore” contenute nello start tag con una sintassi di questo tipo:

```HTML
<tag attrib1='valore1' attrib2='valore2'>
```

Dove gli apici possono essere indifferentemente singoli o doppi. Se il valore non contiene spazi, allora gli apici possono essere anche omessi.

I colori sono espressi con un nome o in formato RGB `#RRGGBB`.

### Tipi MIME

Lo standard MIME è nato per poter allegare data file ai messaggi di posta elettronica.
\
Oggi è noto come Internet Media Type e rappresenta il tipo di contenuto di un messaggio. Classifica i tipi di contenuto sulla base di una logica a due livelli ed è largamente utilizzata in ambito HTML e delle tecnologie web in generale.

_Esempi_:

`text/plain`: testo semplice.
`text/html`: testo HTML.

### Commenti

```HTML
<!-- Questo è un commento -->
```

## Struttura base di un documento HTML

![struttura_HTML](resources\struttura_HTML.png)

### DTD

Il primo elemento di un documento HTML è la definizione del tipo di documento, cioé del DTD (**Document Type Definition**). Esso serve al browser per identificare le regole di interpretazione e visualizzazione da applicare al documento.

_Esempio_:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" http://www.w3.org/TR/html4/loose.dtd>
```

Analizziamone le parti:

- `HTML`: è il tipo di linguaggio.
- `PUBLIC`: indica che il documento è pubblico
- `-`: indica che le specifiche non sono registrate all'ISO. In caso contrario bisogna sostituire `-` con `+`.
- `W3C`: è l'ente che ha rilasciato le specifiche.
- `DTD HTML 4.01 Transational`: indica la versione di HTML.
- `EN`: è la lingua con cui è scritto il DTD.
- `http://www.w3.org/TR/html4/loose.dtd`: URL delle specifiche.

### Header

L'header di una pagina è identificato dal tag `<head>`. Esso contiene elementi non visualizzati dal browser (informazioni di servizio).
\
Al suo interno si compila con informazioni fornite dai seguenti tag:

- `<title>`: titolo della pagina (viene mostrato nella testata della finestra principale del browser).
- `<meta>`: metadati informazioni utili ad applicazioni esterne, come i motori di ricerca, o al browser, per esempio lingua, codifica dei caratteri utile per la visualizzazione di alfabeti non latini.
- `<base>`: definisce come vengono gestiti i riferimenti relativi nei link.
- `<link>`: collegamenti verso file esterni: CSS, script, icone visualizzabili nella barra degli indirizzi del browser.
- `<script>`: codice eseguibile utilizzato dal documento.
- `<style>`: informazioni di stile (CSS locali).

_Esempio_:

```HTML
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
    <meta name="description" content="Documentation about HTML">
    <meta name="keywords" content="HTML, tags, commands">
    <title>Impariamo l’HTML</title>
    <link href="style.css" rel=stylesheet type="text/css">
</head>
```

#### Elementi `<meta>` <!-- omit from toc -->

Gli elementi di tipo `<meta>` sono caratterizzati da una serie di attributi.
\
Esistono due tipi di elementi meta, distinguibili dal primo attributo: `http-equiv` o `name`.

#### Elementi `<meta>` di tipo `http-equiv` <!-- omit from toc -->

Gli elementi di tipo `http-equiv` danno informazioni al browser su come gestire la pagina.
\
Hanno una struttura di questo tipo:

```HTML
<meta http-equiv=nome content=valore>
```

Elenchiamo alcuni attributi utili:

- `refresh`: indica che la pagina deve essere ricaricata dopo un numero di secondi definito dall’attributo content:
  ```HTML
  <meta http-equiv=refresh content=45>
  ```
- `expires`: stabilisce una data scadenza (fine validità) per il documento:
  ```HTML
  <meta http-equiv=expires content="Tue, 20 Aug 1996 14:25:27 GMT">
  ```
- `content` type: definisce il tipo di dati contenuto nella pagina (di solito il tipo MIME `text/html`):
  ```HTML
  <meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1">
  ```

#### Elementi `<meta>` di tipo `name` <!-- omit from toc -->

Gli elementi di tipo `name` forniscono informazioni utili ma non critiche.
\
Hanno una struttura di questo tipo:

```HTML
<meta name=nome content=valore>
```

Elenchiamo alcuni attributi utili:

- `author`: autore della pagina:
  ```HTML
  <meta name=author content='John Smith'>
  ```
- `description`: descrizione della pagina:
  ```HTML
  <meta name=description content="Home page UNIMORE">
  ```
- `copyright`: indica che la pagina è protetta da un diritto d'autore:
  ```HTML
  <meta name=copyright content="Copyright 2009,John Smith">
  ```
- `keywords`: lista di parole chiave separate da virgole, usate dai motori di ricerca:
  ```HTML
  <meta name=keywords lang="en" content="computer documentation, computers, computer help">
  ```
- `date`: data di creazione del documento:
  ```HTML
  <meta name="date" content="2008-05-07T09:10:56+00:00">
  ```

### Body

Il tag `<body>` delimita il corpo del documento. Esso contiene la parte che viene mostrata dal browser ed ammette diversi attributi, tra cui:
- `background = uri`: definisce l’URI di una immagine da usare comesfondo per la pagina.
- `text = color`: definisce il colore del testo.
- `bgcolor = color`: in alternativa a `background` definisce il colore di sfondo della pagina.
- `lang = linguaggio`: definisce il linguaggio utilizzato nella pagina; ad esempio: `language="it"`.

_Esempio_:

```HTML
<body>
    <h1>Titolo</h1>
    <p>Questo &eacute; un paragrafo completo di un documento.</p>
    <p>Un altro paragrafo<br>conuna capo</p>
    <hr>
    <p>Esempio di lista puntata, la lista della spesa:</p>
    <ul>
        <li>Pane</li>
        <li>Latte</li>
        <li>Prosciutto</li>
        <li>Formaggio</li>
    </ul>
</body>
```
