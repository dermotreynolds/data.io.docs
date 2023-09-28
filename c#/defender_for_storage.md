--
sort: 1
---

# Defender For Cloud Example


1.	Declare some constants for the tags and values that we are looking for:

```C#
static class Constants
{
    public const string MalwareScanTag = "Malware Scanning scan result";
	public const string MaliciousFileValue = "Malicious";
	public const string CleanFileValue = "No threats found";
}
```
	

2.	Get a blob client for the blob

```C#
BlobClient srcBlobClient = sourceContainer.GetBlobClient(blob.Name);
```
	

3.	Get the tags for the blob:

```C#
var tags = blob.GetTags().Value.Tags;
```
	

4.	Check chekc each tag:

```C#
foreach (var tag in tags)
```
	

5.	Check the key:

```C#
if (tag.Key.ToString() == Constants.MalwareScanTag)
{
    ...
}
```
       


6.	Check the value:
	
```C#
if (tag.Value.ToString() == Constants.MaliciousFileValue)
{
    ...
}
```

