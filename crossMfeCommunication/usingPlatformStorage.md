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

## Props
- Available for browsers as well as mobile devices. Local storage for browsers and Async storage for mobile app.
- Less coupling compare to props between the App and mfes but hard to debug which mfe is setting the data.

## Cons
- Not scalable solution. But can be used for small set of data. Always try to namespace the data.
- Never secure.