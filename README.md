![Documentation](http://docs.dial800.com/images/roundtrip.png)

We know. You have two days to integrate with us. Don't worry, it's easy. We're here to help.

## How to submit data

* [if I am using PHP](#using-php)?
* [or C Sharp?](#using-c-sharp)
* [or Python?](#using-python)
* [or Ruby?](#using-ruby)

### Using PHP

```php
<?php

# 1. Generate Payload

$request = new HTTP_Request2('http://routing.dial800.com/roundtrip');
$request->setMethod(HTTP_Request2::METHOD_POST)
    ->setAuth('user','password', HTTP_Request2::AUTH_BASIC)
    ->setHeader('Content-type: application/roundtrip.sales')
    ->setBody(
        "<?xml version=\"1.0\" encoding=\"utf-8\" ?>\r\n" .
        "<Call xmlns=\"http://www.dial800.com/roundtrip/2011-07-15\r\n" .
        "      xmlns:mlf=\"http://www.mercurymedia.com/longform/2011-08-04\">" .
        "<ANI>tel:3105555555</ANI>\r\n" .
        "<Target>tel:3109999999</Target>\r\n" . 
        "<CallStart>2011-07-15T01:02:03-08:00</CallStart>\r\n" .
        "<mlf:TelemarketingCompanyCode>UMG</mlf:TelemarketingCompanyCode>\r\n" .
        "<mlf:ProductCode>GDV3</mlf:ProductCode>\r\n" .
        "<mlf:RecordStatus>V</mlf:RecordStatus>\r\n" .
        "<mlf:City>Los Angeles</mlf:City>\r\n" .
        "<mlf:State>CA</mlf:State>\r\n" .
        "<mlf:ZipCode>90210</mlf:ZipCode>\r\n" .
        "<mlf:MediaSource>XBDC</mlf:MediaSource>\r\n" .
        "<mlf:OrderDateTime>2011-08-03T00:00:00Z</mlf:OrderDateTime>\r\n" .
        "<mlf:UniqueOrderNumber>ORD123456</mlf:UniqueOrderNumber>\r\n" .
        "<mlf:PaymentCode>A</mlf:PaymentCode>\r\n" .
        "<mlf:MenuResponses>5</mlf:MenuResponses>\r\n" .
        "<mlf:Items>\r\n" .
        "    <mlf:Item mlf:id=\"ItemId\">\r\n" .
        "		<mlf:UnitPrice>199.97</mlf:UnitPrice>\r\n" .
        "		<mlf:IsUpsell>0</mlf:IsUpsell>\r\n" .
        "		<mlf:Quantity>1</mlf:Quantity>\r\n" .
        "	</mlf:Item>\r\n" .
        "	<mlf:Item mlf:id=\"ItemId2\">\r\n" .
        "		<mlf:UnitPrice>50.00</mlf:UnitPrice>\r\n" .
        "		<mlf:IsUpsell>1</mlf:IsUpsell>\r\n" .
        "		<mlf:Quantity>1</mlf:Quantity>\r\n" .
        "	</mlf:Item>\r\n" .
        "</mlf:Items>" .
        "</Call>"
    );

# 2. Submit

echo $request->send()->getBody();
?>
```

### Using C Sharp

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;


namespace Dial800
{
    class Program
    {
        static void Main(string[] args)
        {
            byte[] postDataBytes;
            const string userName    = "user";
            const string password    = "password";
            const string contentType = "application/roundtrip.sales";
            const string postMethod  = "POST";
            const string postData    
            = @"<?xml version="1.0" encoding="utf-8" ?>
                <Call xmlns="http://www.dial800.com/roundtrip/2011-07-15"
                      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-04">      
                <ANI>tel:3105555555</ANI>
                <Target>tel:3109999999</Target>
                <CallStart>2011-07-15T01:02:03-08:00</CallStart>
                <mlf:TelemarketingCompanyCode>UMG</mlf:TelemarketingCompanyCode>
                <mlf:ProductCode>GDV3</mlf:ProductCode>
                <mlf:RecordStatus>V</mlf:RecordStatus>
                <mlf:City>Los Angeles</mlf:City>
                <mlf:State>CA</mlf:State>
                <mlf:ZipCode>90210</mlf:ZipCode>
                <mlf:MediaSource>XBDC</mlf:MediaSource>
                <mlf:OrderDateTime>2011-08-03T00:00:00Z</mlf:OrderDateTime>
                <mlf:UniqueOrderNumber>ORD123456</mlf:UniqueOrderNumber>
                <mlf:PaymentCode>A</mlf:PaymentCode>
                <mlf:MenuResponses>5</mlf:MenuResponses>
                <mlf:Items>
                    <mlf:Item mlf:id="ItemId">
                		<mlf:UnitPrice>199.97</mlf:UnitPrice>
                		<mlf:IsUpsell>0</mlf:IsUpsell>
                		<mlf:Quantity>1</mlf:Quantity>
                	</mlf:Item>
                	<mlf:Item mlf:id="ItemId2">
                		<mlf:UnitPrice>50.00</mlf:UnitPrice>
                		<mlf:IsUpsell>1</mlf:IsUpsell>
                		<mlf:Quantity>1</mlf:Quantity>
                	</mlf:Item>
                </mlf:Items>
                </Call>";

            const string uri = "http://roundtrip.dial800.com/roundtrip";

            postDataBytes = Encoding.UTF8.GetBytes( postData );

            var urlEndpoint = new Uri(uri);
            var request = WebRequest.Create( urlEndpoint ) as HttpWebRequest;

            request.Credentials   = new NetworkCredential( userName, password );
            request.Method        = postMethod;
            request.ContentType   = contentType;
            request.ContentLength = postDataBytes.Length;

            using(Stream postStream = request.GetRequestStream())
            {
                postStream.Write( postDataBytes, 0, postDataBytes.Length );
            }

            try
            {
                using ( HttpWebResponse response = request.GetResponse() as HttpWebResponse )
                {
                    var reader = new StreamReader( response.GetResponseStream() );
                    Console.WriteLine( reader.ReadToEnd() );
                }
            }
            catch (WebException ex)
            {
                if (ex.Response != null)
                {
                    using (HttpWebResponse errorResponse = (HttpWebResponse)ex.Response)
                    {
                        Console.WriteLine(
                            "The server returned '{0}' with the status code {1} ({2:d}).",
                            errorResponse.StatusDescription, errorResponse.StatusCode,
                            errorResponse.StatusCode);
                    }
                }
            }
            Console.ReadKey();
        }
    }
}
```

### Using Python

```python
from requests.auth import HTTPBasicAuth
payload = '''
<?xml version="1.0" encoding="utf-8" ?>
<Call xmlns="http://www.dial800.com/roundtrip/2011-07-15"
      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-04">      
    <ANI>tel:3105555555</ANI>
    <Target>tel:3109999999</Target>
    <CallStart>2011-07-15T01:02:03-08:00</CallStart>
    <mlf:TelemarketingCompanyCode>UMG</mlf:TelemarketingCompanyCode>
    <mlf:ProductCode>GDV3</mlf:ProductCode>
    <mlf:RecordStatus>V</mlf:RecordStatus>
    <mlf:City>Los Angeles</mlf:City>
    <mlf:State>CA</mlf:State>
    <mlf:ZipCode>90210</mlf:ZipCode>
    <mlf:MediaSource>XBDC</mlf:MediaSource>
    <mlf:OrderDateTime>2011-08-03T00:00:00Z</mlf:OrderDateTime>
    <mlf:UniqueOrderNumber>ORD123456</mlf:UniqueOrderNumber>
    <mlf:PaymentCode>A</mlf:PaymentCode>
    <mlf:MenuResponses>5</mlf:MenuResponses>
    <mlf:Items>
        <mlf:Item mlf:id="ItemId">
    		<mlf:UnitPrice>199.97</mlf:UnitPrice>
    		<mlf:IsUpsell>0</mlf:IsUpsell>
    		<mlf:Quantity>1</mlf:Quantity>
    	</mlf:Item>
    	<mlf:Item mlf:id="ItemId2">
    		<mlf:UnitPrice>50.00</mlf:UnitPrice>
    		<mlf:IsUpsell>1</mlf:IsUpsell>
    		<mlf:Quantity>1</mlf:Quantity>
    	</mlf:Item>
    </mlf:Items>
</Call>
'''
r = request.post('http://routing.dial800.com/routing',
                 auth=HTTPBasicAuth('user','password'),
                 headers={'content-type': 'application/roundtrip.sales'},
                 data=payload)
```

### Using Ruby

```ruby
require "net/http"
require "uri"

uri = URI.parse("http://routing.dial800.com/routing")

http         = Net::HTTP.new(uri.host, uri.port)
request      = Net::HTTP::Post.new(uri.host,uri.port)
request.body = xml_string
request.basic_auth("user","password")
request.content_type = "application/roundtrip.sales"
response     = http.request(request)
```

## Reference

### Authentication

### POST

#### Request

```
POST /calls
Content-Type: application/mercury.longform
```

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Call xmlns="http://www.dial800.com/roundtrip/2011-07-15"
      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-03">      
    <ANI>tel:3105555555</ANI>
    <Target>tel:3109999999</Target>
    <CallStart>2011-07-15T01:02:03-08:00</CallStart>
    <mlf:TelemarketingCompanyCode>TEST</mlf:TelemarketingCompanyCode>
    <mlf:ProductCode>TEST</mlf:ProductCode>
    <mlf:RecordStatus>V</mlf:RecordStatus>
    <mlf:City>Los Angeles</mlf:City>
    <mlf:State>CA</mlf:State>
    <mlf:ZipCode>90036</mlf:ZipCode>
    <mlf:MediaSource>TEST</mlf:MediaSource>
    <mlf:OrderDateTime>2011-08-03T00:00:00Z</mlf:OrderDateTime>
    <mlf:UniqueOrderNumber>123456</mlf:UniqueOrderNumber>
    <mlf:PaymentCode>A</mlf:PaymentCode>
    <mlf:MenuResponses>5</mlf:MenuResponses>
    <mlf:Items>
        <mlf:Item mlf:id="ItemId">
            <mlf:UnitPrice>200</mlf:UnitPrice>
            <mlf:IsUpsell>0</mlf:IsUpsell>
            <mlf:Quantity>1</mlf:Quantity>
        </mlf:Item>
        <mlf:Item mlf:id="ItemId2">
            <mlf:UnitPrice>50</mlf:UnitPrice>
            <mlf:IsUpsell>1</mlf:IsUpsell>
            <mlf:Quantity>1</mlf:Quantity>
        </mlf:Item>
    </mlf:Items>
</Call>
```

#### Response

Call successfully matched.

```xml
200 OK
<ID>12345678990</ID>
```

No match for the call.

```
404 Not Found
```

### PUT

```
PUT /calls
Content-Type: application/mercury.longform
```

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Call xmlns="http://www.dial800.com/roundtrip/2011-07-15"
      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-03">      
    <ID>12345678990</ID>
    <mlf:TelemarketingCompanyCode>TEST</mlf:TelemarketingCompanyCode>
    <mlf:ProductCode>TEST</mlf:ProductCode>
    <mlf:RecordStatus>V</mlf:RecordStatus>
    <mlf:City>Los Angeles</mlf:City>
    <mlf:State>CA</mlf:State>
    <mlf:ZipCode>90036</mlf:ZipCode>
    <mlf:MediaSource>TEST</mlf:MediaSource>
    <mlf:OrderDateTime>2011-08-03T00:00:00Z</mlf:OrderDateTime>
    <mlf:UniqueOrderNumber>123456</mlf:UniqueOrderNumber>
    <mlf:PaymentCode>A</mlf:PaymentCode>
    <mlf:MenuResponses>5</mlf:MenuResponses>
    <mlf:Items>
        <mlf:Item mlf:id="ItemId">
            <mlf:UnitPrice>200</mlf:UnitPrice>
            <mlf:IsUpsell>0</mlf:IsUpsell>
            <mlf:Quantity>1</mlf:Quantity>
        </mlf:Item>
        <mlf:Item mlf:id="ItemId2">
            <mlf:UnitPrice>50</mlf:UnitPrice>
            <mlf:IsUpsell>1</mlf:IsUpsell>
            <mlf:Quantity>1</mlf:Quantity>
        </mlf:Item>
    </mlf:Items>
</Call>
</Call>
```

#### Response

Call successfully matched.

```xml
200 OK
<ID>12345678990</ID>
```

No match for the call.

```
404 Not Found
```

## Other integrations

### Dial800

[Our native interface](http://docs.dial800.com/).

### MercuryMedia

[Mercury Short Form](http://docs.dial800.com/dial800/msf).

### Euro

Are you using Euro's format? You can find the details [here](http://docs.dial800.com/dial800/euro).