# Refactoring


## Stap 1: Controllers naar `App`

Mogelijk heb je nu de Controllers in de *namespace* (en directory) `Framework` gezet, maar feitelijk is dit informatie die thuishoort in de `App`: dat is immers wat anders is voor elke applicatie die gebruik maakt van ons mooie framework. 

Verplaats daarom de Controllers naar de directory `App` (in de directory `Controllers`). Pas de betreffende code aan, zodat het framework wel blijft werken. In de volgende iteratie zullen we reflectie gebruiken om de controllers te instantiëren.



