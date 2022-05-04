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

## Pros
- Inbuilt solution in browser platform
- Very much close to asynchronous event based architecture in microservices world. You can publish an event and subscribe to it.
- High setup cost but easy to scale.
- Build a generic mechanism which all the mfe teams can follow.

## Cons
- Not achievable in case of mobile micro frontends
- Can work only if the cross communication is within the same page as events need to be subscribed before publishing
- Be careful when using events across pages. The event needs to get unsubscribed on unmount of a Component.