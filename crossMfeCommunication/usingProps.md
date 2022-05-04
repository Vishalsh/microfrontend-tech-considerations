``` js
import {Catalog} from "@mfe/cataog";
import {Cart} from "@mfe/cart";

const App = () => {
  const [productsInCartCount, setProductsInCartCount] = useState(0);

  const addToCart = () => {
    setProductsInCartCount(productsInCartCount + 1);
  }

  return (
    <Catalog onAddToCart={addToCart} />
    <Cart productsCount={productsInCartCount} />
  )
}
```

## Props
- One of the Most well know way in component based architecture.
- Most of the frameworks support this way.
- Rely on frameworks ways to avoid prop drilling issues.

## Cons
- Add a lot of coupling between the Mfes and the container app. 
- Hard to achieve if two MFEs are not using the same framework