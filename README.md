# Invoices API
API to create invoices in your organization.

<details>
<br>
<summary>Invoices request (Deprecated, will be replaced by Incomes request)</summary>
Your request must have the following informations:

* Headers  
`Authorization: Bearer your_token`

* Method  
`POST`
  
  * Path
`/sales/incomes`

* Content-Type  
`application/json`

* Body  
```json
{
  "addressRaw": "9 rue Lacuée, 75012 Paris",
  "address": {
    "city": "Paris",
    "country": "France",
    "postalCode": "75012",
    "street": "9 rue Lacuée",
  },
  "amountExcludingTaxes": 100,
  "client": "Client",
  "dueDateDays": 30,
  "externalId": "ExternalId",
  "invoiceDate": "01/01/2020",
  "reference": "reference",
  "siret": "888888888888",
  "vat": 20,
  "vatNumber": "FR99999999999",
}
```  
```ts
Interface Address = {
  city: string;
  country: string; 
  postalCode: string;
  street: string;
}

Interface Invoices = {
  addressRaw: string; // Address to String with street, postal code & city
  address: Address; // Client address
  amountExcludingTaxes: number; // HT amount.
  client: string;  // Client name.
  dueDateDays: number; // Payments at X days. If 0, payment on receipt of invoice.
  externalId: string; // Your own invoice id.
  externalNumber?: string; // Your own invoice number.
  invoiceDate: string;  // Format: yyyy/mm/dd.
  reference?: string; // Invoice's references, ex: Consultant name etc.
  siret?: string // Client siret
  vat: number;  // TVA amount.
  vatNumber?: string; // Client VAT number
}
```

:warning: A least one of __addressRaw__ or __address__ fields are required.

## Response

#### Succes 
```json
{
  "statusCode": 200,
  "message": "OK"
}
```
#### Errors  
- Bad Request :
```json
{
    "statusCode": 400,
    "message": "Bad Request"
}
```
<br>

- Unauthorized :
```json
{
    "statusCode": 401,
    "message": "Unauthorized"
}
```
<br>

- VAT error :
```json
{
    "statusCode": 400,
    "error": "vat percentage must be an official vat, 0 - 2.1 - 5.5 - 10 - 20. Your percentage is equal to 25% !"
}
```
<br>

- Server Error :
```json
{
    "statusCode": 500,
    "message": "Internal Server Error",
}
```
<br>
</details>

<details>
<br>
<summary>Incomes request</summary>
Your request must have the following informations:

* Headers  
  `Authorization: Bearer your_token`

* Method  
  `POST`

* Path
 `/sales/uploadable-incomes`

* Content-Type  
  `multipart/mixed`

* Multipart Body

  - addressRaw: string
  - amountExcludingTaxes: string
  - client: string
  - city: string
  - country: string
  - currencyCode: string
  - dueDateDays: string
  - externalId: string
  - externalNumber: string
  - file: File
  - invoiceDate: string
  - postalCode: string
  - reference: string
  - siret: string
  - street: string
  - vat: string
  - vatNumber: string

```ts
interface IncomeInvoice {
    addressRaw: string; // Address to String with street, postal code & city
    amountExcludingTaxes: string; // HT amount.
    client: string;  // Client name.
    city: string; // Client city 
    country: string; // Client country 
    currencyCode: string;  // currencyCode EUR USD
    dueDateDays: string; // Payments at X days. If 0, payment on receipt of invoice.
    externalId: string; // Your own invoice id.
    externalNumber?: string; // Your own invoice number.
    file?: File, // Invoice pdf file to upload
    invoiceDate: string;  // Format: yyyy/mm/dd.
    postalCode: string; // Client postalCode 
    reference?: string; // Invoice's references, ex: Consultant name etc.
    siret?: string // Client siret
    street: string; // Client street 
    vat: string;  // TVA amount.
    vatNumber?: string; // Client VAT number
}
```

Attention le fichier ne doit pas peser plus de 15MO.


## Response

#### Succes
```json
{
  "statusCode": 200,
  "message": "OK"
}
```
#### Errors
- Bad Request :
```json
{
    "statusCode": 400,
    "message": "Bad Request"
}
```
<br>

- Unauthorized :
```json
{
    "statusCode": 401,
    "message": "Unauthorized"
}
```
<br>

- VAT error :
```json
{
    "statusCode": 400,
    "error": "vat percentage must be an official vat, 0 - 2.1 - 5.5 - 10 - 20. Your percentage is equal to 25% !"
}
```
<br>

- Server Error :
```json
{
    "statusCode": 500,
    "message": "Internal Server Error",
}
```
<br>
</details>
