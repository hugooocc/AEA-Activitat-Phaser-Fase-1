# poc-fase1-vue-phaser

Prova de Concepte de la **Fase 1**: encapsular un llenç de **Phaser 4** dins del cicle de vida de **Vue 3** (`onMounted` / `onUnmounted`) i comprovar el Game Loop a 60 FPS.

## Execució

```bash
npm install      # instal·la les dependències
npm run dev      # servidor de desenvolupament → http://localhost:5173
npm run build    # versió de producció a dist/
npm run preview  # serveix dist/ en local
```

## Estructura

| Fitxer | Responsabilitat |
| --- | --- |
| `src/App.vue` | Capa HTML/CSS de Vue: input del nom, color, estat, botons Pausa i Mostra/Amaga. |
| `src/components/PhaserCanvas.vue` | Component pont: crea i destrueix `Phaser.Game` i escolta les props amb `watch()`. |
| `src/game/MainScene.js` | Codi Phaser pur: avatar, textos, bucle `update()` i mètodes públics. |

## Repte d'autonomia

1. **Selector de color**: `<input type="color">` → prop `colorAvatar` → `canviarColor()` amb `setFillStyle()`.
2. **Indicador d'estat**: Disponible / Ocupat / Absent → prop `estat` → `this.textEstat`, ancorat a `update()`.
3. **Pausa del Game Loop**: botó Pausa / Reprèn → prop `pausat` → `escena.scene.pause()` / `resume()`.

## Checklist d'entrega

- [ ] **Neteja de memòria**: en clicar Amaga/Mostra repetidament hi ha un sol `<canvas>` i cap error a la consola.
- [ ] **Separació Vue / Phaser**: tots els controls són HTML a `App.vue`; Phaser només gestiona l'avatar i els textos.
- [ ] **Reactivitat en directe**: nom, color i estat canvien a l'instant dins del `<canvas>`.
- [ ] **Control del Game Loop**: els textos segueixen l'avatar i el botó Pausa atura/reprèn l'escena.

**Lliurament**: comprimir la carpeta en un `.zip` **sense `node_modules`**, o lliurar l'enllaç al repositori Git.
