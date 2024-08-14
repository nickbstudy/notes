Most important ones in bold.:

**`[FromBody]` Request body**
`[FromForm]` Form data in the request body
**`[FromHeader]` Request header**
**`[FromQuery]` Query string parameters**
**`[FromRoute]` Route data in the current request (used by default)**
`[FromServices]` The service(s) injected as an action parameter
`[AsParameters]` Method parameters

- `FromBody` inferred for complex types
- `FromForm` inferred for action parameters of type IFormFile and IFormFileCollection
- `FromRoute` inferred for any action parameter name matching a parameter in the route template
- `FromQuery` inferred for any other action parameters
