```  js
class MessageBus {
  private publishedEventRegistry = {};

  publishEvent(eventName, data) {
    const registeredEvent = this.publishedEventRegistry[eventName];

    this.publishedEventRegistry = {
      ...this.publishedEventRegistry,
      [eventName]: {
        ...registeredEvent,
        name: eventName,
        data: data
      }
    };
    this.publishedEventRegistry[eventName].handlers.forEach(handler => {
      handler(this.publishedEventRegistry[eventName].data);
    });
  }

  subscribeEvent(eventName, handler) {
    const registeredEvent = this.publishedEventRegistry[eventName];

    this.publishedEventRegistry = {
      ...this.publishedEventRegistry,
      [eventName]: {
        ...registeredEvent,
        name: eventName,
        handlers: [...registeredEvent.handlers, handler]
      }
    };
  }

  unsubscribe(eventName) {
    this.publishedEventRegistry = {
      ...this.publishedEventRegistry,
      [eventName]: undefined
    }
  }
}


// App.js
import {Catalog} from "@mfe/cataog";
import {Cart} from "@mfe/cart";
import {MessageBus} from "@mfe-message-bus";

cont App = () => {
  const messageBus = new MessageBus();

  return (
    <>
      <Catalog messageBus={messageBus} />
      <Cart messageBus={messageBus} />
    </>
  )  
}


// Catalog MFE
const Catalog = ({ messageBus }) => {
  const addToCart = () => {    
    // get initial count first
    const [productsInCartCount, setProductsInCartCount] = useState(initialCount || 0);

    messageBus.publishEvent('ADD_TO_CART', { count: productsInCartCount + 1 });
  }

  return (
    <Product onAddToCart={addToCart} />
  )
}

// Cart MFE
const Cart = ({ messageBus }) => {
  // get initial count first
  const [productsInCartCount, setProductsInCartCount] = useState(initialCount || 0);

  useEffect(() => {
    const listener = (event) => {
      setProductsInCartCount(event.data.count);
    }

    messageBus.subscribeEvent('ADD_TO_CART', listener);

    return () => {
      messageBus.unsubscribeEvent('ADD_TO_CART', listener);
    }
  }, []);

  return (
    <NumberOfProductsAdded count={productsInCartCount} />
  )
};
```