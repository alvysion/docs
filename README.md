# Invoices API
API to create invoices in your organization.

## Request
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
  "amountExcludingTaxes": 100
  "client": "Client",
  "dueDateDays": 30,
  "externalId": "ExternalId",
  "invoiceDate": "01/01/2020",
  "siret": "888888888888",
  "vat": 20,
}
```
```ts
Interface Invoices = {
  amountExcludingTaxes: number; // HT amount.
  client?: string;  // Client name.
  dueDateDays: number; // Payments at X days. If 0, payment on receipt of invoice.
  externalId: string; // Your own invoice id.
  invoiceDate: string;  // Format: dd/mm/yyyy.
  siret?: string // Client siret
  vat: number;  // TVA amount.
}
```
:warning: At least the **client** or the **siret** must be completed. If the siret is completed, the API will query [gouv API](https://entreprise.data.gouv.fr/api_doc_sirene "API Sirene")'s establishments. If an establishment is found, you will get more information such as address or VAT number. 

## Response

#### Succes 
```json
{
  "statusCode": 200,
  "message": "Income created succefully"
}
```

#### Errors  
- Bad Request
```json
{
    "statusCode": 400,
    "message": "Bad Request"
}
```
- Unauthorized
```json
{
    "statusCode": 401,
    "message": "Unauthorized"
}
```

- Required fields
```json
{
  "statusCode": 400,
  "message": "Error with the following values or fields :",
  "list": {
      "invoiceDate": "required"
  }
}
```
- VAT error
```json
{
    "statusCode": 400,
    "error": "vat percentage must be an official vat, 0 - 2.1 - 5.5 - 10 - 20. Your percentage is equal to 25% !"
}
```
- Server Error
```json
{
    "statusCode": 500,
    "message": "Internal Server Error",
    "error": {
        "clientMessage": "Un problème est survenue lors de la création de cet élement."
    }
}
```
