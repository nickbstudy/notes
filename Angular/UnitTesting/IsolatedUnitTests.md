#### Testing a pipe

```
import { StrengthPipe } from "./strength.pipe"

describe('StrengthPipe', () => {
    it('weak if str < 5', () => {
        let pipe = new StrengthPipe();

        expect(pipe.transform(5)).toEqual('5 (weak)')
    })

    it('strong if str > 10', () => {
        let pipe = new StrengthPipe();

        expect(pipe.transform(10)).toEqual('10 (strong)')
    }) 
})
```

---

#### Testing a service with no dependencies

```
import { MessageService } from "./message.service";

describe('MessageService', () => {
    let service: MessageService;

    beforeEach(() => {
    })

    it('should have no messages to start', () => {
        service = new MessageService();

        expect(service.messages.length).toBe(0);
    })
    
    it('should add a test', () => {
        service = new MessageService();

        service.add('message1');
        
        expect(service.messages.length).toBe(1);
    })

    it('should clear all tests', () => {
        service = new MessageService();
        service.add('message1');
        
        service.clear();

        expect(service.messages.length).toBe(0);
    })
})
```

#### Testing a component

```

```