# Mobile-Frontend
Mobile-Frontend


``` bash
npm install -g @ionic/cli
```

``` bash
npm i
pnpm i
yarn add
```

``` bash
ionic serve █
```
 
Model wird zurzeit noch Manuell importiert:
public/[...] --> bin files <br>
public/model.json --> Model <br>


## Architekur:
- Es wird Tensorflow JS verwendet, sprich die Predection wird local auf dem Device mittels Javascript gemacht.


Eigentlich kümmert uns nur die View: <br> 
views/Tab1Page.vue --> darin wird Tensor aufgebaut, daten gelesen, predicted und ausgegeben.

##  Aktuell kann der Magentometer dank Safari Browser nicht verewendet werden.

# Link für Showcase:
https://activity-recognition.netlify.app
- Bitte per PWA installieren.

**Android:**

1. Öffne den Chrome-Browser auf deinem Android-Gerät.
2. Besuche die Webseite der PWA, die du installieren möchtest.
3. Wähle im Chrome-Menü die Option "Zum Startbildschirm hinzufügen" aus.
4. Es erscheint ein Popup-Fenster, in dem du den Namen der App anpassen kannst. Tippe auf "Hinzufügen" oder "Installieren".
5. Die PWA wird jetzt auf deinem Startbildschirm angezeigt und du kannst sie wie eine normale App öffnen.

**iPhone:**

1. Öffne den Safari-Browser auf deinem iPhone.
2. Besuche die Webseite der PWA, die du installieren möchtest.
3. Tippe auf das "Teilen"-Symbol am unteren Bildschirmrand (das Symbol in der Mitte der Symbolleiste).
4. Wähle im Teilen-Menü die Option "Zum Home-Bildschirm" aus.
5. Es erscheint ein Popup-Fenster, in dem du den Namen der App anpassen kannst. Tippe auf "Hinzufügen".
6. Die PWA wird jetzt auf deinem Home-Bildschirm angezeigt und du kannst sie wie eine normale App öffnen.

Bitte beachte, dass nicht alle PWAs auf beiden Plattformen gleich gut unterstützt werden. Manche Funktionen einer PWA könnten auf einem Gerät möglicherweise besser funktionieren als auf einem anderen.
