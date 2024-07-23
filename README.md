# Ordering-page
<<<<<< Dashboard-components
// src/Dashboard.js
import React, { useEffect, useState } from 'react';
import { db } from './firebase';
import { auth } from './firebase';

const Dashboard = () => {
  const [orders, setOrders] = useState([]);
  const [customerName, setCustomerName] = useState('');
  const [product, setProduct] = useState('');
  const [quantity, setQuantity] = useState(1);
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Check user authentication
    const unsubscribe = auth.onAuthStateChanged(setUser);
    fetchOrders();
    return () => unsubscribe();
  }, []);

  const fetchOrders = async () => {
    const snapshot = await db.collection('orders').get();
    const ordersData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
    setOrders(ordersData);
  };

  const placeOrder = async () => {
    await db.collection('orders').add({
      customerName,
      product,
      quantity,
      status: 'Pending',
      createdAt: new Date()
    });
    fetchOrders();
    setCustomerName('');
    setProduct('');
    setQuantity(1);
  };

  const updateOrderStatus = async (id, status) => {
    await db.collection('orders').doc(id).update({ status });
    fetchOrders();
  };

  const generateProforma = (order) => {
    const proforma = `Proforma Invoice for Order ID: ${order.id}\nCustomer: ${order.customerName}\nProduct: ${order.product}\nQuantity: ${order.quantity}\nStatus: ${order.status}`;
    alert(proforma);
  };

  const generateInvoice = (order) => {
    const invoice = `Invoice for Order ID: ${order.id}\nCustomer: ${order.customerName}\nProduct: ${order.product}\nQuantity: ${order.quantity}\nStatus: ${order.status}`;
    alert(invoice);
  };

  return (
    <div>
      <h1>Online Purchase Order Dashboard</h1>
      {user ? (
        <>
          <h2>Place Order</h2>
          <input value={customerName} onChange={(e) => setCustomerName(e.target.value)} placeholder="Customer Name" />
          <input value={product} onChange={(e) => setProduct(e.target.value)} placeholder="Product" />
          <input type="number" value={quantity} onChange={(e) => setQuantity(e.target.value)} placeholder="Quantity" />
          <button onClick={placeOrder}>Place Order</button>

          <h2>Orders</h2>
          <ul>
            {orders.map(order => (
              <li key={order.id}>
                {order.customerName} ordered {order.quantity} of {order.product} - Status: {order.status}
                <button onClick={() => updateOrderStatus(order.id, 'Completed')}>Complete Order</button>
                <button onClick={() => generateProforma(order)}>Generate Proforma</button>
                <button onClick={() => generateInvoice(order)}>Generate Invoice</button>
              </li>
            ))}
          </ul>
        </>
      ) : (
        <p>Please log in to access the dashboard.</p>
      )}
    </div>
  );
}

export default Dashboard;
=======
<<<<<< App.js
// src/App.js
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Dashboard from './Dashboard';
import Login from './Login';
import Signup from './Signup';

const App = () => {
  return (
    <Router>
      <Switch>
        <Route path="/" exact component={Dashboard} />
        <Route path="/login" component={Login} />
        <Route path="/signup" component={Signup} />
      </Switch>
    </Router>
  );
}

export default App;
======
// src/firebase.js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);

const db = firebase.firestore();
const auth = firebase.auth();

export { db, auth };
>>>>>> main
>>>>>> main
