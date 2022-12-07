dit is het proces boek van Tom Zeeuwe, minor programmeren.
Project data science.

De titel van mijn project is Dog adoption.
De voortgang daarvan wortd hieronder ge-commit.

Processbook Data Science
dinsdag 29 november:

skelet voor het github project gemaakt
data transformation
F-test for significance of variables
test model
model
visualization of the results

in het begin vond ik het moeilijk om te bedenken wat er precies in zo’n skelet moet. Maar toen heb ik de module scikit-learn nog gemaakt van de Labs. en daaruit kreeg ik een redelijk idee welke stappen ik moet gaan doen om tot een goed onderzoek te komen. 

Github repository  werkbaar gemaakt en gebruik gemaakt van git add, commit en push. 

begin gemaakt met een jupyter notebook voor de data transformation.
Ik heb gekozen om het project in Jupyter notebook te doen omdat ik het
heel handig vind dat je de individuele cellen kunt runnen. dus bijv als ik een
cel maak met data frame dan kan ik direct naar head() kijken zonder dat ik
de rest van me code moet runnen. 

Probleem met het verwerken van de data: in mijn dataframe zitten JSON elementen die ik wil gebruiken voor mijn onderzoek. een voorbeeld hiervan is: 

"{'spayed_neutered': 'True', 'house_trained': 'True', 'declawed': 'None', 'special_needs': 'False', 'shots_current': 'True'}"

ik heb geprobeerd dit op te lossen middels de volgende suggesties van stack overflow:


df.join(df["attributes"].apply(json.loads).apply(pd.Series)


een parser
def CustomParser(data):
   import json
   j1 = json.loads(data)
   return j1

die vervolgens wordt aangeroepen:

df = pd.read_csv("/Users/tomzeeuwe/Documents/Programming/project data science/Dog-Adoption-Data-Science-Project/Project Data Science/pet-adoption.csv", converters={"attributes":CustomParser},header=0)
df[sorted(df['attributes'][0].keys())] = df['attributes'].apply(pandas.Series)

Maar allebei de oplossingen gaven mij de volgende error:
JSONDecodeError: Expecting property name enclosed in double quotes: line 1 column 2 (char 1)

ik ben bang dat in de csv het JSON formaat niet correct staat waardoor hij het niet correct kan omzetten naar normale data. Ik ga dit morgen vragen aan de begeleiders want dit kleine dingetje zou heel veel werk kunnen zijn bij de data transformatie. 
Maar ik hoop dat ik ongelijk heb en dat ik gewoon iets fout doe.
woensdag 30 november
Had even gesproken met wesley over mijn probleem met de data, en hij heeft aan mij bevestigd dat het inderdaad aan het JSON formaat ligt wat in de data zit. 

Daarnaast heb ik even op een site gekeken om valid JSON vs. invalid JSON te vergelijken en als ik de regel:
"{'primary': 'Brindle', 'secondary': 'Brown / Chocolate', 'tertiary': 'None'}" invoer dan krijg ik valid terug.
en:
{'primary': 'Brindle', 'secondary': 'Brown / Chocolate', 'tertiary': 'None'}
is invalid. 

Ik moet dus een manier gaan verzinnen om of de invalid ‘json’ string op te splitsen naar rijen data. 
OF
ik moet van de invalid JSON -> valid JSON maken zodat hij wel uitgelezen kan worden door deze lijn code:

df.join(df["attributes"].apply(json.loads).apply(pd.Series)


Mijn eerste poging hiertoe is de volgende regels:

for row in df['attributes']:
   i = ('"' + row + '"')
   print(i)
df['attributes']

zo dacht ik een “ voor, en een “ achter de string {invalid JSON} te zetten. Alleen nu had ik nog niet daadwerkelijk de df[‘attributes’] aangepast. want als ik hem print dan zie ik wel de “”. maar als ik df[‘attributes’] print dan is het nog niet veranderd. 

gelukt met de volgende code:

for i, row in enumerate(df['attributes']):
   df.loc[i].at['attributes'] = ('"' + row + '"')
 
for i, row in enumerate(df['environment']):
   df.loc[i].at['environment'] = ('"' + row + '"')

alleen nu is het volgende probleem dat deze lijn:

df.join(df["attributes"].apply(json.loads).apply(pd.Series)

niet de data split zoals ik had gehoopt. 
ik krijg zelfs een error: 

 Cell In [84], line 1
    df.join(df["attributes"].apply(json.loads).apply(pd.Series)
                                                               ^
SyntaxError: incomplete input

Een andere oplossing die ik heb gevonden is pd.json_normalize(df[‘attributes’])
als ik dit uitvoer krijg ik echter alleen lege indexes terug. 

ik heb geprobeerd om het volgende te doen:

df['attributes'] = df['attributes'].apply(json.loads)
df = pd.json_normalize(df['attributes'])

maar ik krijg nog steeds een lijst van lege indexes terug als ik om df.head() vraag. 



zelfs als ik de onderliggende data typen wil veranderen met json.loads krijg ik geen resultaat….

dit gaat moeilijker worden dan ik dacht.

Mijn errors worden raarder en raarder.. 

ik heb in excel de data van de csv veranderd
 van:
“{‘spayed_neutered’: ‘True’, ‘house_trained’: ‘True’, ‘declawed’: ‘None’, ‘special_needs’: ‘False’, ‘shots_current’: ‘True}”
naar:
'{"spayed_neutered": "True", "house_trained": "True", "declawed": "None", "special_needs": "False", "shots_current": "True"}'

om met 1 lijn te experimenteren of ik een goed JSON formaat kon krijgen. 

maar als ik nu de csv inlaat krijg ik:

File /usr/local/lib/python3.10/site-packages/pandas/_libs/parsers.pyx:1973, in pandas._libs.parsers.raise_parser_error()

ParserError: Error tokenizing data. C error: Expected 26 fields in line 6, saw 31

en als ik het volgende toevoeg aan de read csv functie: , on_bad_lines='skip'

dan krijg ik een df terug van 5 regels ipv 722 met alleen maar Nan values.


Donderdag 1 December
Het is gelukt om de JSON code te kraken.

values = [json.loads(value.replace("'", "\"")) for value in df['attributes'].values]
 
df1 = json_normalize(values)

Door bij elke row een wijziging te doen door “ toe te voegen kon ik het nu wel met pd.json_normalize() uitpakken. nu worden de variabelen gewoon als kolommen toegevoegd aan het dataframe. Ditzelfde heb ik gedaan voor environment (socialized) en dit is ook goed uitgepakt en heeft 3 kolommen aan de dataframe toegevoegd. 

Vervolgens wilde ik de volgorde van de kolommen veranderen, omdat er geen logische volgorde in stond. dit doormiddel van df.loc[ :, [ goede volgorde] ].

nu is dat probleem opgelost, en kan ik naar het volgende: een nieuwe kolom maken met het verschil tussen de aankomst van een dier en de datum waarop hij wordt geadopteerd. 

Ik wilde eerste weten wat voor type de timestamps uit df[published_at] waren.
v.b: df[published_at][0] = 2020-07-21 20:52:42+00:00
het typen van deze data is helaas string. anders kon ik datetime gebruiken om het verschil uit te rekenen tussen 2 data. 

Ik moet dus nu van dit soort strings 2020-07-21 20:52:42+00:00 omzetten naar het typen datetime. 
in eerste instantie dacht ik dat ik het variabel het best kon opsplitsen in een aparte year, month, day per row. maar dit werd vrij messy, en het bereikte niet heel veel. 

toen las ik op stackoverflow dat je strings naar class datetime kunt veranderen met de functie: datetime.datetime.strptime() 

deze heb ik direct geprobeerd, maar in het verkeerde format. 
Ik kreeg steeds errors over het format en toen ben ik filmpjes gaan kijken over de module datetime. 

Ik heb ook vrij veel gegoogled naar de docs van de module, web3 pagina en geeks for geeks. En overal was hetzelfde formaat. 

Toen kwam ik 1 voorbeeld code tegen die in plaats van %Y/%m/%d -> %Y-%m-%d
en toen realiseerde me ik me hoe ik de syntax error kon laten stoppen. 

datetime.datetime.strptime(time_in[0:10], "%Y-%m-%d")

Dit was uiteindelijk het haalbaarste. Ik heb geprobeerd om uren toe te voegen, alleen dat werd weer ingewikkeld en toen besloot ik om als outcome variabele dagen te hebben. Omdat het niet per se gaat om de tijd waarop een hond wordt geadopteerd, dat is vrij arbitrair en kan meer noise veroorzaken in mijn model. dus daarom gekozen voor y-m-d en uren/minuten/seconde laten vallen. 

uiteindelijk had ik werkbare variabelen gemaakt van time in en time out. 
toen deed ik een for loop door elke index van het df,
om het verschil in dagen te berekenen tussen t in, t out.
deze kwamen in een list

nieuwe kolom[‘ time in rescure’ ] = list. 
nu moet ik van Y de kolom time in rescue maken
en aan X voeg ik de volgende variabelen toe;


Maandag 5 december:

begon met het verwijderen van de days_in_rescue values van 0 days aka honden die niet zijn geadopteerd, omdat ze niet deel zullen zijn van het model. dit is gelukt met de volgende uitdrukking: 

df.drop(df[df.time_in_rescue == '0 days'].index, inplace=True)

Daarna wilde ik de overgebleven resultaten herindexeren omdat er nu 432 rijen overblijven, maar de index is niet veranderd, dus de index van de 432e rij is 722. 

dit dacht ik te doen door:

new_index = np.arange(0,432)

df = df.reindex(index=new_index)

alleen dit zorgt ervoor dat ik het originele df terug krijg met nu das allemaal missing values. 
skip voor nu

ik heb de volgende bewerkingen gedaan om true naar 1 te veranderen en false naar 0

eerst heb ik van alle kolommen met True, False, None gemerkt dat het string zijn
verander voor alle data string naar np.bool
daarna met de volgede regel code:

data1_bool["column"] = data1_bool["column"].astype(int)

de bool naar een int veranderd.

Daarna gechecked op missing data, en die is er niet. 

De data is nu bijna klaar om naar de volgende fase te gaan, het modeleren.

Ik heb nog de Y en de X gedefinieerd en een test run gemaakt om te zien of sklearn kan rekenen met de data, en dat is het geval.

nu ga ik alleen nog de data splitten in train en test data.



woensdag 7 december

ik heb een correlation matrix gemaakt van de variabelen die nog over zijn met time in rescue en ik heb een paar problemen:

- er is geen 1 correlatie hoger dan 0.2 en 0.2 is al een weak correlation.
- blijkbaar is er geen 1 hond declawed, daar kom ik nu pas achter en dit is dus geen bruikbaar variabel. 
- ik kan geen plots maken met timedelta data wat mijn dependant variabel is. 
- 

omdat de leads die ik nu heb om mee te werken heel dun zijn ga ik alsnog breeds toevoegen aan het df om te kijken of het iets van verschil maakt. 

net met wesley gesproken over mijn mogelijke beren op de weg, 
- een lage correlatie is niet erg als er wel significantie is
- het is niet erg om een model te hebben met een lage voorspellingswaarde
- hij vond de getransformeerde data er prima uit zien.

best positief dus. 

Ik heb vandaag gespeeld met verschillende soorten plots, en tot nu toe heb ik beschrijvende plots van de data toegevoegd 
om een beeld te geven van de data subject.

wat me verbaasde is dat de outcome variable niet normaal verdeelt is. 
de outcome variabel heeft een log normal distribution

toegevoegd: visuele representaties van alle indep variables.