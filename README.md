Pokémon Versioning
==================

Summary
-------

Given a pokémon version:

1. Switch to the next unevolved pokémon when you make incompatible API changes,
    and
1. Evolve the pokémon when you add functionality in a backwards-compatible
   manner.

Additional labels for pre-release and build metadata are available as extensions
to the Pokémon format.

Introduction
------------

In the world of software management there exists a dreaded place called
"dependency hell." The bigger your system grows and the more packages you
integrate into your software, the more likely you are to find yourself, one
day, in this pit of despair.

In systems with many dependencies, releasing new package versions can quickly
become a nightmare. If the dependency specifications are too tight, you are in
danger of version lock (the inability to upgrade a package without having to
release new versions of every dependent package). If dependencies are
specified too loosely, you will inevitably be bitten by version promiscuity
(assuming compatibility with more future versions than is reasonable).
Dependency hell is where you are when version lock and/or version promiscuity
prevent you from easily and safely moving your project forward.

As a solution to this problem, I propose a simple set of rules and
requirements that dictate how version pokémon are assigned and incremented.
These rules are based on but not necessarily limited to pre-existing
widespread common practices in use in both closed and open-source software.
For this system to work, you first need to declare a public API. This may
consist of documentation or be enforced by the code itself. Regardless, it is
important that this API be clear and precise. Once you identify your public
API, you communicate changes to it with specific increments to your version
number.  Backwards compatible API additions/changes evolve the pokémon, and
backwards incompatible API switch to the next unevolved pokémon.

I call this system "Pokémon Versioning." Under this scheme, version pokémon
and the way they change convey meaning about the underlying code and what has
been modified from one version to the next.


Pokémon Versioning Specification (PokeVer)
------------------------------------------

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).

1. Software using Pokémon Versioning MUST declare a public API. This API
could be declared in the code itself or exist strictly in documentation.
However it is done, it SHOULD be precise and comprehensive.

1. A normal version pokémon MUST take the form of a Pokémon name listed in the Pokedex.
Each pokémon MUST increase numerically. For instance: Bulbasaur -> Ivysaur -> Venusaur.

1. Once a versioned package has been released, the contents of that version
MUST NOT be modified. Any modifications MUST be released as a new version.

1. Pokémon who can level up to learn Self-Destruct are for initial development.
Anything MAY change at any time. The public API SHOULD NOT be considered stable.

1. Bulbasaur first defines the public API. The way in which the version pokémon
is incremented after this release is dependent on this public API and how it
changes.

1. Evolutions MUST occur if new, backwards
compatible functionality is introduced to the public API. It MUST be
evolved if any public API functionality is marked as deprecated. It MAY be
evolved if substantial new functionality or improvements are introduced
within the private code.

1. Pokémon MUST be switched to the new unevolved pokémon if any backwards
incompatible changes are introduced to the public API. It MAY also include evolution
level changes. Evolution MUST be reset when switching to an unevolved version.

1. A pre-release version MAY be denoted by appending a hyphen and a
series of dot separated identifiers immediately following the
version. Identifiers MUST comprise only ASCII alphanumerics and hyphen
[0-9A-Za-z-]. Identifiers MUST NOT be empty. Numeric identifiers MUST
NOT include leading zeroes. Pre-release versions have a lower
precedence than the associated normal version. A pre-release version
indicates that the version is unstable and might not satisfy the
intended compatibility requirements as denoted by its associated
normal version. Examples: Bublesaur-alpha, Bublesaur-alpha.1, Bublesaur-Diglet,
Bublesaur-Diglet.92.

1. Build metadata MAY be denoted by appending a plus sign and a series of dot
separated identifiers immediately following the pokémon or pre-release version.
Identifiers MUST comprise only ASCII alphanumerics and hyphen [0-9A-Za-z-].
Identifiers MUST NOT be empty. Build metadata MUST be ignored when determining
version precedence. Thus two versions that differ only in the build metadata,
have the same precedence. Examples: Bublesaur-alpha+001, Bublesaur+20130313144700,
Bublesaur-beta+exp.sha.5114f85.

1. Precedence refers to how versions are compared to each other when ordered.
Precedence MUST be calculated by separating the version into pokémon
and pre-release identifiers in that order (Build metadata does not figure
into precedence). Precedence is determined by the first difference when
comparing each of these identifiers from left to right as follows: unevolved and evolved versions are always compared according to their Pokedex numbers. Example: Bulbasaur < Charmander <
Charmeleon < Charizard. When pokémon are equal, a pre-release version has
lower precedence than a normal version. Example: Bulbasaur-alpha < Bulbasaur. Precedence
for two pre-release versions with the same pokémon version MUST
be determined by comparing each dot separated identifier from left to right
until a difference is found as follows: identifiers consisting of only digits
are compared numerically and identifiers with letters or hyphens are compared
lexically in ASCII sort order. Numeric identifiers always have lower precedence
than non-numeric identifiers. A larger set of pre-release fields has a higher
precedence than a smaller set, if all of the preceding identifiers are equal.
Example: Bulbasaur-alpha < Bulbasaur-alpha.1 < Bulbasaur-alpha.beta < Bulbasaur-beta <
Bulbasaur-beta.2 < Bulbasaur-beta.11 < Bulbasaur-rc.1 < Bulbasaur.

Backus–Naur Form Grammar for Valid PokeVer Versions
--------------------------------------------------

    <valid PokeVer> ::= <version core>
                     | <version core> "-" <pre-release>
                     | <version core> "+" <build>
                     | <version core> "-" <pre-release> "+" <build>

    <version core> ::= <pokémon> | "Shiny " <pokémon>

    <pokémon> ::= "Bulbasaur" | "Ivysaur" | "Venusaur"
                | "Charmander" | "Charmeleon" | "Charizard"
                | "Squirtle" | "Wartortle" | "Blastoise"
                | "Caterpie" | "Metapod" | "Butterfree"
                | "Weedle" | "Kakuna" | "Beedrill"
                | "Pidgey" | "Pidgeotto" | "Pidgeot"
                | "Rattata" | "Raticate" | "Spearow"
                | "Fearow" | "Ekans" | "Arbok"
                | "Pikachu" | "Raichu" | "Sandshrew"
                | "Sandslash" | "Nidoran♀" | "Nidorina"
                | "Nidoqueen" | "Nidoran♂" | "Nidorino"
                | "Nidoking" | "Clefairy" | "Clefable"
                | "Vulpix" | "Ninetales" | "Jigglypuff"
                | "Wigglytuff" | "Zubat" | "Golbat"
                | "Oddish" | "Gloom" | "Vileplume"
                | "Paras" | "Parasect" | "Venonat"
                | "Venomoth" | "Diglett" | "Dugtrio"
                | "Meowth" | "Persian" | "Psyduck"
                | "Golduck" | "Mankey" | "Primeape"
                | "Growlithe" | "Arcanine" | "Poliwag"
                | "Poliwhirl" | "Poliwrath" | "Abra"
                | "Kadabra" | "Alakazam" | "Machop"
                | "Machoke" | "Machamp" | "Bellsprout"
                | "Weepinbell" | "Victreebel" | "Tentacool"
                | "Tentacruel" | "Geodude" | "Graveler"
                | "Golem" | "Ponyta" | "Rapidash"
                | "Slowpoke" | "Slowbro" | "Magnemite"
                | "Magneton" | "Farfetch'd" | "Doduo"
                | "Dodrio" | "Seel" | "Dewgong"
                | "Grimer" | "Muk" | "Shellder"
                | "Cloyster" | "Gastly" | "Haunter"
                | "Gengar" | "Onix" | "Drowzee"
                | "Hypno" | "Krabby" | "Kingler"
                | "Voltorb" | "Electrode" | "Exeggcute"
                | "Exeggutor" | "Cubone" | "Marowak"
                | "Hitmonlee" | "Hitmonchan" | "Lickitung"
                | "Koffing" | "Weezing" | "Rhyhorn"
                | "Rhydon" | "Chansey" | "Tangela"
                | "Kangaskhan" | "Horsea" | "Seadra"
                | "Goldeen" | "Seaking" | "Staryu"
                | "Starmie" | "Mr. Mime" | "Scyther"
                | "Jynx" | "Electabuzz" | "Magmar"
                | "Pinsir" | "Tauros" | "Magikarp"
                | "Gyarados" | "Lapras" | "Ditto"
                | "Eevee" | "Vaporeon" | "Jolteon"
                | "Flareon" | "Porygon" | "Omanyte"
                | "Omastar" | "Kabuto" | "Kabutops"
                | "Aerodactyl" | "Snorlax" | "Articuno"
                | "Zapdos" | "Moltres" | "Dratini"
                | "Dragonair" | "Dragonite" | "Mewtwo"
                | "Mew" | "Chikorita" | "Bayleef"
                | "Meganium" | "Cyndaquil" | "Quilava"
                | "Typhlosion" | "Totodile" | "Croconaw"
                | "Feraligatr" | "Sentret" | "Furret"
                | "Hoothoot" | "Noctowl" | "Ledyba"
                | "Ledian" | "Spinarak" | "Ariados"
                | "Crobat" | "Chinchou" | "Lanturn"
                | "Pichu" | "Cleffa" | "Igglybuff"
                | "Togepi" | "Togetic" | "Natu"
                | "Xatu" | "Mareep" | "Flaaffy"
                | "Ampharos" | "Bellossom" | "Marill"
                | "Azumarill" | "Sudowoodo" | "Politoed"
                | "Hoppip" | "Skiploom" | "Jumpluff"
                | "Aipom" | "Sunkern" | "Sunflora"
                | "Yanma" | "Wooper" | "Quagsire"
                | "Espeon" | "Umbreon" | "Murkrow"
                | "Slowking" | "Misdreavus" | "Unown"
                | "Wobbuffet" | "Girafarig" | "Pineco"
                | "Forretress" | "Dunsparce" | "Gligar"
                | "Steelix" | "Snubbull" | "Granbull"
                | "Qwilfish" | "Scizor" | "Shuckle"
                | "Heracross" | "Sneasel" | "Teddiursa"
                | "Ursaring" | "Slugma" | "Magcargo"
                | "Swinub" | "Piloswine" | "Corsola"
                | "Remoraid" | "Octillery" | "Delibird"
                | "Mantine" | "Skarmory" | "Houndour"
                | "Houndoom" | "Kingdra" | "Phanpy"
                | "Donphan" | "Porygon2" | "Stantler"
                | "Smeargle" | "Tyrogue" | "Hitmontop"
                | "Smoochum" | "Elekid" | "Magby"
                | "Miltank" | "Blissey" | "Raikou"
                | "Entei" | "Suicune" | "Larvitar"
                | "Pupitar" | "Tyranitar" | "Lugia"
                | "Ho-Oh" | "Celebi" | "Treecko"
                | "Grovyle" | "Sceptile" | "Torchic"
                | "Combusken" | "Blaziken" | "Mudkip"
                | "Marshtomp" | "Swampert" | "Poochyena"
                | "Mightyena" | "Zigzagoon" | "Linoone"
                | "Wurmple" | "Silcoon" | "Beautifly"
                | "Cascoon" | "Dustox" | "Lotad"
                | "Lombre" | "Ludicolo" | "Seedot"
                | "Nuzleaf" | "Shiftry" | "Taillow"
                | "Swellow" | "Wingull" | "Pelipper"
                | "Ralts" | "Kirlia" | "Gardevoir"
                | "Surskit" | "Masquerain" | "Shroomish"
                | "Breloom" | "Slakoth" | "Vigoroth"
                | "Slaking" | "Nincada" | "Ninjask"
                | "Shedinja" | "Whismur" | "Loudred"
                | "Exploud" | "Makuhita" | "Hariyama"
                | "Azurill" | "Nosepass" | "Skitty"
                | "Delcatty" | "Sableye" | "Mawile"
                | "Aron" | "Lairon" | "Aggron"
                | "Meditite" | "Medicham" | "Electrike"
                | "Manectric" | "Plusle" | "Minun"
                | "Volbeat" | "Illumise" | "Roselia"
                | "Gulpin" | "Swalot" | "Carvanha"
                | "Sharpedo" | "Wailmer" | "Wailord"
                | "Numel" | "Camerupt" | "Torkoal"
                | "Spoink" | "Grumpig" | "Spinda"
                | "Trapinch" | "Vibrava" | "Flygon"
                | "Cacnea" | "Cacturne" | "Swablu"
                | "Altaria" | "Zangoose" | "Seviper"
                | "Lunatone" | "Solrock" | "Barboach"
                | "Whiscash" | "Corphish" | "Crawdaunt"
                | "Baltoy" | "Claydol" | "Lileep"
                | "Cradily" | "Anorith" | "Armaldo"
                | "Feebas" | "Milotic" | "Castform"
                | "Kecleon" | "Shuppet" | "Banette"
                | "Duskull" | "Dusclops" | "Tropius"
                | "Chimecho" | "Absol" | "Wynaut"
                | "Snorunt" | "Glalie" | "Spheal"
                | "Sealeo" | "Walrein" | "Clamperl"
                | "Huntail" | "Gorebyss" | "Relicanth"
                | "Luvdisc" | "Bagon" | "Shelgon"
                | "Salamence" | "Beldum" | "Metang"
                | "Metagross" | "Regirock" | "Regice"
                | "Registeel" | "Latias" | "Latios"
                | "Kyogre" | "Groudon" | "Rayquaza"
                | "Jirachi" | "Deoxys" | "Turtwig"
                | "Grotle" | "Torterra" | "Chimchar"
                | "Monferno" | "Infernape" | "Piplup"
                | "Prinplup" | "Empoleon" | "Starly"
                | "Staravia" | "Staraptor" | "Bidoof"
                | "Bibarel" | "Kricketot" | "Kricketune"
                | "Shinx" | "Luxio" | "Luxray"
                | "Budew" | "Roserade" | "Cranidos"
                | "Rampardos" | "Shieldon" | "Bastiodon"
                | "Burmy" | "Wormadam" | "Mothim"
                | "Combee" | "Vespiquen" | "Pachirisu"
                | "Buizel" | "Floatzel" | "Cherubi"
                | "Cherrim" | "Shellos" | "Gastrodon"
                | "Ambipom" | "Drifloon" | "Drifblim"
                | "Buneary" | "Lopunny" | "Mismagius"
                | "Honchkrow" | "Glameow" | "Purugly"
                | "Chingling" | "Stunky" | "Skuntank"
                | "Bronzor" | "Bronzong" | "Bonsly"
                | "Mime Jr." | "Happiny" | "Chatot"
                | "Spiritomb" | "Gible" | "Gabite"
                | "Garchomp" | "Munchlax" | "Riolu"
                | "Lucario" | "Hippopotas" | "Hippowdon"
                | "Skorupi" | "Drapion" | "Croagunk"
                | "Toxicroak" | "Carnivine" | "Finneon"
                | "Lumineon" | "Mantyke" | "Snover"
                | "Abomasnow" | "Weavile" | "Magnezone"
                | "Lickilicky" | "Rhyperior" | "Tangrowth"
                | "Electivire" | "Magmortar" | "Togekiss"
                | "Yanmega" | "Leafeon" | "Glaceon"
                | "Gliscor" | "Mamoswine" | "Porygon-Z"
                | "Gallade" | "Probopass" | "Dusknoir"
                | "Froslass" | "Rotom" | "Uxie"
                | "Mesprit" | "Azelf" | "Dialga"
                | "Palkia" | "Heatran" | "Regigigas"
                | "Giratina" | "Cresselia" | "Phione"
                | "Manaphy" | "Darkrai" | "Shaymin"
                | "Arceus" | "Victini" | "Snivy"
                | "Servine" | "Serperior" | "Tepig"
                | "Pignite" | "Emboar" | "Oshawott"
                | "Dewott" | "Samurott" | "Patrat"
                | "Watchog" | "Lillipup" | "Herdier"
                | "Stoutland" | "Purrloin" | "Liepard"
                | "Pansage" | "Simisage" | "Pansear"
                | "Simisear" | "Panpour" | "Simipour"
                | "Munna" | "Musharna" | "Pidove"
                | "Tranquill" | "Unfezant" | "Blitzle"
                | "Zebstrika" | "Roggenrola" | "Boldore"
                | "Gigalith" | "Woobat" | "Swoobat"
                | "Drilbur" | "Excadrill" | "Audino"
                | "Timburr" | "Gurdurr" | "Conkeldurr"
                | "Tympole" | "Palpitoad" | "Seismitoad"
                | "Throh" | "Sawk" | "Sewaddle"
                | "Swadloon" | "Leavanny" | "Venipede"
                | "Whirlipede" | "Scolipede" | "Cottonee"
                | "Whimsicott" | "Petilil" | "Lilligant"
                | "Basculin" | "Sandile" | "Krokorok"
                | "Krookodile" | "Darumaka" | "Darmanitan"
                | "Maractus" | "Dwebble" | "Crustle"
                | "Scraggy" | "Scrafty" | "Sigilyph"
                | "Yamask" | "Cofagrigus" | "Tirtouga"
                | "Carracosta" | "Archen" | "Archeops"
                | "Trubbish" | "Garbodor" | "Zorua"
                | "Zoroark" | "Minccino" | "Cinccino"
                | "Gothita" | "Gothorita" | "Gothitelle"
                | "Solosis" | "Duosion" | "Reuniclus"
                | "Ducklett" | "Swanna" | "Vanillite"
                | "Vanillish" | "Vanilluxe" | "Deerling"
                | "Sawsbuck" | "Emolga" | "Karrablast"
                | "Escavalier" | "Foongus" | "Amoonguss"
                | "Frillish" | "Jellicent" | "Alomomola"
                | "Joltik" | "Galvantula" | "Ferroseed"
                | "Ferrothorn" | "Klink" | "Klang"
                | "Klinklang" | "Tynamo" | "Eelektrik"
                | "Eelektross" | "Elgyem" | "Beheeyem"
                | "Litwick" | "Lampent" | "Chandelure"
                | "Axew" | "Fraxure" | "Haxorus"
                | "Cubchoo" | "Beartic" | "Cryogonal"
                | "Shelmet" | "Accelgor" | "Stunfisk"
                | "Mienfoo" | "Mienshao" | "Druddigon"
                | "Golett" | "Golurk" | "Pawniard"
                | "Bisharp" | "Bouffalant" | "Rufflet"
                | "Braviary" | "Vullaby" | "Mandibuzz"
                | "Heatmor" | "Durant" | "Deino"
                | "Zweilous" | "Hydreigon" | "Larvesta"
                | "Volcarona" | "Cobalion" | "Terrakion"
                | "Virizion" | "Tornadus" | "Thundurus"
                | "Reshiram" | "Zekrom" | "Landorus"
                | "Kyurem" | "Keldeo" | "Meloetta"
                | "Genesect" | "Chespin" | "Quilladin"
                | "Chesnaught" | "Fennekin" | "Braixen"
                | "Delphox" | "Froakie" | "Frogadier"
                | "Greninja" | "Bunnelby" | "Diggersby"
                | "Fletchling" | "Fletchinder" | "Talonflame"
                | "Scatterbug" | "Spewpa" | "Vivillon"
                | "Litleo" | "Pyroar" | "Flabébé"
                | "Floette" | "Florges" | "Skiddo"
                | "Gogoat" | "Pancham" | "Pangoro"
                | "Furfrou" | "Espurr" | "Meowstic"
                | "Honedge" | "Doublade" | "Aegislash"
                | "Spritzee" | "Aromatisse" | "Swirlix"
                | "Slurpuff" | "Inkay" | "Malamar"
                | "Binacle" | "Barbaracle" | "Skrelp"
                | "Dragalge" | "Clauncher" | "Clawitzer"
                | "Helioptile" | "Heliolisk" | "Tyrunt"
                | "Tyrantrum" | "Amaura" | "Aurorus"
                | "Sylveon" | "Hawlucha" | "Dedenne"
                | "Carbink" | "Goomy" | "Sliggoo"
                | "Goodra" | "Klefki" | "Phantump"
                | "Trevenant" | "Pumpkaboo" | "Gourgeist"
                | "Bergmite" | "Avalugg" | "Noibat"
                | "Noivern" | "Xerneas" | "Yveltal"
                | "Zygarde" | "Diancie" | "Hoopa"

    <pre-release> ::= <dot-separated pre-release identifiers>

    <dot-separated pre-release identifiers> ::= <pre-release identifier>
                                              | <pre-release identifier> "." <dot-separated pre-release identifiers>

    <build> ::= <dot-separated build identifiers>

    <dot-separated build identifiers> ::= <build identifier>
                                        | <build identifier> "." <dot-separated build identifiers>

    <pre-release identifier> ::= <alphanumeric identifier>
                               | <numeric identifier>

    <build identifier> ::= <alphanumeric identifier>
                         | <digits>

    <alphanumeric identifier> ::= <non-digit>
                                | <non-digit> <identifier characters>
                                | <identifier characters> <non-digit>
                                | <identifier characters> <non-digit> <identifier characters>

    <numeric identifier> ::= "0"
                           | <positive digit>
                           | <positive digit> <digits>

    <identifier characters> ::= <identifier character>
                              | <identifier character> <identifier characters>

    <identifier character> ::= <digit>
                             | <non-digit>

    <non-digit> ::= <letter>
                  | "-"

    <digits> ::= <digit>
               | <digit> <digits>

    <digit> ::= "0"
              | <positive digit>

    <positive digit> ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

    <letter> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J"
               | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T"
               | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d"
               | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n"
               | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
               | "y" | "z"


Why Use Pokémon Versioning?
----------------------------

This is not a new or revolutionary idea. In fact, you probably do something
close to this already. The problem is that "close" isn't good enough. Without
compliance to some sort of formal specification, version pokémon are
essentially useless for dependency management. By giving a name and clear
definition to the above ideas, it becomes easy to communicate your intentions
to the users of your software. Once these intentions are clear, flexible (but
not too flexible) dependency specifications can finally be made.

A simple example will demonstrate how Pokémon Versioning can make dependency
hell a thing of the past. Consider a library called "Firetruck." It requires a
Pokémonally Versioned package named "Ladder." At the time that Firetruck is
created, Ladder is at version Charmeleon. Since Firetruck uses some functionality
that was first introduced in Charmeleon, you can safely specify the Ladder
dependency as greater than or equal to Charmeleon but less than Squirtle. Now, when
Ladder version Charizard becomes available, you can release them to your
package management system and know that they will be compatible with existing
dependent software.

As a responsible developer you will, of course, want to verify that any
package upgrades function as advertised. The real world is a messy place;
there's nothing we can do about that but be vigilant. What you can do is let
Pokémon Versioning provide you with a sane way to release and upgrade
packages without having to roll new versions of dependent packages, saving you
time and hassle.

If all of this sounds desirable, all you need to do to start using Pokémon
Versioning is to declare that you are doing so and then follow the rules. Link
to this website from your README so others know the rules and can benefit from
them.


FAQ
---

### How should I deal with revisions in the Self-Destruct initial development phase?

The simplest thing to do is start your initial development release at Geodude
and then evolve for each subsequent release.

### How do I know when to release Bulbasaur?

If your software is being used in production, it should probably already be
Bulbasaur. If you have a stable API on which users have come to depend, you should
be Bulbasaur. If you're worrying a lot about backwards compatibility, you should
probably already be Bulbasaur.

### Doesn't this discourage rapid development and fast iteration?

unevolved version zero is all about rapid development. If you're changing the API
every day you should either still be in explosive versions or on a separate
development branch working on the next unevolved version.

### If even the tiniest backwards incompatible changes to the public API require a unevolved version bump, won't I end up at version MewTwo very rapidly?

This is a question of responsible development and foresight. Incompatible
changes should not be introduced lightly to software that has a lot of
dependent code. The cost that must be incurred to upgrade can be significant.
Having to bump unevolved versions to release incompatible changes means you'll
think through the impact of your changes, and evaluate the cost/benefit ratio
involved.

### Documenting the entire public API is too much work!

It is your responsibility as a professional developer to properly document
software that is intended for use by others. Managing software complexity is a
hugely important part of keeping a project efficient, and that's hard to do if
nobody knows how to use your software, or what methods are safe to call. In
the long run, Pokémon Versioning, and the insistence on a well defined public
API can keep everyone and everything running smoothly.

### What do I do if I accidentally release a backwards incompatible change as a evolved version?

As soon as you realize that you've broken the Pokémon Versioning spec, fix
the problem and release a new evolved version that corrects the problem and
restores backwards compatibility. Even under this circumstance, it is
unacceptable to modify versioned releases. If it's appropriate,
document the offending version and inform your users of the problem so that
they are aware of the offending version.

### What should I do if I update my own dependencies without changing the public API?

That would be considered compatible since it does not affect the public API.
Software that explicitly depends on the same dependencies as your package
should have their own dependency specifications and the author will notice any
conflicts. Determining whether the change is a evolved or unevolved level
modification depends on whether you updated your dependencies in order to fix
a bug or introduce new functionality. I would usually expect additional code
for the latter instance, in which case it's obviously an evolve level increment.

### What if I inadvertently alter the public API in a way that is not compliant with the version pokémon change (i.e. the code incorrectly introduces a unevolved breaking change in an evolved release)

Use your best judgment. If you have a huge audience that will be drastically
impacted by changing the behavior back to what the public API intended, then
it may be best to perform a unevolved version release. Remember, Pokémon Versioning is all
about conveying meaning by how the version pokémon changes. If these changes
are important to your users, use the version pokémon to inform them.

### How should I handle deprecating functionality?

Deprecating existing functionality is a normal part of software development and
is often required to make forward progress. When you deprecate part of your
public API, you should do two things: (1) update your documentation to let
users know about the change, (2) issue a new evolved release with the deprecation
in place. Before you completely remove the functionality in a new unevolved release
there should be at least one evolved release that contains the deprecation so
that users can smoothly transition to the new API.

### Does PokeVer have a size limit on the version string?

No, but use good judgment. A 255 character version string is probably overkill,
for example. Also, specific systems may impose their own limits on the size of
the string.

### Is "v1.2.3" a Pokémon version?

No, "v1.2.3" is not a Pokémon version. Prefixing a Pokémon version
with a "v" is a common way (in English) to indicate it is a version pokémon.
Abbreviating "version" as "v" is often seen with version control. Example:
`git tag vBulbasaur -m "Release version Bulbasaur"`, in which case "vBulbasaur" is a tag
name and the Pokémon version is "Bulbasaur".

### What do I do if I run out of evolutions for a pokémon?

Professional software developers do not often break things frequently enough
for this to be a common occurrence. However if it does become necessary, you
may use the "Shiny" prefix, cycling back to the most recent unevolved pokémon.
For example, Venusaur can be replaced with Shiny Bulbasaur if necessary.
As a last resort, move on to the next unevolved pokémon as you normally would
 for a breaking change.

About
-----

The Pokémon Versioning specification is authored by [Chris Andrejewski](http://chrisandrejewski.com/), inventor of versioning spec trolling and
cofounder of [@compooter on Twitter](https://twitter.com/compooter).

If you'd like to leave feedback, please [open an issue on
GitHub](https://github.com/andrejewski/pokever/issues).


License
-------

Creative Commons - CC BY 3.0
http://creativecommons.org/licenses/by/3.0/
