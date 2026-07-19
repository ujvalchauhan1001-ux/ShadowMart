# ShadowMart# ShadowMart

A premium modern e-commerce platform built with:

- Next.js
- React
- Tailwind CSS
- Node.js
- Express.js
- MongoDB
- JWT Authentication
- Razorpay
- Cloudinary

## Features

- User Authentication
- Product Catalog
- Cart & Wishlist
- Admin Dashboard
- Order Management
- Coupon System
- Payment Gateway
- Responsive Design

Author: Ujvalnode_modules
.next
.env
.env.local
dist
build
coverage
.vscode
.DS_Store{
  "name": "shadowmart",
  "private": true,
  "workspaces": [
    "client",
    "server"
  ],
  "scripts": {
    "client": "npm run dev --workspace client",
    "server": "npm run dev --workspace server",
    "dev": "concurrently \"npm run client\" \"npm run server\""
  },
  "devDependencies": {
    "concurrently": "^9.0.1"
  }
}
src/
 ├── context/
 │     CartContext.jsx
 ├── pages/
 │     ProductDetails.jsx
 │     Cart.jsx
 └── components/
       ProductCard.jsx (Update)import { createContext, useContext, useEffect, useState } from "react";

const CartContext = createContext();

export const useCart = () => useContext(CartContext);

export default function CartProvider({ children }) {

const [cart,setCart]=useState(()=>{
const saved=localStorage.getItem("shadow-cart");
return saved?JSON.parse(saved):[];
});

useEffect(()=>{
localStorage.setItem("shadow-cart",JSON.stringify(cart));
},[cart]);

const addToCart=(product)=>{

const exist=cart.find(i=>i.id===product.id);

if(exist){

setCart(cart.map(i=>

i.id===product.id
?{...i,qty:i.qty+1}
:i

));

}else{

setCart([...cart,{...product,qty:1}]);

}

};

const removeFromCart=id=>{
setCart(cart.filter(i=>i.id!==id));
};

const increase=id=>{
setCart(cart.map(i=>i.id===id?{...i,qty:i.qty+1}:i));
};

const decrease=id=>{

setCart(cart.map(i=>

i.id===id

?{...i,qty:Math.max(1,i.qty-1)}

:i

));

};

return(

<CartContext.Provider value={{

cart,

addToCart,

removeFromCart,

increase,

decrease

}}>

{children}

</CartContext.Provider>

);

}import { useCart } from "../context/CartContext";

export default function Cart(){

const {cart,increase,decrease,removeFromCart}=useCart();

const total=cart.reduce((a,b)=>a+b.price*b.qty,0);

return(

<div className="container py-5">

<h2>Your Cart</h2>

{cart.length===0&&<h4>Cart Empty</h4>}

{cart.map(item=>(

<div
key={item.id}
className="card p-3 mb-3 d-flex flex-row justify-content-between align-items-center"
>

<div>

<h5>{item.title}</h5>

<p>₹{item.price}</p>

</div>

<div>

<button
className="btn btn-dark"
onClick={()=>decrease(item.id)}
>

-

</button>

<span className="mx-3">{item.qty}</span>

<button
className="btn btn-dark"
onClick={()=>increase(item.id)}
>

+

</button>

</div>

<button
className="btn btn-danger"
onClick={()=>removeFromCart(item.id)}
>

Remove

</button>

</div>

))}

<h3>Total ₹{total}</h3>

</div>

);

}import { useParams } from "react-router-dom";
import products from "../data/products";
import { useCart } from "../context/CartContext";

export default function ProductDetails(){

const {id}=useParams();

const product=products.find(i=>i.id===Number(id));

const {addToCart}=useCart();

return(

<div className="container py-5">

<div className="row">

<div className="col-md-6">

<img
src={product.image}
className="img-fluid"
/>

</div>

<div className="col-md-6">

<h2>{product.title}</h2>

<h3>₹{product.price}</h3>

<p>{product.description}</p>

<button
className="btn btn-primary"
onClick={()=>addToCart(product)}
>

Add To Cart

</button>

</div>

</div>

</div>

);import { useParams } from "react-router-dom";
import products from "../data/products";
import { useCart } from "../context/CartContext";

export default function ProductDetails(){

const {id}=useParams();

const product=products.find(i=>i.id===Number(id));

const {addToCart}=useCart();

return(

<div className="container py-5">

<div className="row">

<div className="col-md-6">

<img
src={product.image}
className="img-fluid"
/>

</div>

<div className="col-md-6">

<h2>{product.title}</h2>

<h3>₹{product.price}</h3>

<p>{product.description}</p>

<button
className="btn btn-primary"
onClick={()=>addToCart(product)}
>

Add To Cart

</button>

</div>

</div>

</div>

);

}import { Link } from "react-router-dom";

<Link to={`/product/${product.id}`}>

<button className="btn btn-dark w-100">

View Product

</button>

</Link>import CartProvider from "./context/CartContext";

<CartProvider>

<App/>

</CartProvider><Route path="/cart" element={<Cart/>}/>

<Route path="/product/:id" element={<ProductDetails/>}/>  shadowmart/
│
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── context/
│   ├── pages/
│   ├── admin/
│   ├── firebase/
│   ├── hooks/
│   ├── services/
│   ├── data/
│   ├── css/
│   ├── App.jsx
│   └── main.jsx
│
├── package.json
├── vite.config.js
└── README.mdimport { createContext, useContext, useEffect, useState } from "react";

const WishlistContext = createContext();

export const useWishlist = () => useContext(WishlistContext);

export default function WishlistProvider({ children }) {
  const [wishlist, setWishlist] = useState(() => {
    const saved = localStorage.getItem("shadow-wishlist");
    return saved ? JSON.parse(saved) : [];
  });

  useEffect(() => {
    localStorage.setItem("shadow-wishlist", JSON.stringify(wishlist));
  }, [wishlist]);

  const toggleWishlist = (product) => {
    const exists = wishlist.find((item) => item.id === product.id);

    if (exists) {
      setWishlist(wishlist.filter((item) => item.id !== product.id));
    } else {
      setWishlist([...wishlist, product]);
    }
  };

  const isWishlisted = (id) => {
    return wishlist.some((item) => item.id === id);
  };

  return (
    <WishlistContext.Provider
      value={{
        wishlist,
        toggleWishlist,
        isWishlisted,
      }}
    >
      {children}
    </WishlistContext.Provider>
  );
}import { useWishlist } from "../context/WishlistContext";

export default function Wishlist() {
  const { wishlist } = useWishlist();

  return (
    <div className="container py-5">
      <h2>My Wishlist</h2>

      {wishlist.length === 0 ? (
        <h5>No products added.</h5>
      ) : (
        <div className="row">
          {wishlist.map((item) => (
            <div className="col-md-3 mb-4" key={item.id}>
              <div className="card h-100">
                <img
                  src={item.image}
                  className="card-img-top"
                  alt={item.title}
                />

                <div className="card-body">
                  <h5>{item.title}</h5>
                  <p>₹{item.price}</p>
                </div>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}export default function SearchBar({
search,
setSearch
}){

return(

<input
className="form-control my-4"
placeholder="Search Products..."
value={search}
onChange={(e)=>setSearch(e.target.value)}
/>

);

}<select
className="form-select"

onChange={(e)=>setCategory(e.target.value)}
>

<option value="">
All Categories
</option>

<option value="Electronics">
Electronics
</option>

<option value="Fashion">
Fashion
</option>

<option value="Shoes">
Shoes
</option>

<option value="Mobiles">
Mobiles
</option>

</select><input

type="range"

min="100"

max="100000"

step="100"

onChange={(e)=>setPrice(e.target.value)}

/>
