# Iteratie 4: meer paden

## Stap 1: De klasse Uri 

Bij [de laatste refactoring](../week3%20-%20http/refactor.md) hebben we de klasse `Uri` nog genegeerd - we zeiden dat we daar vooralsnog een string in mochten stoppen. Nu is het tijd om dit stuk code op te ruimen.

Maak, opnieuw in de directory `Http`, een klasse `Uri` die (vanzelfsprekend) de interface `UriInterface` implementeert. Laat phpstorm de null-implementaties van de gewenste methode creëren.

Zoals je kunt lezen is dit een uitwerking van [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986). Bestudeer [de presentatie van het theoriecollege](../files/presentatie-http.pdf) om te weten welke onderdelen van een url corresponderen met welke term uit deze RFC.


!!! Info "URI, URL en URN"
    Er zijn feitelijk drie verschillende termen die horen bij een internetadres: URL, URN en URI. Een URI (Uniform Resource Identifier) is een string van karakters waarmee een naam, een adres of een *resource* op het internet geïdentificeerd kan worden. 

    Een URI (*Uniform Resource Identifier*) is een overkoepelende term voor een string die een specifieke resource identificeert, hetzij door lokalisatie, naamgeving, of beide. Binnen deze categorie vallen URL's (Uniform Resource Locators), die niet alleen *identificeren* maar ook de locatie en het toegangsmechanisme (zoals `http://` of `ftp://`) specificeren. Een URN (*Uniform Resource Name*), tenslotte, is een URI die uitsluitend bedoeld is als permanente naam zonder dat deze een specifieke locatie of toegangsmethode hoeft te bevatten, zoals een ISBN voor boeken. Kortom, elke URL en URN is een URI, maar een URI hoeft niet per se een URL of een URN te zijn.

    Bestudeer eventueel ook [de documentatie op w3.org](https://www.w3.org/TR/uri-clarification/).

![De algemene vorm van een URL](../imgs/uri.png)

Bestudeer de interface om te kijken wat er zoal van de klasse gevraagd wordt. Zoals je ziet heeft deze interface een hele zooi methoden. We gaan die in eerste instantie niet allemaal uitwerken: vooralsnog gebruiken we alleen de methoden die we nodig hebben om relatief eenvoudige requests zoals die hierboven is weergegeven te representeren. Zorg ervoor dat deze gegevens via de constructor van `Uri` worden opgeslagen. Implementeer ook de corresponderende `get`- en `with`-methoden.

!!! Tip
    We slaan de gegevens voor nu in `Uri` nog even op als string. Op een later tijdstip zullen we dit nog moeten aanpassen, omdat niet alle karakters zonder meer in een webadres kunnen voorkomen, én omdat we mogelijk nog een eigen protocol gaan ontwerpen.

Maak vervolgens in `ServerRequest` een instantie aan van `Uri`, waarin de gevraagde gegevens via de waarden uit de betreffende encapsulaties van de superglobals worden opgeslagen. Pas het type van de parameter uit `Request` aan dat deze geen strings meer kan bevatten en geef het zojuist gecreëerde object vanuit `ServerRequest` mee.

