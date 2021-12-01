#fedex-client

## Install

`npm i @kronsi/fedex`

## Usage

```js
  var fedexAPI = require('@kronsi/fedex');

  var fedex = new fedexAPI({
    environment: 'sandbox', // or live
    debug: true,
    key: 'KEY',
    password: 'DEVPASSWORD',
    account_number: 'ACCOUNT#',
    meter_number: 'METER#',
    imperial: true // set to false for metric
  });
  /**
   * Tracking
   */
  fedex.track({
    SelectionDetails: {
      PackageIdentifier: {
        Type: 'TRACKING_NUMBER_OR_DOORTAG',
        Value: '123456789012'
      }
    }
  }, function(err, res) {
    if(err) {
      return console.log(err);
    }

    console.log(res);
  });

  /**
   * Ship
   */
  var date = new Date();
  fedex.ship({
    RequestedShipment: {
      ShipTimestamp: new Date(date.getTime() + (24*60*60*1000)).toISOString(),
      DropoffType: 'REGULAR_PICKUP',
      ServiceType: 'FEDEX_GROUND',
      PackagingType: 'YOUR_PACKAGING',
      Shipper: {
        Contact: {
          PersonName: 'Sender Name',
          CompanyName: 'Company Name',
          PhoneNumber: '5555555555'
        },
        Address: {
          StreetLines: [
            'Address Line 1'
          ],
          City: 'Collierville',
          StateOrProvinceCode: 'TN',
          PostalCode: '38017',
          CountryCode: 'US'
        }
      },
      Recipient: {
        Contact: {
          PersonName: 'Recipient Name',
          CompanyName: 'Company Receipt Name',
          PhoneNumber: '5555555555'
        },
        Address: {
          StreetLines: [
            'Address Line 1'
          ],
          City: 'Charlotte',
          StateOrProvinceCode: 'NC',
          PostalCode: '28202',
          CountryCode: 'US',
          Residential: false
        }
      },
      ShippingChargesPayment: {
        PaymentType: 'SENDER',
        Payor: {
          ResponsibleParty: {
            AccountNumber: fedex.options.account_number
          }
        }
      },
      LabelSpecification: {
        LabelFormatType: 'COMMON2D',
        ImageType: 'PDF',
        LabelStockType: 'PAPER_4X6'
      },
      PackageCount: '1',
      RequestedPackageLineItems: [{
        SequenceNumber: 1,
        GroupPackageCount: 1,
        Weight: {
          Units: 'LB',
          Value: '50.0'
        },
        Dimensions: {
          Length: 108,
          Width: 5,
          Height: 5,
          Units: 'IN'
        }
      }]
    }
  }, function(err, res) {
    if(err) {
      return console.log(util.inspect(err, {depth: null}));
    }

    console.log(util.inspect(res, {depth: null}));
  });

  /**
   * Pickup
   */
  fedex.pickup({  
    AssociatedAccountNumber: {
      Type: "FEDEX_EXPRESS",
      AccountNumber: fedex.options.account_number
    },
    OriginDetail: {
      UseAccountAddress: false,
      PickupLocation: {
        Contact: {
          PersonName: 'Sender Name',
          CompanyName: 'Company Name',
          PhoneNumber: '5555555555'
        },
        Address: {
          StreetLines: [
            'Address Line 1'
          ],
          City: 'Collierville',
          StateOrProvinceCode: 'TN',
          PostalCode: '38017',
          CountryCode: 'US'
        }
      },
      PackageLocation: "NONE",
      ReadyTimestamp: "2021-12-02T09:00:57.282Z",
      CompanyCloseTime: "16:00:00",        
    },
    PackageCount: '1',
    TotalWeight: {
      Units: "KG",
      Value: "1"
    },
    CarrierCode: "FDXE"
  }, function(err, res) {
    if(err) {
      return console.log(util.inspect(err, {depth: null}));
    }

    console.log(util.inspect(res, {depth: null}));
  });
```

See `example/index.js` for working samples.

## Credits

Originally forked from [mojo5000,fportela-ns,MrBenJ,RyanYocum](https://github.com/thebouqs/fedex-node-client).

## License

(The MIT License)

Copyright 2015 uh-sem-blee, Co. All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.