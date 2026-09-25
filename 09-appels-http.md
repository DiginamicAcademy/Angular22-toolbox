[← 08 · Les pipes](08-pipe.md) · [Sommaire](Readme.md) · [10 · La gestion de session →](10-gestion-session.md)

# Les appels HTTP

Parler au serveur, c'est deux gestions : **charger** des données — et en Angular 22, `httpResource` expose la réponse directement en signaux — et **modifier** (`POST`, `DELETE`), qui passe par `HttpClient`.

## L'essentiel

### 1. Charger : httpResource

`src/app/features/devs/dev-repository.ts`

```ts
@Service()
export class DevRepository {
  private readonly remote = httpResource(() => '/data/devs.json', {
    parse: parseDevs,     // type guard : valider à la frontière
    defaultValue: [],
  });

  readonly devs = computed(() => (this.remote.hasValue() ? this.remote.value() : []));
  readonly loading = this.remote.isLoading;
}
```

- La fonction passée en premier argument est **réactive** : si elle lit un signal et que ce signal change, la requête est relancée. Si elle renvoie `undefined`, aucune requête n'est envoyée.
- `parse` reçoit la réponse brute (`unknown`) et renvoie une valeur typée. C'est l'endroit où l'on **valide** les données. Si `parse` lève une erreur, la ressource passe en état d'erreur.
- **Lire `value()` sur une ressource en erreur lève une exception.** On teste `hasValue()` avant, dans un `computed`.
- `HttpClient` est disponible par défaut. `provideHttpClient()` ne sert qu'à ajouter des options, comme les intercepteurs.

### 2. Le trajet d'une requête

```mermaid
flowchart LR
    A[Composant] --> B[httpResource]
    B --> C[Intercepteur 1]
    C --> D[Intercepteur 2]
    D --> E[Serveur]
    E -->|réponse| F[parse — validation]
    F --> G[value, isLoading, error]
    G --> A
```

### 3. Modifier : HttpClient

```ts
private readonly http = inject(HttpClient);

async addDev(dev: Omit<Dev, 'id'>): Promise<void> {
  await firstValueFrom(this.http.post<Dev>('/api/devs', dev));
  this.remote.reload(); // rafraîchir la ressource
}
```

### 4. Les intercepteurs

Un intercepteur s'exécute pour **chaque** requête : ajout d'en-têtes, journalisation, indicateur de chargement, nouvelle tentative.

`src/app/core/loading-interceptor.ts`

```ts
export const loadingInterceptor: HttpInterceptorFn = (req, next) => {
  // avant la requête
  return next(req).pipe(finalize(() => { /* après, succès ou erreur */ }));
};
```

`next(req)` renvoie un `Observable` RxJS. C'est l'un des rares endroits où RxJS reste nécessaire : `pipe` et `finalize` suffisent ici.

Enregistrement dans `app.config.ts` :

```ts
provideHttpClient(withInterceptors([loadingInterceptor])),
```

Angular 22 envoie ses requêtes avec l'API **Fetch** du navigateur (fini XMLHttpRequest) ; `withXhr()` le rétablit si un cas l'exige.

## Pièges courants

- **Ne pas valider la réponse** : sans `parse`, la donnée reste `unknown`. Validez à la frontière.
- **Lire `value()` sans précaution** : sur une ressource en erreur, cela lève — passez par `hasValue()`.
- **Oublier `reload()` après une modification** : la ressource affiche l'ancien état.
- **Fabriquer un client HTTP générique** : une ressource = une donnée avec ses dépendances réactives, pas un fourre-tout pour tous les appels.

## Approfondir

- [HTTP Client — angular.dev](https://angular.dev/guide/http) (en anglais)
- Cours Angular : chapitre [06 · Services, injection et HTTP](https://github.com/DiginamicAcademy/Angular/blob/main/06-services-http.md)

---

[← 08 · Les pipes](08-pipe.md) · [Sommaire](Readme.md) · [10 · La gestion de session →](10-gestion-session.md)
