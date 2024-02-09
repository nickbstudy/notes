A nice tidy implementation:

#### Program.cs:
```
services.Configure<EmailOptions>(builder.Configuration.GetSection("EmailOptions"));
services.AddScoped<EmailService>();
```
---
#### /Configuration/EmailOptions.cs
```
public class EmailOptions
{
    public string ConnectionString { get; set; } = string.Empty;
    public string From { get; set; } = string.Empty;
    public string Recipient { get; set; } = string.Empty;
}
```
---
#### /Services/EmailService.cs
```
public class EmailService
{
    private readonly EmailOptions _emailOptions;
    private readonly ILogger<HomeController> _logger;

    public EmailService(ILogger<HomeController> logger, IOptions<EmailOptions> emailOptions)
    {
        _emailOptions = emailOptions.Value;
        _logger = logger;
    }

    public async Task<bool> SendEmail(string id)
    {
        var htmlContent = $"<html><body><p>A new Routine WD with ID#{id} has been submitted.<br><br><a href=\"http://nzsiisprod/wastedeclarations/Admin/Routine/NewPendingSelected/{id}\">You can begin the approval process here.</a></p></body></html>";
        var subject = $"New WD submitted - ID #{id}";

        try
        {
            var emailClient = new EmailClient(_emailOptions.ConnectionString);
            var emailSendOperation = await emailClient.SendAsync(
                wait: Azure.WaitUntil.Completed,
                senderAddress: _emailOptions.From,
                recipientAddress: _emailOptions.Recipient,
                subject: subject,
                htmlContent: htmlContent);
            _logger.LogInformation($"Email sent. Status: {emailSendOperation.Value.Status}");
            _logger.LogInformation($"Email operation ID: {emailSendOperation.Id}");
            return true;
        }
        catch (RequestFailedException ex)
        {
            _logger.LogError($"Email send operation failed with error code: {ex.ErrorCode}, message: {ex.Message}");
            return false;
        }
    }
}
```
---
#### /appsettings.json
```
"EmailOptions": {
  "ConnectionString": "your-connection-string-goes-here",
  "From": "NoReply@yourdomain.com",
  "Recipient": "nick.brooker@bluescopesteel.com"
},
```