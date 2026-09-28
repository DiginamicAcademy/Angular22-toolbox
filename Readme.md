# Angular 22 — Toolbox

12 fiches notions Angular 22, la référence rapide du cours [Angular](https://github.com/DiginamicAcademy/Angular) (CDA).

Chaque fiche présente une notion importante du framework : à quoi elle sert, quand l'utiliser, l'essentiel du code, les pièges courants. Une fiche = une notion, lisible seule ; pour apprendre Angular en construisant, suivez le cours — le projet Pokedev d'abord, puis Time To en autonomie.

## Sommaire

| # | Fiche | Notion |
|---|---|---|
| 01 | [L'application](01-app.md) | Bootstrap, composant racine, providers, zoneless |
| 02 | [Le composant de page](02-composant-page.md) | `@Component`, template, OnPush, entrées / sorties |
| 03 | [Le routeur](03-routeur.md) | Routes, chargement différé, gardes, paramètres |
| 04 | [`signal()`](04-signal.md) | État réactif : créer, lire, modifier |
| 05 | [`computed()`](05-computed.md) | Valeurs dérivées, mémoïsation |
| 06 | [Les services](06-service.md) | `@Service()`, `inject()`, injection de dépendances |
| 07 | [Les directives](07-directive.md) | Attribut, control flow, personnalisée |
| 08 | [Les pipes](08-pipe.md) | Intégrés, personnalisés, pur / impur |
| 09 | [Les appels HTTP](09-appels-http.md) | `HttpClient`, `httpResource`, intercepteurs |
| 10 | [La gestion de session](10-gestion-session.md) | Authentification, gardes, jeton, intercepteur |
| 11 | [WebSocket](11-websocket.md) | Temps réel, reconnexion, signaux |
| 12 | [Les tests avec Vitest](12-tests-vitest.md) | `ng test`, spec, `TestBed`, couverture |

Ce tableau est le sommaire de la toolbox : il tient lieu de plan. Les fiches se suivent dans l'ordre — chacune s'appuie sur les notions des précédentes — mais chacune reste lisible seule.

## Versions

Angular 22, TypeScript 6, Vitest 4, RxJS 7.8, Node.js 22 ou 24 LTS.

Angular publie une version majeure environ tous les six mois. La commande `npm view @angular/cli version` affiche la version courante.

## Comment lire une fiche

Chaque fiche suit le même plan :

- **À quoi ça sert** — la notion en deux phrases, et quand l'utiliser.
- **L'essentiel** — le code minimal qui couvre l'essentiel des usages.
- **Pièges courants** — les erreurs classiques de débutant.
- **Approfondir** — la documentation officielle, et le chapitre du cours qui traite la notion en détail.

Les fiches 10 (session) et 11 (WebSocket) vont au-delà du cours ; la fiche 09 y ajoute les mutations (`POST`) et le transport Fetch. Elles complètent le socle pour des applications réelles, avec l'appui des chapitres 06 (services, HTTP, intercepteur) et 09 (état, persistance).
