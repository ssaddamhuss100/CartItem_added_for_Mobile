# CartItem_added_for_Mobile
 Update Your Cart Item Structure
In the App.js (or wherever your cart items are being stored), you need to add a description field to each product in your list.

Updated App.js:
You should modify the initial productList and ensure that every product has a description field.

javascript
Copy code
import React, { useState } from 'react';
import ProductList from './Components/ProductList';
import Footer from './Components/Footer';
import AddItem from './Components/AddItem';
import Navbar from './Components/Navbar';

function App() {
  // Create the product list with initial values, now including descriptions
  const initialProductList = [
    { name: "IPhone 10s Max", price: 140434, quantity: 0, description: "A high-end smartphone with a large display and great performance." },
    { name: "Real Me 10X", price: 10000, quantity: 2, description: "Affordable smartphone with good camera and battery life." },
  ];

  const [productList, setProductList] = useState(initialProductList);
  const [totalAmount, setTotalAmount] = useState(0);

  // Increment the quantity of a product
  const incrementQuantity = (index) => {
    const newProductList = [...productList];
    if (newProductList[index].quantity < 10) {
      let newTotalAmount = totalAmount;
      newProductList[index].quantity++;
      newTotalAmount += newProductList[index].price;
      setTotalAmount(newTotalAmount);
      setProductList(newProductList);
    }
  };

  // Decrement the quantity of a product
  const decrementQuantity = (index) => {
    const newProductList = [...productList];
    if (newProductList[index].quantity > 1) {
      let newTotalAmount = totalAmount;
      newProductList[index].quantity--;
      newTotalAmount -= newProductList[index].price;
      setProductList(newProductList);
      setTotalAmount(newTotalAmount);
    }
  };

  // Reset all product quantities to 0
  const resetQuantity = () => {
    let newProductList = [...productList];
    newProductList.map((product) => {
      product.quantity = 0;
    });
    setProductList(newProductList);
    setTotalAmount(0);
  };

  // Remove item from the product list
  const removeItem = (index) => {
    let newProductList = [...productList];
    let removedProduct = newProductList.splice(index, 1)[0];

    let newTotalAmount = newProductList.reduce((total, product) => {
      return total + (product.price * product.quantity);
    }, 0);

    setProductList(newProductList);
    setTotalAmount(newTotalAmount);
  };

  // Function to add item
  const addItem = (name, price, description) => {
    let newProductList = [...productList];
    newProductList.push({
      name: name,
      price: price,
      quantity: 0,
      description: description,  // New description field
    });
    setProductList(newProductList);
  };

  return (
    <>
      <Navbar />
      <main className="container mt-5">
        <AddItem addItem={addItem} />
      </main>

      <ProductList
        productList={productList}
        incrementQuantity={incrementQuantity}
        decrementQuantity={decrementQuantity}
        removeItem={removeItem}
      />

      <Footer totalAmount={totalAmount} handleResetButton={resetQuantity} />
    </>
  );
}

export default App;
2. Modify AddItem Component to Include a Description
In the AddItem.js component, you need to update the form to accept a description field. The user will be able to input a description along with the name and price of the product.

Updated AddItem.js:
javascript
Copy code
import React, { useState } from "react";

export default function AddItem({ addItem }) {
  // Local state for name, price, and description
  const [name, setName] = useState("");
  const [price, setPrice] = useState(0);
  const [description, setDescription] = useState("");  // New state for description

  // Handler for name change
  const handleNameChange = (e) => {
    setName(e.target.value);
  };

  // Handler for price change
  const handlePriceChange = (e) => {
    setPrice(e.target.value);
  };

  // Handler for description change
  const handleDescriptionChange = (e) => {
    setDescription(e.target.value);
  };

  // Submit handler when adding an item
  const handleSubmit = () => {
    if (name && price > 0 && description) {
      addItem(name, parseFloat(price), description); // Pass description along with name and price
      setName("");  // Reset name
      setPrice(0);  // Reset price
      setDescription("");  // Reset description
    } else {
      alert("Please fill out all fields with valid data!");
    }
  };

  return (
    <form className="row mt-3 mx-3">
      <div className="form-group col-12 col-md-6 col-lg-4 mb-2">
        <label htmlFor="name">Product Name</label>
        <input
          type="text"
          className="form-control"
          id="name"
          placeholder="Enter name here"
          value={name}
          onChange={handleNameChange}
        />
      </div>

      <div className="form-group col-12 col-md-6 col-lg-4 mb-2">
        <label htmlFor="Price">Product Price</label>
        <input
          type="number"
          className="form-control"
          id="Price"
          value={price}
          onChange={handlePriceChange}
          placeholder="Enter amount of the item here"
        />
      </div>

      <div className="form-group col-12 col-md-6 col-lg-4 mb-2">
        <label htmlFor="description">Product Description</label>
        <textarea
          className="form-control"
          id="description"
          placeholder="Enter product description here"
          value={description}
          onChange={handleDescriptionChange}
        />
      </div>

      <button
        type="button"
        className="btn btn-outline-success col-12 col-md-6 col-lg-4 mb-2"
        onClick={handleSubmit}
      >
        Add Item
      </button>
    </form>
  );
}
3. Display Description in ProductList
In the ProductList component, you will need to update the code to show the description of each item in the list.

Updated ProductList.js (example):
javascript
Copy code
import React from 'react';

export default function ProductList({ productList, incrementQuantity, decrementQuantity, removeItem }) {
  return (
    <div className="row">
      {productList.map((product, index) => (
        <div key={index} className="col-12 col-md-6 col-lg-4 mb-4">
          <div className="card">
            <div className="card-body">
              <h5 className="card-title">{product.name}</h5>
              <p className="card-text">{product.description}</p>  {/* Displaying description */}
              <p className="card-text">Price: ₹{product.price}</p>
              <p className="card-text">Quantity: {product.quantity}</p>

              <button onClick={() => incrementQuantity(index)} className="btn btn-success">+</button>
              <button onClick={() => decrementQuantity(index)} className="btn btn-danger">-</button>
              <button onClick={() => removeItem(index)} className="btn btn-warning">Remove</button>
            </div>
          </div>
        </div>
      ))}
    </div>
  );
}
Summary of Changes:
In App.js:

Added description to each product object.
Updated addItem to accept and pass the description along with name and price.
In AddItem.js:

Added a description input field.
Managed state for name, price, and description with useState.
Passed the description value when calling addItem to add a new product.
In ProductList.js:

Displayed the description for each item in the list.
Now, every product will have a name, price, and description, and you'll be able to add items to the cart with full details.
