# Refactoring

Nu we een eenvoudige round-trip hebben kunnen maken, is het tijd om de code wat te verbeteren. Omdat we in deze eerste iteratie eigenlijk maar maar met drie klassen hebben gewerkt, hoeven we alleen deze maar te bekijken.

## De klassen `ServerRequest`, `Request` en `Message`

De belangrijkste omissie die we in deze iteratie hebben laten liggen is dat we de exacte constructors van de superklassen van `ServerRequest` nog hebben genegeerd. Bestudeer de methoden uit de interface van de verschillende klassen om te kijken welke informatie in welke klasse moet worden opgeslagen. Zo heeft bijvoorbeeld de klasse `Request` een methode `getMethod()`, die de betreffende [htt-method]() (`POST`, `GET`, `UPDATE`, `DELETE`, ...) als string teruggeeft.

Omdat we (nog) geen instantie van `Request` direct aanmaken, moeten we deze informatie tijdens het aanmaken van de subklasse, `ServerRequest` meegeven. Gelukkig hebben we in deze subklasse de beschikking over deze informatie: die kun je vinden in `$_SERVER['REQUEST_METHOD']`, die juist door deze klasse wordt geëncapsuleerd. 

Een ander voorbeeld: de klasse `Message` heeft een methode `getHeaders()` die alle http-headers als array teruggeeft. Ook deze gegevens moeten via `ServerRequest` en `Request` aan de `Message` doorgegeven worden. Let op: alle http-headers zitten in de superglobal `$_SERVER`: dat zijn namelijk die gegevens uit deze array waarvan de *naam* (de *key*) met `HTTP_` begint. Je zult deze gegevens dus uit deze array moeten filteren en de liggende streepjes vervangen door koppeltekens.

!!! Tip
    Je kunt natuurlijk gewoon door de `$_SERVER` heen itereren, checken of de sleutel (*key*) begint met `HTTP_` en in dat geval de corresponderende waarde gebruiken. Maar je kunt ook gebruik maken van de methoden [`array_filter()`]() en [`array_map`]().

Het is een klein beetje arbitrair waar je deze wijzigingen exact uitvoert, maar het is het netste als je dit doet in `ServerRequest`, zodat zowel `Request` als `Message` gewoon een nette array ontvangen.

!!! Warning
    We hebben het hierboven wel steeds over de superglobal, maar het is natuurlijk niet de bedoeling dat je *deze* gebruikt. Je moet telkens de geëncapsuleerde versie uit `ServerRequest` gebruiken.

Doe dit voor alle velden uit de verschillende klassen waarvan je ziet dat ze die gegevens nodig hebben. Als het goed is zul je zien dat de constructor van `ServerRequest` vijf superglobals nodig heeft. Let op dat de parameter `$body` in `Message` ook leeg kan zijn: een `GET`-request heeft in de regel geen body, net zo min als een `POST`-response.

!!! Tip
    De klasse `Request` heeft een property van het type `UriInterface` nodig. Het is prima om die voor nu even optioneel te maken, of om hier ook een string in te kunnen zetten (gebruik hiervoor `UriInterface|string`): in [de volgende iteratie] gaan we hier uitgebreid op in.

## De klasse `Response`

Ga door de methoden van `Response` om te onderzoeken welke velden deze klasse allemaal nodig heeft (feitelijk op dezelfde manier als hierboven). Let er ook op dat deze klasse ook overerft van `Message`, dus ook hier moeten we de constructor aanpassen zodat die `Message` op een goede manier wordt aangemaakt. 

Zorg voor logische standaardwaarden van alle velden (`status-code` = 200, bijvoorbeeld, of `protocol-version` = "1.1"): dat maakt het aanmaken van instanties van deze klasse een stuk eenvoudiger.

In de vorige iteratie hebben we gewoon `echo $response->getBody()` gebruikt om de response naar de client te sturen. Het is veel netter om hier een speciale statische functie voor te maken, die een `Response`-object ontvangt, de body hiervan uitprint en niet teruggeeft: dan hebben we fijn één specifieke plek waar we de boel naar de client terugsturen en weten we waar we op een later tijdstip moeten zijn om (bijvoorbeeld) de headers mee te sturen.

Maak in `Response` een methode `static send(Response $response)` die precies dit doet. Voor nu is het nog voldoende om gewoon die body uit te printen – op een later tijdstip gaan we dit nog wel (significant) aanpassen.

## Laatste stappen

Implementeer de methoden die nu nog commentaar hebben dat ze geïmplementeerd moeten worden.

Pas de *frontcontroller* aan, zodat deze gebruik maakt van de statische methode `Response::send()`.

Check opnieuw de werking van het geheel. Bij een refactorslag is het niet de bedoeling dat je nieuwe functionaliteit toevoegt, dus als het goed is, is er niks veranderd.

