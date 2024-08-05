Types of mocks:
- Dummies - simple objects to pass around
- Stub - can be given behaviour
- Spies - what/how many times things are called
- True Mock - bit more complex but fully customizable.

#### Using Jasmine to mock:

```
import { of } from "rxjs";
import { HeroesComponent } from "./heroes.component"

describe('HeroesComponent', () => {
    let component: HeroesComponent;
    let HEROES;

    let mockHeroService; 

    beforeEach(() => {
        HEROES = [
            {id:1, name: 'Bob', strength: 99},
            {id:2, name: 'Ben', strength: 50},
            {id:3, name: 'Alf', strength: 4}
        ]
    })

    // This needs a HeroesService - jasmine mocking to the rescue:
    mockHeroService = jasmine.createSpyObj(['getHeroes', 'addHero', 'deleteHero']) // names of methods go here
    component = new HeroesComponent(mockHeroService)

    describe('delete', () => {
        it('should remove the hero', () => {

            mockHeroService.deleteHero.and.returnValue(of(true)) // whatever the return should be
            component.heroes = HEROES;

            component.delete(HEROES[2])

            expect(component.heroes.length[2])
        })

        it('should call delete hero', () => {

            mockHeroService.deleteHero.and.returnValue(of(true))
            component.heroes = HEROES;

            component.delete(HEROES[2])

            expect(mockHeroService.deleteHero).toHaveBeenCalledWith(HEROES[2]);
        })
    })
})
```