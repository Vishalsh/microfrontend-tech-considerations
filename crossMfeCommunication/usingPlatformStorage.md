``` js
// storage util
const storage = {
  getItem: (item) => {
    JSON.parse(localStorage.getItem(item));
  }

  setItem: (item, value) => {
    JSON.stringify(localStorage.setItem(item, value));
  }
}

// Catalog MFE
import {storage} from "@mfe-utils";

const Catalog = () => {
  const addToCart = () => {    
    const productsInCartCount = storage.getItem('productsInCartCount');

    storage.setItem('productsInCartCount', productsInCartCount + 1);
  }

  return (
    <Product onAddToCart={addToCart} />
  )
}

// Cart MFE
import {storage} from "@mfe-utils";

const Cart = () => {
  const productsInCartCount = storage.getItem('productsInCartCount');
  
  return (
    <NumberOfProductsAdded count={productsInCartCount} />
  )
};
```