---
layout: post
title: "Handling SOAP responses"
category: 
date: " 2014-08-30 20:48:37"
tags: [symfony2, soap]
---

![SOAP]({{ site.url }}/assets/soap.jpg ){: .center-image }

SOAP, originally defined as Simple Object Access protocol, is a protocol specification for exchanging structured
information in the implementation of web services in computer networks. It relies on XML Information Set for its
message format, and usually relies on other application layer protocols, most notably Hypertext Transfer Protocol
(HTTP) or Simple Mail Transfer Protocol (SMTP), for message negotiation and transmission.

<a title="SOAP Wikipedia Article" href="http://en.wikipedia.org/wiki/SOAP" target="_blank">Taken from Wikipedia</a>

I will talk only about part of SOAP and that is how to handle SOAP response using PHP.

So let's say you just made a SOAP request and you got response that looks something like this

{% highlight xml+mako %}
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soap:Body>
        <tns:PoslovniProstorOdgovor Id="G0x7fc3172220f8-4D"
                                    xsi:schemaLocation="http://www.apis-it.hr/fin/2012/types/f73 FiskalizacijaSchema.xsd "
                                    xmlns:tns="http://www.apis-it.hr/fin/2012/types/f73">
            <tns:Zaglavlje>
                <tns:IdPoruke>XXXXXXXXXXX</tns:IdPoruke>
                <tns:DatumVrijeme>XXXXXXXXXXX</tns:DatumVrijeme>
            </tns:Zaglavlje>
            <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
                <SignedInfo>
                    <CanonicalizationMethod Algorithm="http://www.w3.org/TR/2001/REC-xml-c14n-20010315"/>
                    <SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                    <Reference URI="#XXXXXXXXXXXXXX">
                        <Transforms>
                            <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                            <Transform Algorithm="http://www.w3.org/TR/2001/REC-xml-c14n-20010315"/>
                        </Transforms>
                        <DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                        <DigestValue>XXXXXXXXX</DigestValue>
                    </Reference>
                </SignedInfo>
                <SignatureValue>
                    XXXXXXXXXXXXX
                </SignatureValue>
                <KeyInfo>
                    <X509Data>
                        <X509Certificate>
                            XXXXXXXXXXXXX
                        </X509Certificate>
                        <X509IssuerSerial>
                            <X509IssuerName>XXXXXX</X509IssuerName>
                            <X509SerialNumber>XXXXXXX</X509SerialNumber>
                        </X509IssuerSerial>
                    </X509Data>
                </KeyInfo>
            </Signature>
        </tns:PoslovniProstorOdgovor>
    </soap:Body>
</soap:Envelope>
{% endhighlight %}
<!--more-->

You may think hey it's easy I just need to use simplexml_load_string or simplxml_load_file and that's it.

If you were to do

{% highlight php5 %}
    $xml = simplexml_load_string($soapResponse);

    var_dump($xml); die();
{% endhighlight %}

You'd get empty SimpleXML object.

The reason why it didn't work is because you need to register namespaces.
If you were to look again at that XML you'd notice that there are 'soap' and 'tns' prefixes.
To successfully read our XML we need to register those namespaces.

{% highlight php5 %}
    $xml = simplexml_load_string($soapResponse);
    $xml = $xml->children('soap', true);
    $xml = $xml->children('tns', true);

    var_dump($xml); die();
{% endhighlight %}

Now if you were to var_dump it you'd get all needed data.

The second parameter in children method that is set to true tells it that first parameter is a prefix.
We could have also used it like this


{% highlight php5 %}
    $xml = simplexml_load_string($soapResponse);
    $xml = $xml->children('http://schemas.xmlsoap.org/soap/envelope/');
    $xml = $xml->children('http://www.apis-it.hr/fin/2012/types/f73');

    var_dump($xml); die();
{% endhighlight %}

And if would have worked as well.

If you have a better way to handle SOAP responses comment it down below.