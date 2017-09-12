# `Array.from()`

Este o metodă introdusă de versiunea standardului ECMAScript 2015 și creează o instanță `new Array` din orice obiect care arată ca un array sau care este iterabil. Obiectele din care se creează array-urile trebuie să aibe o lungime și elementele să fie indexate. Obiectul `arguments` are deja implementat protocolul de iterare și poate fi exploatat cu `Array.from()`.

## Aplicarea pe `arguments`

Această metodă este un ajutor foarte util în lucrul cu obiectul asemănător unui array care este `arguments`. Înainte de această actualizare a standardului singura metodă de a transforma argumentele într-un array era aplicarea funcției slice astfel: `[].slice.call(arguments)`.

```javascript
function transforma(){
  return [].slice.call(arguments);
};
```

Acum avem o viață mult mai ușoară, dar am prezentat metoda „veche” pentru că este posibil să o întâlniți destul de des în codul scris anterior și pe care este nevoie să-l înțelegeți.

```javascript
function sparge() {
  return Array.from(arguments);
};
sparge(1, 2, 3); // [1, 2, 3]
```

## Congruiență cu operatorul spread

Un operator nou introdus de ECMAScript 2015 care face același lucru. Este vorba despre operatorul spread. Acest operator folosește protocolul de iterare ceea ce înseamnă că obiectele pe care dorim să le transformăm, trebuie să aibe implementat @@iterator prin intermediul lui `Symbol.iterator`. `arguments` are deja implementat protocolul de iterare în ECMAScript 2015. ATENȚIE! Operatorul spread se bazează pe existența implementării protocolului de iterare, pe când `Array.from()`, nu se bazează doar pe acesta. Această metodă are capacitatea de a procesa și structuri de date „array-like”.


```javascript
function transforma(){
  return [...arguments];
};
transforma("unu","doi",3); // Array [ "unu", "doi", 3 ]
```

## Aplicarea pe șirurile de caractere.

```javascript
Array.from("foo");
// ["f", "o", "o"]
```

# Constituirea unei colecții de elemente DOM

Acest lucru este posibil pentru că `NodeList` permite protocolul de iterare. Efectul este convertirea unui `NodeList` într-un Array.

```javascript
var divuri = Array.from(document.querySelectorAll('div'));

// ca alternativă folosim operatorul spread
function colectDivs(){
  return [...document.querySelectorAll('div')];
};

// sau:
var divuri = [...document.querySelectorAll('div')];
```

## Nu poți aplica `slice` pe array-ul rezultat

Cu `Array.from()` nu se poate face `slice()`, dar poți să indici ce sunt părțile.

```javascript
function ceEste(){
  return Array.from(arguments, valoare => typeof valoare);
};
ceEste("ceva", null, true, undefined, NaN, 23);
```

Deja am introdus cu acest exemplu posibilitatea de a aplica câte o funcție pe fiecare element.

## Mapping pe fiecare element

`Array.from()` poate avea trei argumente în ordinea menționării:

- obiectul iterabil pe care vrei să-l transformi
- o funcție de mapping, care să fie apelată pentru fiecare dintre elementele din input
- și obiectul `this` necesar la apelarea funcției de mapare.

Putem imagina cu ușurință un scenariu în care, pentru fiecare element care va intra în viitorul array, se aplică o funcție care are capacitatea de a transforma elementul în altceva și mai util. Putem transforma elementele unui array existent aducându-le informații suplimentare.

```javascript
var linii = ['o linie', 'alta'];
var arr = Array.from(linii, (value) => `<p>${value}</p>`);
console.log(arr); // [ '<p>o linie</p>', '<p>alta</p>' ]
```

Efectul transformativ nu se aplică doar la nivelul elementelor unui array, ci și pe obiecte. În exemplul următor vom îmbina mai multe tehnici și vom împleti mai multe tehnici de prelucrare pentru a obține un rezultat mai aproape de lucrul de zi cu zi. Să ne imaginăm că dorim să facem un colorizator de cod pentru scripturi de JavaScript. Ca să ținem din scurt, vom lua doar două elemente, care, de fapt sunt două cuvinte cheie din JavaScript și în funcție de ceea ce fac, le vom colora construind un fragment HTML pentru a fi afișat.

```javascript
var colectie = {
  unu: "red",
  doi: "green",
  alternativ (val) {
    let htmlFragment = ``;
    if (val === "var") {
      htmlFragment += `<p style="color: ${this.unu}">${val}</p>\n`;
    } else {
      htmlFragment += `<p style="color: ${this.doi}">${val}</p>\n`;
    };
    return htmlFragment;
  }
};
var rezervate = ['var', 'typeof'];

function transforma (valori) {
  return Array.from(valori, colectie.alternativ, colectie);
};
var transformate = transforma(rezervate);
console.log(transformate.join(''));
// ["<p style="color: red">var</p>", "<p style="color: green">typeof</p>"]
```

Ceea ce este observabil este că în momentul în care te-ai decis să folosești o metodă din obiectul parcurs, va trebui să pasezi ca al treilea argument și referința pentru obiect ca fiind `this`.

Oricare obiect care are proprietatea `Symbol.iterator` poate fi convertit într-un array. 