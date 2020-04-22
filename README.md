# Invoices API
API to create invoices in your organization.

<details>
<br>
<summary>Invoices request</summary>
Your request must have the following informations:

* Headers  
`Authorization: Bearer your_token`

* Method  
`POST`

* Content-Type  
`application/json`

* Body  
```json
{
  "address": {
    "city": "city",
    "country": "country",
    "postalCode": "postalCode",
    "street": "street",
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
  address: Address; // Client address
  amountExcludingTaxes: number; // HT amount.
  client: string;  // Client name.
  dueDateDays: number; // Payments at X days. If 0, payment on receipt of invoice.
  externalId: string; // Your own invoice id.
  invoiceDate: string;  // Format: yyyy/mm/dd.
  reference?: string; // Invoice's references, ex: Consultant name etc.
  siret?: string // Client siret
  vat: number;  // TVA amount.
  vatNumber?: string; // Client VAT number
}
```

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
<summary>Credit note request</summary>
<br>
Your request must have the following informations:

* Headers  
`Authorization: Bearer your_token`

* Method  
`POST`

* Content-Type  
`application/json`

* Body  
```json
{
  "amountExcludingTaxes": 100,
  "refIncome": "refIncome",
  "invoiceDate": "01/01/2020",
}
```
```ts
Interface Invoices = {
  amountExcludingTaxes: number; // HT amount.
  refIncome: string; // Your own income id.
  invoiceDate: string;  // Format: yyyy/mm/dd.
}
``` 

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

- Server Error :
```json
{
    "statusCode": 500,
    "message": "Internal Server Error",
}
```
<br>
</details>
