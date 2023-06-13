# Mobile-Frontend
Mobile-Frontend


``` bash
npm install -g @ionic/cli
```

``` bash
npm i ||
pnpm i ||
yarn add ||
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

## ## Aktuell kann der Magentometer dank Safari Browser nicht verewendet werden.
