```
import {Catalog} from "@mfe/cataog";
import {Cart} from "@mfe/cart";

const App = () => {
  const [authToken, setAuthToken] = useState('hagaSDjbscwbjcbwc');
  const [refreshToken, setRefreshToken] = useState('hagaSDjbscwbjcbwc');

  const refreshToken = () => {
    fetch(`<domain>/tokens?refreshToken=${refreshToken}`)
      .then(({ authToken, refreshToken }) => {
        setAuthToken(authToken);
        setRefreshToken(refreshToken);
      })
      .catch(() => {
        console.log('something went wrong')
      });
  }

  return (
    <>
      <Catalog authToken={authToken} onTokenExpiry={refreshToken} />
      <Cart authToken={authToken} onTokenExpiry={refreshToken} />
    </>  
  )
}
```