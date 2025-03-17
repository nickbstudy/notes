A good pattern to use for API repository calls is the 'Result' method.  This gives you the ability to return a `Result.Success` or `Result.Failure("Message here")`

### Result.cs
```csharp
public class Result
{
	public bool IsSuccess { get; private set; }
	public string? Error { get; private set; }

	private Result(bool isSuccess, string? error = null)
	{
		IsSuccess = isSuccess;
		Error = error;
	}

	public static Result Success() => new Result(true);

	public static Result Failure(string error) => new Result(false, error);
}

public class Result<T>
{
	public bool IsSuccess { get; private set; }
	public T? Value { get; private set; }
	public string? Error { get; private set; }

	private Result(T? value, bool isSuccess, string? error = null)
	{
		IsSuccess = isSuccess;
		Value = value;
		Error = error;
	}

	public static Result<T> Success(T value) => new Result<T>(value, true);
	public static Result<T> Failure(string error) => new Result<T>(default, false, error);
}
```

### ResultExtensions.cs
These extension methods allow easier returning of an `IActionResult`
```csharp
public static class ResultExtensions
{
	public static IActionResult ToActionResult(this Result result, ControllerBase controller, int errorStatusCode = 500)
	{
		return result.IsSuccess
			? controller.Ok()
			: controller.StatusCode(errorStatusCode, result.Error);
	}

	public static IActionResult ToActionResult<T>(this Result<T> result, ControllerBase controller, int errorStatusCode = 500)
	{
		return result.IsSuccess
			? controller.Ok(result.Value)
			: controller.StatusCode(errorStatusCode, result.Error);
	}
}
```

Then you can use a `Result` or `Result<T>` to send an `IActionResult`

```csharp
[HttpPost("edit/test-movement-actions/{packageId}")]
public async Task<IActionResult> SaveTestMovementActionsAsync(int packageId, TestMovementDto dto)
{
	var result = await _packageRepository.SaveTestMovementActionsAsync(packageId, dto);
	return result.ToActionResult(this);
}
```

Or if you want to use a specific HTTP status code:

```csharp
[HttpGet("{id}")]
public IActionResult GetUser(int id)
{
    var user = _userRepository.GetById(id);
    
    if (user == null)
    {
        // Return a Result with 404 status code
        return Result.Failure($"User with ID {id} not found").ToActionResult(this, 404);
    }
    
    return Result<User>.Success(user).ToActionResult(this);
}
```