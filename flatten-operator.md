# mergeMap / switchMap / concatMap / exhaustMap 
# Comprendre mergeMap vs. switchMap vs. concatMap vs. exhaustMap par l'exemple


Notions: Inner Observable / Higher Order Observable 


Intro: expliquer pourquoi on utilise ces opérateurs
expliquer pourquoi ca peut paraitre confusing
=> peut être intéressant pour travailler avec des web sockets


Cela fait presque 3 ans que je travaille avec RxJS (couplé à angular), malgré la lecture de plusieurs articles cela fait 
à peine quelques mois que j'ai réellement compris le fonctionnement et l'intérêt de cette notion sombre que sont les flattering operators.
Mais qu'est ce qu'un flattering operator ?
Flattering = il s'agit de subscribe dans un subscribe
Mapping = transformation 
Il s'agit d'un operateur permettant à un observable de retourner lui même un observable et d'y souscrire automatiquement. C'est comme une inception de subscribe.
Vous avez peut être été déjà confronté à cette problèmatique si vous avez utilisé NgRX Store et les effects.
 au contraine d'un opérateur de mapping qui permet seulement de 
transformer le résultat d'un observable, celui ci renvoie quand même une value.

Il y a 4 stratégies de mapping/flattening
merge (operateur mergeMap) : 
switch (operateur switchMap) : Désouscrit au dernier observable mappé quand un nouveau arrive
cconcat (operateur concatMap) : ne souscrit au nouveau que si le dernier a été complete 
exhaust : ignore et ne souscrit pas si le current n'a pas été complete 

```typescript
const users$ = from(['John', 'Jane']);
message$.pipe(
    map(message => 'I am user ' + message),
).subscribe((transformedMessage) => console.log(transformedMessage));

// Log:
// I am user John
// I am user Jane

    const users$ = from(['John', 'Jane']);

    users$.pipe(
      mergeMap(user => from([`${user} says coucou`, `${user} says saloute`])),
      ).subscribe(message => console.log(message));

    users$.subscribe((user) => {
      from([`${user} says coucou`, `${user} says saloute`]).subscribe((message) => {
        console.log(message);
      });
    });

// Log:
// John says coucou
// John says saloute
// Jane says coucou
// Jane says saloute

```

