This article goes over the process to use AWS SES as a custom email sender with ASP .Net Core Identity. I'm using this inside a MVC project. 

This code depends on AWSSDK.SimpleEmail (which can be installed from nuget) and Microsoft.AspNetCore.Identity.UI (should be included in the default install of identity).

The custom email sender needs to implement IEmailSender, which only has one method, SendEmailAsync; apart from this, you can do whatever you like. My implementation below should work fine for you if you can have have AWS credentials set up on the machine as it uses the AWS SDK. Alternatively, you can access SES via SMTP and drop your implementation in. Apart from this, you simply need to add the custom email sender in the ConfigureServices method of Startup.cs. To do this, simply add this line:
```csharp
services.AddTransient<IEmailSender, SESEmailSender>();
```


 #### Custom Email Sender implementation
```csharp
using Amazon;
using Amazon.SimpleEmail;
using Amazon.SimpleEmail.Model;
using Microsoft.AspNetCore.Identity.UI.Services;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace YOUR_NAMESPACE
{
    public class SESEmailSender : IEmailSender
    {
        public async Task SendEmailAsync(string receiverAddress, string subject, string htmlBody)
        {
            // Change to your from email
            string senderAddress = "sender@example.com";
            // Change to your region
            using (var client = new AmazonSimpleEmailServiceClient(RegionEndpoint.EUWest2))
            {
                var sendRequest = new SendEmailRequest
                {
                    Source = senderAddress,
                    Destination = new Destination
                    {
                        ToAddresses =
                        new List<string> { receiverAddress }
                    },
                    Message = new Message
                    {
                        Subject = new Content(subject),
                        Body = new Body
                        {
                            Html = new Content
                            {
                                Charset = "UTF-8",
                                Data = htmlBody
                            },
                            Text = new Content
                            {
                                Charset = "UTF-8",
                                Data = htmlBody
                            }
                        }
                    }
                };
                var response = await client.SendEmailAsync(sendRequest);
            }
        }
    }
}
```
