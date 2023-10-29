# Petropia

## Vorwort

Dies ist das offizielle Archiv des Petropia Netzwerks. Dieses Netzwerk war als ein revolutionäres, selbst programmiertes Minecraft Netzwerk gedacht. Leider ging diese Vision nie in Erfüllung, selbst nach etwa 2 Jahren Entwicklung, da das Verhalten der hauptverantwortlichen Leitung keine andere Wahl ließ, als sich von dieser zu trennen, aufgrund von schwerwiegenden Enthüllungen über diese. Dennoch ist diese Organisation als ein Archiv der bisherigen Projekte und als Inspiration für andere Entwickler gedacht.

## Infrastruktur

Die Infrastruktur basierte auf einem 64GB/12-Kern Linux Server. Als Management für die Minecraft Server wurde CloudNet v4 genutzt, dessen API und MessageChannels das Rückgrat aller internen API bildeten. Als Minecraft Server Software wurde Paper 1.20.1 genutzt. Als Proxy wurde Velocity in der neuesten Version verwendet.

## Spielmodi

Im Folgenden sind alle Spielmodi aufgelistet, die fertiggestellt wurden oder noch in Arbeit waren.

### Bingo (fertiggestellt)

Bei Bingo geht es darum, auf einer zufälligen Vanilla-Welt 5 Items in einer Reihe von der Bingokarte zu sammeln. Bingo kann alleine als 8x1 oder in Teams als 4x2 mit PvP gespielt werden. Die Bingokarte wird zufällig generiert, wobei jedem Item ein bestimmter Schwierigkeitsgrad zugewiesen wird. Leichte Items werden durch einen Algorithmus häufiger auf der Karte landen als schwere Items. Jeder Bingoserver konnte mehrere Arenen parallel hosten, was den Aufwand für mehrere Minecraft Server minimierte.

### ChickenLeague (fertiggestellt)

ChickenLeague ist eine Art RocketLeague in Minecraft. Jeder Spieler hat 3 Schläger mit verschiedenen Effekten, mit denen er ein Huhn (den Ball) ins gegnerische Tor befördern muss. Zudem spawnen auf Teppichen spezielle Items, die für mehr Abwechslung sorgen. ChickenLeague kann als 1vs1, 3vs3 oder 5vs5 gespielt werden. Wie bei Bingo, werden auf jedem Server mehrere Arenen gehostet.

### Spacelife (Work in Progress)

Spacelife ist der Survival-Modus des Netzwerkes. Spacelife besteht aus mehreren Komponenten:
- Spawn: Hier können 16x16x16 Blöcke große Shops gemietet, Jobverdienste ausgezahlt und Bau/Farm/Redstone-Inseln betreten werden und vieles mehr.
- Bauwelt: Hier können Gebiete auf einer Vanilla-Welt gekauft und erweitert werden. Jedes Gebiet besitzt diverse Einstellungen und kann mit anderen Spielern geteilt werden.
- Farmwelt: Hier kann auf einer Vanilla-Welt gefarmt werden, und mit Jobs kann Geld verdient werden. Diese Jobs beinhalten z.B. das Abbauen von Blöcken, das Erkunden von Biomen, das Töten von Mobs und vieles mehr.
- Redstonewelt: Hier hätten Farmen gebaut werden sollen. Zur Entwicklung dieser ist es jedoch nie gekommen.

Zudem gibt es ein Kernmodul namens SpaceLifeCore. Dieses hat Inventories serialisiert und gespeichert in MongoDB, teleportiert Spieler serverübergreifend, verwaltet Warps, verwaltet Geld und stellt diverse Grundfunktionen bereit.

## TurtleServer

TurtleServer ist die Kern-API aller Plugins. Diese Kern-API hat z.B.:
- Chatformatierung pro Plugin verwaltet
- Spielerprofile hinterlegt und in MongoDB gespeichert (Namenshistorie, Skins, Freunde, letzter Login, usw.)
- Spielerstatistiken angelegt
- Welten serialisiert und verwaltet (Welten vom Bau-Server werden in der Datenbank gespeichert, asynchron geladen und gespeichert)
- Minigame-Joins vereinheitlicht (mehr dazu unter Minigame-Joins)
- Chat-Eingaben abfragen
- Prefixe und Tablist-Formatierung
- und mehr...

## Minigame Joins

Da wir den Aufwand von mehreren Minecraft-Servern für mehrere Arenen minimieren wollen, werden pro Minecraft-Server mehrere Arenen gehostet. Von diesen Minecraft-Servern können natürlich mehrere existieren. Wenn eine neue Arena erstellt wird, wird zuerst durch TurtleServer die richtige Welt geladen und dann über ein Event die ID und die Art der Arena an alle Lobbies übermittelt. Wenn nun ein Spieler einer Arena beitreten will, wird erst eine Anfrage an den Minigame-Server gestellt, dieser bestätigt diese, merkt sich den Spieler und dessen Arena, danach wird der Spieler auf den Server verbunden und in der entsprechenden Arena zugeteilt. Wenn eine neue Lobby erstellt wird, fragt diese global alle Minigame-Server nach den Arenen ab, diese schicken die Informationen an die Lobby und halten somit den Zustand der Arenen über alle Server konstant.

Dies ist ein Beispiel für die vielen Systeme, die im Hintergrund des Servers arbeiten.
