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
    ->setHeader('Content-Type: application/mercury.longform')
    ->setBody(
        "<?xml version=\"1.0\" encoding=\"utf-8\" ?>\r\n" .
        "<Call xmlns=\"http://www.dial800.com/roundtrip/2011-07-15\r\n" .
        "      xmlns:mlf=\"http://www.mercurymedia.com/longform/2011-08-03\">" .
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
            const string contentType = "application/mercury.longform";
            const string postMethod  = "POST";
            const string postData    
            = @"<?xml version="1.0" encoding="utf-8" ?>
                <Call xmlns="http://www.dial800.com/roundtrip/2011-07-15"
                      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-03">      
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
      xmlns:mlf="http://www.mercurymedia.com/longform/2011-08-03">      
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
                 headers={'Content-Type': 'application/mercury.longform'},
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
request.content_type = "application/mercury.longform"
response     = http.request(request)
```

## Reference

The Reference documentation can be found [here](http://docs.dial800.com/dial800/reference).

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

## Glossary
<dl>
    <dt>ID</dt>
    <dd>String value representing the alphanumeric Call ID of the phone call to be matched for the associated Sales data. (The “&lt;ID&gt;” element must always be passed on its own or an error will be issued.) Optional.</dd>
    
    <dt>DNIS</dt>
    <dd>10-Digit string value representing the DNIS (TFN) of the phone call to be matched for the associated Sales data. (The "&lt;DNIS&gt;" element must be passed if the “&lt;ID&gt;” element is absent.) Optional.</dd>
    
    <dt>ANI</dt>
    <dd>10-Digit string value representing the ANI (Caller #) of the phone call to be matched for the associated Sales data. (The "&lt;ANI&gt;" element must be passed if the “&lt;ID&gt;” element is absent.) Optional.</dd>
    
    <dt>Target</dt>
    <dd>10-Digit string value representing the Target (“RingTo”) number of the phone call to be matched for the associated Sales data. (The "&lt;Target&gt;" element must be passed if the “&lt;ID&gt;” element is absent.) Optional.</dd>
    
    <dt>CallStart</dt>
    <dd>The Call Start Time representing when this call was initiated. This value must be expressed using the standard XML DateTime format which includes the timezone offset identifier(i.e. “YYYY-MM-DDThh:mm:ss±HH:MM” or “YYYY-MM-DDThh:mm:ssZ”). (The "&lt;CallStart&gt;" element may optionally be passed if the “&lt;ID&gt;” element is absent.) Optional.</dd>
    
    <dt>TelemarketingCompanyCode</dt>
    <dd>Company (Call Center) Code to be associated with the call. Usually hardcoded. Required.</dd>
    
    <dt>ProductCode</dt>
    <dd>Product Code (per Mercury setup sheet) to be associated with the call. Required.</dd>
    
    <dt>RecordStatus</dt>
    <dd>Record Status: “V” for valid orders, “I” for Inquiry/No Sale.  Required.</dd>
    
    <dt>City</dt>
    <dd>The City of the caller. Required.</dd>
    
    <dt>State</dt>
    <dd>The State of the caller. Required.</dd>
    
    <dt>ZipCode</dt>
    <dd>The ZIP Code of the caller. Required.</dd>
    
    <dt>MediaSource</dt>
    <dd>The Media Source (Station Call Letters) associated with this call. Required.</dd>
    
    <dt>OrderDateTime</dt>
    <dd>The Order Date/Time at which the order was placed. Required.</dd>
    
    <dt>UniqueOrderNumber</dt>
    <dd>The Unique Order Number associated with this call (generated by Call Center). Required.</dd>
    
    <dt>PaymentCode</dt>
    <dd>The Payment Code associated with this order, as follows:<br/>
    “A”=Amex, “B”=Bill, “C”=COD, “D”=Diner’s “E”=CarteBlanche, “M”= MasterCard, “V”=Visa, “$”=Cash, “+”=Check, “S”=Sears, “Z”=Discover.<br/>
    Required.</dd>
    
    <dt>MenuResponses</dt>
    <dd>Menu Responses: Varies by customer, generally not used. Optional.</dd>
    
    <dt>Items</dt>
    <dd>Collection of one or more items sold during this call. Required.</dd>
    
    <dt>Item mlf:id="ItemId"</dt>
    <dd>Item: Includes ItemID as defined for Mercury for the current campaign. Required.</dd>
    
    <dt>UnitPrice</dt>
    <dd>Unit Price: In standard XML format of dollars & cents. Required.</dd>
    
    <dt>IsUpsell</dt>
    <dd>IsUpsell: “0” = Main Order, “1” if this is an Upsell item. Required.</dd>
    
    <dt>Quantity</dt>
    <dd>Quantity: Integer quantity of this items sold with this order. Required.</dd>
</dl>

## Other Integrations

### Dial800

[Our native interface](http://docs.dial800.com/).

### MercuryMedia

[Mercury Short Form](http://docs.dial800.com/dial800/msf).

### Euro

Are you using Euro's format? You can find the details [here](http://docs.dial800.com/dial800/euro).

### Omni

Working with Omni? You can find more details [here](https://docs.dial800.com/dial800/omni).