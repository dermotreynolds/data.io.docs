--
sort: 1
---

# Defender For Cloud Example

To determine if a blob has been scanned or contains a virus you need to check specific tags that are deployed on the blob by Defender For Cloud.
Below is some code that should demonstrate how this is done.


1.	Declare some constants for the tags and values that we are looking for:

```C#
static class Constants
{
    public const string DefenderTag = "Malware Scanning scan result";
	public const string DefenderHasVirusValue = "Malicious";
	public const string DefenderIsCleanValue = "No threats found";
}
```
	

2.	Get a blob client for the blob

```C#
BlobClient blob = container.GetBlobClient(blob.Name);
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
if (tag.Key.ToString() == Constants.DefenderTag)
{
    ...
}
```
       


6.	Check the value:
	
```C#
if (tag.Value.ToString() == Constants.DefenderHasVirusValue)
{
    ...
}
```

