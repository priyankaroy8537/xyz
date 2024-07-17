
import axios from 'axios';
import React, { useState } from 'react';

const IssueCreditCard= ({ customerId }) => {
  const [productId, setProductId] = useState('');
  const [maker, setMaker] = useState('');
  const [checker, setChecker] = useState('');
  const [creditCard, setCreditCard] = useState(null);
  const [error, setError] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post(`http://localhost:8080/api/customers/${customerId}/issueCreditCard`, null, {
        params: {
          productId: productId,
          maker: maker,
          checker: checker
        }
      });
      setCreditCard(response.data);
      setError('');
    } catch (error) {
      console.error('Error issuing credit card:', error);
      setCreditCard(null);
      setError('Failed to issue credit card');
    }
  };

  return (
    <div>
      <h1>Credit Card details</h1>
      {creditCard ? (
        <div>
          <h2>Credit Card Details</h2>
          <p><strong>Credit Card Number:</strong> {creditCard.creditcardNumber}</p>
          <p><strong>Expiry Date:</strong> {creditCard.creditcardExpiry}</p>
          <p><strong>CVV:</strong> {creditCard.cvv}</p>
          <p><strong>Credit Limit:</strong> {creditCard.creditLimit}</p>
          <p><strong>Status:</strong> {creditCard.Creditcardstatus}</p>
          <p><strong>Maker:</strong> {creditCard.maker}</p>
          <p><strong>Checker:</strong> {creditCard.checker}</p>
          <p><strong>Daily Expense:</strong> {creditCard.dailyExpence}</p>
        </div>
      ) : (
        <form onSubmit={handleSubmit}>
          <div>
            <label>Customer ID:</label>
            <input
              type="text"
              value={customerId}
              readOnly
            />
          </div>
          <div>
            <label>Product ID:</label>
            <input
              type="text"
              value={productId}
              onChange={(e) => setProductId(e.target.value)}
              required
            />
          </div>
          <div>
            <label>Maker:</label>
            <input
              type="text"
              value={maker}
              onChange={(e) => setMaker(e.target.value)}
              required
            />
          </div>
          <div>
            <label>Checker:</label>
            <input
              type="text"
              value={checker}
              onChange={(e) => setChecker(e.target.value)}
              required
            />
          </div>
          <button type="submit"> Credit Card Details</button>
        </form>
      )}
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  );
};

export default IssueCreditCard;
