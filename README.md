# Tripay Client Library

## Installation

```sh
pnpm add tripay-sdk
# or
npm install tripay-sdk
# or
yarn add tripay-sdk
```

## ESM Package.json
If you are using ESM add `"type": "module"` in package.json
```json
{
  "type": "module",
  "dependencies": {
    "tripay-sdk": "^0.1.4"
  }
}
```

# API Docs

## Create Tripay Config
```js
import createTripayConfig from "tripay-sdk"

const tripay = createTripayConfig({
    apiKey: [string | tripay api key],
    privateKey: [string | tripay private api key]
    merchant_code: [string | tripay merchant code]
    isProduction: [boolean | default false or true]
})
```

## Instruction
```js
const instruction = await tripay.instruction({
    code: 'pay code',
    amount: 'amount',
    allow_html: ''
});
```
#### Instruction Function Version
```js
// Contoh function mendapatkan instruksi pembayaran
async function getInstruction() {
  try {
    const instruction = await tripay.instruction({
      code: 'pay_code', // ganti dengan kode pembayaran yang sesuai
      amount: 'amount', // ganti dengan jumlah yang sesuai
      allow_html: ''
    });
    console.log('Payment Instruction:', instruction);
  } catch (error) {
    console.error('Error getting instruction:', error.message);
  }
}
```

## Payment Channel
```js
const paymentChannel = await tripay.paymentChannel();
```
#### Payment Channel Function Version
```js
// Mendapatkan saluran pembayaran yang di terima
async function getPaymentChannel() {
  try {
    const paymentChannel = await tripay.paymentChannel();
    console.log('Payment Channels:', paymentChannel);
  } catch (error) {
    console.error('Error getting payment channels:', error.message);
  }
}
```

## Fee Calculator
```js
const feeCalculator = await tripay.feeCalculator({
    code: 'payment code',
    amount: 'amount'
});
```
#### Fee Calculator Function Version
```js
// Menghitung biaya fee (biaya admin)
async function calculateFee() {
  try {
    const feeCalculator = await tripay.feeCalculator({
      code: 'payment_code', // Ganti dengan kode pembayaran yang sesuai
      amount: 'amount' // Ganti dengan jumlah yang sesuai
    });
    console.log('Fee Calculator:', feeCalculator);
  } catch (error) {
    console.error('Error calculating fee:', error.message);
  }
}
```

# Get Transaction List

## Closed Transactions
Get a *list* of all previous transactions
```js
const transactions = await tripay.transactions({
    page: 'page'
    per_page: 'per page data'
});
```
##### Closed Transactions Function Version
```js

async function getClosedTransactions() {
  try {
    const transactions = await tripay.transactions({
      page: '1', // Ganti dengan nomor halaman yang sesuai
      per_page: '10' // Ganti dengan jumlah data per halaman yang sesuai
    });
    console.log('Closed Transactions:', transactions);
  } catch (error) {
    console.error('Error getting transactions:', error.message);
  }
}
```

##### Open Transactions
```js
const openTransaction = await tripay.openTransactions({
    uuid: 'uuid'
});
```

# Create Transaction

In this section, you will find all the methods and functions necessary to create transactions. These functions will allow you to initiate payment processes and handle various payment methods. Whether you are creating closed transactions or open transactions, you will find the appropriate functions to suit your needs.
> !! Note the environment whether it is in production or simulator state in `createTripayConfig` > `isProduction: [boolean | default false or true]`
## Create Closed Transaction
Make transactions at prices set by merchants easily and safely. "Create Closed Transaction" allows you to complete purchases without the hassle of price negotiations.

> Make a check of any payment method available [here](https://github.com/KiroFyzu/tripay-sdk/tree/main?tab=readme-ov-file#payment-method-list)
```js
const closedTransaction = await tripay.createClosedTransaction({
    method: 'payment method',
    merchant_ref: 'merchant_ref',
    amount: 'amount Transaction',
    customer_name: 'customer name',
    customer_phone: 'customer phone',
    order_items: 'array of item ordered',
    callback_url: 'callback url',
    return_url: 'return_url',
    expired_time: by default 1 hour
});

// Example 
const closedTransaction = await tripay.createClosedTransaction({
    method: 'BRIVA',
    merchant_ref: 'merchant_ref', // Ganti dengan referensi merchant yang sesuai
    amount: '10000',
    customer_name: 'customer name',
    customer_email: 'costumerEmail@domain.com',
    customer_phone: 'customer phone',
    order_items: [
        {
          'sku': 'PRODUK1',
          'name': 'Nama Produk 1',
          'price': 5000,
          'quantity': 1,
          'product_url': 'https://tokokamu.com/product/nama-produk-1',
          'image_url': 'https://tokokamu.com/product/nama-produk-1.jpg'
        },
        {
          'sku': 'PRODUK2',
          'name': 'Nama Produk 2',
          'price': 5000,
          'quantity': 1,
          'product_url': 'https://tokokamu.com/product/nama-produk-2',
          'image_url': 'https://tokokamu.com/product/nama-produk-2.jpg'
        }
      ],
    callback_url: 'https://tokokamu.com/callback',
    return_url: 'https://tokokamu.com/return',
});
```
#### Create Closed Transaction Function Version
```js
// Contoh pembuatan transaksi tertutup
async function createClosedTransaction() {
  try {
    const closedTransaction = await tripay.createClosedTransaction({
      method: 'QRIS2', // Ganti dengan metode pembayaran yang sesuai
      merchant_ref: 'merchant_ref',
      amount: '1000',
      customer_name: 'John Doe',
      customer_email: 'emailpelanggan@domain.com',
      customer_phone: '1234567890',
      order_items: [
      {
        'sku': 'PRODUK1',
        'name': 'Nama Produk 1',
        'price': 500,
        'quantity': 1,
        'product_url': 'https://tokokamu.com/product/nama-produk-1',
        'image_url': 'https://tokokamu.com/product/nama-produk-1.jpg'
      },
      {
        'sku': 'PRODUK2',
        'name': 'Nama Produk 2',
        'price': 500,
        'quantity': 1,
        'product_url': 'https://tokokamu.com/product/nama-produk-2',
        'image_url': 'https://tokokamu.com/product/nama-produk-2.jpg'
      }
    ],
      callback_url: 'https://example.com/callback',
      return_url: 'https://example.com/return'
    });
    console.log('Closed Transaction:', closedTransaction);
  } catch (error) {
    console.error('Error creating closed transaction:', error.message);
  }
}
```

##### Create Open Transaction

```js
const openTransaction = await tripay.createOpenTransaction({
    method: 'payment method'
    merchant_ref: 'merchant_ref',
    customer_name: 'customer name'
});
```

#### Get Transaction Detail

##### Get Closed Transaction Detail
```js
const closedTransactionDetail = await tripay.closedTransactionDetail({
    reference: 'reference number'
});
```

##### Get Open Transaction Detail
```js
const openTransactionDetail = await tripay.openTransactionDetail({
    uuid: 'uuid'
});
```
## Payment Method List
### Closed Transaction
  - MYBVA
  - PERMATAVA
  - BNIVA
  - BRIVA
  - MANDIRIVA
  - BCAVA
  - SMSVA
  - MUAMALATVA
  - CIMBVA
  - SAMPOERNAVA
  - BSIVA
  - DANAMONVA
  - ALFAMART
  - INDOMARET
  - ALFAMIDI
  - OVO
  - QRIS
  - QRIS2
  - QRISC
  - QRISD
  - SHOPEEPAY
### Open Transaction Payment Method List:
  - BNIVAOP
  - HANAVAOP
  - DANAMONOP
  - CIMBVAOP
  - BRIVAOP
  - QRISOP
  - QRISCOP
  - BSIVAOP

### Webhook (Coming Soon)
