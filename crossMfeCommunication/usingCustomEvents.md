``` js
// Event Library
let eventRegistry = {}
const event = {
  create: (eventName, data) => {
    const newEvent = new CustomEvent(eventName, data);
    eventRegistry = {
      ...eventRegistry,
      [eventName]: newEvent
    }
    return newEvent;
  },

  dispatch: (eventName) => {
    window.dispatchEvent(eventName)
  },

  subscribe: (eventName, listener) => {
    window.addEventListener(eventName, listener)
  },

  unsubscribe: (eventName, listener) => {
    window.removeEventListener(eventName, listener)
  }
}

// Catalog MFE
import {event} from "@event-library";

const Catalog = () => {
  // fetch initial count first
  const [productsInCartCount, setProductsInCartCount] = useState(initialCount || 0);

  const addToCart = () => {    
    const addToCartEvent = event.create('ADD_TO_CART', { count: productsInCartCount + 1 });
    event.dispatch(addToCartEvent);
  }

  return (
    <Product onAddToCart={addToCart} />
  )
}

// Cart MFE
import {storage} from "@mfe-utils";

const Cart = () => {
  // fetch initial count first
  const [productsInCartCount, setProductsInCartCount] = useState(initialCount || 0);

  useEffect(() => {
    const listener = (event) => {
      setProductsInCartCount(event.data.count);
    }

    event.subscribe('ADD_TO_CART', listener);

    return () => {
      event.unsubscribe('ADD_TO_CART', listener);
    }
  }, []);

  return (
    <NumberOfProductsAdded count={productsInCartCount} />
  )
};
```