# XRP-CODE-FOR-RENTAL-PAYMENT
This code helps to adequately track out payment successfully
import React from 'react';

function ApartmentListing({ apartment }) {
  return (
    <div className="apartment-listing">
      <img src={apartment.image} alt={apartment.name} />
      <h3>{apartment.name}</h3>
      <p>{apartment.description}</p>
      <p>Price: ${apartment.price} per month</p>
      <button>Make Payment</button>
    </div>
  );
}

function ApartmentList({ apartments }) {
  return (
    <div className="apartment-list">
      {apartments.map((apartment, index) => (
        <ApartmentListing key={index} apartment={apartment} />
      ))}
    </div>
  );
}

const apartments = [
  {
    name: 'Cozy Studio Apartment',
    image: 'studio.jpg',
    description: 'A beautiful studio apartment in the heart of the city.',
    price: 1000
  },
  {
    name: 'Spacious Loft',
    image: 'loft.jpg',
    description: 'A luxurious loft with a stunning view.',
    price: 2000
  }
];

function App() {
  return (
    <div className="app">
      <h1>Available Apartments</h1>
      <ApartmentList apartments={apartments} />
    </div>
  );
}

export default App;


const RippleAPI = require('ripple-lib').RippleAPI;

// Connect to the XRPL network
const api = new RippleAPI({
  server: 'wss://s1.ripple.com' // Use a different server if needed
});

// Set your XRPL account credentials
const address = 'your_xrpl_address';
const secret = 'your_xrpl_secret';

// Function to send a payment
async function sendPayment(destination, amount) {
  try {
    // Connect to the XRPL network
    await api.connect();

    // Prepare the payment transaction
    const preparedTx = await api.prepareTransaction({
      TransactionType: 'Payment',
      Account: address,
      Destination: destination,
      Amount: api.xrpToDrops(amount)
    });

    // Sign the transaction with your secret key
    const signedTx = api.sign(preparedTx.txJSON, secret);

    // Submit the signed transaction to the XRPL network
    const result = await api.submit(signedTx.signedTransaction);
    
    console.log('Payment successful:', result);
  } catch (error) {
    console.error('Error sending payment:', error);
  } finally {
    // Disconnect from the XRPL network
    await api.disconnect();
  }
}

// Usage example:
sendPayment('destination_xrpl_address', 10); // Send 10 XRP to the specified destination address
