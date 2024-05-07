Create a folder called "Profiles" to house the mapping configurations.  Create a class and inherit `: Profile` then in the constructor run `CreateMap<One, Another>();` for each item you need to map.

```
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<JsonDepartment, HumanforceDepartment>().ReverseMap();
        // Adding .ReverseMap() allows for both-way mapping
    }
}
```

Now in Program.cs add:
```
services.AddAutoMapper(AppDomain.CurrentDomain.GetAssemblies());
```
(might be able to tidy that up?)


Then to inject the service `private readonly IMapper _mapper;` and add to constructor parameters.

Then finally to use:
```
var destination = _mapper.Map<DestinationType>(source);
```