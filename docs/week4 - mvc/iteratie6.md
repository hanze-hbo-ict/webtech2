# Iteratie 6: de klasse `Route`

Als laatste stap in deze week moeten we er nog voor zorgen dat de *routes* beter worden gematcht. Vooralsnog gebruikten we een hard string als key in de klasse `Route`, maar da's natuurlijk niet handig. Er kan natuurlijk veel meer in zo'n route zitten terwijl het door dezelde controller moet worden afgehandeld. Bekijk de volgende voorbeelden:


methode | route | match | controller
---|---|---|---
`GET` | `/welkom/piet` | `/welkom` | `WelkomController`
`GET` | `/welkom/Chantal` | `/welkom` | `WelkomController`
`POST` | `/welkom/piet` | `/welkom` | `WelkomController`

Om al deze mogelijke paden op te kunnen vangen, is het nodig dat we een nieuwe klasse `Route` maken, die op basis van een aantal gegevens bepaalt of de opgevraagde URI past bij deze route. Bestudeer het onderstaande klassediagram:

![De verhouding tussen `Route` en `Router`](../imgs/route-router.png)
