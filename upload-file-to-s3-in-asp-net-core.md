The instructions in this article are designed for a C# ASP .NET MVC application. They should work elsewhere are with tweaks for your situation. This assumes that your AWS creds are already setup. It's based on <a href="https://docs.microsoft.com/en-us/aspnet/core/mvc/models/file-uploads?view=aspnetcore-5.0">a Microsoft article about file uploads</a> with tweaks for s3 upload

1. Add dependency
Add the NuGet module AWSSDK.S3 to your project

2. Add the data class
Create a file with this content
```csharp
using Amazon;
using Amazon.S3;
using Amazon.S3.Transfer;
using System;
using System.IO;
using System.Threading.Tasks;

namespace YOUR_NAMESPACE
{
    public class S3Upload
    {
        private static readonly RegionEndpoint bucketRegion = RegionEndpoint.EUWest2;
        private static IAmazonS3 s3Client;

        public static async Task UploadFileAsync(Stream FileStream, string bucketName, string keyName)
        {
            s3Client = new AmazonS3Client(bucketRegion);
            var fileTransferUtility = new TransferUtility(s3Client);
            await fileTransferUtility.UploadAsync(FileStream, bucketName, keyName);
        }
    }
}
```

3. Add a new modal
Add a new modal called FileUploadFormModal.cs to your project with this content

```csharp
public class FileUploadFormModal
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

4. Add a view
Create a view called FileUploadForm.cshtml with this content in Views/Home

```html
@model FileUploadFormModal
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUploadFormModal.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUploadFormModal.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

5. Edit HomeController.cs controller
Add a using statement for the data class and add the following code to the HomeController.cs

```csharp
public async Task<IActionResult> FileUploadForm()
{
    return View();
}

[HttpPost]
public async Task<IActionResult> FileUploadForm(FileUploadFormModal FileUpload)
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            await S3Upload.UploadFileAsync(memoryStream, "YOUR_BUCKET_NAME", "YOUR_KEY_NAME");
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return View();
}
```
