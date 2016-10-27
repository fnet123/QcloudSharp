QcloudSharp: Unoffical Qcloud.com API wrapper for .Net
===
[![Build status](https://ci.appveyor.com/api/projects/status/b96llok223xfhmx5?svg=true)](https://ci.appveyor.com/project/7IN0SAN9/qcloudsharp)
[![NuGet version](https://badge.fury.io/nu/QcloudSharp.svg)](https://www.nuget.org/packages/QcloudSharp)

### Installation
To install QcloudSharp, run the following command in the Package Manager Console
```powershell
PM> Install-Package QcloudSharp
```

### Example
```csharp
using QcloudSharp;
using Enum = QcloudSharp.Enum;

dynamic client = new QcloudClient
{
    SecretId = "Your_Secret_Id",
    SecretKey = "Your_Secret_Key"
};

var resultString = client.DescribeUserInfo(Enum.Endpoint.Trade, Enum.Endpoint.Region.CAN);
dynamic result = JsonConvert.DeserializeObject<ApiResult>(resultString);
```

Or you can try [QcloudCvmHelper](https://github.com/labs7in0/QcloudCvmHelper)

### Enums

All Enums are provided in class `QcloudSharp.Enum`.

```csharp
public enum Endpoint // Abbreviation for endpoint domain
```

Members
* Account
* Bill
* Cbs
* Cdb
* Cdn
* Cmem
* Cvm
* Eip
* Image
* Lb
* Live
* Market
* Monitor
* Scaling
* Sec
* Snapshot
* Tdsql
* Trade
* Vod
* Vpc
* Wenzhi
* Yunsou

```csharp
public enum Region // IATA code for Region city
```

Members
* BJS
* CAN
* SHA
* HKG
* YTO

### Classes

#### QcloudClient

```csharp
public class QcloudClient : DynamicObject
```

Constructors
* `QcloudClient()`
* `QcloudClient(SecretId, SecretKey)`

Properties
* string `SecretId`
* string `SecretKey`
* Enum.Region `Region`
* Enum.Endpoint `Endpoint`

Methods
* `void AddParameter(KeyValuePair<string, string>)`
* `void AddParameter(IEnumerable<KeyValuePair<string, string>>)`
* `void ClearParameter()`
* `Submit(Enum.Endpoint, Enum.Region, string)`
* `Submit(Enum.Endpoint, string)`
* `Submit(string)`

Dynamic Methods
* `{Action}(Enum.Endpoint, Enum.Region)`
* `{Action}(Enum.Endpoint, Enum.Region, IEnumerable<KeyValuePair<string, string>>)`
* `{Action}(Enum.Endpoint endpoint, Enum.Region region,KeyValuePair<string, string>, ...)`

#### ApiResult

```csharp
public class ApiResult : DynamicObject
```

Constructors
* `ApiResult()`

Properties
* int `Code`
* string `Message`

Dynamic Properties
* object Any { Get; Set; }

Example
```csharp
using QcloudSharp;
using Newtonsoft.Json;

var resultString = "{\"code\":0,\"message\": \"\",\"userInfo\":{\"name\":\"compName\",\"isOwner\":1,\"mailStatus\":1,\"mail\":\"112233@qq.com\",\"phone\":\"13811112222\"}}";
dynamic result = JsonConvert.DeserializeObject<ApiResult>(resultString);

try
{
    Console.WriteLine(result.Code);
	Console.WriteLine(result.userInfo.name);
	Console.WriteLine(result.null); // Will throw an ArgumentNullException
}
catch(Exception ex)
{
	Console.WriteLine(ex.message);
}

```