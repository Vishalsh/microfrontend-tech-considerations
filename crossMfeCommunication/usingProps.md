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