# Introductie

De Stichting Kinderhouse heeft behoefte aan een nieuwe website omdat de oude gebaseerd op een niet meer werkende versie van het CMS Joomla (versie 1.5), een update naar een nieuwe versie is erg lastig en bovendien is er nauwelijks behoefte aan de dynamiek en de functies van een CMS.

Om de kosten te drukken van de webhosting is op zoek gegaan naar een goedkoper alternatief en dat is gevonden in de vorm van een aantal gratis diensten die in combinatie de mogelijkheid bieden om een website te genereren obv een aantal in markdown opgestelde bronbestanden. De bronbestanden worden opgeslagen in Github zodat broncode beheerd kan worden in een sourcecode repository. Voor de generatie van de statische html wordt gebruik gemaakt van een static site generator in de vorm van het opensource product mkdocs.
Voor de hosting wordt gebruik gemaakt van de gratis versie van **netlify**, deze ondersteunt een continuous delivery werkwijze waarbij automatisch uit de code repository bij een aanpassing de nieuwe site wordt gegenereerd en beschikbaar gemaakt en het niet noodzakelijk is om zelf mkdocs beschikbaar te hebben.

# Overzicht oplossingen

* markdown gebaseerde content
* [mkdocs](http://mkdocs.org) als basis voor de generatie beschikbaar gesteld op
* [netlify](http://www.netlify.io) webhosting provider, free plan, continuous delivery gekoppeld aan
* github repo met [sourcecode](http://www.github.com/rickpeters/kinderhouse)

## Netlify

Netlify is een moderne webhosting oplossing gericht op het hosten van websites gebaseerd op statische html. De sourcecode van de website wordt beheerd in een code repository en door de ondersteuning van diverse static-site generators wordt automatisch (continuous delivery) een website gegenereerd en gepubliceerd op netlify.

De login van Netlify is gekoppeld aan mijn github account en aan de code repository voor de website.

* [Beheerportaal Netlify](https://app.netlify.com)

## Byte internetprovider

Stichting Kinderhouse heeft nu een (relatief duur) webhosting product bij de nederlandse provider **Byte**.

* [Beheerportaal Byte](https://service.byte.nl)

Dit webhosting abonnement kan worden afgebouwd omdat uitsluitend de **domeinnaamregistratie* van belang is. De domeinnaamregistratie kan evt. worden overgebracht naar een nieuwe hostingprovider (bijv. www.mijndomein.nl).

Voor nu gaan we gebruik maken van de **Netlify** optie om een externe DNS provider te koppelen en gaan we in het **Byte** beheerportaal instellen dat de DNS entry voor www.kinderhouse.nl doorgezet moet worden naar netlify.

### DNS koppeling Byte naar Netlify

Hiervoor zijn de instructies van Netlify gevolgd op [Netlify Custom Domains](https://www.netlify.com/docs/custom-domains/).

Complicerende factor is dat *www.kinderhouse.nl* een redirect is (presence domain) naar *www.kinderhouse.eu*. Het is de vraag of DNS aanpassing in *www.kinderhouse.nl* zin heeft.

* In *Netlify* dashboard custom DNS domain ingesteld op *www.kinderhouse.nl*
* De *Name* aangepast naar *kinderhouse* zodat het domein *kinderhouse.netlify.com* bestaat
* Een *CNAME* record aangemaakt voor *www.kinderhouse.nl* dat wijst naar *kinderhouse.netlify.com*
* Het bestaande *A-record* voor *www.kinderhouse.nl* verwijderd (wees naar de DNS server van byte zelf)
* Een *CNAME* record aangemaakt voor *www.kinderhouse.eu* dat wijst naar *kinderhouse.netlify.com*
* Het bestaande *A-record* voor *www.kinderhouse.eu* verwijderd (wees naar de DNS server van byte zelf, 82.94.214.144)
* Het bestaande *A-record* voor **.kinderhouse.eu* verwijderd
* Het bestaande *A-record* voor het full domain (apex), *kinderhouse.eu* ingesteld naar *netlify* (104.198.14.52)

www.kinderhouse.nl lijkt inmiddels te werken...

### DNS koppeling mijndomein naar Netlify

* In mijndomein dashboard bij DNS beheer:
* Een *CNAME* record aangemaakt voor *www.kinderhouse.nl* dat wijst naar *kinderhouse.netlify.com*
* Het bestaande *A-record* voor *www.kinderhouse.nl* verwijderd (wees naar de DNS server van mijndomein zelf)

Dat zou genoeg moeten zijn. Nu (13-1-2017) de verhuizing geactiveerd met de juiste autorisatiecode

Als dat werkt dan kunnen de oude abonenmenten alsnog per direct worden opgezegd.

Het commando `dig www.kinderhouse.nl` laat zien hoe de doorverwijzing werkt.
Nog geen *dnssec* geactiveerd.