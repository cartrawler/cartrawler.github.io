---
layout: default
title: Reservation Process
parent: In Path
grand_parent: iOS Integration
nav_order: 3
permalink: /docs/ios/inpath/reservation-process
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
<dt><small>[COUNTRYNAMECODE]</small></dt><dd>Address line country code</dd>
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
    "@xmlns": "http://www.opentravel.org/OTA/2003/05",
    "@Version": "3.000",
    "@Target": "Test",
    "@PrimaryLangID": "en",
    "POS": {
        "Source": [
            {
                "@ERSP_UserID": "MO",
                "@ISOCurrency": "EUR",
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "787697",
                    "@ID_Context": "CARTRAWLER"
                }
            },
            {
                "RequestorID": {
                    "@Type": "16",
                    "@ID": "[BOOKINGREF]",
                    "@ID_Context": "ORDERID"
                }
            }
        ]
    },
    "VehResRQCore": {
        "@Status": "All",
        "VehRentalCore": {
            "@PickUpDateTime": "2019-11-12T14:26:37",
            "@ReturnDateTime": "2019-11-15T14:26:37",
            "PickUpLocation": {
                "@CodeContext": "CARTRAWLER",
                "@LocationCode": "470"
            },
            "ReturnLocation": {
                "@CodeContext": "CARTRAWLER",
                "@LocationCode": "470"
            }
        },
        "Customer": {
            "Primary": {
                "PersonName": {
                    "GivenName": "[FIRSTNAME]",
                    "Surname": "[SURNAME]"
                },
                "Telephone": [
                    {
                        "@PhoneTechType": "1",
                        "@CountryAccessCode": "353",
                        "@PhoneNumber": "[TELEPHONE]"
                    }
                ],
                "Email": {
                    "@EmailType": "2",
                    "#text": "[EMAIL]"
                },
                "Address": {
                    "@Type": "2",
                    "AddressLine": ["[ADDRESSLINE1]"],
                    "CityName": "[CITY]",
                    "PostalCode": "[POSTCODE]",
                    "CountryName": {
                        "@Code": "[COUNTRYNAMECODE]"
                    }
                },
                "CitizenCountryName": {
                    "@Code": "IE"
                },
                "CustLoyalty": [{
					"@ProgramID": "CARTRAWLER",
					"@MembershipID": "12345"
				}],
                "TPA_Extensions": {
                    "Benefits": {
                        "Benefit": [
                            {
                                "@xmlType": "AV",
                                "@type": "CORPORATE_RATE",
                                "@code": "X098765"
                            },
                            {
                                "@xmlType": "BG",
                                "@type": "CORPORATE_RATE",
                                "@code": "Y123456"
                            }
                        ]
                    }
                }
            }
        },
        "DriverType": {
            "@Age": "24"
        }
    },
    "VehResRQInfo": {
        "@PassengerQty": "1",
        "ArrivalDetails": {
            "@TransportationCode": "14",
            "@Number": "1234",
            "OperatingCompany": "RY"
        },
        "RentalPaymentPref": {
            "PaymentCard": {
                "@CardCode": "[CARDCODE]",
                "@ExpireDate": "[EXPIREDATE]",
                "CardHolderName": "[CARDHOLDERNAME]",
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
                        "@CAVV": "[CAVV]",
                        "@TransactionID": "[TRANSACTIONID]",
                        "@XID": "[XID]"
                    }
                },
                "TPA_Extensions": {
                    "ThreeDsVersion": "[THREEDSVERSION]"
                }
            }
        },
        "Reference": {
            "@Type": "16",
            "@ID": "1284794686",
            "@ID_Context": "CARTRAWLER",
            "@DateTime": "2019-11-11T14:26:38.290Z",
            "@URL": "9c537843-fbdc-4c86-910b-28b159c237c2.64"
        },
        "TPA_Extensions": {
            "Reference": {
                "@Type": "16",
                "@ID": "AXA.2317317.64",
                "@ID_Context": "INSURANCE",
                "@Amount": "49.56",
                "@CurrencyCode": "EUR",
                "@URL": "http://www.cartrawler.com/res/conditions/insurance/CT_AXA_TC_IE.pdf"
            },
            "Window": {
                "@engine": "Android Engine",
                "@name": "ANDROID-V1",
                "@region": "en-us",
                "@svn": "1.4.9-14"
            },
            "Tracking": {
                "CustomerID": "901573475928606",
                "SessionID": "601573475732337",
                "EngineLoadID": "601573475732337",
                "QueryID": "901573475930192"
            },
            "Persona": {
                "Characteristic": [
                    {
                        "@Name": "MyAccountId",
                        "@Value": "er89952d1333"
                    },
                    {
                        "@Name": "VisitorId",
                        "@Value": ""
                    }
                ]
            }
        }
    }
}
```
