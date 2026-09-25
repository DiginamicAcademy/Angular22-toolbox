[← 04 · signal()](04-signal.md) · [Sommaire](Readme.md) · [06 · Les services →](06-service.md)

# computed()

`computed()` crée une valeur **dérivée** d'autres signaux : elle se recalcule quand une dépendance change — et seulement si quelqu'un la lit. Tout ce qui peut être calculé à partir d'un autre état est un `computed`, jamais une copie tenue à jour à la main.

## L'essentiel

### 1. Dériver

```ts
const cups = signal(0);
const energy = computed(() => (cups() > 3 ? 'survolté' : 'efficace')); // lecture seule
```

Dans un service :

```ts
readonly members = computed(() => this.ids().map((id) => this.repository.byId(id)));
readonly size = computed(() => this.ids().length);
```

(`repository` : le service qui charge les données — fiche [09 · Les appels HTTP](09-appels-http.md))

### 2. Mémoïsé : recalculé seulement si nécessaire

Un `computed` ne se recalcule que si un signal qu'il a lu a changé **et** si on le lit. Dix lectures sans changement, zéro recalcul.

```mermaid
flowchart LR
    A[ids change] --> B[computed marqué périmé]
    B -->|lecture| C[recalcul]
    B -->|pas de lecture| D[rien]
```

### 3. Les dépendances sont suivies automatiquement

Le calcul lui-même déclare ses dépendances — celles qu'il lit réellement, branche par branche :

```ts
const label = computed(() => (isAdmin() ? name() : 'anonyme'));
// si isAdmin() est true, name() est une dépendance ; sinon, non
```

### 4. En lecture seule

Un `computed` n'a ni `set` ni `update`. Pour un état dérivé **modifiable**, réinitialisé quand une source change, Angular fournit `linkedSignal()`.

## Pièges courants

- **Écrire dans un `computed`** : c'est une valeur en lecture seule ; la synchronisation avec l'extérieur est le travail d'un `effect`.
- **Lire une valeur non réactive** : un `computed` qui lit une simple propriété (pas un signal) ne se mettra jamais à jour.
- **Supposer des dépendances fixes** : elles sont déterminées par les branches réellement exécutées.

## Approfondir

- [Signaux — angular.dev](https://angular.dev/guide/signals) (en anglais)
- Cours Angular : chapitres [05 · Composants et signaux](https://github.com/DiginamicAcademy/Angular/blob/main/05-composants-signaux.md) et [09 · État, persistance et architecture](https://github.com/DiginamicAcademy/Angular/blob/main/09-etat-persistance-architecture.md)

---

[← 04 · signal()](04-signal.md) · [Sommaire](Readme.md) · [06 · Les services →](06-service.md)
