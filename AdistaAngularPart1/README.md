# Adista Angular #1
Formation de démarrage sur Angular pour débutants - Partie 1

## Install des outils

- Vérifier la version de node : ``node --version``
- Installer Angular CLI : ``npm install -g @angular/cli``
- Vérifier la version : ``ng version``
- Mémo des commandes Angular : https://gist.github.com/VikashSaharan1/e5b7dfd3b3084a1385270e8896aa7174

## Création d'une application Angular

```
ng new appTest
cd appTest
code .
```

Pour démarrer l'application, dans le terminale de VsCode : ``ng serve``

## Tutoriel Angular 

Tutoriel en mode local : https://angular.dev/tutorials/first-app

### -> Créer un composant Accueil

Dans les composants, remplacer le template Html par : ``templateUrl: './app.component.html'``


### -> Créer une interface

``` Typescript
//Exemple d'interface et de classe
export interface IMyObject {
  id: number;
  label: string;
}

export class MyObject implements IMyObject {
  public id: number;
  public label: string;

  public constructor(id: number, label: string) {
    this.id = id;
    this.label = label;
  }

  public get shortlabel(): string {
    return this.label.substring(0, 32);
  }
}
```

### -> Utilisez *ngFor pour répertorier les objets du composant

Depuis Angular 17, on a une nouvelle  syntaxe de flux de contrôle : https://blog.angular-university.io/angular-for/

La documentation : https://angular.dev/guide/templates/control-flow

### -> Ajouter des itinéraires à l'application

A la fin : comparer avec le routing de l'application appTest ...

### -> Ajoutez la communication HTTP à votre application

**Au lieu d'utiliser JSON Server : une petite API en C# ?** 😉

1. Créer une Api Asp.Net MVC Core (.Net 8 pour avoir Swagger)
2. Utiliser CoPilot pour convertir en C# le code Typescript du tableau *housingLocationList: HousingLocationType[]* 
3. Créer un contrôleur d'Api avec actions de lecture/ecriture : *LocationsController*

``` Csharp
public class LocationsController : ControllerBase
{
    // GET: api/Locations
    [HttpGet]
    public IEnumerable<HousingLocation> Get()
    {
        return HousingLocationService.HousingLocationList;
    }

    // GET api/Locations/5
    [HttpGet("{id}")]
    public HousingLocation? Get(int id)
    {
        return HousingLocationService.HousingLocationList.Find(item => item.Id == id);
    }
}
```

4. Terminer le tutoriel, en appellant l'Api dans le service Angular :

``` Typescript
export class HousingService {
  private url = 'https://localhost:7117/api/Locations'; 

  public async getAllHousingLocations(): Promise<HousingLocationType[]> {
    const data = await fetch(this.url);
    return (await data.json()) ?? [];
  }

  public async getHousingLocationById( id: number ): Promise<HousingLocationType | undefined> {
    const datum = await fetch(`${this.url}/${id}`);
    return (await datum.json()) ?? {};
  }
  ...
```

## Implémenter l'authentification Azure

Suite : [Adista Angular #2](https://github.com/nclavere/AdistaAngularPart2)