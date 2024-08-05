Deals with testing templates and child components.

import { ComponentFixture, TestBed } from '@angular/core/testing'
import { HeroComponent } from './hero.component'
import { NO_ERRORS_SCHEMA } from '@angular/core';
import { By } from '@angular/platform-browser';

describe('HeroComponent (shallow tests)', () => {

    let fixture: ComponentFixture<HeroComponent>;

    beforeEach(() => {
        TestBed.configureTestingModule({ // needed to be imported manually, then declare necessities below
            declarations: [HeroComponent],
            schemas: [NO_ERRORS_SCHEMA] // fixes the issue of importing routes throwing an error
        });
        fixture = TestBed.createComponent(HeroComponent);
    })

    it('should have the correct hero', () => {
        fixture.componentInstance.hero = { id: 1, name: 'Bob', strength: 3 };

        expect(fixture.componentInstance.hero.name).toEqual('Bob');
    })

    it('should render the hero name in an anchor tag', () => {
        fixture.componentInstance.hero = { id: 1, name: 'Bob', strength: 3 };
        fixture.detectChanges(); // runs change detection to update bindings etc

        // import 'By' from platform-browser - it is a wrapper around the DOM, and can access things nativeElement can't.
        expect(fixture.debugElement.query(By.css('a')).nativeElement.textContent).toContain('Bob') // can be used with JQuery syntax to select more easily.
        expect(fixture.nativeElement.querySelector('a').textContent).toContain('Bob'); // nativeElement gives access to the DOM - any JS will work
    })
})