[← 06 · Les services](06-service.md) · [Sommaire](Readme.md) · [08 · Les pipes →](08-pipe.md)

# Les directives

Une directive change l'apparence ou le comportement d'un élément **existant**, sans créer de balise. Les **directives d'attribut** s'appliquent à un élément (`<span appTypeColor>`) ; les **structurelles** ajoutent ou retirent des morceaux de template — en Angular 22, ce sont `@if`, `@for`, `@defer`.

## L'essentiel

### 1. Une directive d'attribut

`src/app/shared/type-color.ts`

```ts
@Directive({
  selector: '[appTypeColor]',
  host: {
    '[style.--type-color]': 'color()',
    '[attr.data-type]': 'appTypeColor()',
  },
})
export class TypeColor {
  readonly appTypeColor = input.required<DevType>();
  protected readonly color = computed(() => TYPE_COLORS[this.appTypeColor()]);
}
```

```html
<span appTypeColor="frontend">Front-end</span>
```

- Le sélecteur entre **crochets** vise un attribut, pas une balise.
- `host` lie la directive à son **élément hôte** : ici une variable CSS et un attribut `data-*`.
- La directive lit une **entrée** comme un composant (`input.required`).

### 2. hostDirectives

Un composant peut recevoir des directives d'hôte directement dans son décorateur :

```ts
@Component({
  hostDirectives: [{ directive: TypeColor, inputs: ['appTypeColor: type'] }],
})
export class DevBadge {}
// <app-dev-badge type="frontend" /> — TypeColor s'applique à l'hôte
```

### 3. Les structurelles : le control flow

`@if`, `@for`, `@switch`, `@defer` sont des directives structurelles intégrées (fiche [02 · Le composant de page](02-composant-page.md)). Les anciennes `*ngIf` / `*ngFor` sont du **code historique** — à savoir lire, à ne plus écrire.

### 4. Le bon choix

| Besoin | Outil |
|---|---|
| Changer l'apparence d'un élément | directive d'attribut |
| Ajouter ou retirer du contenu | `@if` / `@for` (intégrés) |
| Une balise à soi, avec template | composant |

## Pièges courants

- **Sélecteur sans crochets** : `[appTypeColor]` cible un attribut ; `appTypeColor` créerait une balise — c'est un composant.
- **Oublier `host`** : sans liaisons d'hôte, la directive ne touche pas son élément.
- **Réécrire `*ngIf`** : le control flow natif le remplace, sans import.

## Approfondir

- [Directives — angular.dev](https://angular.dev/guide/directives) (en anglais)
- Cours Angular : chapitre [07 · Directives et pipes](https://github.com/DiginamicAcademy/Angular/blob/main/07-directives-pipes.md)

---

[← 06 · Les services](06-service.md) · [Sommaire](Readme.md) · [08 · Les pipes →](08-pipe.md)
