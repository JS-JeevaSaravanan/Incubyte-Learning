```js
  

export class MarketableLocationDetailBing {

id: string;

name: string;

street1: string;

street2: string;

city: string;

state: string;

country: string = US_COUNTRY_CODE;

zip: string;

phone: string;

primaryCategory: string;

additionalCategories: string[];

email: string;

website: string;

hoursOfOperation: string[];

coordinates?: {

lat: string;

lon: string;

};

  

public static buildFromMarketableLocation(marketableLocation: MarketableLocation): MarketableLocationDetailBing {

const marketableLocationDetailBing: MarketableLocationDetailBing = new MarketableLocationDetailBing();

}


/*
  

MarketableLocationDetailBing {

country: 'US_COUNTRY_CODE', // Assuming US_COUNTRY_CODE is defined elsewhere

id: undefined,

name: undefined,

street1: undefined,

street2: undefined,

city: undefined,

state: undefined,

zip: undefined,

phone: undefined,

primaryCategory: undefined,

additionalCategories: undefined,

email: undefined,

website: undefined,

hoursOfOperation: undefined,

coordinates: { lat: undefined, lon: undefined }

}
*/
```