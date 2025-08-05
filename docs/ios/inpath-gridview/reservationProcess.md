---
layout: default
title: Reservation Process
parent: In Path with Grid View
grand_parent: iOS Integration
nav_order: 3
permalink: /docs/ios/inpath-gridview/reservation-process
---

# Reservation Process

{: .no_toc }

Following the In Path process, a payload is returned in JSON Format which can later be used to make a reservation with our backend system.

---

The OTA message will contain a list of placeholder fields which are preset to default values.
It is expected that these fields are overridden with meaningful booking data and the message is processed directly with our OTA_VehResRQ endpoint. 

Full details on using the OTA_VehResRQ endpoint and our API in general can be found in our <a href="http://docs.cartrawler.com/docs/xml/">API Docs</a>.

--- 

## Payload Structure

This is the payload structure that is passed back via call backs in our native SDK. A placeholder tag will only be present in the case that input data is not supplied for that tag. 

For example if the user's country name code is empty/null on input, this will be replaced with the placeholder tag.

### Placeholder Fields

#### Passenger details
<dl>
<dt>[FIRSTNAME]</dt><dd>Passenger's firstname</dd>
<dt>[SURNAME]</dt><dd>Passenger's surname</dd>
<dt>[TELEPHONE]</dt><dd>Passenger's telephone number</dd>
<dt>[EMAIL]</dt><dd>Passenger's email address</dd>
<dt>[ADDRESSLINE1]</dt><dd>Passenger's address</dd>
<dt>[CITY]</dt><dd>Passenger's city</dd>
<dt>[POSTCODE]</dt><dd> Passenger's postcode</dd>
<dt><small>[COUNTRY]</small></dt><dd>Address line country code</dd>
</dl>

#### Credit Card Payment
<dl>
<dt>[CARDCODE]</dt><dd>e.g. VI = Visa, MC = Mastercard</dd>
<dt>[CARDNUMBER]</dt><dd>Credit card number</dd>
<dt>[EXPIREDATE]</dt><dd>expiry date of credit card in format MMYY (MM being Month, YY being Year)</dd>
<dt>[SERIESCODE]</dt><dd>Credit card verification value</dd>
<dt>[CARDHOLDERNAME]</dt><dd>Credit card holder name</dd>
</dl>

#### Additional Flight Information
<dl>
<dt>[BOOKINGREF]</dt><dd>Flight PNR or Booking reference</dd>
</dl>

#### Example JSON Payload:

```json
{
    "@Target": "Test",
    "@PrimaryLangID": "en",
    "POS": {
        "Source": [
            {
                "@ERSP_UserID": "SB",
                "@ISOCurrency": "EUR",
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "412005",
                    "@ID_Context": "CARTRAWLER"
                }
            },
            {
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "2691754392246810",
                    "@ID_Context": "CUSTOMERID"
                }
            },
            {
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "2691754392246810",
                    "@ID_Context": "ENGINELOADID"
                }
            },
            {
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "2",
                    "@ID_Context": "BROWSERTYPE"
                }
            },
            {
                "RequestorID": {
                    "@Type": "16",
                    "@ID_Context": "ORDERID",
                    "@ID": "[BOOKINGREF]"
                }
            }
        ]
    },
    "@xmlns": "http://www.opentravel.org/OTA/2003/05",
    "@xmlns:xsi": "http://www.w3.org/2001/XMLSchema-instance",
    "@Version": "3.000",
    "VehResRQCore": {
        "@Status": "All",
        "VehRentalCore": {
            "@PickUpDateTime": "2025-10-16T07:55:00",
            "@ReturnDateTime": "2025-10-22T19:50:00",
            "PickUpLocation": {
                "@LocationCode": "4513",
                "@CodeContext": "CARTRAWLER"
            },
            "ReturnLocation": {
                "@LocationCode": "4513",
                "@CodeContext": "CARTRAWLER"
            }
        },
        "Customer": {
            "Primary": {
                "PersonName": {
                    "GivenName": "John",
                    "Surname": "Doe"
                },
                "Telephone": [
                    {
                        "@PhoneTechType": "1",
                        "@PhoneNumber": "[TELEPHONE]"
                    }
                ],
                "Email": {
                    "@EmailType": "2",
                    "#text": "john.doe@cartrawler.com"
                },
                "Address": {
                    "@Type": "2",
                    "AddressLine": [
                        "[ADDRESSLINE1]"
                    ],
                    "CityName": "[CITY]",
                    "PostalCode": "[POSTCODE]",
                    "CountryName": {
                        "@Code": "IE"
                    }
                },
                "CitizenCountryName": {
                    "@Code": "IE"
                }
            }
        },
        "DriverType": {
            "@Age": 30
        }
    },
    "VehResRQInfo": {
        "ArrivalDetails": {
            "@TransportationCode": "14",
            "OperatingCompany": "XX",
            "@Number": "8719"
        },
        "RentalPaymentPref": {
            "PaymentCard": {
                "@ExpireDate": "[EXPIREDATE]",
                "CardHolderName": "[CARDHOLDERNAME]",
                "@CardCode": "[CARDCODE]",
                "CardNumber": {
                    "PlainText": "[CARDNUMBER]"
                },
                "SeriesCode": {
                    "PlainText": "[SERIESCODE]"
                },
                "ThreeDomainSecurity": {
                    "Gateway": {
                        "@ECI": "[ECI]"
                    },
                    "Results": {
                        "@CAVV": "[CAVV]"
                    }
                }
            }
        },
        "Reference": {
            "@Type": "16",
            "@ID": "896109193",
            "@ID_Context": "CARTRAWLER",
            "@DateTime": "2025-08-05T13:25:33.772+01:00",
            "@URL": "2b3a7810-941b-4f9c-822f-ca5550554489.411"
        },
        "TPA_Extensions": {
            "MarketingEmail": {
                "@save": false
            },
            "ConsumerIP": "37.228.201.120",
            "Window": {
                "@name": "Car rental results and booking",
                "@engine": "CTSB-V4.0.230",
                "@svn": "4.0.230-0.0.8",
                "@region": "en",
                "UserAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148",
                "BrowserName": "WebKit",
                "BrowserVersion": "605.1.15",
                "OS": "18.0",
                "URL": "https://external-dev-ajax.cartrawler.com/smartblock/sb4/iframes/ryanair.html?customerEmail=john.doe@cartrawler.com&firstName=John&pax=2:0:0:0&baggage=2&flightReturn=LHR%7CDUB%7C2025-10-22T21:20:00%7C2025-10-23T22:45:00%7CXX8720&context=INPATH&flightOrigin=DUB%7CLHR%7C2025-10-16T06:05:00%7C2025-10-16T07:25:00%7CXX8719&flightFare=101.99&flightClass=business&residence=IE&tier=platinum&language=en&surName=Doe&currency=EUR&membershipId=WZ123456789"
            },
            "Tracking": {
                "CustomerID": "2691754392246810",
                "EngineLoadID": "2691754392246810",
                "SessionID": "2691754392246810",
                "QueryID": ""
            },
            "Reference": {
                "@Type": "16",
                "@ID": "AXA.3175918.411",
                "@ID_Context": "INSURANCE",
                "@Amount": "44.10",
                "@CurrencyCode": "EUR"
            }
        }
    }
}
```
