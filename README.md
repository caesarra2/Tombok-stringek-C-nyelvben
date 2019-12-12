# Tömbök, stringek - Rövid ismétlés

## Tömbök
Egy tömb létrehozásakor az alábbi információkat kell biztosítanunk:
- Milyen típusú értékeket szeretnénk tárolni (pl: double, char, etc.)
- A tömb neve.
- Elemek száma a tömbben.

Szintaxis, valamint példa egy tömb létrehozására:

**_típús_ tömb_neve[tömb_méret];**
```C
int salt[6]; // Salt az egy tömb mely 6 elemből áll, és az elemei integer típúst tudnak tárolni.
```
A tömb egyes elemeire tekinthetünk változókként, melyekhez hozzáférhetünk külön-külön, a következő képpen: tömb_név[melyik_elem];
```C
salt[0] = 8; // Salt nevű tömb első elemének az értéke 8-ra állítása. (A tömbök elemeinek számozása nullától kezdődik.)
salt[3] = -2; // Salt nevű tömb negyedik elemének az értéke -2-re állítása.
salt[8] = 5; // INVALID, túlléptünk a tömbünk méretén. Mindig ügyeljünk arra, hogy az adott tömb eleme amihez éppen hozzá szeretnénk férni a kereteinken belül legyen!
```
Fontos: A "tömb_méret", kifejezés olyan konstans érték lehet, melyet már ismerünk a programunk compileolásakor, tehát a "tömb_méret" nem lehet egy olyan változó, amely a program futása közben kap értéket; 
```C
int first;
const int second = 8;
scanf("%d", &first);

double arr[first]; // INVALID, compilekor nem ismerjük a size értékét!
double arr2[second]; // VALID, compilekor ismerjük az értéket, ami 8.
double arr3[5]; // VALID, compilekor ismerjük az értéket, ami 5.
```
(Dinamikus memória foglalásnak a lényege, hogy a program futása közben foglalhatunk le, valamint szabadíthatunk fel általunk kívánt memóriát, így a fentebb említett probléma megkerülhető.)
```C
int amount;
scanf("%d", &amount);

double* arr = (double*)malloc(amount * sizeof(double)); // Valid, a felhasználó által megadott számú, double méretű memóriát foglalunk le.	
					    		// Erre a lefoglalt tartományra tekinthetünk hasonló képpen mintha egy tömb lenne.
			
free(arr);
// Miután már nincs szükségünk a lefoglalt memóriára, a free() függvénnyel visszaadhatjuk a memória pool-ba, tehát az adott memória tartomány újra felhasználható lesz. 
```
----------------------------------------------------------------------------------------------------------------------------------------
# Stringek
Amikor létrehozunk egy tömböt, akkor annak az elemei a memóriában egymás mellett foglalnak helyet; valamint tudjuk, hogy egy string az egy adott számú karakter, egymást követő bájtokban tárolva a memóriában. Ebből kikövetkeztethetjük, hogy egy char típusú tömb alkalmas string tárolására.

**Szabály:** C nyelvben minden string utolsó karaktere egy null karakter( '\0' ); Ez jelöli a string végét.

Nézzünk meg kettő példát:
```C
char elso[5] = {'h','e','l','l','o'}; // Nem string, mivel nem '\0'-re végződik a karakterláncunk.
char masodik[6] = {'h','e','l','l','o', '\0'}; // Ez viszont már string!
```
Egy stringek a karaktereit nem muszáj egyesével, aposztróffal elválasztva megadni, használhatunk " jeleket is, ebben az esetben a '\0' automatikusan a karakterlánc végére kerül.
```C
char harmadik[12] = "hello world"; // Ez egy string!
```
String deklaráláskor a méret elhagyható, ilyenkor a méret automatikusan megadódik. Ez az eset viszont csak akkor használható, ha egyből rendelünk is stringet a tömbhöz.
```C
char negyedik[] = "hello world"; // Valid
char otodik[]; // Error! Ismeretlen mértű tömböt próbálunk meg létrehozni.
```
Mi történik, ha a *char* tömbünk nagyobb, mint ahány karakternyi stringet rendelünk hozzá?
Példa:
```C
char tomb[8] = "hello";
```
Ilyenkor a maradék helyeken null karakter lesz.(Jelen esetben a tömb így nézne ki: 'h', 'e', 'l', 'l', 'o', '\0', '\0', '\0' )

Szintén fontos megjegyezni, hogy a " (idézőjel) valamint ' (aposztróf) jelentése nem azonos.
Nézzünk meg egy példát:
```C
char first = 'a'; // Valid
char second = "a" // Error! Ebben az esetben az a betűt stringként kezeljük, ezért hozzáadódik a null karakter is automatikusan, tehát 2 karaktert próbálunk meg egy változóhoz rendelni.
```
Tehát 'a' egy karakter, viszont "a" egy string mely az alábbi két karaktert tartalmazza: 'a', '\0'.

----------------------------------------------------------------------------------------------------------------------------------------

# Néhány egyszerű példa kód stringekkel, string kezelő függvényekkel.

 > **String kiírása "*%s*" printf paraméterrel.**

A *%s* jelölés azt jelenti a *printf* függvénynél, hogy a kapott paramétert stringként kezelje, tehát addig írja ki a karaktereket amíg el nem ér a stringet lezáró null karakterig.

```C
char str1[11] = { 'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd' }; // Nem String, mivel NEM null karakterrel végződik.
char str2[12] = { 'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '\0' }; // String, mivel null karakterrel végződik.
char str3[12] = "hello world"; // String, mivel ebben a deklarálási esetben automatikusan megkapta a végére a null karaktert.


printf("%s", str1); // ILYET NE! Ami nem string azt ne próbáljuk meg %s paraméterrel kiiratni!
printf("%s", str2); // VALID! Egy string, az output: "hello world".
printf("%s", str3); // VALID! Szintén egy string, az output: "hello world".
```
**Fontos:** Ahogy említettem, addig próbál meg a *printf* az *%s* paraméterrel karaktereket kiírni amíg el nem ér egy null karakterig. str1-ben viszont nem szerepel null karakter, ezért a *printf* nem fog megállni a kiírással a karakterlánc végén, hanem elkezd tovább haladni a memóriában és printelni mindent amíg el nem ér egy kósza null karakterig. Az output ilyen esetben hasonlóan nézne ki: hello world╠╠╠╠╠îäş-. 

Ez természetesen nem egészséges, tehát mindig ügyeljünk arra, hogy a megfelelő paraméterekkel lássunk el egy függvényt.

----------------------------------------------------------------------------------------------------------------------------------------

> **Egy teljes sor beolvasása a felhasználótól, majd műveletek a stringgel.**

Adott az alábbi tömb.
```C
char name[50];
```
Ha valakinek a teljes nevét(Ami állhat több szóból is) szeretnénk beolvasni a *name* tömbbe, akkor azt megtehetjük egy általunk írt ciklussal; karakterenként belemásolgatni az inputot a tömbünkbe, ellenőrizni a méretet, stb, stb. Vagy használhatjuk a **gets_s** függvényt, mely paraméterként kér egy célt, hogy hova olvassa be a karaktereket, valamint egy maximum méretet, ami után már nem olvas be többet. (Ez a függvény addig olvas be a karaktereket amíg el nem jut a "newline" karakterig. (ami automatikusan szerepel az inputban a sor végén amikor entert nyomunk.))

```C
	char name[50]; // Tömb létrehozása, mely képes 50 char tárolására.

	printf("Kérjük írja be a teljes nevét:");
	gets_s(name, sizeof(name)); // Megadjuk a célt, valamint a maximum méretet. A "sizeof(name)" visstatérési értéke a name tömbünk mérete bájtokban.

	int length = strlen(name); // A length nevű változóba másoljuk a felhasználó által beírt stringben szereplő karakterek számát.

	printf("Hello %s. A neved %d karakterből áll.", name, length); 
```
**Megjegyzés:** Ha az inputunk "hello world" volt, akkor az output a következő lesz: "Hello hello world. A neved 11 karakterből áll".
A *strlen()* függvény visszatérési értéke az egy általunk megadott stringben szereplő karakterek száma, nem számítva a string végi null karaktert.

----------------------------------------------------------------------------------------------------------------------------------------

*A string.h függvénykönyvtár tartalmaz számos string kezelő függvényt, melyek megkönnyítik a munkánkat ha stringekkel dolgozunk.
A függvények teljes listája, valamint dokumentációja melyeket használhatunk ha includeoltuk a string.h-t megtalálható az alábbi linken:* https://en.cppreference.com/w/c/string/byte


