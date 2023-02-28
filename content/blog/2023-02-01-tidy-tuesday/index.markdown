---
title: tidy tuesday
subtitle: 
author: Jessy Gleeson
date: '2023-02-01'
slug: []
categories: []
tags: []
layout: single-sidebar
---

Load relevant libraries

```r
library(tidytuesdayR)
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   0.3.5 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)
```

```
## Loading required package: timechange
## 
## Attaching package: 'lubridate'
## 
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(stringr)
library(rmapshaper)
```

```
## Registered S3 method overwritten by 'geojsonlint':
##   method         from 
##   print.location dplyr
```

```r
library(sf)
```

```
## Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE
```



```r
data <- read.csv("/Users/jessygleeson/Rstudio-Files/Pet-Cats-Australia.csv")
ref_data <- read.csv("/Users/jessygleeson/Rstudio-Files/Pet-Cats-Australia-reference-data.csv")
```

# A quick glance at the data

```r
print(ref_data)
```

```
##                       tag.id             animal.id animal.taxon
## 1                   SnowyTag                 Snowy  Felis catus
## 2                      Gomez                 Gomez  Felis catus
## 3                    Frizzle               Frizzle  Felis catus
## 4                  MonMonTag                MonMon  Felis catus
## 5                   BuddyTag                 Buddy  Felis catus
## 6                   TigerTag                 Tiger  Felis catus
## 7                   JamieTag                 Jamie  Felis catus
## 8   BruceWillisPussingtonTag BruceWillisPussington  Felis catus
## 9                SkittlesTag              Skittles  Felis catus
## 10               PrincessTag              Princess  Felis catus
## 11                  LoftyTag                 Lofty  Felis catus
## 12                 ZazzieTag                Zazzie  Felis catus
## 13                CrumpetTag               Crumpet  Felis catus
## 14                   TobyTag                  Toby  Felis catus
## 15                 IsabelTag                Isabel  Felis catus
## 16                  PennyTag                 Penny  Felis catus
## 17                  MistyTag                 Misty  Felis catus
## 18                 SookieTag                Sookie  Felis catus
## 19                 PippinTag                Pippin  Felis catus
## 20                 SqueakTag                Squeak  Felis catus
## 21                 AnakinTag                Anakin  Felis catus
## 22                  ErnieTag                 Ernie  Felis catus
## 23                  WallyTag                 Wally  Felis catus
## 24                  BellaTag                 Bella  Felis catus
## 25                    JaxTag                   Jax  Felis catus
## 26                 RamsesTag                Ramses  Felis catus
## 27                  TiggaTag                 Tigga  Felis catus
## 28             FantaPantsTag            FantaPants  Felis catus
## 29                MatildaTag               Matilda  Felis catus
## 30                  TilenTag                 Tilen  Felis catus
## 31                    BelTag                   Bel  Felis catus
## 32                  Lucy2Tag                 Lucy2  Felis catus
## 33                  OllieTag                 Ollie  Felis catus
## 34                CharlieTag               Charlie  Felis catus
## 35                  SissyTag                 Sissy  Felis catus
## 36                 LouiseTag                Louise  Felis catus
## 37                 JasperTag                Jasper  Felis catus
## 38              LightningTag             Lightning  Felis catus
## 39                  MandyTag                 Mandy  Felis catus
## 40                  SaffyTag                 Saffy  Felis catus
## 41                 ShellyTag                Shelly  Felis catus
## 42                   RubyTag                  Ruby  Felis catus
## 43                 ThomasTag                Thomas  Felis catus
## 44                  MystiTag                 Mysti  Felis catus
## 45                  CostaTag                 Costa  Felis catus
## 46                MoochieTag               Moochie  Felis catus
## 47               SapphireTag              Sapphire  Felis catus
## 48                 RaizorTag                Raizor  Felis catus
## 49                NekoMeiTag               NekoMei  Felis catus
## 50                  MillyTag                 Milly  Felis catus
## 51                SlasherTag               Slasher  Felis catus
## 52         DetectiveTeddyTag        DetectiveTeddy  Felis catus
## 53                 Misty2Tag                Misty2  Felis catus
## 54               BigFellaTag              BigFella  Felis catus
## 55             LadyVelvetTag            LadyVelvet  Felis catus
## 56                    MaxTag                   Max  Felis catus
## 57                   MikaTag                  Mika  Felis catus
## 58                   AtikTag                  Atik  Felis catus
## 59                 KallieTag                Kallie  Felis catus
## 60                  PixieTag                 Pixie  Felis catus
## 61                  GenieTag                 Genie  Felis catus
## 62                 Milly2Tag                Milly2  Felis catus
## 63                 SkittyTag                Skitty  Felis catus
## 64                   Max2Tag                  Max2  Felis catus
## 65                  KittyTag                 Kitty  Felis catus
## 66                  DrakeTag                 Drake  Felis catus
## 67                DelilahTag               Delilah  Felis catus
## 68                 KramerTag                Kramer  Felis catus
## 69                  MollyTag                 Molly  Felis catus
## 70                  AdeleTag                 Adele  Felis catus
## 71                  HenryTag                 Henry  Felis catus
## 72                    RioTag                   Rio  Felis catus
## 73                 SmokeyTag                Smokey  Felis catus
## 74                  SushiTag                 Sushi  Felis catus
## 75                 MidoriTag                Midori  Felis catus
## 76                  Toby2Tag                 Toby2  Felis catus
## 77                    TomTag                   Tom  Felis catus
## 78                  MingoTag                 Mingo  Felis catus
## 79                  SassyTag                 Sassy  Felis catus
## 80                 BanditTag                Bandit  Felis catus
## 81                 HunterTag                Hunter  Felis catus
## 82                 DarkieTag                Darkie  Felis catus
## 83                  MangoTag                 Mango  Felis catus
## 84                 CasperTag                Casper  Felis catus
## 85                   MiloTag                  Milo  Felis catus
## 86              CharlotteTag             Charlotte  Felis catus
## 87                 WizzerTag                Wizzer  Felis catus
## 88                 MickeyTag                Mickey  Felis catus
## 89                 JohnnyTag                Johnny  Felis catus
## 90             MapleSyrupTag            MapleSyrup  Felis catus
## 91                MugginsTag               Muggins  Felis catus
## 92                CaspianTag               Caspian  Felis catus
## 93                   LiefTag                  Lief  Felis catus
## 94                  RufusTag                 Rufus  Felis catus
## 95                    UnoTag                   Uno  Felis catus
## 96                 NellieTag                Nellie  Felis catus
## 97                    BobTag                   Bob  Felis catus
## 98                 FergieTag                Fergie  Felis catus
## 99                MarblesTag               Marbles  Felis catus
## 100                 ClawdTag                 Clawd  Felis catus
## 101                RodneyTag                Rodney  Felis catus
## 102                 NinjaTag                 Ninja  Felis catus
## 103                 Lucy3Tag                 Lucy3  Felis catus
## 104                MarthaTag                Martha  Felis catus
## 105                  MomoTag                  Momo  Felis catus
## 106              LilliputTag              Lilliput  Felis catus
## 107                LucimsTag                Lucims  Felis catus
## 108                 TipsyTag                 Tipsy  Felis catus
## 109              MissyMooTag              MissyMoo  Felis catus
## 110                 MintyTag                 Minty  Felis catus
## 111                 TillyTag                 Tilly  Felis catus
## 112                TipsieTag                Tipsie  Felis catus
## 113                 PearlTag                 Pearl  Felis catus
## 114                 RingoTag                 Ringo  Felis catus
## 115          LibertyBelleTag          LibertyBelle  Felis catus
## 116                SmudgeTag                Smudge  Felis catus
## 117                 ScoutTag                 Scout  Felis catus
## 118                  RoxiTag                  Roxi  Felis catus
## 119                 DarcyTag                 Darcy  Felis catus
## 120                   ZakTag                   Zak  Felis catus
## 121                Kitty2Tag                Kitty2  Felis catus
## 122               Millie2Tag               Millie2  Felis catus
## 123                   CatTag                   Cat  Felis catus
## 124               Darcie2Tag                Darcy2  Felis catus
## 125                 BugsyTag                 Bugsy  Felis catus
## 126                StumpyTag                Stumpy  Felis catus
## 127                 RumpyTag                 Rumpy  Felis catus
## 128               EldrickTag               Eldrick  Felis catus
## 129                 LatteTag                 Latte  Felis catus
## 130                MerlotTag                Merlot  Felis catus
## 131                 HollyTag                 Holly  Felis catus
## 132                GeorgeTag                George  Felis catus
## 133               SparkleTag               Sparkle  Felis catus
## 134                 BasilTag                 Basil  Felis catus
## 135               MilfredTag               Milfred  Felis catus
## 136               ZazzieMTag               ZazzieM  Felis catus
## 137                SylviaTag                Sylvia  Felis catus
## 138                   LeoTag                   Leo  Felis catus
## 139                 OscarTag                 Oscar  Felis catus
## 140                PepperTag                Pepper  Felis catus
## 141               JerichoTag               Jericho  Felis catus
## 142               IronManTag               IronMan  Felis catus
## 143                 MoggyTag                 Moggy  Felis catus
## 144                 LouisTag                 Louis  Felis catus
## 145              ThorntonTag              Thornton  Felis catus
## 146                  CleoTag                  Cleo  Felis catus
## 147                 AngelTag                 Angel  Felis catus
## 148          CharlieFrankTag          CharlieFrank  Felis catus
## 149                BungeyTag                Bungey  Felis catus
## 150                   GusTag                   Gus  Felis catus
## 151              PusskinsTag              Pusskins  Felis catus
## 152                 EddieTag                 Eddie  Felis catus
## 153            GoldfingerTag            Goldfinger  Felis catus
## 154           PussyGaloreTag           PussyGalore  Felis catus
## 155                 TashaTag                 Tasha  Felis catus
## 156           ChuckNorrisTag           ChuckNorris  Felis catus
## 157               PhoenixTag               Phoenix  Felis catus
## 158                  JackTag                  Jack  Felis catus
## 159                  CosiTag                  Cosi  Felis catus
## 160                 BootsTag                 Boots  Felis catus
## 161                  YodaTag                  Yoda  Felis catus
## 162                BuddhaTag                Buddha  Felis catus
## 163              BagheeraTag              Bagheera  Felis catus
## 164                 Milo2Tag                 Milo2  Felis catus
## 165                WinnieTag                Winnie  Felis catus
## 166                XanderTag                Xander  Felis catus
## 167                 PatraTag                 Patra  Felis catus
## 168                 PantsTag                 Pants  Felis catus
## 169                  Leo2Tag                  Leo2  Felis catus
## 170                 LillyTag                 Lilly  Felis catus
## 171                 BennyTag                 Benny  Felis catus
## 172                  MussTag                  Muss  Felis catus
## 173                    DCTag                    DC  Felis catus
## 174                 Milo3Tag                 Milo3  Felis catus
## 175                MerlinTag                Merlin  Felis catus
## 176                 TippyTag                 Tippy  Felis catus
## 177                  HaHaTag                  HaHa  Felis catus
## 178                  PaulTag                  Paul  Felis catus
## 179                 Ruby2Tag                 Ruby2  Felis catus
## 180               PantherTag               Panther  Felis catus
## 181               MittensTag               Mittens  Felis catus
## 182                JesterTag                Jester  Felis catus
## 183                 Ruby3Tag                 Ruby3  Felis catus
## 184                   AliTag                   Ali  Felis catus
## 185                StinkyTag                Stinky  Felis catus
## 186                ShadowTag                Shadow  Felis catus
## 187              KikkiDeeTag              KikkiDee  Felis catus
## 188               Jasper2Tag               Jasper2  Felis catus
## 189                  SootyTa                 Sooty  Felis catus
## 190                 RosieTag                 Rosie  Felis catus
## 191              MonaLisaTag              MonaLisa  Felis catus
## 192                 BoofyTag                 Boofy  Felis catus
## 193                Kitty3Tag                Kitty3  Felis catus
## 194                 BruceTag                 Bruce  Felis catus
## 195          LucarioBlackTag          LucarioBlack  Felis catus
## 196              TruffulaTag              Truffula  Felis catus
## 197                Rosie2Tag                Rosie2  Felis catus
## 198                JazzieTag                Jazzie  Felis catus
## 199                  LilaTag                  Lila  Felis catus
## 200           MrLittleCatTag           MrLittleCat  Felis catus
## 201                   AshTag                   Ash  Felis catus
## 202                Kitty4Tag                Kitty4  Felis catus
## 203                 Ruby4Tag                 Ruby4  Felis catus
## 204                 FelixTag                 Felix  Felis catus
## 205                Bella5Tag                Bella5  Felis catus
## 206                Tiger3Tag                Tiger3  Felis catus
## 207                CheekyTag                Cheeky  Felis catus
## 208                  MacaTag                  Maca  Felis catus
## 209                 IvoryTag                 Ivory  Felis catus
## 210                  SoxyTag                  Soxy  Felis catus
## 211                Tilly2Tag                Tilly2  Felis catus
## 212                 HarryTag                 Harry  Felis catus
## 213                Misty3Tag                Misty3  Felis catus
## 214                   BooTag                   Boo  Felis catus
## 215                 GrimmTag                 Grimm  Felis catus
## 216                 BessyTag                 Bessy  Felis catus
## 217                NelsonTag                Nelson  Felis catus
## 218              Marbles2Tag              Marbles2  Felis catus
## 219                   TuxTag                   Tux  Felis catus
## 220                RipleyTag                Ripley  Felis catus
## 221               Merlin2Tag               Merlin2  Felis catus
## 222            MrWhiskersTag            MrWhiskers  Felis catus
## 223              Charlie2Tag              Charlie2  Felis catus
## 224                 HazelTag                 Hazel  Felis catus
## 225                  Leo3Tag                  Leo3  Felis catus
## 226                 Soxy2Tag                 Soxy2  Felis catus
## 227                  TheoTag                  Theo  Felis catus
## 228                GeezerTag                Geezer  Felis catus
## 229                Harry2Tag                Harry2  Felis catus
## 230              MidnightTag              Midnight  Felis catus
## 231                RupertTag                Rupert  Felis catus
## 232               JasmineTag               Jasmine  Felis catus
## 233                  LilyTag                  Lily  Felis catus
## 234                Buddy2Tag                Buddy2  Felis catus
## 235              SibeliusTag              Sibelius  Felis catus
## 236                 TimmyTag                 Timmy  Felis catus
## 237              FlandersTag              Flanders  Felis catus
## 238                   ZoeTag                   Zoe  Felis catus
## 239               Pepper2Tag               Pepper2  Felis catus
## 240              Charlie3Tag              Charlie3  Felis catus
## 241                 SunnyTag                 Sunny  Felis catus
## 242                RascalTag                Rascal  Felis catus
## 243               SchnoozTag               Schnooz  Felis catus
## 244              AmbaRoseTag              AmbaRose  Felis catus
## 245                 TobinTag                 Tobin  Felis catus
## 246               Rupert2Tag               Rupert2  Felis catus
## 247                 AlfieTag                 Alfie  Felis catus
## 248                Felix2Tag                Felix2  Felis catus
## 249                 SammiTag                 Sammi  Felis catus
## 250               PicassoTag               Picasso  Felis catus
## 251                Tiger4Tag                Tiger4  Felis catus
## 252                 MontyTag                 Monty  Felis catus
## 253                AmeliaTag                Amelia  Felis catus
## 254                 Theo2Tag                 Theo2  Felis catus
## 255               SputnikTag               Sputnik  Felis catus
## 256                 KennyTag                 Kenny  Felis catus
## 257                DharmaTag                Dharma  Felis catus
## 258              Charlie4Tag              Charlie4  Felis catus
## 259                ImeldaTag                Imelda  Felis catus
## 260                 TiggyTag                 Tiggy  Felis catus
## 261                 SammyTag                 Sammy  Felis catus
## 262                 FudgeTag                 Fudge  Felis catus
## 263               GeorgieTag               Georgie  Felis catus
## 264               Martha2Tag               Martha2  Felis catus
## 265                 RaffyTag                 Raffy  Felis catus
## 266                Boots2Tag                Boots2  Felis catus
## 267                YorikoTag                Yoriko  Felis catus
## 268                  ZivaTag                  Ziva  Felis catus
## 269               Millie3Tag               Millie3  Felis catus
## 270               BentleyTag               Bentley  Felis catus
## 271              DiamondsTag              Diamonds  Felis catus
## 272                Monty2Tag                Monty2  Felis catus
## 273                  RudiTag                  Rudi  Felis catus
## 274                Bella6Tag                Bella6  Felis catus
## 275               SqueakyTag               Squeaky  Felis catus
## 276                StalkyTag                Stalky  Felis catus
## 277            UnfriendlyTag            Unfriendly  Felis catus
## 278                 BlueyTag                 Bluey  Felis catus
## 279                  ShanTag                  Shan  Felis catus
## 280                StitchTag                Stitch  Felis catus
## 281                  PudsTag                  Puds  Felis catus
## 282                 TommyTag                 Tommy  Felis catus
## 283                  Max3Tag                  Max3  Felis catus
## 284                MoscowTag                Moscow  Felis catus
## 285              MrThomasTag              MrThomas  Felis catus
## 286             MissyMitzTag             MissyMitz  Felis catus
## 287            JessieBeauTag            JessieBeau  Felis catus
## 288               SkoozieTag               Skoozie  Felis catus
## 289               FrankieTag               Frankie  Felis catus
## 290                 AnnieTag                 Annie  Felis catus
## 291                   ZacTag                   Zac  Felis catus
## 292                 PollyTag                 Polly  Felis catus
## 293                  OnyxTag                  Onyx  Felis catus
## 294                 ChaseTag                 Chase  Felis catus
## 295                 AstroTag                 Astro  Felis catus
## 296                  LokiTag                  Loki  Felis catus
## 297                IsobelTag                Isobel  Felis catus
## 298              SquiggleTag              Squiggle  Felis catus
## 299                   OliTag                   Oli  Felis catus
## 300                  SpotTag                  Spot  Felis catus
## 301               Jasper3Tag               Jasper3  Felis catus
## 302                 JeddaTag                 Jedda  Felis catus
## 303                ShmiffTag                Shmiff  Felis catus
## 304               Nelson3Tag               Nelson3  Felis catus
## 305                ClaudeTag                Claude  Felis catus
## 306                MaggieTag                Maggie  Felis catus
## 307                Harry3Tag                Harry3  Felis catus
## 308                Daisy2Tag                Daisy2  Felis catus
## 309                    BoTag                    Bo  Felis catus
## 310                AlbertTag                Albert  Felis catus
## 311                CaddieTag                Caddie  Felis catus
## 312                 KamirTag                 Kamir  Felis catus
## 313               FizzgigTag               Fizzgig  Felis catus
## 314                 ParisTag                 Paris  Felis catus
## 315                Felix3Tag                Felix3  Felis catus
## 316                  Bel2Tag                  Bel2  Felis catus
## 317               MeggsieTag               Meggsie  Felis catus
## 318               Rupert3Tag               Rupert3  Felis catus
## 319                CelineTag                Celine  Felis catus
## 320                 BillyTag                 Billy  Felis catus
## 321                Oscar2Tag                Oscar2  Felis catus
## 322                   BozTag                   Boz  Felis catus
## 323             Truffles2Tag             Truffles2  Felis catus
## 324                Kitty5Tag                Kitty5  Felis catus
## 325                  JinxTag                  Jinx  Felis catus
## 326                ChanelTag                Chanel  Felis catus
## 327       LouisTheBruiserTag       LouisTheBruiser  Felis catus
## 328                 PaulaTag                 Paula  Felis catus
## 329                MrGrayTag                MrGray  Felis catus
## 330              MalachieTag              Malachie  Felis catus
## 331                  GinaTag                  Gina  Felis catus
## 332                Banjo2Tag                Banjo2  Felis catus
## 333              JunipurrTag              Junipurr  Felis catus
## 334                ClancyTag                Clancy  Felis catus
## 335                Tiger5Tag                Tiger5  Felis catus
## 336                TarzanTag                Tarzan  Felis catus
## 337                 Jinx2Tag                 Jinx2  Felis catus
## 338                 BrunoTag                 Bruno  Felis catus
## 339                CollinTag                Collin  Felis catus
## 340               TwinkleTag               Twinkle  Felis catus
## 341                WhiskyTag                Whisky  Felis catus
## 342                  CocoTag                  Coco  Felis catus
## 343        CaptainSoldierTag        CaptainSoldier  Felis catus
## 344               RonaldoTag               Ronaldo  Felis catus
## 345                 BambiTag                 Bambi  Felis catus
## 346                HestorTag                Hestor  Felis catus
## 347                 BanjoTag                 Banjo  Felis catus
## 348                 GarryTag                 Garry  Felis catus
## 349                  AllyTag                  Ally  Felis catus
## 350                Oscar3Tag                Oscar3  Felis catus
## 351                  YogiTag                  Yogi  Felis catus
## 352                  PussTag                  Puss  Felis catus
## 353                 SmallTag                 Small  Felis catus
## 354               ScratchTag               Scratch  Felis catus
## 355                DieselTag                Diesel  Felis catus
## 356               StanleyTag               Stanley  Felis catus
## 357                  Tom2Tag                  Tom2  Felis catus
## 358               TruffleTag               Truffle  Felis catus
## 359                 MissyTag                 Missy  Felis catus
## 360                RafikiTag                Rafiki  Felis catus
## 361              CarmelloTag              Carmello  Felis catus
## 362         MilkoMcCavityTag         MilkoMcCavity  Felis catus
## 363                  Leo4Tag                  Leo4  Felis catus
## 364                 LeilaTag                 Leila  Felis catus
## 365                Kitty6Tag                Kitty6  Felis catus
## 366                SamsonTag                Samson  Felis catus
## 367              Delilah2Tag              Delilah2  Felis catus
## 368             SylvesterTag             Sylvester  Felis catus
## 369                    BBTag                    BB  Felis catus
## 370                 SallyTag                 Sally  Felis catus
## 371                  ElmoTag                  Elmo  Felis catus
## 372               StealthTag               Stealth  Felis catus
## 373                 JonesTag                 Jones  Felis catus
## 374                  MimiTag                  Mimi  Felis catus
## 375              Charlie6Tag              Charlie6  Felis catus
## 376                  Sox2Tag                  Sox2  Felis catus
## 377                 FrankTag                 Frank  Felis catus
## 378                  DanaTag                  Dana  Felis catus
## 379                  GigiTag                  Gigi  Felis catus
## 380                 VenomTag                 Venom  Felis catus
## 381               WigglesTag               Wiggles  Felis catus
## 382                 MagicTag                 Magic  Felis catus
## 383                MowdiiTag                Mowdii  Felis catus
## 384                  MynxTag                  Mynx  Felis catus
## 385                Sooty2Tag                Sooty2  Felis catus
## 386                  SethTag                  Seth  Felis catus
## 387                Henry2Tag                Henry2  Felis catus
## 388                 EestiTag                 Eesti  Felis catus
## 389                 RoseyTag                 Rosey  Felis catus
## 390                  RemyTag                  Remy  Felis catus
## 391                  LunaTag                  Luna  Felis catus
## 392                   TiaTag                   Tia  Felis catus
## 393               Jasper4Tag               Jasper4  Felis catus
## 394                Drake2Tag                Drake2  Felis catus
## 395     GagglesHieronymusTag     GagglesHieronymus  Felis catus
## 396                  BibiTag                  Bibi  Felis catus
## 397                MeeshaTag                Meesha  Felis catus
## 398               FlossieTag               Flossie  Felis catus
## 399                 MultiTag                 Multi  Felis catus
## 400                 FurryTag                 Furry  Felis catus
## 401              SugarRaeTag              SugarRae  Felis catus
## 402                TrixieTag                Trixie  Felis catus
## 403               KeytuynTag               Keytuyn  Felis catus
## 404                KittenTag                Kitten  Felis catus
## 405                  JoeyTag                  Joey  Felis catus
## 406                  Ash2Tag                  Ash2  Felis catus
## 407                 Rudi2Tag                 Rudi2  Felis catus
## 408          SophiePossumTag          SophiePossum  Felis catus
## 409                   RooTag                   Roo  Felis catus
## 410                 RockyTag                 Rocky  Felis catus
## 411               George2Tag               George2  Felis catus
## 412                Tilly3Tag                Tilly3  Felis catus
## 413                FrostyTag                Frosty  Felis catus
## 414                MysticTag                Mystic  Felis catus
## 415               Stumpy2Tag               Stumpy2  Felis catus
## 416               BigglesTag               Biggles  Felis catus
## 417                 Cleo2Tag                 Cleo2  Felis catus
## 418              SparklesTag              Sparkles  Felis catus
## 419     GracieLouFreebushTag     GracieLouFreebush  Felis catus
## 420                Simba2Tag                Simba2  Felis catus
## 421                 ZorraTag                 Zorra  Felis catus
## 422                WeaselTag                Weasel  Felis catus
## 423              BellaBooTag              BellaBoo  Felis catus
## 424                 GizmoTag                 Gizmo  Felis catus
## 425                 RustyTag                 Rusty  Felis catus
## 426               StefanoTag               Stefano  Felis catus
## 427                ZepherTag                Zepher  Felis catus
## 428                 ArielTag                 Ariel  Felis catus
## 429                  EnzoTag                  Enzo  Felis catus
## 430                AlrariTag                Alrari  Felis catus
## 431             MissChiefTag             MissChief  Felis catus
## 432                  LexiTag                  Lexi  Felis catus
## 433                RodgerTag                Rodger  Felis catus
## 434                  TimoTag                  Timo  Felis catus
## 435              Jasmine2Tag              Jasmine2  Felis catus
## 436                  PoloTag                  Polo  Felis catus
## 437                 MaeveTag                 Maeve  Felis catus
## 438            Charlotte2Tag            Charlotte2  Felis catus
## 439                ListerTag                Lister  Felis catus
## 440              GarfieldTag              Garfield  Felis catus
## 441                 BelleTag                 Belle  Felis catus
## 442                GyhjorTag                Gyhjor  Felis catus
##              deploy.on.date         deploy.off.date             animal.comments
## 1   2015-03-03 06:42:09.000 2015-03-10 16:06:57.000    Hunt: 0; prey_p_month: 0
## 2   2015-03-06 15:56:26.000 2015-03-11 14:25:34.000    Hunt: 0; prey_p_month: 0
## 3   2015-03-31 01:32:04.000 2015-04-07 15:57:04.000  Hunt: Yes; prey_p_month: 0
## 4   2015-03-31 01:32:22.000 2015-04-07 15:59:55.000  Hunt: Yes; prey_p_month: 3
## 5   2015-03-31 07:59:33.000 2015-04-08 02:01:21.000  Hunt: Yes; prey_p_month: 1
## 6   2015-04-01 01:32:24.000 2015-04-07 03:54:13.000  Hunt: Yes; prey_p_month: 1
## 7   2015-04-01 01:32:31.000 2015-04-09 15:57:56.000  Hunt: Yes; prey_p_month: 3
## 8   2015-04-02 01:31:46.000 2015-04-09 20:06:25.000  Hunt: Yes; prey_p_month: 1
## 9   2015-04-18 16:02:05.000 2015-04-27 13:16:01.000 Hunt: Yes; prey_p_month: 11
## 10  2015-04-18 16:02:59.000 2015-04-23 19:05:03.000  Hunt: Yes; prey_p_month: 6
## 11  2015-04-18 16:17:51.000 2015-04-26 00:37:44.000 Hunt: Yes; prey_p_month: 21
## 12  2015-04-18 16:23:59.000 2015-04-26 14:45:28.000  Hunt: Yes; prey_p_month: 0
## 13  2015-04-18 16:25:34.000 2015-04-25 04:44:34.000  Hunt: Yes; prey_p_month: 3
## 14  2015-04-23 16:01:52.000 2015-05-01 06:28:53.000  Hunt: Yes; prey_p_month: 1
## 15  2015-04-23 16:20:45.000 2015-05-02 08:38:20.000  Hunt: Yes; prey_p_month: 1
## 16  2015-04-24 16:01:57.000 2015-05-03 06:28:16.000  Hunt: Yes; prey_p_month: 2
## 17  2015-04-24 16:02:34.000 2015-05-02 23:29:13.000  Hunt: Yes; prey_p_month: 9
## 18  2015-04-24 16:02:51.000 2015-05-04 06:28:36.000 Hunt: Yes; prey_p_month: 21
## 19  2015-04-24 16:02:57.000 2015-05-04 10:02:00.000  Hunt: Yes; prey_p_month: 7
## 20  2015-04-24 16:39:45.000 2015-04-30 00:35:51.000 Hunt: Yes; prey_p_month: 11
## 21  2015-04-30 15:02:04.000 2015-05-10 05:27:08.000  Hunt: Yes; prey_p_month: 1
## 22  2015-05-13 15:05:01.000 2015-05-18 15:28:10.000  Hunt: Yes; prey_p_month: 0
## 23  2015-05-13 15:05:17.000 2015-05-19 21:29:56.000  Hunt: Yes; prey_p_month: 0
## 24  2015-05-14 00:34:26.000 2015-05-22 14:58:51.000   Hunt: No; prey_p_month: 0
## 25  2015-05-14 15:02:03.000 2015-05-24 05:29:34.000  Hunt: Yes; prey_p_month: 6
## 26  2015-05-14 15:02:39.000 2015-05-22 10:08:41.000  Hunt: Yes; prey_p_month: 1
## 27  2015-05-14 15:18:26.000 2015-05-22 15:35:49.000  Hunt: Yes; prey_p_month: 3
## 28  2015-05-15 00:32:04.000 2015-05-23 15:07:04.000  Hunt: Yes; prey_p_month: 2
## 29  2015-05-15 00:50:12.000 2015-05-21 09:27:23.000  Hunt: Yes; prey_p_month: 1
## 30  2015-05-15 05:43:39.000 2015-05-22 07:32:07.000     Hunt: ; prey_p_month: 0
## 31  2015-05-15 11:46:20.000 2015-05-19 00:29:48.000  Hunt: Yes; prey_p_month: 4
## 32  2015-05-15 15:01:55.000 2015-05-24 05:28:33.000   Hunt: No; prey_p_month: 0
## 33  2015-05-15 15:02:04.000 2015-05-23 05:27:03.000  Hunt: Yes; prey_p_month: 4
## 34  2015-05-16 00:31:29.000 2015-05-25 14:57:16.000  Hunt: Yes; prey_p_month: 2
## 35  2015-05-16 00:31:58.000 2015-05-25 14:59:24.000  Hunt: Yes; prey_p_month: 1
## 36  2015-05-16 00:32:29.000 2015-05-25 14:56:58.000  Hunt: Yes; prey_p_month: 3
## 37  2015-05-16 00:33:46.000 2015-05-25 14:57:56.000   Hunt: No; prey_p_month: 0
## 38  2015-05-16 00:55:04.000 2015-05-24 02:23:39.000     Hunt: ; prey_p_month: 0
## 39  2015-05-16 15:00:59.000 2015-05-23 00:44:02.000  Hunt: Yes; prey_p_month: 6
## 40  2015-05-21 00:31:59.000 2015-06-03 20:55:01.000  Hunt: Yes; prey_p_month: 1
## 41  2015-05-21 00:33:54.000 2015-05-27 03:40:42.000     Hunt: ; prey_p_month: 0
## 42  2015-05-22 00:31:03.000 2015-05-29 08:26:27.000  Hunt: Yes; prey_p_month: 3
## 43  2015-05-22 00:32:22.000 2015-05-30 07:35:58.000  Hunt: Yes; prey_p_month: 0
## 44  2015-05-22 00:51:17.000 2015-05-29 00:26:38.000  Hunt: Yes; prey_p_month: 1
## 45  2015-05-22 01:32:04.000 2015-05-31 18:35:45.000                        #N/A
## 46  2015-05-22 02:26:00.000 2015-05-29 02:42:11.000   Hunt: No; prey_p_month: 0
## 47  2015-05-29 00:33:45.000 2015-06-06 02:23:41.000    Hunt: 0; prey_p_month: 0
## 48  2015-05-30 00:30:41.000 2015-06-09 20:18:43.000  Hunt: Yes; prey_p_month: 5
## 49  2015-05-30 00:30:57.000 2015-06-10 14:06:09.000   Hunt: No; prey_p_month: 0
## 50  2015-05-30 00:31:42.000 2015-06-10 19:10:07.000  Hunt: Yes; prey_p_month: 5
## 51  2015-05-30 00:32:41.000 2015-06-10 02:12:40.000  Hunt: Yes; prey_p_month: 9
## 52  2015-05-30 00:32:56.000 2015-06-08 14:58:50.000  Hunt: Yes; prey_p_month: 5
## 53  2015-05-30 00:32:58.000 2015-06-06 02:47:39.000 Hunt: Yes; prey_p_month: 17
## 54  2015-05-30 00:33:30.000 2015-06-07 02:05:57.000  Hunt: Yes; prey_p_month: 2
## 55  2015-05-30 00:33:32.000 2015-06-08 14:58:50.000   Hunt: No; prey_p_month: 1
## 56  2015-05-30 00:36:05.000 2015-06-03 07:24:11.000 Hunt: Yes; prey_p_month: 21
## 57  2015-05-30 15:02:43.000 2015-06-10 04:28:36.000  Hunt: Yes; prey_p_month: 1
## 58  2015-06-05 00:33:26.000 2015-06-15 14:57:42.000  Hunt: Yes; prey_p_month: 5
## 59  2015-06-05 00:33:56.000 2015-06-14 14:57:44.000  Hunt: Yes; prey_p_month: 5
## 60  2015-06-05 00:34:28.000 2015-06-12 14:59:41.000  Hunt: Yes; prey_p_month: 3
## 61  2015-06-05 00:47:26.000 2015-06-12 15:31:43.000  Hunt: Yes; prey_p_month: 2
## 62  2015-06-05 00:50:34.000 2015-06-11 22:27:29.000  Hunt: Yes; prey_p_month: 3
## 63  2015-06-05 15:02:33.000 2015-06-10 09:54:26.000  Hunt: Yes; prey_p_month: 6
## 64  2015-06-06 00:33:56.000 2015-06-12 22:28:17.000  Hunt: Yes; prey_p_month: 2
## 65  2015-06-06 00:47:46.000 2015-06-12 09:23:07.000  Hunt: Yes; prey_p_month: 1
## 66  2015-06-06 01:39:01.000 2015-06-12 23:07:00.000  Hunt: Yes; prey_p_month: 1
## 67  2015-06-06 01:46:17.000 2015-06-12 22:55:06.000  Hunt: Yes; prey_p_month: 5
## 68  2015-06-06 03:52:52.000 2015-06-11 00:15:28.000  Hunt: Yes; prey_p_month: 4
## 69  2015-06-06 09:31:32.000 2015-06-13 03:44:14.000    Hunt: 0; prey_p_month: 0
## 70  2015-06-08 15:05:06.000 2015-06-13 14:56:58.000  Hunt: Yes; prey_p_month: 6
## 71  2015-06-13 00:30:34.000 2015-06-21 14:58:33.000  Hunt: Yes; prey_p_month: 4
## 72  2015-06-13 00:31:14.000 2015-06-20 15:05:09.000  Hunt: Yes; prey_p_month: 0
## 73  2015-06-13 00:31:36.000 2015-06-21 14:57:38.000  Hunt: Yes; prey_p_month: 1
## 74  2015-06-13 00:31:56.000 2015-06-18 17:04:28.000   Hunt: No; prey_p_month: 0
## 75  2015-06-13 00:32:16.000 2015-06-21 14:59:18.000  Hunt: Yes; prey_p_month: 2
## 76  2015-06-13 00:32:23.000 2015-06-20 14:57:36.000   Hunt: No; prey_p_month: 0
## 77  2015-06-13 00:32:34.000 2015-06-21 14:58:33.000  Hunt: Yes; prey_p_month: 5
## 78  2015-06-13 00:32:45.000 2015-06-20 15:03:20.000  Hunt: Yes; prey_p_month: 1
## 79  2015-06-13 00:33:04.000 2015-06-21 14:57:10.000  Hunt: Yes; prey_p_month: 1
## 80  2015-06-13 00:33:44.000 2015-06-21 14:58:55.000 Hunt: Yes; prey_p_month: 13
## 81  2015-06-13 00:51:48.000 2015-06-21 05:36:12.000  Hunt: Yes; prey_p_month: 0
## 82  2015-06-13 02:50:03.000 2015-06-19 12:08:25.000  Hunt: Yes; prey_p_month: 6
## 83  2015-06-13 03:00:22.000 2015-06-20 00:09:56.000  Hunt: Yes; prey_p_month: 3
## 84  2015-06-14 00:32:34.000 2015-06-20 18:56:05.000  Hunt: Yes; prey_p_month: 3
## 85  2015-06-14 00:33:41.000 2015-06-21 13:51:27.000   Hunt: No; prey_p_month: 0
## 86  2015-06-14 00:36:01.000 2015-06-20 06:10:03.000  Hunt: Yes; prey_p_month: 3
## 87  2015-06-14 01:34:35.000 2015-06-21 18:41:26.000  Hunt: Yes; prey_p_month: 3
## 88  2015-06-14 01:40:22.000 2015-06-21 13:22:37.000  Hunt: Yes; prey_p_month: 6
## 89  2015-06-14 03:07:49.000 2015-06-21 15:29:23.000 Hunt: Yes; prey_p_month: 11
## 90  2015-06-19 00:32:51.000 2015-06-25 07:08:30.000   Hunt: No; prey_p_month: 0
## 91  2015-06-19 01:32:23.000 2015-06-27 15:59:38.000  Hunt: Yes; prey_p_month: 1
## 92  2015-06-19 02:03:41.000 2015-06-25 23:07:27.000 Hunt: Yes; prey_p_month: 11
## 93  2015-06-19 02:51:37.000 2015-06-23 08:57:40.000 Hunt: Yes; prey_p_month: 11
## 94  2015-06-19 03:01:34.000 2015-06-25 22:21:27.000 Hunt: Yes; prey_p_month: 21
## 95  2015-06-20 00:32:04.000 2015-06-27 15:08:11.000  Hunt: Yes; prey_p_month: 1
## 96  2015-06-20 00:32:35.000 2015-06-29 14:58:59.000  Hunt: Yes; prey_p_month: 2
## 97  2015-06-20 00:33:27.000 2015-06-27 22:42:58.000  Hunt: Yes; prey_p_month: 4
## 98  2015-06-20 00:35:45.000 2015-06-25 20:21:16.000   Hunt: No; prey_p_month: 4
## 99  2015-06-20 00:51:52.000 2015-06-27 13:26:05.000  Hunt: Yes; prey_p_month: 0
## 100 2015-06-20 01:12:04.000 2015-06-28 11:50:58.000  Hunt: Yes; prey_p_month: 3
## 101 2015-06-20 01:33:27.000 2015-06-29 15:59:17.000  Hunt: Yes; prey_p_month: 2
## 102 2015-06-20 02:31:24.000 2015-06-27 00:01:53.000  Hunt: Yes; prey_p_month: 6
## 103 2015-06-25 02:33:34.000 2015-07-03 14:29:26.000  Hunt: Yes; prey_p_month: 6
## 104 2015-06-25 13:54:26.000 2015-06-30 12:58:20.000  Hunt: Yes; prey_p_month: 1
## 105 2015-06-25 15:24:42.000 2015-07-02 00:09:31.000  Hunt: Yes; prey_p_month: 1
## 106 2015-06-26 00:30:58.000 2015-07-04 21:56:37.000  Hunt: Yes; prey_p_month: 3
## 107 2015-06-26 00:31:47.000 2015-07-04 22:18:36.000   Hunt: No; prey_p_month: 0
## 108 2015-06-26 00:31:59.000 2015-07-06 03:56:30.000   Hunt: No; prey_p_month: 0
## 109 2015-06-26 00:32:20.000 2015-07-01 04:15:17.000     Hunt: ; prey_p_month: 0
## 110 2015-06-26 00:33:59.000 2015-07-02 05:16:36.000     Hunt: ; prey_p_month: 1
## 111 2015-06-26 00:52:59.000 2015-07-03 03:09:25.000     Hunt: ; prey_p_month: 0
## 112 2015-06-26 19:47:26.000 2015-07-03 01:35:22.000  Hunt: Yes; prey_p_month: 4
## 113 2015-06-26 23:06:49.000 2015-07-03 14:58:04.000   Hunt: No; prey_p_month: 0
## 114 2015-06-27 01:03:07.000 2015-07-01 22:25:18.000  Hunt: Yes; prey_p_month: 1
## 115 2015-06-27 01:55:31.000 2015-07-01 13:53:57.000  Hunt: Yes; prey_p_month: 3
## 116 2015-06-27 06:45:32.000 2015-07-04 19:46:38.000     Hunt: ; prey_p_month: 0
## 117 2015-06-27 10:39:31.000 2015-07-02 21:45:05.000     Hunt: ; prey_p_month: 0
## 118 2015-07-01 00:31:50.000 2015-07-14 19:54:11.000 Hunt: Yes; prey_p_month: 11
## 119 2015-07-01 00:32:30.000 2015-07-09 14:58:45.000  Hunt: Yes; prey_p_month: 4
## 120 2015-07-01 01:32:34.000 2015-07-12 21:49:26.000 Hunt: Yes; prey_p_month: 11
## 121 2015-07-03 00:32:59.000 2015-07-11 13:35:42.000  Hunt: Yes; prey_p_month: 1
## 122 2015-07-03 00:33:35.000 2015-07-11 03:53:01.000     Hunt: ; prey_p_month: 0
## 123 2015-07-03 00:34:05.000 2015-07-10 14:59:41.000  Hunt: Yes; prey_p_month: 1
## 124 2015-07-03 00:34:09.000 2015-07-08 02:39:41.000  Hunt: Yes; prey_p_month: 1
## 125 2015-07-03 00:35:03.000 2015-07-10 13:10:46.000  Hunt: Yes; prey_p_month: 3
## 126 2015-07-08 00:32:35.000 2015-07-15 14:57:58.000    Hunt: 0; prey_p_month: 0
## 127 2015-07-08 00:49:15.000 2015-07-15 14:59:12.000  Hunt: Yes; prey_p_month: 3
## 128 2015-07-08 03:22:14.000 2015-07-16 15:28:10.000 Hunt: Yes; prey_p_month: 11
## 129 2015-07-08 10:35:53.000 2015-07-16 03:47:25.000  Hunt: Yes; prey_p_month: 3
## 130 2015-07-09 00:33:10.000 2015-07-16 19:27:28.000  Hunt: Yes; prey_p_month: 8
## 131 2015-07-09 00:35:08.000 2015-07-14 13:40:55.000  Hunt: Yes; prey_p_month: 7
## 132 2015-07-15 00:32:49.000 2015-07-25 14:59:52.000  Hunt: Yes; prey_p_month: 3
## 133 2015-07-15 00:47:10.000 2015-07-25 14:57:37.000  Hunt: Yes; prey_p_month: 5
## 134 2015-07-15 01:38:34.000 2015-07-21 13:24:13.000  Hunt: Yes; prey_p_month: 2
## 135 2015-07-16 00:32:11.000 2015-07-27 03:14:48.000  Hunt: Yes; prey_p_month: 7
## 136 2015-07-16 00:33:04.000 2015-07-23 08:09:14.000    Hunt: 0; prey_p_month: 0
## 137 2015-07-16 00:33:51.000 2015-07-21 15:34:05.000  Hunt: Yes; prey_p_month: 3
## 138 2015-07-16 00:34:18.000 2015-07-22 21:46:49.000  Hunt: Yes; prey_p_month: 3
## 139 2015-07-16 00:48:20.000 2015-07-21 14:22:05.000  Hunt: Yes; prey_p_month: 1
## 140 2015-07-16 01:02:09.000 2015-07-21 12:38:19.000  Hunt: Yes; prey_p_month: 2
## 141 2015-07-17 00:30:33.000 2015-07-21 23:06:52.000     Hunt: ; prey_p_month: 0
## 142 2015-07-17 00:34:10.000 2015-07-22 09:06:11.000    Hunt: 0; prey_p_month: 0
## 143 2015-07-21 00:49:41.000 2015-07-27 23:55:14.000  Hunt: Yes; prey_p_month: 1
## 144 2015-07-22 00:34:27.000 2015-07-30 06:37:21.000   Hunt: No; prey_p_month: 0
## 145 2015-07-22 00:48:56.000 2015-07-29 05:46:49.000  Hunt: Yes; prey_p_month: 0
## 146 2015-07-22 00:51:30.000 2015-07-28 16:11:07.000  Hunt: Yes; prey_p_month: 0
## 147 2015-07-22 07:03:05.000 2015-07-29 01:21:37.000  Hunt: Yes; prey_p_month: 5
## 148 2015-07-22 08:16:55.000 2015-07-28 13:24:04.000   Hunt: No; prey_p_month: 0
## 149 2015-07-22 09:38:33.000 2015-07-27 11:17:27.000  Hunt: Yes; prey_p_month: 1
## 150 2015-07-23 00:41:01.000 2015-07-28 18:09:34.000  Hunt: Yes; prey_p_month: 3
## 151 2015-07-23 01:10:38.000 2015-07-29 01:43:17.000     Hunt: ; prey_p_month: 0
## 152 2015-07-29 00:32:04.000 2015-08-05 13:20:23.000   Hunt: No; prey_p_month: 0
## 153 2015-07-29 00:32:05.000 2015-08-07 18:31:33.000   Hunt: No; prey_p_month: 0
## 154 2015-07-29 00:32:27.000 2015-08-09 09:02:50.000     Hunt: ; prey_p_month: 1
## 155 2015-07-29 00:32:34.000 2015-08-06 02:20:26.000  Hunt: Yes; prey_p_month: 1
## 156 2015-07-29 04:19:13.000 2015-08-03 03:32:51.000  Hunt: Yes; prey_p_month: 2
## 157 2015-07-30 00:30:33.000 2015-08-07 04:46:32.000  Hunt: Yes; prey_p_month: 0
## 158 2015-07-30 00:31:33.000 2015-08-07 14:06:25.000  Hunt: Yes; prey_p_month: 5
## 159 2015-07-30 00:32:03.000 2015-08-09 07:39:25.000  Hunt: Yes; prey_p_month: 3
## 160 2015-07-30 00:32:03.000 2015-08-09 19:37:03.000  Hunt: Yes; prey_p_month: 3
## 161 2015-07-30 00:32:04.000 2015-08-06 14:57:47.000  Hunt: Yes; prey_p_month: 0
## 162 2015-07-30 00:33:14.000 2015-08-06 14:56:53.000  Hunt: Yes; prey_p_month: 2
## 163 2015-07-30 02:16:44.000 2015-08-05 22:51:34.000                            
## 164 2015-07-31 03:54:37.000 2015-08-06 17:07:30.000 Hunt: Yes; prey_p_month: 16
## 165 2015-08-01 00:30:35.000 2015-08-07 15:46:53.000   Hunt: No; prey_p_month: 0
## 166 2015-08-01 01:49:44.000 2015-08-09 22:17:39.000  Hunt: Yes; prey_p_month: 3
## 167 2015-08-05 00:32:05.000 2015-08-12 14:57:07.000  Hunt: Yes; prey_p_month: 2
## 168 2015-08-06 00:33:26.000 2015-08-12 13:20:23.000  Hunt: Yes; prey_p_month: 4
## 169 2015-08-12 00:32:34.000 2015-08-20 14:59:12.000   Hunt: No; prey_p_month: 0
## 170 2015-08-12 00:32:47.000 2015-08-20 14:58:19.000     Hunt: ; prey_p_month: 0
## 171 2015-08-12 00:33:04.000 2015-08-20 14:54:56.000  Hunt: Yes; prey_p_month: 1
## 172 2015-08-13 00:30:57.000 2015-08-20 14:59:44.000  Hunt: Yes; prey_p_month: 1
## 173 2015-08-13 00:31:03.000 2015-08-23 00:34:16.000   Hunt: No; prey_p_month: 0
## 174 2015-08-13 00:32:34.000 2015-08-17 19:56:30.000  Hunt: Yes; prey_p_month: 2
## 175 2015-08-13 00:32:55.000 2015-08-18 20:47:45.000  Hunt: Yes; prey_p_month: 4
## 176 2015-08-13 00:50:34.000 2015-08-21 02:27:01.000     Hunt: ; prey_p_month: 0
## 177 2015-08-14 00:30:56.000 2015-08-21 00:13:55.000  Hunt: Yes; prey_p_month: 0
## 178 2015-08-14 00:34:56.000 2015-08-20 15:18:06.000   Hunt: No; prey_p_month: 0
## 179 2015-08-14 00:48:53.000 2015-08-22 22:19:03.000  Hunt: Yes; prey_p_month: 6
## 180 2015-08-14 02:43:21.000 2015-08-20 16:42:58.000  Hunt: Yes; prey_p_month: 0
## 181 2015-08-15 01:19:43.000 2015-08-20 06:10:20.000  Hunt: Yes; prey_p_month: 2
## 182 2015-08-15 01:28:34.000 2015-08-20 20:57:40.000  Hunt: Yes; prey_p_month: 2
## 183 2015-08-19 00:51:33.000 2015-08-25 12:53:55.000  Hunt: Yes; prey_p_month: 1
## 184 2015-08-19 01:34:26.000 2015-08-26 15:56:45.000  Hunt: Yes; prey_p_month: 5
## 185 2015-08-19 03:52:56.000 2015-08-25 08:29:34.000   Hunt: No; prey_p_month: 0
## 186 2015-08-20 00:32:32.000 2015-08-26 05:48:30.000 Hunt: Yes; prey_p_month: 13
## 187 2015-08-20 00:35:19.000 2015-08-26 09:18:30.000  Hunt: Yes; prey_p_month: 0
## 188 2015-08-21 00:48:25.000 2015-08-29 03:38:03.000  Hunt: Yes; prey_p_month: 2
## 189 2015-08-21 05:43:18.000 2015-08-27 08:00:06.000  Hunt: Yes; prey_p_month: 2
## 190 2015-08-21 05:43:24.000 2015-08-27 20:32:41.000  Hunt: Yes; prey_p_month: 3
## 191 2015-08-26 00:32:13.000 2015-09-02 14:57:42.000   Hunt: No; prey_p_month: 0
## 192 2015-08-26 05:14:13.000 2015-08-30 13:46:14.000  Hunt: Yes; prey_p_month: 1
## 193 2015-08-27 00:30:34.000 2015-09-06 16:02:35.000     Hunt: ; prey_p_month: 0
## 194 2015-08-27 00:34:08.000 2015-09-03 09:55:31.000  Hunt: Yes; prey_p_month: 2
## 195 2015-08-27 00:34:26.000 2015-09-02 15:04:07.000  Hunt: Yes; prey_p_month: 3
## 196 2015-08-27 01:52:07.000 2015-09-04 06:26:57.000  Hunt: Yes; prey_p_month: 3
## 197 2015-08-27 03:00:52.000 2015-09-02 14:11:18.000  Hunt: Yes; prey_p_month: 2
## 198 2015-08-27 03:45:41.000 2015-09-01 16:16:12.000  Hunt: Yes; prey_p_month: 5
## 199 2015-08-28 00:32:28.000 2015-09-04 11:40:25.000  Hunt: Yes; prey_p_month: 1
## 200 2015-08-28 00:32:45.000 2015-09-05 14:57:06.000  Hunt: Yes; prey_p_month: 3
## 201 2015-08-28 00:35:50.000 2015-09-03 07:32:10.000  Hunt: Yes; prey_p_month: 2
## 202 2015-08-28 00:38:57.000 2015-09-06 08:33:25.000  Hunt: Yes; prey_p_month: 4
## 203 2015-08-28 01:41:02.000 2015-09-02 04:26:09.000  Hunt: Yes; prey_p_month: 2
## 204 2015-09-02 00:32:04.000 2015-09-09 09:38:47.000  Hunt: Yes; prey_p_month: 2
## 205 2015-09-02 00:33:26.000 2015-09-10 14:59:59.000   Hunt: No; prey_p_month: 0
## 206 2015-09-02 01:40:02.000 2015-09-07 20:16:39.000     Hunt: ; prey_p_month: 0
## 207 2015-09-02 02:23:08.000 2015-09-05 04:24:27.000     Hunt: ; prey_p_month: 1
## 208 2015-09-03 00:31:31.000 2015-09-09 07:31:07.000  Hunt: Yes; prey_p_month: 9
## 209 2015-09-03 00:32:04.000 2015-09-11 15:22:09.000  Hunt: Yes; prey_p_month: 2
## 210 2015-09-03 00:32:22.000 2015-09-15 03:19:02.000  Hunt: Yes; prey_p_month: 1
## 211 2015-09-03 00:32:22.000 2015-09-08 12:08:58.000  Hunt: Yes; prey_p_month: 6
## 212 2015-09-03 00:32:29.000 2015-09-12 04:50:23.000     Hunt: ; prey_p_month: 0
## 213 2015-09-03 00:32:51.000 2015-09-08 23:44:23.000  Hunt: Yes; prey_p_month: 1
## 214 2015-09-03 00:35:44.000 2015-09-11 00:19:25.000     Hunt: ; prey_p_month: 0
## 215 2015-09-03 01:48:51.000 2015-09-10 20:10:00.000     Hunt: ; prey_p_month: 0
## 216 2015-09-03 02:09:55.000 2015-09-08 16:10:57.000 Hunt: Yes; prey_p_month: 21
## 217 2015-09-03 09:08:59.000 2015-09-09 00:09:42.000  Hunt: Yes; prey_p_month: 5
## 218 2015-09-04 00:30:41.000 2015-09-11 15:05:44.000   Hunt: No; prey_p_month: 0
## 219 2015-09-04 00:32:04.000 2015-09-11 15:03:26.000   Hunt: No; prey_p_month: 0
## 220 2015-09-04 00:32:33.000 2015-09-13 14:59:37.000  Hunt: Yes; prey_p_month: 4
## 221 2015-09-04 00:32:41.000 2015-09-11 15:05:41.000     Hunt: ; prey_p_month: 0
## 222 2015-09-04 01:39:21.000 2015-09-13 14:57:26.000   Hunt: No; prey_p_month: 0
## 223 2015-09-04 15:05:30.000 2015-09-11 14:50:43.000     Hunt: ; prey_p_month: 0
## 224 2015-09-09 00:30:55.000 2015-09-16 14:59:48.000  Hunt: Yes; prey_p_month: 3
## 225 2015-09-09 00:31:02.000 2015-09-16 14:57:31.000  Hunt: Yes; prey_p_month: 3
## 226 2015-09-09 00:31:51.000 2015-09-21 10:30:27.000   Hunt: No; prey_p_month: 0
## 227 2015-09-09 00:32:12.000 2015-09-16 14:57:44.000     Hunt: ; prey_p_month: 0
## 228 2015-09-09 00:33:55.000 2015-09-16 14:59:36.000     Hunt: ; prey_p_month: 0
## 229 2015-09-10 00:31:58.000 2015-09-18 14:48:52.000   Hunt: No; prey_p_month: 0
## 230 2015-09-10 00:32:11.000 2015-09-17 14:58:53.000  Hunt: Yes; prey_p_month: 1
## 231 2015-09-10 00:32:29.000 2015-09-20 03:52:57.000  Hunt: Yes; prey_p_month: 0
## 232 2015-09-10 00:32:51.000 2015-09-11 18:05:59.000  Hunt: Yes; prey_p_month: 1
## 233 2015-09-10 00:33:56.000 2015-09-17 14:58:50.000     Hunt: ; prey_p_month: 0
## 234 2015-09-10 00:35:25.000 2015-09-16 20:38:11.000  Hunt: Yes; prey_p_month: 4
## 235 2015-09-11 02:44:04.000 2015-09-18 22:42:23.000  Hunt: Yes; prey_p_month: 4
## 236 2015-09-16 00:31:04.000 2015-09-23 21:34:07.000  Hunt: Yes; prey_p_month: 6
## 237 2015-09-16 00:32:23.000 2015-09-23 14:57:46.000   Hunt: No; prey_p_month: 0
## 238 2015-09-30 00:30:33.000 2015-10-07 13:59:09.000  Hunt: Yes; prey_p_month: 2
## 239 2015-09-30 00:30:57.000 2015-10-08 13:59:42.000  Hunt: Yes; prey_p_month: 0
## 240 2015-09-30 00:30:58.000 2015-10-08 13:59:53.000  Hunt: Yes; prey_p_month: 1
## 241 2015-09-30 00:31:38.000 2015-10-08 13:59:46.000  Hunt: Yes; prey_p_month: 3
## 242 2015-09-30 00:32:35.000 2015-10-02 00:37:45.000   Hunt: No; prey_p_month: 0
## 243 2015-09-30 00:32:35.000 2015-10-08 13:58:51.000  Hunt: Yes; prey_p_month: 3
## 244 2015-09-30 00:32:42.000 2015-10-07 13:58:33.000  Hunt: Yes; prey_p_month: 2
## 245 2015-09-30 00:32:58.000 2015-10-07 13:57:57.000  Hunt: Yes; prey_p_month: 0
## 246 2015-09-30 00:33:02.000 2015-10-06 17:03:42.000  Hunt: Yes; prey_p_month: 3
## 247 2015-09-30 00:34:33.000 2015-10-10 06:32:00.000     Hunt: ; prey_p_month: 0
## 248 2015-09-30 00:35:49.000 2015-10-09 00:26:59.000  Hunt: Yes; prey_p_month: 1
## 249 2015-10-01 00:32:06.000 2015-10-08 13:59:36.000   Hunt: No; prey_p_month: 0
## 250 2015-10-01 00:32:07.000 2015-10-09 13:59:10.000  Hunt: Yes; prey_p_month: 2
## 251 2015-10-01 00:32:18.000 2015-10-11 13:58:04.000  Hunt: Yes; prey_p_month: 5
## 252 2015-10-01 00:32:27.000 2015-10-11 13:57:13.000    Hunt: 0; prey_p_month: 0
## 253 2015-10-01 00:32:34.000 2015-10-08 13:57:10.000     Hunt: ; prey_p_month: 1
## 254 2015-10-01 00:32:34.000 2015-10-11 13:57:48.000    Hunt: 0; prey_p_month: 0
## 255 2015-10-01 00:33:26.000 2015-10-09 01:29:53.000  Hunt: Yes; prey_p_month: 1
## 256 2015-10-01 11:44:55.000 2015-10-07 10:19:42.000     Hunt: ; prey_p_month: 0
## 257 2015-10-08 00:32:05.000 2015-10-15 21:30:29.000   Hunt: No; prey_p_month: 1
## 258 2015-10-08 00:32:34.000 2015-10-15 22:27:23.000   Hunt: No; prey_p_month: 0
## 259 2015-10-08 00:35:27.000 2015-10-15 14:05:45.000  Hunt: Yes; prey_p_month: 6
## 260 2015-10-08 01:03:12.000 2015-10-13 12:20:54.000  Hunt: Yes; prey_p_month: 2
## 261 2015-10-08 01:32:27.000 2015-10-14 09:22:24.000  Hunt: Yes; prey_p_month: 6
## 262 2015-10-08 01:32:34.000 2015-10-15 14:59:28.000  Hunt: Yes; prey_p_month: 5
## 263 2015-10-08 01:32:35.000 2015-10-17 06:46:02.000  Hunt: Yes; prey_p_month: 5
## 264 2015-10-08 01:47:34.000 2015-10-14 01:01:12.000  Hunt: Yes; prey_p_month: 5
## 265 2015-10-08 14:00:18.000 2015-10-18 16:18:21.000  Hunt: Yes; prey_p_month: 6
## 266 2015-10-08 14:02:49.000 2015-10-19 13:59:01.000  Hunt: Yes; prey_p_month: 1
## 267 2015-10-08 15:00:30.000 2015-10-18 18:55:30.000  Hunt: Yes; prey_p_month: 1
## 268 2015-10-09 00:30:35.000 2015-10-14 18:09:48.000  Hunt: Yes; prey_p_month: 9
## 269 2015-10-09 00:32:04.000 2015-10-16 19:43:34.000  Hunt: Yes; prey_p_month: 1
## 270 2015-10-09 01:31:05.000 2015-10-18 13:09:11.000  Hunt: Yes; prey_p_month: 2
## 271 2015-10-09 01:32:00.000 2015-10-19 14:57:39.000     Hunt: ; prey_p_month: 0
## 272 2015-10-09 01:32:04.000 2015-10-16 15:32:10.000 Hunt: Yes; prey_p_month: 15
## 273 2015-10-09 01:35:11.000 2015-10-16 22:08:10.000   Hunt: No; prey_p_month: 0
## 274 2015-10-11 15:01:02.000 2015-10-16 07:44:23.000 Hunt: Yes; prey_p_month: 11
## 275 2015-10-14 15:00:30.000 2015-10-22 14:56:58.000     Hunt: ; prey_p_month: 0
## 276 2015-10-14 15:02:10.000 2015-10-22 21:39:58.000     Hunt: ; prey_p_month: 0
## 277 2015-10-14 15:02:40.000 2015-10-22 14:59:00.000     Hunt: ; prey_p_month: 1
## 278 2015-10-15 01:32:21.000 2015-10-22 14:57:47.000     Hunt: ; prey_p_month: 0
## 279 2015-10-15 01:32:30.000 2015-10-22 14:55:44.000  Hunt: Yes; prey_p_month: 1
## 280 2015-10-15 04:12:37.000 2015-10-21 08:05:16.000 Hunt: Yes; prey_p_month: 11
## 281 2015-10-15 15:00:16.000 2015-10-20 20:32:39.000   Hunt: No; prey_p_month: 0
## 282 2015-10-16 01:32:03.000 2015-10-23 14:57:06.000   Hunt: No; prey_p_month: 1
## 283 2015-10-16 01:32:21.000 2015-10-23 14:59:13.000  Hunt: Yes; prey_p_month: 2
## 284 2015-10-16 01:32:52.000 2015-10-23 14:57:45.000  Hunt: Yes; prey_p_month: 0
## 285 2015-10-16 01:32:58.000 2015-10-24 09:09:03.000   Hunt: No; prey_p_month: 0
## 286 2015-10-16 01:34:55.000 2015-10-22 14:02:02.000  Hunt: Yes; prey_p_month: 2
## 287 2015-10-16 01:36:08.000 2015-10-22 18:02:11.000 Hunt: Yes; prey_p_month: 19
## 288 2015-10-17 03:09:27.000 2015-10-23 08:53:32.000  Hunt: Yes; prey_p_month: 0
## 289 2015-11-11 01:48:33.000 2015-11-18 21:24:23.000  Hunt: Yes; prey_p_month: 9
## 290 2015-11-13 01:32:54.000 2015-11-21 14:57:05.000  Hunt: Yes; prey_p_month: 1
## 291 2015-11-13 01:32:57.000 2015-11-21 14:59:43.000  Hunt: Yes; prey_p_month: 1
## 292 2015-11-13 08:27:48.000 2015-11-18 20:20:49.000   Hunt: No; prey_p_month: 0
## 293 2015-11-13 15:02:23.000 2015-11-17 08:11:31.000 Hunt: Yes; prey_p_month: 21
## 294 2015-11-13 15:07:22.000 2015-11-19 18:14:06.000 Hunt: Yes; prey_p_month: 21
## 295 2015-11-20 01:32:05.000 2015-11-27 14:59:55.000  Hunt: Yes; prey_p_month: 8
## 296 2015-11-20 01:32:12.000 2015-11-28 06:50:11.000  Hunt: Yes; prey_p_month: 1
## 297 2015-11-20 01:32:52.000 2015-12-01 20:12:11.000  Hunt: Yes; prey_p_month: 1
## 298 2015-11-23 15:01:16.000 2015-11-28 01:18:20.000  Hunt: Yes; prey_p_month: 0
## 299 2015-11-23 15:01:52.000 2015-11-28 05:01:26.000  Hunt: Yes; prey_p_month: 0
## 300 2015-11-23 15:02:09.000 2015-11-27 04:38:03.000  Hunt: Yes; prey_p_month: 0
## 301 2015-11-23 15:11:55.000 2015-11-27 21:28:28.000     Hunt: ; prey_p_month: 0
## 302 2015-11-28 02:19:41.000 2015-12-05 12:12:45.000  Hunt: Yes; prey_p_month: 7
## 303 2015-12-09 01:32:04.000 2015-12-16 14:58:33.000  Hunt: Yes; prey_p_month: 1
## 304 2015-12-09 15:00:46.000 2015-12-15 17:41:11.000  Hunt: Yes; prey_p_month: 0
## 305 2015-12-11 01:30:35.000 2015-12-20 14:59:14.000  Hunt: Yes; prey_p_month: 2
## 306 2015-12-11 01:31:58.000 2015-12-18 19:27:28.000  Hunt: Yes; prey_p_month: 1
## 307 2015-12-11 01:34:44.000 2015-12-18 05:32:12.000  Hunt: Yes; prey_p_month: 1
## 308 2015-12-11 01:35:33.000 2015-12-15 14:51:50.000  Hunt: Yes; prey_p_month: 5
## 309 2015-12-11 02:18:20.000 2015-12-18 16:20:38.000  Hunt: Yes; prey_p_month: 2
## 310 2015-12-11 15:02:13.000 2015-12-16 14:59:40.000  Hunt: Yes; prey_p_month: 1
## 311 2015-12-16 01:32:07.000 2015-12-22 15:24:22.000  Hunt: Yes; prey_p_month: 4
## 312 2015-12-16 01:32:33.000 2015-12-23 14:58:24.000  Hunt: Yes; prey_p_month: 4
## 313 2015-12-16 15:00:46.000 2015-12-23 14:58:31.000 Hunt: Yes; prey_p_month: 16
## 314 2015-12-16 15:02:25.000 2015-12-23 14:51:50.000   Hunt: No; prey_p_month: 2
## 315 2015-12-17 01:32:34.000 2015-12-24 13:11:00.000  Hunt: Yes; prey_p_month: 4
## 316 2015-12-20 23:25:37.000 2015-12-23 21:29:09.000   Hunt: No; prey_p_month: 0
## 317 2015-12-21 15:02:00.000 2015-12-29 11:21:29.000  Hunt: Yes; prey_p_month: 4
## 318 2015-12-22 15:04:39.000 2015-12-29 03:27:48.000  Hunt: Yes; prey_p_month: 1
## 319 2015-12-23 01:32:28.000 2015-12-30 14:59:45.000  Hunt: Yes; prey_p_month: 1
## 320 2015-12-24 01:30:57.000 2016-01-01 07:38:15.000   Hunt: No; prey_p_month: 0
## 321 2015-12-24 02:15:55.000 2016-01-01 03:59:43.000  Hunt: Yes; prey_p_month: 5
## 322 2015-12-24 02:52:56.000 2015-12-31 13:27:47.000   Hunt: No; prey_p_month: 0
## 323 2015-12-24 04:47:10.000 2015-12-30 14:57:26.000  Hunt: Yes; prey_p_month: 1
## 324 2015-12-24 15:02:57.000 2016-01-02 05:12:21.000  Hunt: Yes; prey_p_month: 5
## 325 2015-12-25 01:47:43.000 2016-01-01 14:59:56.000  Hunt: Yes; prey_p_month: 7
## 326 2016-01-15 01:32:04.000 2016-01-21 21:18:57.000  Hunt: Yes; prey_p_month: 4
## 327 2016-01-15 02:34:12.000 2016-01-22 14:57:42.000  Hunt: Yes; prey_p_month: 2
## 328 2016-01-15 15:02:47.000 2016-01-19 18:43:22.000     Hunt: ; prey_p_month: 0
## 329 2016-01-15 15:17:03.000 2016-01-22 05:40:10.000  Hunt: Yes; prey_p_month: 5
## 330 2016-01-18 11:11:03.000 2016-01-23 13:36:28.000  Hunt: Yes; prey_p_month: 5
## 331 2016-01-18 15:02:15.000 2016-01-22 14:32:52.000  Hunt: Yes; prey_p_month: 2
## 332 2016-01-21 01:31:34.000 2016-01-27 15:04:10.000  Hunt: Yes; prey_p_month: 9
## 333 2016-01-21 02:01:40.000 2016-01-27 11:23:01.000 Hunt: Yes; prey_p_month: 13
## 334 2016-01-21 08:08:50.000 2016-01-28 14:59:53.000  Hunt: Yes; prey_p_month: 2
## 335 2016-01-22 01:32:50.000 2016-01-29 19:58:52.000 Hunt: Yes; prey_p_month: 10
## 336 2016-01-25 15:01:15.000 2016-01-29 09:29:56.000 Hunt: Yes; prey_p_month: 11
## 337 2016-01-28 01:32:27.000 2016-02-04 01:14:37.000 Hunt: Yes; prey_p_month: 10
## 338 2016-01-28 02:15:14.000 2016-02-05 10:48:33.000    Hunt: 0; prey_p_month: 0
## 339 2016-01-28 15:02:10.000 2016-02-04 23:35:09.000  Hunt: Yes; prey_p_month: 6
## 340 2016-01-29 15:02:50.000 2016-02-04 14:59:51.000  Hunt: Yes; prey_p_month: 2
## 341 2016-01-30 01:32:26.000 2016-02-07 15:01:16.000  Hunt: Yes; prey_p_month: 4
## 342 2016-01-30 01:34:27.000 2016-02-07 18:03:40.000  Hunt: Yes; prey_p_month: 3
## 343 2016-02-04 01:32:04.000 2016-02-11 14:28:04.000     Hunt: ; prey_p_month: 0
## 344 2016-02-04 01:32:04.000 2016-02-11 10:56:46.000  Hunt: Yes; prey_p_month: 3
## 345 2016-02-04 05:28:19.000 2016-02-11 20:53:31.000  Hunt: Yes; prey_p_month: 5
## 346 2016-02-04 15:02:05.000 2016-02-11 14:59:50.000  Hunt: Yes; prey_p_month: 5
## 347 2016-02-04 15:05:46.000 2016-02-09 01:20:33.000  Hunt: Yes; prey_p_month: 5
## 348 2016-02-05 01:34:13.000 2016-02-10 22:24:03.000  Hunt: Yes; prey_p_month: 3
## 349 2016-02-05 14:55:30.000 2016-02-11 03:06:01.000     Hunt: ; prey_p_month: 0
## 350 2016-02-05 15:02:15.000 2016-02-14 20:51:38.000 Hunt: Yes; prey_p_month: 16
## 351 2016-02-06 01:32:22.000 2016-02-14 22:48:31.000  Hunt: Yes; prey_p_month: 2
## 352 2016-02-12 01:32:23.000 2016-02-24 14:59:47.000  Hunt: Yes; prey_p_month: 1
## 353 2016-02-13 05:44:24.000 2016-02-18 09:01:25.000  Hunt: Yes; prey_p_month: 3
## 354 2016-02-17 01:31:51.000 2016-02-24 14:58:11.000  Hunt: Yes; prey_p_month: 1
## 355 2016-02-17 01:32:15.000 2016-02-24 16:54:00.000  Hunt: Yes; prey_p_month: 1
## 356 2016-02-17 01:34:59.000 2016-02-23 10:08:10.000  Hunt: Yes; prey_p_month: 3
## 357 2016-02-17 07:13:20.000 2016-02-24 23:18:33.000  Hunt: Yes; prey_p_month: 1
## 358 2016-02-18 01:31:42.000 2016-02-25 14:59:12.000     Hunt: ; prey_p_month: 1
## 359 2016-02-18 01:32:33.000 2016-02-19 14:00:48.000  Hunt: Yes; prey_p_month: 2
## 360 2016-02-18 08:22:05.000 2016-02-24 02:42:57.000  Hunt: Yes; prey_p_month: 4
## 361 2016-02-18 13:58:01.000 2016-02-25 16:06:56.000  Hunt: Yes; prey_p_month: 0
## 362 2016-02-25 01:32:17.000 2016-03-03 14:57:17.000  Hunt: Yes; prey_p_month: 0
## 363 2016-02-25 01:33:28.000 2016-03-03 14:58:05.000   Hunt: No; prey_p_month: 0
## 364 2016-02-25 01:34:27.000 2016-03-01 19:05:03.000 Hunt: Yes; prey_p_month: 11
## 365 2016-02-25 03:41:51.000 2016-03-02 17:40:19.000  Hunt: Yes; prey_p_month: 1
## 366 2016-02-25 08:45:57.000 2016-03-01 04:46:01.000  Hunt: Yes; prey_p_month: 1
## 367 2016-02-25 08:48:20.000 2016-03-02 13:17:59.000  Hunt: Yes; prey_p_month: 6
## 368 2016-02-26 01:33:02.000 2016-03-05 09:03:27.000   Hunt: No; prey_p_month: 0
## 369 2016-02-26 01:33:19.000 2016-03-04 15:06:00.000   Hunt: No; prey_p_month: 1
## 370 2016-02-26 07:13:04.000 2016-03-04 14:59:10.000  Hunt: Yes; prey_p_month: 1
## 371 2016-03-04 01:30:39.000 2016-03-14 19:28:38.000  Hunt: Yes; prey_p_month: 2
## 372 2016-03-04 01:32:04.000 2016-03-11 12:45:29.000   Hunt: No; prey_p_month: 0
## 373 2016-03-04 01:32:33.000 2016-03-14 11:19:07.000     Hunt: ; prey_p_month: 0
## 374 2016-03-04 01:35:57.000 2016-03-11 17:48:08.000   Hunt: No; prey_p_month: 0
## 375 2016-03-04 02:12:09.000 2016-03-13 02:52:13.000  Hunt: Yes; prey_p_month: 9
## 376 2016-03-04 04:10:25.000 2016-03-11 23:31:43.000   Hunt: No; prey_p_month: 0
## 377 2016-03-04 11:55:03.000 2016-03-10 06:59:21.000   Hunt: No; prey_p_month: 0
## 378 2016-03-10 01:32:03.000 2016-03-17 06:39:51.000  Hunt: Yes; prey_p_month: 2
## 379 2016-03-10 01:32:39.000 2016-03-18 15:05:05.000  Hunt: Yes; prey_p_month: 1
## 380 2016-03-10 01:32:56.000 2016-03-17 06:15:47.000  Hunt: Yes; prey_p_month: 4
## 381 2016-03-10 01:33:03.000 2016-03-23 23:37:51.000 Hunt: Yes; prey_p_month: 11
## 382 2016-03-10 05:49:31.000 2016-03-15 15:26:30.000  Hunt: Yes; prey_p_month: 3
## 383 2016-03-10 12:59:04.000 2016-03-17 13:31:52.000  Hunt: Yes; prey_p_month: 3
## 384 2016-03-11 01:31:24.000 2016-03-20 14:59:43.000  Hunt: Yes; prey_p_month: 4
## 385 2016-03-11 01:32:26.000 2016-03-20 14:57:11.000  Hunt: Yes; prey_p_month: 1
## 386 2016-03-11 01:33:55.000 2016-03-20 14:59:10.000     Hunt: ; prey_p_month: 0
## 387 2016-03-18 01:32:31.000 2016-03-24 04:28:59.000  Hunt: Yes; prey_p_month: 0
## 388 2016-03-18 01:33:21.000 2016-03-28 14:58:11.000  Hunt: Yes; prey_p_month: 1
## 389 2016-03-18 01:50:56.000 2016-03-24 12:42:32.000  Hunt: Yes; prey_p_month: 1
## 390 2016-03-24 03:10:03.000 2016-03-31 14:59:17.000 Hunt: Yes; prey_p_month: 21
## 391 2016-03-24 22:10:45.000 2016-03-29 22:35:09.000  Hunt: Yes; prey_p_month: 0
## 392 2016-03-25 02:17:55.000 2016-03-31 04:42:26.000  Hunt: Yes; prey_p_month: 3
## 393 2016-03-25 09:22:33.000 2016-03-30 11:19:09.000  Hunt: Yes; prey_p_month: 2
## 394 2016-04-01 01:32:06.000 2016-04-12 22:18:35.000    Hunt: 0; prey_p_month: 0
## 395 2016-04-01 01:32:44.000 2016-04-10 05:33:12.000     Hunt: ; prey_p_month: 0
## 396 2016-04-09 00:32:33.000 2016-04-17 14:58:23.000    Hunt: 0; prey_p_month: 0
## 397 2016-04-09 00:32:34.000 2016-04-16 21:31:19.000  Hunt: Yes; prey_p_month: 0
## 398 2016-04-09 01:19:37.000 2016-04-15 20:03:32.000  Hunt: Yes; prey_p_month: 3
## 399 2016-04-09 05:00:33.000 2016-04-14 23:42:55.000  Hunt: Yes; prey_p_month: 0
## 400 2016-04-09 05:30:33.000 2016-04-15 07:59:40.000  Hunt: Yes; prey_p_month: 5
## 401 2016-04-10 00:17:43.000 2016-04-13 15:46:49.000  Hunt: Yes; prey_p_month: 8
## 402 2016-04-14 00:32:30.000 2016-04-21 20:31:57.000  Hunt: Yes; prey_p_month: 5
## 403 2016-04-14 00:33:13.000 2016-04-26 14:58:33.000  Hunt: Yes; prey_p_month: 5
## 404 2016-04-17 15:57:05.000 2016-04-23 04:53:45.000    Hunt: 0; prey_p_month: 0
## 405 2016-04-18 10:18:39.000 2016-04-21 01:25:40.000  Hunt: Yes; prey_p_month: 1
## 406 2016-04-22 00:32:21.000 2016-04-28 12:24:08.000  Hunt: Yes; prey_p_month: 8
## 407 2016-04-22 00:33:29.000 2016-04-28 22:36:55.000   Hunt: No; prey_p_month: 0
## 408 2016-04-22 01:30:33.000 2016-04-30 02:34:06.000     Hunt: ; prey_p_month: 0
## 409 2016-04-29 00:31:33.000 2016-05-08 11:36:35.000  Hunt: Yes; prey_p_month: 1
## 410 2016-04-29 02:09:33.000 2016-05-06 15:03:45.000  Hunt: Yes; prey_p_month: 2
## 411 2016-04-30 02:39:13.000 2016-05-07 22:37:10.000  Hunt: Yes; prey_p_month: 7
## 412 2016-04-30 02:46:27.000 2016-05-06 21:53:57.000  Hunt: Yes; prey_p_month: 7
## 413 2016-04-30 16:01:02.000 2016-05-06 20:08:51.000  Hunt: Yes; prey_p_month: 1
## 414 2016-05-05 01:32:34.000 2016-05-12 15:57:48.000     Hunt: ; prey_p_month: 0
## 415 2016-05-06 00:33:35.000 2016-05-12 08:30:31.000  Hunt: Yes; prey_p_month: 4
## 416 2016-05-06 03:40:39.000 2016-05-12 10:56:39.000   Hunt: No; prey_p_month: 1
## 417 2016-05-07 00:32:21.000 2016-05-14 05:34:33.000  Hunt: Yes; prey_p_month: 3
## 418 2016-05-07 03:33:43.000 2016-05-14 19:52:26.000  Hunt: Yes; prey_p_month: 3
## 419 2016-05-12 00:30:34.000 2016-05-20 13:35:21.000  Hunt: Yes; prey_p_month: 6
## 420 2016-05-12 00:32:03.000 2016-05-18 21:40:33.000   Hunt: No; prey_p_month: 0
## 421 2016-05-12 12:57:35.000 2016-05-18 13:49:57.000   Hunt: No; prey_p_month: 0
## 422 2016-05-13 00:32:22.000 2016-05-23 00:02:03.000  Hunt: Yes; prey_p_month: 4
## 423 2016-05-13 00:33:21.000 2016-05-24 15:42:32.000  Hunt: Yes; prey_p_month: 7
## 424 2016-05-13 00:42:39.000 2016-05-21 08:26:14.000   Hunt: No; prey_p_month: 0
## 425 2016-05-13 01:44:10.000 2016-05-22 10:09:49.000   Hunt: No; prey_p_month: 0
## 426 2016-05-20 00:37:08.000 2016-05-24 22:17:30.000  Hunt: Yes; prey_p_month: 1
## 427 2016-05-20 00:49:50.000 2016-05-25 09:37:09.000  Hunt: Yes; prey_p_month: 1
## 428 2016-05-26 00:32:22.000 2016-06-05 14:58:01.000  Hunt: Yes; prey_p_month: 5
## 429 2016-05-26 00:32:36.000 2016-05-31 08:22:02.000  Hunt: Yes; prey_p_month: 2
## 430 2016-05-26 00:33:13.000 2016-06-01 22:58:54.000  Hunt: Yes; prey_p_month: 2
## 431 2016-05-27 00:31:34.000 2016-06-06 09:17:28.000  Hunt: Yes; prey_p_month: 1
## 432 2016-05-27 04:24:25.000 2016-06-02 09:52:28.000  Hunt: Yes; prey_p_month: 5
## 433 2016-06-02 01:32:03.000 2016-06-09 15:59:47.000  Hunt: Yes; prey_p_month: 3
## 434 2016-06-02 01:48:33.000 2016-06-08 11:16:14.000  Hunt: Yes; prey_p_month: 3
## 435 2016-06-02 06:49:55.000 2016-06-07 21:55:07.000     Hunt: ; prey_p_month: 0
## 436 2016-06-09 00:32:45.000 2016-06-12 10:17:23.000     Hunt: ; prey_p_month: 0
## 437 2016-06-09 00:35:33.000 2016-06-14 16:12:01.000     Hunt: ; prey_p_month: 0
## 438 2016-06-09 01:09:03.000 2016-06-14 06:28:42.000  Hunt: Yes; prey_p_month: 2
## 439 2016-06-09 08:42:19.000 2016-06-16 22:42:10.000   Hunt: No; prey_p_month: 0
## 440 2016-06-10 08:51:36.000 2016-06-15 19:32:45.000  Hunt: Yes; prey_p_month: 1
## 441 2016-06-24 00:49:04.000 2016-06-30 01:35:10.000  Hunt: Yes; prey_p_month: 1
## 442 2016-07-06 15:01:41.000 2016-07-12 13:58:12.000  Hunt: Yes; prey_p_month: 1
##     animal.life.stage animal.reproductive.condition animal.sex attachment.type
## 1             0 years                                                   collar
## 2             0 years                                                   collar
## 3             3 years                      Neutered          m          collar
## 4             4 years                        Spayed          f          collar
## 5             2 years                      Neutered          m          collar
## 6             5 years                      Neutered          m          collar
## 7             9 years                      Neutered          m          collar
## 8             7 years                      Neutered          m          collar
## 9             2 years                      Neutered          m          collar
## 10            0 years                        Spayed          f          collar
## 11           13 years                      Neutered          m          collar
## 12           12 years                      Neutered          m          collar
## 13            4 years                        Spayed          f          collar
## 14            0 years                      Neutered          m          collar
## 15            3 years                        Spayed          f          collar
## 16            2 years                        Spayed          f          collar
## 17           10 years                        Spayed          f          collar
## 18            6 years                        Spayed          f          collar
## 19            5 years                      Neutered          m          collar
## 20            2 years                        Spayed          f          collar
## 21            2 years                      Neutered          m          collar
## 22           11 years                      Neutered          m          collar
## 23            2 years                      Neutered          m          collar
## 24           14 years                        Spayed          f          collar
## 25            2 years                      Neutered          m          collar
## 26            6 years                      Neutered          m          collar
## 27           12 years                      Neutered          m          collar
## 28            5 years                      Neutered          m          collar
## 29            8 years                        Spayed          f          collar
## 30             1 year                         Fixed                     collar
## 31             1 year                        Spayed          f          collar
## 32             1 year                        Spayed          f          collar
## 33            5 years                      Neutered          m          collar
## 34            5 years                      Neutered          m          collar
## 35            3 years                        Spayed          f          collar
## 36            4 years                        Spayed          f          collar
## 37            5 years                      Neutered          m          collar
## 38             1 year                        Spayed          f          collar
## 39             1 year                        Spayed          f          collar
## 40            2 years                        Spayed          f          collar
## 41            6 years                        Spayed          f          collar
## 42           10 years                        Spayed          f          collar
## 43            5 years                      Neutered          m          collar
## 44           18 years                        Spayed          f          collar
## 45                                                                      collar
## 46            4 years                        Spayed          f          collar
## 47            0 years                                                   collar
## 48            2 years                        Spayed          f          collar
## 49            9 years                        Spayed          f          collar
## 50            4 years                        Spayed          f          collar
## 51            8 years                         Fixed                     collar
## 52            8 years                      Neutered          m          collar
## 53            8 years                        Spayed          f          collar
## 54            7 years                         Fixed                     collar
## 55            8 years                        Spayed          f          collar
## 56            8 years                      Neutered          m          collar
## 57            3 years                        Spayed          f          collar
## 58            2 years                        Spayed          f          collar
## 59             1 year                        Spayed          f          collar
## 60            5 years                        Spayed          f          collar
## 61            4 years                        Spayed          f          collar
## 62           11 years                        Spayed          f          collar
## 63            5 years                      Neutered          m          collar
## 64            3 years                      Neutered          m          collar
## 65            4 years                      Neutered          m          collar
## 66           10 years                        Spayed          f          collar
## 67            2 years                        Spayed          f          collar
## 68           12 years                      Neutered          m          collar
## 69            0 years                                                   collar
## 70          0.9 years                        Spayed          f          collar
## 71             1 year                      Neutered          m          collar
## 72            4 years                        Spayed          f          collar
## 73            3 years                        Spayed          f          collar
## 74            3 years                        Spayed          f          collar
## 75            7 years                        Spayed          f          collar
## 76            2 years                      Neutered          m          collar
## 77           12 years                      Neutered          m          collar
## 78            5 years                        Spayed          f          collar
## 79            6 years                        Spayed          f          collar
## 80            3 years                        Spayed          f          collar
## 81             1 year                      Neutered          m          collar
## 82            2 years                        Spayed          f          collar
## 83             1 year                      Neutered          m          collar
## 84            8 years                      Neutered          m          collar
## 85         0.58 years                        Spayed          f          collar
## 86            5 years                        Spayed          f          collar
## 87           12 years                        Spayed          f          collar
## 88            4 years                      Neutered          m          collar
## 89            2 years                      Neutered          m          collar
## 90           10 years                        Spayed          f          collar
## 91            0 years                      Neutered          m          collar
## 92           10 years                      Neutered          m          collar
## 93            3 years                      Neutered          m          collar
## 94            6 years                      Neutered          m          collar
## 95           13 years                      Neutered          m          collar
## 96            4 years                        Spayed          f          collar
## 97            7 years                      Neutered          m          collar
## 98             1 year                        Spayed          f          collar
## 99            7 years                      Neutered          m          collar
## 100           7 years                      Neutered          m          collar
## 101           4 years                      Neutered          m          collar
## 102            1 year                        Spayed          f          collar
## 103           3 years                        Spayed          f          collar
## 104           2 years                        Spayed          f          collar
## 105           2 years                        Spayed          f          collar
## 106           3 years                        Spayed          f          collar
## 107          15 years                        Spayed          f          collar
## 108          10 years                         Fixed                     collar
## 109           8 years                        Spayed          f          collar
## 110           3 years                        Spayed          f          collar
## 111           7 years                        Spayed          f          collar
## 112           3 years                        Spayed          f          collar
## 113           0 years                        Spayed          f          collar
## 114           9 years                      Neutered          m          collar
## 115           2 years                        Spayed          f          collar
## 116           9 years                      Neutered          m          collar
## 117           8 years                      Neutered          m          collar
## 118           2 years                        Spayed          f          collar
## 119           8 years                      Neutered          m          collar
## 120           2 years                      Neutered          m          collar
## 121           5 years                      Neutered          m          collar
## 122          11 years                        Spayed          f          collar
## 123           6 years                      Neutered          m          collar
## 124           8 years                      Neutered          m          collar
## 125           5 years                      Neutered          m          collar
## 126           0 years                                                   collar
## 127          11 years                      Neutered          m          collar
## 128           2 years                      Neutered          m          collar
## 129           4 years                        Spayed          f          collar
## 130           9 years                      Neutered          m          collar
## 131           3 years                        Spayed          f          collar
## 132           3 years                      Neutered          m          collar
## 133           4 years                        Spayed          f          collar
## 134           3 years                      Neutered          m          collar
## 135           3 years                      Neutered          m          collar
## 136           0 years                                                   collar
## 137          10 years                        Spayed          f          collar
## 138           2 years                      Neutered          m          collar
## 139          10 years                      Neutered          m          collar
## 140           2 years                      Neutered          m          collar
## 141          12 years                      Neutered          m          collar
## 142           0 years                                                   collar
## 143          11 years                        Spayed          f          collar
## 144          12 years                      Neutered          m          collar
## 145           2 years                      Neutered          m          collar
## 146           4 years                        Spayed          f          collar
## 147           3 years                        Spayed          f          collar
## 148           4 years                      Neutered          m          collar
## 149           6 years                      Neutered          m          collar
## 150           3 years                      Neutered          m          collar
## 151           7 years                      Neutered          m          collar
## 152          11 years                      Neutered          m          collar
## 153           4 years                      Neutered          m          collar
## 154         0.25years                        Spayed          f          collar
## 155           9 years                        Spayed          f          collar
## 156           3 years                      Neutered          m          collar
## 157           4 years                      Neutered          m          collar
## 158           6 years                      Neutered          m          collar
## 159           8 years                        Spayed          f          collar
## 160           3 years                        Spayed          f          collar
## 161           5 years                      Neutered          m          collar
## 162           6 years                      Neutered          m          collar
## 163                                                                     collar
## 164           2 years                      Neutered          m          collar
## 165          14 years                        Spayed          f          collar
## 166           2 years                      Neutered          m          collar
## 167           8 years                        Spayed          f          collar
## 168           7 years                      Neutered          m          collar
## 169          10 years                      Neutered          m          collar
## 170           4 years                        Spayed          f          collar
## 171           7 years                      Neutered          m          collar
## 172           7 years                      Neutered          m          collar
## 173           6 years                      Neutered          m          collar
## 174           4 years                      Neutered          m          collar
## 175            1 year                      Neutered          m          collar
## 176          12 years                     Not Fixed          m          collar
## 177           2 years                      Neutered          m          collar
## 178         0.4 years                      Neutered          m          collar
## 179           2 years                        Spayed          f          collar
## 180           2 years                        Spayed          f          collar
## 181           5 years                        Spayed          f          collar
## 182           5 years                      Neutered          m          collar
## 183           9 years                        Spayed          f          collar
## 184        0.67 years                        Spayed          f          collar
## 185          14 years                        Spayed          f          collar
## 186           8 years                        Spayed          f          collar
## 187           8 years                        Spayed          f          collar
## 188           2 years                      Neutered          m          collar
## 189          10 years                        Spayed          f          collar
## 190           4 years                        Spayed          f          collar
## 191           9 years                        Spayed          f          collar
## 192           9 years                      Neutered          m          collar
## 193            1 year                        Spayed          f          collar
## 194           2 years                      Neutered          m          collar
## 195           3 years                      Neutered          m          collar
## 196           5 years                        Spayed          f          collar
## 197          11 years                        Spayed          f          collar
## 198            1 year                        Spayed          f          collar
## 199           7 years                        Spayed          f          collar
## 200           8 years                      Neutered          m          collar
## 201           8 years                      Neutered          m          collar
## 202           2 years                     Not Fixed          m          collar
## 203           3 years                        Spayed          f          collar
## 204           2 years                      Neutered          m          collar
## 205           5 years                        Spayed          f          collar
## 206          15 years                      Neutered          m          collar
## 207          15 years                      Neutered          m          collar
## 208          10 years                        Spayed          f          collar
## 209           2 years                        Spayed          f          collar
## 210          14 years                      Neutered          m          collar
## 211          10 years                        Spayed          f          collar
## 212           2 years                      Neutered          m          collar
## 213           0 years                        Spayed          f          collar
## 214           4 years                      Neutered          m          collar
## 215        0.58 years                      Neutered          m          collar
## 216           8 years                        Spayed          f          collar
## 217           3 years                      Neutered          m          collar
## 218           2 years                        Spayed          f          collar
## 219           6 years                      Neutered          m          collar
## 220           3 years                        Spayed          f          collar
## 221           9 years                      Neutered          m          collar
## 222           5 years                        Spayed          f          collar
## 223           3 years                      Neutered          m          collar
## 224            1 year                         Fixed                     collar
## 225           6 years                      Neutered          m          collar
## 226           2 years                      Neutered          m          collar
## 227           6 years                      Neutered          m          collar
## 228           5 years                      Neutered          m          collar
## 229           2 years                      Neutered          m          collar
## 230          11 years                      Neutered          m          collar
## 231            1 year                      Neutered          m          collar
## 232          11 years                        Spayed          f          collar
## 233           3 years                        Spayed          f          collar
## 234           4 years                      Neutered          m          collar
## 235           3 years                      Neutered          m          collar
## 236           3 years                      Neutered          m          collar
## 237            1 year                      Neutered          m          collar
## 238           7 years                                        f          collar
## 239          10 years                        Spayed          f          collar
## 240          12 years                        Spayed          f          collar
## 241          15 years                      Neutered          m          collar
## 242        0.67 years                      Neutered          m          collar
## 243           8 years                      Neutered          m          collar
## 244          10 years                        Spayed          f          collar
## 245           6 years                      Neutered          m          collar
## 246           3 years                      Neutered          m          collar
## 247                                                                     collar
## 248           4 years                        Spayed          f          collar
## 249          11 years                        Spayed          f          collar
## 250           8 years                        Spayed          f          collar
## 251           7 years                      Neutered          m          collar
## 252           0 years                                                   collar
## 253          10 years                        Spayed          f          collar
## 254           0 years                                                   collar
## 255           6 years                      Neutered          m          collar
## 256           4 years                      Neutered          m          collar
## 257          17 years                      Neutered          m          collar
## 258           6 years                      Neutered          m          collar
## 259            1 year                        Spayed          f          collar
## 260           2 years                      Neutered          m          collar
## 261           7 years                      Neutered          m          collar
## 262           4 years                      Neutered          m          collar
## 263           9 years                      Neutered          m          collar
## 264           3 years                        Spayed          f          collar
## 265          14 years                      Neutered          m          collar
## 266          14 years                      Neutered          m          collar
## 267          13 years                        Spayed          f          collar
## 268            1 year                        Spayed          f          collar
## 269           7 years                        Spayed          f          collar
## 270          12 years                      Neutered          m          collar
## 271           2 years                        Spayed          f          collar
## 272           7 years                      Neutered          m          collar
## 273           2 years                      Neutered          m          collar
## 274           8 years                        Spayed          f          collar
## 275           0 years                     Not Fixed          m          collar
## 276          10 years                        Spayed          f          collar
## 277          10 years                        Spayed          f          collar
## 278           9 years                      Neutered          m          collar
## 279           3 years                      Neutered          m          collar
## 280           6 years                      Neutered          m          collar
## 281          11 years                        Spayed          f          collar
## 282           2 years                      Neutered          m          collar
## 283           2 years                      Neutered          m          collar
## 284           4 years                      Neutered          m          collar
## 285          10 years                      Neutered          m          collar
## 286           8 years                        Spayed          f          collar
## 287           2 years                      Neutered          m          collar
## 288           6 years                        Spayed          f          collar
## 289           0 years                         Fixed                     collar
## 290           2 years                        Spayed          f          collar
## 291           2 years                      Neutered          m          collar
## 292           4 years                        Spayed          f          collar
## 293            1 year                        Spayed          f          collar
## 294            1 year                        Spayed          f          collar
## 295           2 years                      Neutered          m          collar
## 296           8 years                      Neutered          m          collar
## 297           8 years                        Spayed          f          collar
## 298           2 years                      Neutered          m          collar
## 299            1 year                      Neutered          m          collar
## 300           3 years                      Neutered          m          collar
## 301            1 year                      Neutered          m          collar
## 302           4 years                        Spayed          f          collar
## 303           6 years                        Spayed          f          collar
## 304           7 years                      Neutered          m          collar
## 305           8 years                      Neutered          m          collar
## 306           7 years                        Spayed          f          collar
## 307           4 years                      Neutered          m          collar
## 308           6 years                        Spayed          f          collar
## 309           4 years                      Neutered          m          collar
## 310           9 years                      Neutered          m          collar
## 311           6 years                        Spayed          f          collar
## 312          13 years                        Spayed          f          collar
## 313           6 years                        Spayed          f          collar
## 314          13 years                      Neutered          m          collar
## 315           2 years                      Neutered          m          collar
## 316          12 years                        Spayed          f          collar
## 317           6 years                      Neutered          m          collar
## 318          10 years                      Neutered          m          collar
## 319           3 years                         Fixed                     collar
## 320          11 years                      Neutered          m          collar
## 321           7 years                      Neutered          m          collar
## 322           6 years                      Neutered          m          collar
## 323          14 years                      Neutered          m          collar
## 324           2 years                        Spayed          f          collar
## 325            1 year                        Spayed          f          collar
## 326           8 years                        Spayed          f          collar
## 327          12 years                      Neutered          m          collar
## 328           4 years                        Spayed          f          collar
## 329            1 year                      Neutered          m          collar
## 330           3 years                      Neutered          m          collar
## 331           3 years                        Spayed          f          collar
## 332           9 years                        Spayed          f          collar
## 333           8 years                        Spayed          f          collar
## 334           9 years                        Spayed          f          collar
## 335          10 years                      Neutered          m          collar
## 336           2 years                      Neutered          m          collar
## 337           6 years                        Spayed          f          collar
## 338           0 years                                                   collar
## 339           5 years                         Fixed                     collar
## 340          14 years                      Neutered          m          collar
## 341           4 years                        Spayed          f          collar
## 342           2 years                        Spayed          f          collar
## 343           3 years                      Neutered          m          collar
## 344        0.58 years                      Neutered          m          collar
## 345           3 years                        Spayed          f          collar
## 346          11 years                      Neutered          m          collar
## 347            1 year                      Neutered          m          collar
## 348           6 years                      Neutered          m          collar
## 349           6 years                        Spayed          f          collar
## 350           8 years                      Neutered          m          collar
## 351            1 year                      Neutered          m          collar
## 352           9 years                        Spayed          f          collar
## 353           9 years                      Neutered          m          collar
## 354          14 years                        Spayed          f          collar
## 355           2 years                      Neutered          m          collar
## 356           7 years                      Neutered          m          collar
## 357           7 years                      Neutered          m          collar
## 358           3 years                        Spayed          f          collar
## 359           3 years                        Spayed          f          collar
## 360            1 year                        Spayed          f          collar
## 361           9 years                      Neutered          m          collar
## 362           6 years                      Neutered          m          collar
## 363          16 years                      Neutered          m          collar
## 364           8 years                        Spayed          f          collar
## 365           5 years                        Spayed          f          collar
## 366           5 years                      Neutered          m          collar
## 367           6 years                        Spayed          f          collar
## 368           7 years                      Neutered          m          collar
## 369            1 year                        Spayed          f          collar
## 370           4 years                        Spayed          f          collar
## 371          12 years                      Neutered          m          collar
## 372            1 year                      Neutered          m          collar
## 373          11 years                      Neutered          m          collar
## 374          14 years                        Spayed          f          collar
## 375           2 years                      Neutered          m          collar
## 376         0.4 years                     Not Fixed          f          collar
## 377         0.5 years                     Not Fixed          m          collar
## 378           4 years                        Spayed          f          collar
## 379          13 years                        Spayed          f          collar
## 380           7 years                                                   collar
## 381           2 years                      Neutered          m          collar
## 382           6 years                        Spayed          f          collar
## 383           8 years                        Spayed          f          collar
## 384           2 years                      Neutered          m          collar
## 385          11 years                        Spayed          f          collar
## 386            1 year                      Neutered          m          collar
## 387           8 years                      Neutered          m          collar
## 388           5 years                      Neutered          m          collar
## 389           6 years                        Spayed          f          collar
## 390          10 years                      Neutered          m          collar
## 391           0 years                        Spayed          f          collar
## 392          10 years                        Spayed          f          collar
## 393           2 years                      Neutered          m          collar
## 394           0 years                                                   collar
## 395           5 years                      Neutered          m          collar
## 396           0 years                                                   collar
## 397           8 years                        Spayed          f          collar
## 398           5 years                        Spayed          f          collar
## 399         0.5 years                     Not Fixed          f          collar
## 400           6 years                        Spayed          f          collar
## 401           2 years                        Spayed          f          collar
## 402           9 years                         Fixed                     collar
## 403            1 year                      Neutered          m          collar
## 404           0 years                                                   collar
## 405           5 years                      Neutered          m          collar
## 406           3 years                      Neutered          m          collar
## 407          10 years                        Spayed          f          collar
## 408           3 years                        Spayed          f          collar
## 409           5 years                        Spayed          f          collar
## 410           4 years                      Neutered          m          collar
## 411           3 years                      Neutered          m          collar
## 412           3 years                        Spayed          f          collar
## 413            1 year                      Neutered          m          collar
## 414           2 years                        Spayed          f          collar
## 415           4 years                        Spayed          f          collar
## 416           4 years                      Neutered          m          collar
## 417           4 years                        Spayed          f          collar
## 418           3 years                        Spayed          f          collar
## 419           2 years                        Spayed          f          collar
## 420         0.4 years                     Not Fixed          m          collar
## 421          11 years                        Spayed          f          collar
## 422           2 years                      Neutered          m          collar
## 423           2 years                        Spayed          f          collar
## 424          19 years                      Neutered          m          collar
## 425          15 years                        Spayed          f          collar
## 426           2 years                      Neutered          m          collar
## 427           4 years                      Neutered          m          collar
## 428            1 year                      Neutered          m          collar
## 429           2 years                      Neutered          m          collar
## 430           9 years                      Neutered          m          collar
## 431          16 years                        Spayed          f          collar
## 432           4 years                        Spayed          f          collar
## 433           5 years                      Neutered          m          collar
## 434           5 years                      Neutered          m          collar
## 435           8 years                        Spayed          f          collar
## 436           7 years                      Neutered          m          collar
## 437           3 years                        Spayed          f          collar
## 438            1 year                        Spayed          f          collar
## 439          19 years                      Neutered          m          collar
## 440           9 years                      Neutered          m          collar
## 441           4 years                        Spayed          f          collar
## 442           8 years                      Neutered          m          collar
##     data.processing.software deployment.end.type         deployment.id
## 1                   @Trip PC             removal                 Snowy
## 2                   @Trip PC             removal                 Gomez
## 3                   @Trip PC             removal               Frizzle
## 4                   @Trip PC             removal                MonMon
## 5                   @Trip PC             removal                 Buddy
## 6                   @Trip PC             removal                 Tiger
## 7                   @Trip PC             removal                 Jamie
## 8                   @Trip PC             removal BruceWillisPussington
## 9                   @Trip PC             removal              Skittles
## 10                  @Trip PC             removal              Princess
## 11                  @Trip PC             removal                 Lofty
## 12                  @Trip PC             removal                Zazzie
## 13                  @Trip PC             removal               Crumpet
## 14                  @Trip PC             removal                  Toby
## 15                  @Trip PC             removal                Isabel
## 16                  @Trip PC             removal                 Penny
## 17                  @Trip PC             removal                 Misty
## 18                  @Trip PC             removal                Sookie
## 19                  @Trip PC             removal                Pippin
## 20                  @Trip PC             removal                Squeak
## 21                  @Trip PC             removal                Anakin
## 22                  @Trip PC             removal                 Ernie
## 23                  @Trip PC             removal                 Wally
## 24                  @Trip PC             removal                 Bella
## 25                  @Trip PC             removal                   Jax
## 26                  @Trip PC             removal                Ramses
## 27                  @Trip PC             removal                 Tigga
## 28                  @Trip PC             removal            FantaPants
## 29                  @Trip PC             removal               Matilda
## 30                  @Trip PC             removal                 Tilen
## 31                  @Trip PC             removal                   Bel
## 32                  @Trip PC             removal                 Lucy2
## 33                  @Trip PC             removal                 Ollie
## 34                  @Trip PC             removal               Charlie
## 35                  @Trip PC             removal                 Sissy
## 36                  @Trip PC             removal                Louise
## 37                  @Trip PC             removal                Jasper
## 38                  @Trip PC             removal             Lightning
## 39                  @Trip PC             removal                 Mandy
## 40                  @Trip PC             removal                 Saffy
## 41                  @Trip PC             removal                Shelly
## 42                  @Trip PC             removal                  Ruby
## 43                  @Trip PC             removal                Thomas
## 44                  @Trip PC             removal                 Mysti
## 45                  @Trip PC             removal                 Costa
## 46                  @Trip PC             removal               Moochie
## 47                  @Trip PC             removal              Sapphire
## 48                  @Trip PC             removal                Raizor
## 49                  @Trip PC             removal               NekoMei
## 50                  @Trip PC             removal                 Milly
## 51                  @Trip PC             removal               Slasher
## 52                  @Trip PC             removal        DetectiveTeddy
## 53                  @Trip PC             removal                Misty2
## 54                  @Trip PC             removal              BigFella
## 55                  @Trip PC             removal            LadyVelvet
## 56                  @Trip PC             removal                   Max
## 57                  @Trip PC             removal                  Mika
## 58                  @Trip PC             removal                  Atik
## 59                  @Trip PC             removal                Kallie
## 60                  @Trip PC             removal                 Pixie
## 61                  @Trip PC             removal                 Genie
## 62                  @Trip PC             removal                Milly2
## 63                  @Trip PC             removal                Skitty
## 64                  @Trip PC             removal                  Max2
## 65                  @Trip PC             removal                 Kitty
## 66                  @Trip PC             removal                 Drake
## 67                  @Trip PC             removal               Delilah
## 68                  @Trip PC             removal                Kramer
## 69                  @Trip PC             removal                 Molly
## 70                  @Trip PC             removal                 Adele
## 71                  @Trip PC             removal                 Henry
## 72                  @Trip PC             removal                   Rio
## 73                  @Trip PC             removal                Smokey
## 74                  @Trip PC             removal                 Sushi
## 75                  @Trip PC             removal                Midori
## 76                  @Trip PC             removal                 Toby2
## 77                  @Trip PC             removal                   Tom
## 78                  @Trip PC             removal                 Mingo
## 79                  @Trip PC             removal                 Sassy
## 80                  @Trip PC             removal                Bandit
## 81                  @Trip PC             removal                Hunter
## 82                  @Trip PC             removal                Darkie
## 83                  @Trip PC             removal                 Mango
## 84                  @Trip PC             removal                Casper
## 85                  @Trip PC             removal                  Milo
## 86                  @Trip PC             removal             Charlotte
## 87                  @Trip PC             removal                Wizzer
## 88                  @Trip PC             removal                Mickey
## 89                  @Trip PC             removal                Johnny
## 90                  @Trip PC             removal            MapleSyrup
## 91                  @Trip PC             removal               Muggins
## 92                  @Trip PC             removal               Caspian
## 93                  @Trip PC             removal                  Lief
## 94                  @Trip PC             removal                 Rufus
## 95                  @Trip PC             removal                   Uno
## 96                  @Trip PC             removal                Nellie
## 97                  @Trip PC             removal                   Bob
## 98                  @Trip PC             removal                Fergie
## 99                  @Trip PC             removal               Marbles
## 100                 @Trip PC             removal                 Clawd
## 101                 @Trip PC             removal                Rodney
## 102                 @Trip PC             removal                 Ninja
## 103                 @Trip PC             removal                 Lucy3
## 104                 @Trip PC             removal                Martha
## 105                 @Trip PC             removal                  Momo
## 106                 @Trip PC             removal              Lilliput
## 107                 @Trip PC             removal                Lucims
## 108                 @Trip PC             removal                 Tipsy
## 109                 @Trip PC             removal              MissyMoo
## 110                 @Trip PC             removal                 Minty
## 111                 @Trip PC             removal                 Tilly
## 112                 @Trip PC             removal                Tipsie
## 113                 @Trip PC             removal                 Pearl
## 114                 @Trip PC             removal                 Ringo
## 115                 @Trip PC             removal          LibertyBelle
## 116                 @Trip PC             removal                Smudge
## 117                 @Trip PC             removal                 Scout
## 118                 @Trip PC             removal                  Roxi
## 119                 @Trip PC             removal                 Darcy
## 120                 @Trip PC             removal                   Zak
## 121                 @Trip PC             removal                Kitty2
## 122                 @Trip PC             removal               Millie2
## 123                 @Trip PC             removal                   Cat
## 124                 @Trip PC             removal                Darcy2
## 125                 @Trip PC             removal                 Bugsy
## 126                 @Trip PC             removal                Stumpy
## 127                 @Trip PC             removal                 Rumpy
## 128                 @Trip PC             removal               Eldrick
## 129                 @Trip PC             removal                 Latte
## 130                 @Trip PC             removal                Merlot
## 131                 @Trip PC             removal                 Holly
## 132                 @Trip PC             removal                George
## 133                 @Trip PC             removal               Sparkle
## 134                 @Trip PC             removal                 Basil
## 135                 @Trip PC             removal               Milfred
## 136                 @Trip PC             removal               ZazzieM
## 137                 @Trip PC             removal                Sylvia
## 138                 @Trip PC             removal                   Leo
## 139                 @Trip PC             removal                 Oscar
## 140                 @Trip PC             removal                Pepper
## 141                 @Trip PC             removal               Jericho
## 142                 @Trip PC             removal               IronMan
## 143                 @Trip PC             removal                 Moggy
## 144                 @Trip PC             removal                 Louis
## 145                 @Trip PC             removal              Thornton
## 146                 @Trip PC             removal                  Cleo
## 147                 @Trip PC             removal                 Angel
## 148                 @Trip PC             removal          CharlieFrank
## 149                 @Trip PC             removal                Bungey
## 150                 @Trip PC             removal                   Gus
## 151                 @Trip PC             removal              Pusskins
## 152                 @Trip PC             removal                 Eddie
## 153                 @Trip PC             removal            Goldfinger
## 154                 @Trip PC             removal           PussyGalore
## 155                 @Trip PC             removal                 Tasha
## 156                 @Trip PC             removal           ChuckNorris
## 157                 @Trip PC             removal               Phoenix
## 158                 @Trip PC             removal                  Jack
## 159                 @Trip PC             removal                  Cosi
## 160                 @Trip PC             removal                 Boots
## 161                 @Trip PC             removal                  Yoda
## 162                 @Trip PC             removal                Buddha
## 163                 @Trip PC             removal              Bagheera
## 164                 @Trip PC             removal                 Milo2
## 165                 @Trip PC             removal                Winnie
## 166                 @Trip PC             removal                Xander
## 167                 @Trip PC             removal                 Patra
## 168                 @Trip PC             removal                 Pants
## 169                 @Trip PC             removal                  Leo2
## 170                 @Trip PC             removal                 Lilly
## 171                 @Trip PC             removal                 Benny
## 172                 @Trip PC             removal                  Muss
## 173                 @Trip PC             removal                    DC
## 174                 @Trip PC             removal                 Milo3
## 175                 @Trip PC             removal                Merlin
## 176                 @Trip PC             removal                 Tippy
## 177                 @Trip PC             removal                  HaHa
## 178                 @Trip PC             removal                  Paul
## 179                 @Trip PC             removal                 Ruby2
## 180                 @Trip PC             removal               Panther
## 181                 @Trip PC             removal               Mittens
## 182                 @Trip PC             removal                Jester
## 183                 @Trip PC             removal                 Ruby3
## 184                 @Trip PC             removal                   Ali
## 185                 @Trip PC             removal                Stinky
## 186                 @Trip PC             removal                Shadow
## 187                 @Trip PC             removal              KikkiDee
## 188                 @Trip PC             removal               Jasper2
## 189                 @Trip PC             removal                 Sooty
## 190                 @Trip PC             removal                 Rosie
## 191                 @Trip PC             removal              MonaLisa
## 192                 @Trip PC             removal                 Boofy
## 193                 @Trip PC             removal                Kitty3
## 194                 @Trip PC             removal                 Bruce
## 195                 @Trip PC             removal          LucarioBlack
## 196                 @Trip PC             removal              Truffula
## 197                 @Trip PC             removal                Rosie2
## 198                 @Trip PC             removal                Jazzie
## 199                 @Trip PC             removal                  Lila
## 200                 @Trip PC             removal           MrLittleCat
## 201                 @Trip PC             removal                   Ash
## 202                 @Trip PC             removal                Kitty4
## 203                 @Trip PC             removal                 Ruby4
## 204                 @Trip PC             removal                 Felix
## 205                 @Trip PC             removal                Bella5
## 206                 @Trip PC             removal                Tiger3
## 207                 @Trip PC             removal                Cheeky
## 208                 @Trip PC             removal                  Maca
## 209                 @Trip PC             removal                 Ivory
## 210                 @Trip PC             removal                  Soxy
## 211                 @Trip PC             removal                Tilly2
## 212                 @Trip PC             removal                 Harry
## 213                 @Trip PC             removal                Misty3
## 214                 @Trip PC             removal                   Boo
## 215                 @Trip PC             removal                 Grimm
## 216                 @Trip PC             removal                 Bessy
## 217                 @Trip PC             removal                Nelson
## 218                 @Trip PC             removal              Marbles2
## 219                 @Trip PC             removal                   Tux
## 220                 @Trip PC             removal                Ripley
## 221                 @Trip PC             removal               Merlin2
## 222                 @Trip PC             removal            MrWhiskers
## 223                 @Trip PC             removal              Charlie2
## 224                 @Trip PC             removal                 Hazel
## 225                 @Trip PC             removal                  Leo3
## 226                 @Trip PC             removal                 Soxy2
## 227                 @Trip PC             removal                  Theo
## 228                 @Trip PC             removal                Geezer
## 229                 @Trip PC             removal                Harry2
## 230                 @Trip PC             removal              Midnight
## 231                 @Trip PC             removal                Rupert
## 232                 @Trip PC             removal               Jasmine
## 233                 @Trip PC             removal                  Lily
## 234                 @Trip PC             removal                Buddy2
## 235                 @Trip PC             removal              Sibelius
## 236                 @Trip PC             removal                 Timmy
## 237                 @Trip PC             removal              Flanders
## 238                 @Trip PC             removal                   Zoe
## 239                 @Trip PC             removal               Pepper2
## 240                 @Trip PC             removal              Charlie3
## 241                 @Trip PC             removal                 Sunny
## 242                 @Trip PC             removal                Rascal
## 243                 @Trip PC             removal               Schnooz
## 244                 @Trip PC             removal              AmbaRose
## 245                 @Trip PC             removal                 Tobin
## 246                 @Trip PC             removal               Rupert2
## 247                 @Trip PC             removal                 Alfie
## 248                 @Trip PC             removal                Felix2
## 249                 @Trip PC             removal                 Sammi
## 250                 @Trip PC             removal               Picasso
## 251                 @Trip PC             removal                Tiger4
## 252                 @Trip PC             removal                 Monty
## 253                 @Trip PC             removal                Amelia
## 254                 @Trip PC             removal                 Theo2
## 255                 @Trip PC             removal               Sputnik
## 256                 @Trip PC             removal                 Kenny
## 257                 @Trip PC             removal                Dharma
## 258                 @Trip PC             removal              Charlie4
## 259                 @Trip PC             removal                Imelda
## 260                 @Trip PC             removal                 Tiggy
## 261                 @Trip PC             removal                 Sammy
## 262                 @Trip PC             removal                 Fudge
## 263                 @Trip PC             removal               Georgie
## 264                 @Trip PC             removal               Martha2
## 265                 @Trip PC             removal                 Raffy
## 266                 @Trip PC             removal                Boots2
## 267                 @Trip PC             removal                Yoriko
## 268                 @Trip PC             removal                  Ziva
## 269                 @Trip PC             removal               Millie3
## 270                 @Trip PC             removal               Bentley
## 271                 @Trip PC             removal              Diamonds
## 272                 @Trip PC             removal                Monty2
## 273                 @Trip PC             removal                  Rudi
## 274                 @Trip PC             removal                Bella6
## 275                 @Trip PC             removal               Squeaky
## 276                 @Trip PC             removal                Stalky
## 277                 @Trip PC             removal            Unfriendly
## 278                 @Trip PC             removal                 Bluey
## 279                 @Trip PC             removal                  Shan
## 280                 @Trip PC             removal                Stitch
## 281                 @Trip PC             removal                  Puds
## 282                 @Trip PC             removal                 Tommy
## 283                 @Trip PC             removal                  Max3
## 284                 @Trip PC             removal                Moscow
## 285                 @Trip PC             removal              MrThomas
## 286                 @Trip PC             removal             MissyMitz
## 287                 @Trip PC             removal            JessieBeau
## 288                 @Trip PC             removal               Skoozie
## 289                 @Trip PC             removal               Frankie
## 290                 @Trip PC             removal                 Annie
## 291                 @Trip PC             removal                   Zac
## 292                 @Trip PC             removal                 Polly
## 293                 @Trip PC             removal                  Onyx
## 294                 @Trip PC             removal                 Chase
## 295                 @Trip PC             removal                 Astro
## 296                 @Trip PC             removal                  Loki
## 297                 @Trip PC             removal                Isobel
## 298                 @Trip PC             removal              Squiggle
## 299                 @Trip PC             removal                   Oli
## 300                 @Trip PC             removal                  Spot
## 301                 @Trip PC             removal               Jasper3
## 302                 @Trip PC             removal                 Jedda
## 303                 @Trip PC             removal                Shmiff
## 304                 @Trip PC             removal               Nelson3
## 305                 @Trip PC             removal                Claude
## 306                 @Trip PC             removal                Maggie
## 307                 @Trip PC             removal                Harry3
## 308                 @Trip PC             removal                Daisy2
## 309                 @Trip PC             removal                    Bo
## 310                 @Trip PC             removal                Albert
## 311                 @Trip PC             removal                Caddie
## 312                 @Trip PC             removal                 Kamir
## 313                 @Trip PC             removal               Fizzgig
## 314                 @Trip PC             removal                 Paris
## 315                 @Trip PC             removal                Felix3
## 316                 @Trip PC             removal                  Bel2
## 317                 @Trip PC             removal               Meggsie
## 318                 @Trip PC             removal               Rupert3
## 319                 @Trip PC             removal                Celine
## 320                 @Trip PC             removal                 Billy
## 321                 @Trip PC             removal                Oscar2
## 322                 @Trip PC             removal                   Boz
## 323                 @Trip PC             removal             Truffles2
## 324                 @Trip PC             removal                Kitty5
## 325                 @Trip PC             removal                  Jinx
## 326                 @Trip PC             removal                Chanel
## 327                 @Trip PC             removal       LouisTheBruiser
## 328                 @Trip PC             removal                 Paula
## 329                 @Trip PC             removal                MrGray
## 330                 @Trip PC             removal              Malachie
## 331                 @Trip PC             removal                  Gina
## 332                 @Trip PC             removal                Banjo2
## 333                 @Trip PC             removal              Junipurr
## 334                 @Trip PC             removal                Clancy
## 335                 @Trip PC             removal                Tiger5
## 336                 @Trip PC             removal                Tarzan
## 337                 @Trip PC             removal                 Jinx2
## 338                 @Trip PC             removal                 Bruno
## 339                 @Trip PC             removal                Collin
## 340                 @Trip PC             removal               Twinkle
## 341                 @Trip PC             removal                Whisky
## 342                 @Trip PC             removal                  Coco
## 343                 @Trip PC             removal        CaptainSoldier
## 344                 @Trip PC             removal               Ronaldo
## 345                 @Trip PC             removal                 Bambi
## 346                 @Trip PC             removal                Hestor
## 347                 @Trip PC             removal                 Banjo
## 348                 @Trip PC             removal                 Garry
## 349                 @Trip PC             removal                  Ally
## 350                 @Trip PC             removal                Oscar3
## 351                 @Trip PC             removal                  Yogi
## 352                 @Trip PC             removal                  Puss
## 353                 @Trip PC             removal                 Small
## 354                 @Trip PC             removal               Scratch
## 355                 @Trip PC             removal                Diesel
## 356                 @Trip PC             removal               Stanley
## 357                 @Trip PC             removal                  Tom2
## 358                 @Trip PC             removal               Truffle
## 359                 @Trip PC             removal                 Missy
## 360                 @Trip PC             removal                Rafiki
## 361                 @Trip PC             removal              Carmello
## 362                 @Trip PC             removal         MilkoMcCavity
## 363                 @Trip PC             removal                  Leo4
## 364                 @Trip PC             removal                 Leila
## 365                 @Trip PC             removal                Kitty6
## 366                 @Trip PC             removal                Samson
## 367                 @Trip PC             removal              Delilah2
## 368                 @Trip PC             removal             Sylvester
## 369                 @Trip PC             removal                    BB
## 370                 @Trip PC             removal                 Sally
## 371                 @Trip PC             removal                  Elmo
## 372                 @Trip PC             removal               Stealth
## 373                 @Trip PC             removal                 Jones
## 374                 @Trip PC             removal                  Mimi
## 375                 @Trip PC             removal              Charlie6
## 376                 @Trip PC             removal                  Sox2
## 377                 @Trip PC             removal                 Frank
## 378                 @Trip PC             removal                  Dana
## 379                 @Trip PC             removal                  Gigi
## 380                 @Trip PC             removal                 Venom
## 381                 @Trip PC             removal               Wiggles
## 382                 @Trip PC             removal                 Magic
## 383                 @Trip PC             removal                Mowdii
## 384                 @Trip PC             removal                  Mynx
## 385                 @Trip PC             removal                Sooty2
## 386                 @Trip PC             removal                  Seth
## 387                 @Trip PC             removal                Henry2
## 388                 @Trip PC             removal                 Eesti
## 389                 @Trip PC             removal                 Rosey
## 390                 @Trip PC             removal                  Remy
## 391                 @Trip PC             removal                  Luna
## 392                 @Trip PC             removal                   Tia
## 393                 @Trip PC             removal               Jasper4
## 394                 @Trip PC             removal                Drake2
## 395                 @Trip PC             removal     GagglesHieronymus
## 396                 @Trip PC             removal                  Bibi
## 397                 @Trip PC             removal                Meesha
## 398                 @Trip PC             removal               Flossie
## 399                 @Trip PC             removal                 Multi
## 400                 @Trip PC             removal                 Furry
## 401                 @Trip PC             removal              SugarRae
## 402                 @Trip PC             removal                Trixie
## 403                 @Trip PC             removal               Keytuyn
## 404                 @Trip PC             removal                Kitten
## 405                 @Trip PC             removal                  Joey
## 406                 @Trip PC             removal                  Ash2
## 407                 @Trip PC             removal                 Rudi2
## 408                 @Trip PC             removal          SophiePossum
## 409                 @Trip PC             removal                   Roo
## 410                 @Trip PC             removal                 Rocky
## 411                 @Trip PC             removal               George2
## 412                 @Trip PC             removal                Tilly3
## 413                 @Trip PC             removal                Frosty
## 414                 @Trip PC             removal                Mystic
## 415                 @Trip PC             removal               Stumpy2
## 416                 @Trip PC             removal               Biggles
## 417                 @Trip PC             removal                 Cleo2
## 418                 @Trip PC             removal              Sparkles
## 419                 @Trip PC             removal     GracieLouFreebush
## 420                 @Trip PC             removal                Simba2
## 421                 @Trip PC             removal                 Zorra
## 422                 @Trip PC             removal                Weasel
## 423                 @Trip PC             removal              BellaBoo
## 424                 @Trip PC             removal                 Gizmo
## 425                 @Trip PC             removal                 Rusty
## 426                 @Trip PC             removal               Stefano
## 427                 @Trip PC             removal                Zepher
## 428                 @Trip PC             removal                 Ariel
## 429                 @Trip PC             removal                  Enzo
## 430                 @Trip PC             removal                Alrari
## 431                 @Trip PC             removal             MissChief
## 432                 @Trip PC             removal                  Lexi
## 433                 @Trip PC             removal                Rodger
## 434                 @Trip PC             removal                  Timo
## 435                 @Trip PC             removal              Jasmine2
## 436                 @Trip PC             removal                  Polo
## 437                 @Trip PC             removal                 Maeve
## 438                 @Trip PC             removal            Charlotte2
## 439                 @Trip PC             removal                Lister
## 440                 @Trip PC             removal              Garfield
## 441                 @Trip PC             removal                 Belle
## 442                 @Trip PC             removal                Gyhjor
##      duty.cycle      manipulation.comments manipulation.type study.site
## 1   3-min fixes  hrs_indoors: 0; n_cats: 2 manipulated other  Australia
## 2   3-min fixes  hrs_indoors: 0; n_cats: 3 manipulated other  Australia
## 3   3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 4   3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 5   3-min fixes hrs_indoors: 21; n_cats: 1 manipulated other  Australia
## 6   3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 7   3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 8   3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 9   3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 10  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 11  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 12  3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 13  3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 14  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 15  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 16  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 17  3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 18  3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 19  3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 20  3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 21  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 22  3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 23  3-min fixes  hrs_indoors: 5; n_cats: 2 manipulated other  Australia
## 24  3-min fixes hrs_indoors: 23; n_cats: 1 manipulated other  Australia
## 25  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 26  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 27  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 28  3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 29  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 30  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 31  3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 32  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 33  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 34  3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 35  3-min fixes hrs_indoors: 18; n_cats: 3 manipulated other  Australia
## 36  3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 37  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 38  3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 39  3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 40  3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 41  3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 42  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 43  3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 44  3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 45  3-min fixes                       #N/A manipulated other  Australia
## 46  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 47  3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 48  3-min fixes  hrs_indoors: 8; n_cats: 2 manipulated other  Australia
## 49  3-min fixes hrs_indoors: 14; n_cats: 3 manipulated other  Australia
## 50  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 51  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 52  3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 53  3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 54  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 55  3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 56  3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 57  3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 58  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 59  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 60  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 61  3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 62  3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 63  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 64  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 65  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 66  3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 67  3-min fixes hrs_indoors: 18; n_cats: 2 manipulated other  Australia
## 68  3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 69  3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 70  3-min fixes hrs_indoors: 24; n_cats: 1 manipulated other  Australia
## 71  3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 72  3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 73  3-min fixes  hrs_indoors: 6; n_cats: 2 manipulated other  Australia
## 74  3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 75  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 76  3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 77  3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 78  3-min fixes hrs_indoors: 19; n_cats: 2 manipulated other  Australia
## 79  3-min fixes hrs_indoors: 19; n_cats: 2 manipulated other  Australia
## 80  3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 81  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 82  3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 83  3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 84  3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 85  3-min fixes hrs_indoors: 14; n_cats: 4 manipulated other  Australia
## 86  3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 87  3-min fixes  hrs_indoors: 8; n_cats: 2 manipulated other  Australia
## 88  3-min fixes  hrs_indoors: 8; n_cats: 2 manipulated other  Australia
## 89  3-min fixes  hrs_indoors: 4; n_cats: 3 manipulated other  Australia
## 90  3-min fixes hrs_indoors: 21; n_cats: 3 manipulated other  Australia
## 91  3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 92  3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 93  3-min fixes hrs_indoors: 24; n_cats: 1 manipulated other  Australia
## 94  3-min fixes hrs_indoors: 18; n_cats: 4 manipulated other  Australia
## 95  3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 96  3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 97  3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 98  3-min fixes hrs_indoors: 24; n_cats: 2 manipulated other  Australia
## 99  3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 100 3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 101 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 102 3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 103 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 104 3-min fixes hrs_indoors: 20; n_cats: 3 manipulated other  Australia
## 105 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 106 3-min fixes hrs_indoors: 23; n_cats: 2 manipulated other  Australia
## 107 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 108 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 109 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 110 3-min fixes hrs_indoors: 16; n_cats: 2 manipulated other  Australia
## 111 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 112 3-min fixes  hrs_indoors: 5; n_cats: 2 manipulated other  Australia
## 113 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 114 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 115 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 116 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 117 3-min fixes  hrs_indoors: 3; n_cats: 3 manipulated other  Australia
## 118 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 119 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 120 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 121 3-min fixes hrs_indoors: 13; n_cats: 2 manipulated other  Australia
## 122 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 123 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 124 3-min fixes hrs_indoors: 23; n_cats: 1 manipulated other  Australia
## 125 3-min fixes  hrs_indoors: 6; n_cats: 2 manipulated other  Australia
## 126 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 127 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 128 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 129 3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 130 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 131 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 132 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 133 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 134 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 135 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 136 3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 137 3-min fixes hrs_indoors: 14; n_cats: 2 manipulated other  Australia
## 138 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 139 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 140 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 141 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 142 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 143 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 144 3-min fixes hrs_indoors: 21; n_cats: 1 manipulated other  Australia
## 145 3-min fixes  hrs_indoors: 1; n_cats: 2 manipulated other  Australia
## 146 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 147 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 148 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 149 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 150 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 151 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 152 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 153 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 154 3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 155 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 156 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 157 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 158 3-min fixes  hrs_indoors: 2; n_cats: 2 manipulated other  Australia
## 159 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 160 3-min fixes  hrs_indoors: 8; n_cats: 3 manipulated other  Australia
## 161 3-min fixes hrs_indoors: 16; n_cats: 2 manipulated other  Australia
## 162 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 163 3-min fixes                            manipulated other  Australia
## 164 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 165 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 166 3-min fixes hrs_indoors: 15; n_cats: 3 manipulated other  Australia
## 167 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 168 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 169 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 170 3-min fixes  hrs_indoors: 1; n_cats: 3 manipulated other  Australia
## 171 3-min fixes hrs_indoors: 13; n_cats: 2 manipulated other  Australia
## 172 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 173 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 174 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 175 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 176 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 177 3-min fixes hrs_indoors: 17; n_cats: 2 manipulated other  Australia
## 178 3-min fixes hrs_indoors: 10; n_cats: 3 manipulated other  Australia
## 179 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 180 3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 181 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 182 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 183 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 184 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 185 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 186 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 187 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 188 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 189 3-min fixes  hrs_indoors: 4; n_cats: 2 manipulated other  Australia
## 190 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 191 3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 192 3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 193 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 194 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 195 3-min fixes hrs_indoors: 19; n_cats: 2 manipulated other  Australia
## 196 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 197 3-min fixes hrs_indoors: 16; n_cats: 2 manipulated other  Australia
## 198 3-min fixes  hrs_indoors: 6; n_cats: 3 manipulated other  Australia
## 199 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 200 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 201 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 202 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 203 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 204 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 205 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 206 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 207 3-min fixes hrs_indoors: 16; n_cats: 2 manipulated other  Australia
## 208 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 209 3-min fixes hrs_indoors: 19; n_cats: 1 manipulated other  Australia
## 210 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 211 3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 212 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 213 3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 214 3-min fixes  hrs_indoors: 0; n_cats: 3 manipulated other  Australia
## 215 3-min fixes  hrs_indoors: 8; n_cats: 4 manipulated other  Australia
## 216 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 217 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 218 3-min fixes  hrs_indoors: 0; n_cats: 3 manipulated other  Australia
## 219 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 220 3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 221 3-min fixes  hrs_indoors: 3; n_cats: 2 manipulated other  Australia
## 222 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 223 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 224 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 225 3-min fixes  hrs_indoors: 5; n_cats: 2 manipulated other  Australia
## 226 3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 227 3-min fixes  hrs_indoors: 5; n_cats: 3 manipulated other  Australia
## 228 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 229 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 230 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 231 3-min fixes  hrs_indoors: 8; n_cats: 3 manipulated other  Australia
## 232 3-min fixes hrs_indoors: 18; n_cats: 2 manipulated other  Australia
## 233 3-min fixes hrs_indoors: 12; n_cats: 4 manipulated other  Australia
## 234 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 235 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 236 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 237 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 238 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 239 3-min fixes  hrs_indoors: 2; n_cats: 2 manipulated other  Australia
## 240 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 241 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 242 3-min fixes hrs_indoors: 16; n_cats: 3 manipulated other  Australia
## 243 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 244 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 245 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 246 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 247 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 248 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 249 3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 250 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 251 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 252 3-min fixes  hrs_indoors: 0; n_cats: 3 manipulated other  Australia
## 253 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 254 3-min fixes  hrs_indoors: 0; n_cats: 2 manipulated other  Australia
## 255 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 256 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 257 3-min fixes  hrs_indoors: 9; n_cats: 2 manipulated other  Australia
## 258 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 259 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 260 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 261 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 262 3-min fixes hrs_indoors: 11; n_cats: 1 manipulated other  Australia
## 263 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 264 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 265 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 266 3-min fixes hrs_indoors: 15; n_cats: 3 manipulated other  Australia
## 267 3-min fixes hrs_indoors: 20; n_cats: 4 manipulated other  Australia
## 268 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 269 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 270 3-min fixes  hrs_indoors: 5; n_cats: 2 manipulated other  Australia
## 271 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 272 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 273 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 274 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 275 3-min fixes hrs_indoors: 24; n_cats: 3 manipulated other  Australia
## 276 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 277 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 278 3-min fixes hrs_indoors: 11; n_cats: 1 manipulated other  Australia
## 279 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 280 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 281 3-min fixes hrs_indoors: 22; n_cats: 1 manipulated other  Australia
## 282 3-min fixes hrs_indoors: 23; n_cats: 2 manipulated other  Australia
## 283 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 284 3-min fixes  hrs_indoors: 8; n_cats: 3 manipulated other  Australia
## 285 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 286 3-min fixes hrs_indoors: 12; n_cats: 3 manipulated other  Australia
## 287 3-min fixes hrs_indoors: 18; n_cats: 4 manipulated other  Australia
## 288 3-min fixes hrs_indoors: 24; n_cats: 1 manipulated other  Australia
## 289 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 290 3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 291 3-min fixes hrs_indoors: 18; n_cats: 2 manipulated other  Australia
## 292 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 293 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 294 3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 295 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 296 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 297 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 298 3-min fixes  hrs_indoors: 9; n_cats: 2 manipulated other  Australia
## 299 3-min fixes  hrs_indoors: 9; n_cats: 4 manipulated other  Australia
## 300 3-min fixes  hrs_indoors: 9; n_cats: 3 manipulated other  Australia
## 301 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 302 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 303 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 304 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 305 3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 306 3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 307 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 308 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 309 3-min fixes hrs_indoors: 14; n_cats: 2 manipulated other  Australia
## 310 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 311 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 312 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 313 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 314 3-min fixes hrs_indoors: 15; n_cats: 2 manipulated other  Australia
## 315 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 316 3-min fixes hrs_indoors: 23; n_cats: 1 manipulated other  Australia
## 317 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 318 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 319 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 320 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 321 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 322 3-min fixes hrs_indoors: 19; n_cats: 1 manipulated other  Australia
## 323 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 324 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 325 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 326 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 327 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 328 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 329 3-min fixes hrs_indoors: 11; n_cats: 1 manipulated other  Australia
## 330 3-min fixes  hrs_indoors: 2; n_cats: 2 manipulated other  Australia
## 331 3-min fixes  hrs_indoors: 6; n_cats: 3 manipulated other  Australia
## 332 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 333 3-min fixes hrs_indoors: 19; n_cats: 1 manipulated other  Australia
## 334 3-min fixes hrs_indoors: 23; n_cats: 2 manipulated other  Australia
## 335 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 336 3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 337 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 338 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 339 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 340 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 341 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 342 3-min fixes  hrs_indoors: 7; n_cats: 2 manipulated other  Australia
## 343 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 344 3-min fixes hrs_indoors: 16; n_cats: 2 manipulated other  Australia
## 345 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 346 3-min fixes  hrs_indoors: 8; n_cats: 2 manipulated other  Australia
## 347 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 348 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 349 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 350 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 351 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 352 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 353 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 354 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 355 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 356 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 357 3-min fixes  hrs_indoors: 3; n_cats: 2 manipulated other  Australia
## 358 3-min fixes  hrs_indoors: 1; n_cats: 2 manipulated other  Australia
## 359 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 360 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 361 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 362 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 363 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 364 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 365 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 366 3-min fixes hrs_indoors: 18; n_cats: 2 manipulated other  Australia
## 367 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 368 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 369 3-min fixes hrs_indoors: 20; n_cats: 2 manipulated other  Australia
## 370 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 371 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 372 3-min fixes hrs_indoors: 20; n_cats: 3 manipulated other  Australia
## 373 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 374 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 375 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 376 3-min fixes hrs_indoors: 24; n_cats: 4 manipulated other  Australia
## 377 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 378 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 379 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 380 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 381 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 382 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 383 3-min fixes hrs_indoors: 19; n_cats: 1 manipulated other  Australia
## 384 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 385 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 386 3-min fixes  hrs_indoors: 8; n_cats: 3 manipulated other  Australia
## 387 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 388 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 389 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 390 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 391 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 392 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 393 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 394 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 395 3-min fixes  hrs_indoors: 2; n_cats: 1 manipulated other  Australia
## 396 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 397 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 398 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 399 3-min fixes  hrs_indoors: 0; n_cats: 1 manipulated other  Australia
## 400 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 401 3-min fixes  hrs_indoors: 9; n_cats: 3 manipulated other  Australia
## 402 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 403 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 404 3-min fixes  hrs_indoors: 0; n_cats: 0 manipulated other  Australia
## 405 3-min fixes  hrs_indoors: 4; n_cats: 1 manipulated other  Australia
## 406 3-min fixes  hrs_indoors: 9; n_cats: 1 manipulated other  Australia
## 407 3-min fixes hrs_indoors: 18; n_cats: 1 manipulated other  Australia
## 408 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 409 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 410 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 411 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 412 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 413 3-min fixes hrs_indoors: 13; n_cats: 1 manipulated other  Australia
## 414 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 415 3-min fixes hrs_indoors: 12; n_cats: 1 manipulated other  Australia
## 416 3-min fixes hrs_indoors: 12; n_cats: 2 manipulated other  Australia
## 417 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 418 3-min fixes  hrs_indoors: 7; n_cats: 1 manipulated other  Australia
## 419 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 420 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 421 3-min fixes  hrs_indoors: 8; n_cats: 1 manipulated other  Australia
## 422 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 423 3-min fixes  hrs_indoors: 1; n_cats: 1 manipulated other  Australia
## 424 3-min fixes  hrs_indoors: 5; n_cats: 2 manipulated other  Australia
## 425 3-min fixes  hrs_indoors: 5; n_cats: 1 manipulated other  Australia
## 426 3-min fixes hrs_indoors: 14; n_cats: 2 manipulated other  Australia
## 427 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 428 3-min fixes hrs_indoors: 17; n_cats: 1 manipulated other  Australia
## 429 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 430 3-min fixes hrs_indoors: 10; n_cats: 2 manipulated other  Australia
## 431 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 432 3-min fixes hrs_indoors: 15; n_cats: 1 manipulated other  Australia
## 433 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
## 434 3-min fixes hrs_indoors: 14; n_cats: 1 manipulated other  Australia
## 435 3-min fixes hrs_indoors: 16; n_cats: 1 manipulated other  Australia
## 436 3-min fixes  hrs_indoors: 0; n_cats: 2 manipulated other  Australia
## 437 3-min fixes hrs_indoors: 20; n_cats: 1 manipulated other  Australia
## 438 3-min fixes hrs_indoors: 23; n_cats: 1 manipulated other  Australia
## 439 3-min fixes hrs_indoors: 24; n_cats: 1 manipulated other  Australia
## 440 3-min fixes hrs_indoors: 10; n_cats: 1 manipulated other  Australia
## 441 3-min fixes  hrs_indoors: 6; n_cats: 1 manipulated other  Australia
## 442 3-min fixes  hrs_indoors: 3; n_cats: 1 manipulated other  Australia
##              tag.manufacturer.name tag.mass tag.model tag.readout.method
## 1   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 2   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 3   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 4   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 5   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 6   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 7   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 8   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 9   Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 10  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 11  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 12  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 13  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 14  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 15  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 16  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 17  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 18  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 19  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 20  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 21  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 22  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 23  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 24  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 25  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 26  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 27  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 28  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 29  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 30  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 31  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 32  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 33  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 34  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 35  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 36  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 37  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 38  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 39  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 40  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 41  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 42  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 43  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 44  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 45  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 46  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 47  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 48  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 49  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 50  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 51  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 52  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 53  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 54  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 55  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 56  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 57  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 58  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 59  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 60  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 61  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 62  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 63  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 64  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 65  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 66  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 67  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 68  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 69  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 70  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 71  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 72  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 73  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 74  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 75  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 76  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 77  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 78  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 79  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 80  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 81  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 82  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 83  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 84  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 85  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 86  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 87  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 88  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 89  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 90  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 91  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 92  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 93  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 94  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 95  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 96  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 97  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 98  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 99  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 100 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 101 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 102 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 103 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 104 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 105 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 106 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 107 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 108 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 109 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 110 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 111 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 112 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 113 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 114 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 115 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 116 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 117 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 118 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 119 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 120 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 121 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 122 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 123 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 124 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 125 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 126 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 127 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 128 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 129 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 130 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 131 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 132 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 133 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 134 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 135 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 136 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 137 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 138 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 139 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 140 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 141 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 142 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 143 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 144 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 145 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 146 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 147 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 148 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 149 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 150 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 151 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 152 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 153 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 154 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 155 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 156 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 157 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 158 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 159 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 160 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 161 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 162 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 163 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 164 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 165 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 166 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 167 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 168 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 169 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 170 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 171 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 172 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 173 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 174 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 175 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 176 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 177 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 178 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 179 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 180 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 181 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 182 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 183 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 184 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 185 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 186 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 187 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 188 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 189 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 190 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 191 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 192 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 193 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 194 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 195 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 196 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 197 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 198 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 199 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 200 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 201 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 202 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 203 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 204 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 205 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 206 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 207 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 208 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 209 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 210 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 211 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 212 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 213 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 214 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 215 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 216 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 217 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 218 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 219 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 220 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 221 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 222 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 223 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 224 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 225 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 226 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 227 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 228 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 229 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 230 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 231 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 232 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 233 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 234 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 235 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 236 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 237 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 238 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 239 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 240 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 241 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 242 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 243 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 244 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 245 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 246 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 247 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 248 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 249 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 250 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 251 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 252 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 253 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 254 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 255 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 256 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 257 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 258 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 259 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 260 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 261 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 262 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 263 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 264 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 265 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 266 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 267 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 268 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 269 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 270 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 271 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 272 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 273 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 274 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 275 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 276 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 277 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 278 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 279 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 280 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 281 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 282 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 283 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 284 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 285 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 286 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 287 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 288 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 289 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 290 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 291 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 292 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 293 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 294 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 295 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 296 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 297 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 298 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 299 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 300 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 301 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 302 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 303 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 304 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 305 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 306 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 307 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 308 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 309 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 310 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 311 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 312 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 313 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 314 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 315 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 316 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 317 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 318 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 319 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 320 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 321 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 322 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 323 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 324 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 325 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 326 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 327 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 328 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 329 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 330 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 331 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 332 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 333 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 334 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 335 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 336 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 337 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 338 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 339 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 340 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 341 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 342 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 343 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 344 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 345 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 346 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 347 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 348 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 349 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 350 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 351 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 352 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 353 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 354 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 355 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 356 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 357 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 358 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 359 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 360 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 361 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 362 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 363 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 364 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 365 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 366 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 367 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 368 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 369 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 370 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 371 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 372 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 373 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 374 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 375 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 376 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 377 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 378 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 379 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 380 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 381 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 382 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 383 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 384 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 385 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 386 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 387 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 388 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 389 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 390 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 391 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 392 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 393 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 394 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 395 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 396 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 397 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 398 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 399 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 400 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 401 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 402 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 403 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 404 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 405 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 406 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 407 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 408 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 409 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 410 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 411 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 412 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 413 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 414 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 415 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 416 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 417 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 418 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 419 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 420 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 421 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 422 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 423 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 424 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 425 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 426 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 427 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 428 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 429 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 430 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 431 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 432 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 433 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 434 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 435 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 436 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 437 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 438 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 439 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 440 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 441 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 442 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
```

There are some columns that need to be separated out:
- animal.comments
- manipulation.comments


```r
cleaned_ref_data <- ref_data %>% 
    separate(animal.comments, c("Hunt", "Prey per Month"), ";") %>% 
    separate(manipulation.comments, c("hrs_indoors", "n_cats"), ";") %>% 
    mutate(Hunt = str_remove(Hunt, "Hunt: "),
           `Prey per Month` = str_remove(`Prey per Month`, "prey_p_month: "),
           animal.life.stage = str_remove(animal.life.stage, " years"),
           animal.life.stage = str_remove(animal.life.stage, " year"),
           animal.life.stage = str_remove(animal.life.stage, "year"),
           hrs_indoors = str_remove(hrs_indoors, "hrs_indoors: "),
           n_cats = str_remove(n_cats, "n_cats: "))
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 2 rows [45, 163].
## Expected 2 pieces. Missing pieces filled with `NA` in 2 rows [45, 163].
```

```r
# Rename the variable for the left_join
data <- data %>% 
    rename(animal.id = individual.local.identifier)

# Join the two datasets together now using left_join
complete_data <- left_join(data, cleaned_ref_data, by = "animal.id")

# Using lubridate to pull info out of the timestamp
complete_data <- complete_data %>% 
    mutate(yr = year(timestamp),
           mth = month(timestamp),
           day = day(timestamp),
           hr = hour(timestamp),
           min = minute(timestamp),
           sec = second(timestamp))
```

Now I want to find a way of determining the gps coordinates of each cat's home. My first thought is to take an average of the longitude and latitude of each cat. That might give a good representation. I figured I would use median rather than mean.

```r
home_location <- complete_data %>% 
    group_by(animal.id) %>% 
    summarise(home.long = median(location.long), home.lat = median(location.lat))
```


```r
aus_sf <- sf::st_read('/Users/jessygleeson/Rstudio-Files/STE_2021_AUST_SHP_GDA2020/STE_2021_AUST_GDA2020.shp')
```

```
## Reading layer `STE_2021_AUST_GDA2020' from data source 
##   `/Users/jessygleeson/Rstudio-Files/STE_2021_AUST_SHP_GDA2020/STE_2021_AUST_GDA2020.shp' 
##   using driver `ESRI Shapefile'
## Simple feature collection with 10 features and 8 fields (with 1 geometry empty)
## Geometry type: MULTIPOLYGON
## Dimension:     XY
## Bounding box:  xmin: 96.81695 ymin: -43.7405 xmax: 167.998 ymax: -9.142163
## Geodetic CRS:  GDA2020
```

```r
aus_sf <- ms_simplify(aus_sf, keep = 0.01, keep_shapes = TRUE)
```

Ok so now I know this study was done in South Australia. There are 442 different subjects. I am curious where these kitties are located relative to any National Parks. I don't know if I can find the relevant shapefile however.

```r
home_location %>% 
    ggplot() +
    geom_sf(data = aus_sf) +
    geom_point(aes(x = home.long, y = home.lat)) +
    coord_sf(xlim = c(132, 142), ylim = c(-30, -39))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/Plotting the home of each kitty-1.png" width="672" />
So at least the study of the Australian cats were in South Australia. Interesting.

I want to try and use the timestamp data together with the GPS coordinates. Particularly if I can map each kitty's distance from home over time. That'd be a cool graph.

## The Distance Formula
It's a spin-off of the Pythagorean Theorem:
`$$c^2 = a^2 + b^2$$`
Using the x and y coordinate system:
`$$D = \sqrt{(y_2 - y_1)^2 + (x_2 - x_1)^2}$$`
The distances are small enough to ignore the curature of the earth. That does simplify the conversion. That is:
`$$1^\circ = \text{60 nautical miles} = 111.12~km = 111,120~m$$`
So if I determine the Angular distance between the two points (home and current position) I could multiply that by 111,120 to give me a distance in metres.
I need to join the home_location data to the complete_data.

```r
complete_data <- home_location %>% 
    left_join(complete_data, by = 'animal.id')
```
Alright, the data has been combined :) 

Let's calculate!

```r
complete_data <- complete_data %>% 
    mutate(distance = (sqrt((home.long - location.long)^2 + (home.lat - location.lat)^2))*111120, na.rm = TRUE)
```

Alrighty, now...a plot to visualise the average distance from home across the dataset.

```r
complete_data %>% 
    filter(distance > 0 & distance <= 1500) %>% 
    ggplot(aes(x = distance)) +
    geom_histogram(colour = "black", fill = "#BC3818") +
    scale_x_log10(labels = scales::comma) +
    xlab("Distance from Home") +
    labs(
        title = "What is the distribution of the distance from home?"
    )
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/Log Distribution of Distance from Home-1.png" width="672" />
Oh ok, so once you add a log transformation then it becomes a fairly normal distribution.

I would like to know the characteristics of those cats who wander between 100 and 1000m from home. are they neutered? Do they hunt? Are they male/female? etc.

```r
complete_data %>% 
    filter(distance > 0 & distance <= 1500) %>% 
    filter(!is.na(animal.sex)) %>% 
    ggplot(aes(x = distance)) +
    geom_histogram(alpha = 0.7) +
    facet_grid(Hunt ~ ., scales = "free_y") +
    scale_x_log10(labels = scales::comma)
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## Missing Data Analysis
I'm not really getting a whole lot out of this data. So I'm going to focus on the missing and NA values to determine a cause for those.

```r
cleaned_ref_data %>% 
    filter(is.na(`n_cats`))
```

```
##        tag.id animal.id animal.taxon          deploy.on.date
## 1    CostaTag     Costa  Felis catus 2015-05-22 01:32:04.000
## 2 BagheeraTag  Bagheera  Felis catus 2015-07-30 02:16:44.000
##           deploy.off.date Hunt Prey per Month animal.life.stage
## 1 2015-05-31 18:35:45.000 #N/A           <NA>                  
## 2 2015-08-05 22:51:34.000                <NA>                  
##   animal.reproductive.condition animal.sex attachment.type
## 1                                                   collar
## 2                                                   collar
##   data.processing.software deployment.end.type deployment.id  duty.cycle
## 1                 @Trip PC             removal         Costa 3-min fixes
## 2                 @Trip PC             removal      Bagheera 3-min fixes
##   hrs_indoors n_cats manipulation.type study.site
## 1        #N/A   <NA> manipulated other  Australia
## 2               <NA> manipulated other  Australia
##            tag.manufacturer.name tag.mass tag.model tag.readout.method
## 1 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 2 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
```
So both of these cats have missing data, so we might remove those from the dataset. If we an up joining the data to longitude data it there'll be many more NA's. One is NA while the other is completely missing. I'm going to label this missing data as missing inputs as there is very few account of the same case happening. 


```r
# Convert all blank data into NA values
cleaned_ref_data[cleaned_ref_data==""] <- NA

#Proceed to remove all NA values from the dataset
cleaned_ref_data <- cleaned_ref_data %>% 
    filter(animal.id != c("Costa", "Bagheera"))
```

There's some data input in such columns as "Hunt", and "animal.reproductive.condition" that have a number of errors in the data. Zeroes for one, they should be replaced with No's as this is the most likely cause. The other NA's could be caused by not inputting data because they cats don't hunt and could also be interpreted as "No's" in the questionnaire. Firsly, I want to find any patterns that may be present with each significant variable. This may be tedious...

```r
cleaned_ref_data %>% 
    group_by(animal.reproductive.condition) %>% 
    summarise(n = n())
```

```
## # A tibble: 5 × 2
##   animal.reproductive.condition     n
##   <chr>                         <int>
## 1 Fixed                             9
## 2 Neutered                        217
## 3 Not Fixed                         7
## 4 Spayed                          191
## 5 <NA>                             17
```
Alright, NA is NA, this is assumed to be a questionnaire for the owners to complete. I will interpret NA as being an incomplete question and will remove the data. Zero however will need a closer look.

```r
cleaned_ref_data %>% 
    filter(Hunt == 0)
```

```
##         tag.id animal.id animal.taxon          deploy.on.date
## 1     SnowyTag     Snowy  Felis catus 2015-03-03 06:42:09.000
## 2        Gomez     Gomez  Felis catus 2015-03-06 15:56:26.000
## 3  SapphireTag  Sapphire  Felis catus 2015-05-29 00:33:45.000
## 4     MollyTag     Molly  Felis catus 2015-06-06 09:31:32.000
## 5    StumpyTag    Stumpy  Felis catus 2015-07-08 00:32:35.000
## 6   ZazzieMTag   ZazzieM  Felis catus 2015-07-16 00:33:04.000
## 7   IronManTag   IronMan  Felis catus 2015-07-17 00:34:10.000
## 8     MontyTag     Monty  Felis catus 2015-10-01 00:32:27.000
## 9     Theo2Tag     Theo2  Felis catus 2015-10-01 00:32:34.000
## 10    BrunoTag     Bruno  Felis catus 2016-01-28 02:15:14.000
## 11   Drake2Tag    Drake2  Felis catus 2016-04-01 01:32:06.000
## 12     BibiTag      Bibi  Felis catus 2016-04-09 00:32:33.000
## 13   KittenTag    Kitten  Felis catus 2016-04-17 15:57:05.000
##            deploy.off.date Hunt Prey per Month animal.life.stage
## 1  2015-03-10 16:06:57.000    0              0                 0
## 2  2015-03-11 14:25:34.000    0              0                 0
## 3  2015-06-06 02:23:41.000    0              0                 0
## 4  2015-06-13 03:44:14.000    0              0                 0
## 5  2015-07-15 14:57:58.000    0              0                 0
## 6  2015-07-23 08:09:14.000    0              0                 0
## 7  2015-07-22 09:06:11.000    0              0                 0
## 8  2015-10-11 13:57:13.000    0              0                 0
## 9  2015-10-11 13:57:48.000    0              0                 0
## 10 2016-02-05 10:48:33.000    0              0                 0
## 11 2016-04-12 22:18:35.000    0              0                 0
## 12 2016-04-17 14:58:23.000    0              0                 0
## 13 2016-04-23 04:53:45.000    0              0                 0
##    animal.reproductive.condition animal.sex attachment.type
## 1                           <NA>                     collar
## 2                           <NA>                     collar
## 3                           <NA>                     collar
## 4                           <NA>                     collar
## 5                           <NA>                     collar
## 6                           <NA>                     collar
## 7                           <NA>                     collar
## 8                           <NA>                     collar
## 9                           <NA>                     collar
## 10                          <NA>                     collar
## 11                          <NA>                     collar
## 12                          <NA>                     collar
## 13                          <NA>                     collar
##    data.processing.software deployment.end.type deployment.id  duty.cycle
## 1                  @Trip PC             removal         Snowy 3-min fixes
## 2                  @Trip PC             removal         Gomez 3-min fixes
## 3                  @Trip PC             removal      Sapphire 3-min fixes
## 4                  @Trip PC             removal         Molly 3-min fixes
## 5                  @Trip PC             removal        Stumpy 3-min fixes
## 6                  @Trip PC             removal       ZazzieM 3-min fixes
## 7                  @Trip PC             removal       IronMan 3-min fixes
## 8                  @Trip PC             removal         Monty 3-min fixes
## 9                  @Trip PC             removal         Theo2 3-min fixes
## 10                 @Trip PC             removal         Bruno 3-min fixes
## 11                 @Trip PC             removal        Drake2 3-min fixes
## 12                 @Trip PC             removal          Bibi 3-min fixes
## 13                 @Trip PC             removal        Kitten 3-min fixes
##    hrs_indoors n_cats manipulation.type study.site
## 1            0      2 manipulated other  Australia
## 2            0      3 manipulated other  Australia
## 3            0      0 manipulated other  Australia
## 4            0      0 manipulated other  Australia
## 5            0      0 manipulated other  Australia
## 6            0      1 manipulated other  Australia
## 7            0      0 manipulated other  Australia
## 8            0      3 manipulated other  Australia
## 9            0      2 manipulated other  Australia
## 10           0      0 manipulated other  Australia
## 11           0      0 manipulated other  Australia
## 12           0      0 manipulated other  Australia
## 13           0      0 manipulated other  Australia
##             tag.manufacturer.name tag.mass tag.model tag.readout.method
## 1  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 2  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 3  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 4  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 5  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 6  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 7  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 8  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 9  Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 10 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 11 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 12 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
## 13 Mobile Action Technology, Inc.      119    i-GotU      tag retreival
```
A closer look here shows consistent missing data across an observation when Hunt is zero. This is indicative of lack of data entered, and should be removed. 

```r
cleaned_ref_data <- cleaned_ref_data %>% 
    filter(Hunt != 0) %>% 
    filter(!is.na(Hunt)) %>% 
    filter(animal.reproductive.condition != "") %>% 
    filter(!is.na(animal.reproductive.condition)) %>% 
    filter(animal.sex != "")
```


```r
cleaned_ref_data %>% 
    filter(animal.sex == "")
```

```
##  [1] tag.id                        animal.id                    
##  [3] animal.taxon                  deploy.on.date               
##  [5] deploy.off.date               Hunt                         
##  [7] Prey per Month                animal.life.stage            
##  [9] animal.reproductive.condition animal.sex                   
## [11] attachment.type               data.processing.software     
## [13] deployment.end.type           deployment.id                
## [15] duty.cycle                    hrs_indoors                  
## [17] n_cats                        manipulation.type            
## [19] study.site                    tag.manufacturer.name        
## [21] tag.mass                      tag.model                    
## [23] tag.readout.method           
## <0 rows> (or 0-length row.names)
```
A closer look at some missing animal sex data shows no additional patterns. I can't think of a reason why it would be missing. I will write it off as errors in the data transfer process. As plenty of pets could easily take gender neutral names, I can't fill in that data with the genders I think would be true and therefore ruin the data. 

Rename all variables in the animal.reproductive.condition to either "fixed" or "not fixed"

```r
cleaned_ref_data <- cleaned_ref_data %>% 
    mutate(animal.reproductive.condition =  recode(animal.reproductive.condition, "Neutered" = "Fixed", "Spayed" = "Fixed"))
```

Alright, I might leave it here. and call it a roughly "clean" dataset. Reflections, is that I sohuld have done this before I started merging the data. But I wouldn't have learned the mistake if I didn't make it. In the future, I will look carfully for the NA values and make early decisions to clean the data. 
