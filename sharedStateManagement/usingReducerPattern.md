``` js
import {catalogReducer} from "@mfe/catalog";
import {cartReducer} from "@mfe/cart";

rootReducer = combineReducers({ catalog: catalogReducer, cart: cartReducer });

// produces the following state object

{
  catalog: {
    // ... list of products
  },
  cart: {
    // ... list of products added to the cart
  }
}
```

## Pros
- Widely pattern in monolith architecture
- Framework agnostic as redux implementation is available in mostly all the frameworks.

## Cons
- Easy to bloat as all the micro frontends will have access to all the application data.
