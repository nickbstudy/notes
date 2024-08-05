
**Tools:**
- Karma
- Jasmine
- Jest
- Web Test Runner

End to end testing = Cypress, Playwright, Selenium & WebDriver

Beginner example of Jasmine test:

```
// Jasmine
describe("My First Test", () => {
    let sut;

    beforeEach(() => {
        sut = {}; // resets sut to an empty object before each run
    })

    it('should be true if true', () => {
        // arrange
        sut.a = false;

        // act
        sut.a = true;

        //assert
        expect(sut.a).toBe(true);
    })
})
```