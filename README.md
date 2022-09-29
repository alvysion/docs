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

<details>
<br>
<summary>Get invoices</summary>
Your request must have the following informations:

* Headers  
  `Authorization: Bearer your_token`

* Method  
  `GET`

* Path
 `/sales/invoices?limit=15&skip=0`

* Content-Type  
  `application/json`

```ts
limit: number; // The number of invoice you want per page
skip: number; // The number of element on previous page (if the limit is 15 for page 2 you need to pass 15 for page 3 30 etc...)
```


## Response

```ts
interface Invoices {
  amountExcludingTaxes: number, // HT amount
  amountIncludingTaxes: number, // TTC amount
  currencyCode: string, // currencyCode EUR USD
  invoiceDate: string,
  invoiceFileName: string,
  number: string,
  organizationId: string,
  vat: number, // VAT amount
  _id: string, // invoice id to give to GET invoice or download invoice
  client: {
      address: {
          city: string,
          country: string,
          postalCode: string,
          street: string
      },
      companyName: string,
      contact: {
          email: string,
          firstname: string,
          name: string,
          phone: string
      },
      dueDateDays: number,
      vatNumber: string,
  },
  clientId string,
  dueDate: string,
  dueDateDays: number,
  items: {
      _id: string, // Item Id
      articleCodeRef: string,
      description: string,
      discount: number,
      quantity: number,
      unitPrice: number,
      vat: number,
      vatKind: string,
      vatPercentage: number
  }[],
  organization: {
      address: {
          city: string,
          country: string,
          postalCode: string,
          street: string
      },
      name: string,
      bank: {
          bic: string,
          iban: string,
          name: string
      },
      email: string,
      legal: {
          capital: string,
          corporateFormShort: string,
          naf: string,
          rcs: string,
          siret: string,
          vatNumber: string
      },
      phone: string,
  },
  status: string,
  language: string,
  reconciled: boolean,
  kind: "IncomeCreditNote" | "Income" | "Deposit"
}[]
```

#### Succes
```json
{
  "statusCode": 200,
  "body": Invoices[]
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

<details>
<br>
<summary>Get invoice</summary>
Your request must have the following informations:

* Headers  
  `Authorization: Bearer your_token`

* Method  
  `GET`

* Path
 `/sales/invoice/:invoiceId`

* Content-Type  
  `application/json`

## Response

```ts
interface Invoice {
  amountExcludingTaxes: number, // HT amount
  amountIncludingTaxes: number, // TTC amount
  currencyCode: string, // currencyCode EUR USD
  invoiceDate: string,
  invoiceFileName: string,
  number: string,
  organizationId: string,
  vat: number, // VAT amount
  _id: string, // invoice id to give to GET invoice or download invoice
  client: {
      address: {
          city: string,
          country: string,
          postalCode: string,
          street: string
      },
      companyName: string,
      contact: {
          email: string,
          firstname: string,
          name: string,
          phone: string
      },
      dueDateDays: number,
      vatNumber: string,
  },
  clientId string,
  dueDate: string,
  dueDateDays: number,
  items: {
      _id: string, // Item Id
      articleCodeRef: string,
      description: string,
      discount: number,
      quantity: number,
      unitPrice: number,
      vat: number,
      vatKind: string,
      vatPercentage: number
  }[],
  organization: {
      address: {
          city: string,
          country: string,
          postalCode: string,
          street: string
      },
      name: string,
      bank: {
          bic: string,
          iban: string,
          name: string
      },
      email: string,
      legal: {
          capital: string,
          corporateFormShort: string,
          naf: string,
          rcs: string,
          siret: string,
          vatNumber: string
      },
      phone: string,
  },
  status: string,
  language: string,
  reconciled: boolean,
  kind: "IncomeCreditNote" | "Income" | "Deposit"
}
```

#### Succes
```json
{
  "statusCode": 200,
  "body": Invoice
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

<details>
<br>
<summary>Get invoice file</summary>
Your request must have the following informations:

* Headers  
  `Authorization: Bearer your_token`

* Method  
  `GET`

* Path
 `/sales/invoice/:invoiceId/download`

## Response

#### Succes
```json
{
  "statusCode": 200,
  "body": PDF blob of invoice file
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
