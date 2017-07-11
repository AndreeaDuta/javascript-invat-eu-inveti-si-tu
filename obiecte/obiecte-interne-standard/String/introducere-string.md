# Obiectul intern `String`

Este un constructor pentru șiruri de caractere, în engleză „strings”.

Stăpânirea consolidată prin practică a șirurilor de caractere va permite manipularea datelor de tip `.txt, .csv, .json, etc.`. Combinarea lucrului pe șiruri de caractere cu lucrul pe array-uri, va permite transformări dintr-un format în altul, de manipulare la transformare, de îmbogățire și de segmentare a datelor.

Obiectul global String este un constructor de șiruri de caractere.

Stringurile pot fi create direct cu `String(ceva)`, în care `ceva` este orice ar putea fi convertit la string.

## Dependințe cognitive
- primitiva string
- Expresii Regulate
- Obiecte
- Obiectul intern RegExp

JavaScript nu face diferență între string-urile între ghilimele și cele duble.

## Mantre
- Pentru a concatena se folosește operatorul `+`.
- Are metoda internă `@@iterator` ceea ce permite iterarea cu `for...of`. Se pot itera și fragmente `code point`-uri UTF.
- JavaScript face o diferență foarte clară între obiectul String și primitiva șir. Același lucru se aplică și în cazul obiectelor Boolean și Number cu ale lor corespondențe la primitive.
- JavaScript face automat conversia de la primitiva șir la obiectul String. Astfel este posibilă aplicarea metodelor obiectului.
- șirurile în JavaScript sunt imuabile (nu se modifică șirul original),
- șirurile sunt „consumate” de JavaScript de la stânga la dreapta,
- din moment ce un caracter a fost „consumat”, acesta nu mai este folosit,
- JavaScript convertește automat primitivele șir în obiecte String, fiind astfel posibilă folosirea metodelor obiectului String pentru primitivele string,
- pentru a te asigura că poți face căutarea fără a te lovi de posibilele majuscule, mai întâi convertește toate caracterele șirului în minuscule folosind toLowerCase(). De exemplu: `var sir = "Acesta este un SIR"; sir.toLowerCase().startsWith("acesta"); // true`.

Caracterele speciale vor putea fi menționate în string-uri folosindu-se notația escape:

| NULL | ghilimele simple | ghilimele duble | backslash | linie nouă | carriage return | tab vertical | tab | backspace | form feed | Unicode | Latin-1 |
|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|
| `\0` | `\'` | `\"` | `\\` | `\n` | `\r` | `\v` | `\t` | `\b` | `\f` | `\uXXXX` | `\xXX` |

## Crearea obiectelor String

### Folosind constructorul: `new String()`

```javascript
var str = new String("test");
```

## Proprietăți

`String.length` și `String.prototype`

## Metode statice

`String.fromCharCode()`, `String.fromCodePoint()` și `String.raw`.

## Anatomia unui șir de caractere

Literele dintr-un `șir` se așează într-o ordine de la stânga la dreapta iar fiecare caracter este indexat purtând câte un număr începând de la 0.
Un șir de caractere are o lungime care poate fi aflată printr-un simplu `var sir = 'cateva caractere'; sir.length;`.
Numărul de index al ultimului caracter se poate afla prin determinarea lungimii șirului din care se scade o unitate: `var sir = 'cateva caractere'; sir.length - 1;`. Acea unitate se scade pentru că numerotarea se începe, de fapt de la 0. Dacă ai 16 caractere, numărul de index al ultimului caracter este 15.

### Lungimea unui șir

Poate fi aflată prin invocarea proprietății `.length` direct pe șir:

```javascript
'cevatext'.length; // 8
```

`'cevatext'.length` este echivalent cu `'cevatext'.lastIndexOf(''); // 8`

`String.length` returnează valoarea 1, iar când stringul este gol este returnată valoarea 0 (`''.length; // 0`)

### Lucrul cu indexul

Este esențială înțelegerea indexului pentru că acesta poate fi considerat ca o adresă a caracterului. Închipuiește-ți că ești în fața unui perete, pe care sunt înșiruite tablouri, numai că în loc de eticheta cu autorul sau descrierea conținutului, este numărul de inventar care pornește de la 0.

Metodele `indexOf()` și `lastIndexOf()` pot fi utilizate pentru căutarea unui substring într-un string. `indexOf("substring")` returnează valoarea indexului de la care începe substringul pasat ca argument.
`indexOf()` și `lastIndexOf()` pot primi un al doilea parametru care indică indexul de la care să pornească căutarea. Dacă al doilea parametru nu este menționat, căutarea se va face de la index 0. Dacă nu este este găsit substringul, va fi returnată valoarea -1.

### Lucrul direct pe caractere și fragmente

#### UTF-16, câteva precizări utile.

UTF (Uniform Transformation Format) este un sistem de codare numerică a caracterelor. Aceste coduri pot fi percepute ca niște identificatori unici pentru caractere.
Codarea adresează ceea ce este numit un „code unit” și se face prin „code points”, codurile de identificare despre care vorbeam. UTF-16 oferă coduri până la limita de 2<sup>16</sup>, valori ce se înscriu în așa-numitul Basic Multilingual Plane (BMP) iar codurile care depășesc această limită sunt codificate prin două coduri de identificare care formează o pereche. Această stare de fapt poate conduce la erori în ceea ce privește manipularea caracterelor în JavaScript.

```javascript
let exemplu = '𝒥';
console.log(exemplu.length); // lungimea textului este 2 în loc de 1 așteptat
console.log(exemplu.charCodeAt(0)); // 55349
console.log(exemplu.charCodeAt(1)); // 56485
```

Regăsirea folosind regex-urile nu se va putea face. Nici `charAt()` nu va funcționa corect, iar `charCodeAt()` va aduce codul pentru fiecare `code unit` separat așa cum arată și exemplul.

- `String.fromCharCode()` este o metodă statică a obiectului String, care transformă secvențe de numere Unicode în caractere.
- `String.fromCodePoint()` este o metodă statică a obiectului String, care transformă o secvență de caractere considerată a fi un cod al unui caracter (a fost adăugată în ECMAScript 6).
- `String.prototype.charAt()` este o metodă aplicabilă direct pe string, care returnează caracterul căutat la indexul specificat ca argument.
- `String.prototype.charCodeAt()` returnează un număr care reprezintă codul UTF-16 a caracterului de la indexul specificat.
- `String.prototype.concat()`
- `String.prototype.endsWith()`

Odată cu ES6, se pot folosi oricare `code point` de Unicode beneficiind de o notație prescurtată.

```javascript
console.log('\u{13165}'); // 𓅥 acum dincolo de 16 biți
// este același lucru ca și
console.log('\uD80C\uDD65'); // 𓅥
```

Poți chiar să numeri câte code point-uri sunt:

```javascript
[...'\u{13165}\u{13189}\u{13197}'].length; // 3
```

Se poate chiar inversa ordinea unor code-point-uri

```javascript
// ordinea
console.log('\u{13165}\u{13189}\u{13197}'); // 𓅥𓆉𓆗
// inversarea ordinii
var sir = '\u{13165}\u{13189}\u{13197}';
console.log([...sir].reverse().join(' ')); // 𓆗 𓆉 𓅥
```

Și încă o chestie supertare este că poți itera un codepoint folosind un `for...of`.

```javascript
for (let point of '𠮷') {
  console.log(point);
};
```

### Metode care folosesc regexuri

- `String.prototype.match()`
- `String.prototype.replace()`

## Fluxuri de lucru

### Rețetar

1. Aflarea dimensiunii în caractere a stringului:

`'string'.length;` sau
`'string'.lastIndexOf('');` sau
`'string'.indexOf('', 9999);` pentru care știi că al doilea parametru depășește cu mult lungimea șirului.

efectul este același: este returnat 6, adică numărul total de caractere din șir

2. Lucrul cu indexul

  A. Care este prima apariție în șirul de caractere:

    a. **a unui caracter de la index 0**:

  `'fragmente'.indexOf('e'); // 5 este returnat indexul la care apare prima dată caracterul căutat`

    b. **a unui fragment de text de la index maxim**:

  `'fragmente de text'.indexOf('de t'); // 10`

  B. Care este ultima apariție în șirul de caractere:

    a. unui caracter sau fragment:

    `'fragmente'.lastIndexOf('nt'); // 6`

## Resurse

[Mozilla Developer Network - Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
