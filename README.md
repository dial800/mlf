![Documentation](http://docs.dial800.com/images/roundtrip.png)

We know. You have two days to integrate with us. Don't worry, it's easy. We're here to help.

## How to submit data

* [if I am using PHP](#using-php)?
* [or C Sharp?](#using-c-sharp)
* [or Python?](#using-python)
* [or Ruby?](#using-ruby)

### Using PHP

1. Contact our team for credentials.
2. Generate Payload
3. Submit

### Using C Sharp

1. Contact our team for credentials.
2. Generate Payload
3. Submit

### Using Python

1. Contact our team for credentials.
2. Generate Payload
3. Submit

### Using Ruby

1. Contact our team for credentials.
2. Generate Payload
3. Submit

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

## Working with Media Agencies

### MercuryMedia

[MercuryMedia](www.mercurymedia.com) uses two slightly different formats, [MLF](http://docs.dial800.com/dial800/mlf) and [MSF](http://docs.dial800.com/dial800/msf).

### Euro

Are you using Euro's format? You can find the details [here](http://docs.dial800.com/dial800/euro).