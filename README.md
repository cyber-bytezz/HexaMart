# **QuitQ E-Commerce Project - Full Technical & Logical Q&A Cheat Sheet**  
**Version:** 1.0  
**Prepared By:** [Your Name]  
**Date:** [Current Date]  

---

## **1. Project Overview & Purpose**  
### **Q1: What is QuitQ E-Commerce and why was it built?**  
**A:** QuitQ E-Commerce is an online shopping platform that allows users to browse products, add them to their cart, checkout with payment methods, and track their orders. It also has an admin panel for product and order management. It was built to **demonstrate a full-stack e-commerce application** using FastAPI (Backend) and React (Frontend).

### **Q2: What problem does this project solve?**  
**A:** QuitQ E-Commerce provides a **user-friendly and admin-manageable** online shopping experience where users can seamlessly browse products, manage their cart, and track their orders while admins can efficiently **manage inventory and orders**.

### **Q3: Who can use this application?**  
**A:**  
- **Customers:** Browse products, add to cart, and checkout.  
- **Admins:** Add/update/delete products, manage orders, and track user purchases.  

---

## **2. Tech Stack & Architecture**  
### **Q4: What are the technologies used in this project?**  
**A:**  
- **Frontend:** React.js, React Router, Context API, Tailwind CSS  
- **Backend:** FastAPI, SQLAlchemy, Pydantic  
- **Database:** MySQL (Hosted on AWS)  
- **Authentication:** JWT (JSON Web Token)  
- **HTTP Client:** Axios  
- **Deployment:** Vercel (Frontend), Render (Backend)  

### **Q5: Can you explain the architecture of this project?**  
**A:**  
- **Frontend (React)** interacts with **Backend (FastAPI)** through REST API calls.  
- **Backend (FastAPI)** handles business logic, user authentication, and data processing.  
- **MySQL database** stores user details, products, orders, and cart items.  
- **JWT Token authentication** secures user access.  
- **CORS Middleware** allows frontend-backend communication.  

### **Q6: What are the main folders and their purpose in the backend?**  
**A:**  
- **app/** - Main backend application folder.  
  - **routes/** - Contains API route definitions (e.g., `auth.py`, `orders.py`).  
  - **models/** - Defines the database schema using SQLAlchemy.  
  - **schemas/** - Defines Pydantic models for request validation.  
  - **utils/** - Utility functions like hashing passwords & JWT authentication.  
  - **database.py** - Configures database connection.  
  - **main.py** - Main entry point for running the FastAPI application.  

---

## **3. User Authentication & Authorization**  
### **Q7: How does user authentication work in this project?**  
**A:**  
1. **User registers** → Password is hashed using bcrypt and stored.  
2. **User logs in** → Backend verifies credentials and generates a **JWT token**.  
3. **JWT Token is stored in localStorage** and sent in every request.  
4. **Protected routes (like `/orders`) require a valid JWT token**.  

### **Q8: How is password security implemented?**  
**A:**  
- Uses **bcrypt** for password hashing.  
- Passwords are **never stored in plain text**.  
- During login, entered passwords are **hashed & compared** with stored passwords.  

### **Q9: How does role-based access control (RBAC) work?**  
**A:**  
- The **users table** has an `is_admin` column (boolean).  
- If `is_admin = True`, user gets **admin privileges**.  
- Routes like `/admin` are restricted using:  
  ```python
  def verify_admin(current_user):
      if not current_user.is_admin:
          raise HTTPException(status_code=403, detail="Admins only!")
  ```
- Unauthorized users get a **403 Forbidden** response.  

---

## **4. Frontend Routing & Navigation**  
### **Q10: How does frontend routing work?**  
**A:**  
- Uses **React Router** for navigation.  
- Routes are defined in `AppRoutes.jsx`.  
- Admin routes are wrapped in a `ProtectedRoute`.  
```jsx
<Route path="/admin" element={<ProtectedRoute><AdminDashboard /></ProtectedRoute>} />
```

### **Q11: How does navigation work in the Navbar?**  
**A:**  
- The navbar **checks authentication state** using `useAuth()`.  
- If the user is logged in, it displays **Cart, Orders, and Profile**.  
- If logged out, it shows **Login & Register** options.  
- Clicking the **profile icon** navigates to `/profile`.  

---

## **5. Product & Cart Management**  
### **Q12: How does product listing and searching work?**  
**A:**  
- Products are fetched using the API `/products/`.  
- Users can **filter products by category** or **search by name**.  
- Products are **sorted by price (Low → High or High → Low)**.

### **Q13: How does the cart system work?**  
**A:**  
1. Users **add products to the cart** → Data is stored in the `carts` table.  
2. The cart shows **product name, price, quantity**.  
3. Users can **increase/decrease quantity** or **remove items**.  
4. The cart is **linked to a user account** (Only logged-in users can access their cart).  

---

## **6. Order Processing & Checkout**  
### **Q14: How does order processing work?**  
**A:**  
1. User **clicks checkout** → Backend verifies cart contents.  
2. **New order is created** in `orders` table.  
3. **Cart items are moved to `order_items` table**.  
4. Order status starts as **"Pending"**.  
5. Admin can update the order to **"Shipped" → "Delivered"**.

### **Q15: What payment methods are supported?**  
**A:**  
✔ Credit Card  
✔ UPI  
✔ Net Banking  
✔ Cash on Delivery  

---

## **7. Admin Panel**  
### **Q16: What actions can an admin perform?**  
**A:**  
✔ **Manage Products** (Add, Update, Delete)  
✔ **Manage Orders** (View all orders, Update order status)  

### **Q17: How does the admin add a new product?**  
**A:**  
- Admin goes to **Manage Products** → Clicks **Add Product**.  
- Enters product details and submits the form.  
- API call:
```js
await axios.post("/products/", productData);
```

---

## **8. Error Handling & Debugging**  
### **Q18: What are some common errors and how did you fix them?**  
| **Error**                     | **Cause**                                      | **Fix**                                      |
|--------------------------------|-----------------------------------------------|---------------------------------------------|
| 401 Unauthorized               | Invalid JWT Token                            | Ensure valid token is sent in the request  |
| CORS Policy Issue              | Backend blocking frontend requests           | Add CORS middleware in FastAPI             |
| IntegrityError on DELETE       | Foreign key constraint error in DB           | Use **CASCADE DELETE** in database         |
| UI Not Updating on State Change | React state not re-rendering                | Use **useEffect** to track changes         |

---

## **9. Deployment & Optimization**  
### **Q19: How did you deploy the project?**  
**A:**  
- **Backend (FastAPI)** → Render  
- **Frontend (React)** → Vercel  

### **Q20: How did you optimize performance?**  
✔ **Lazy Loading** for faster page loads  
✔ **Debounced search** to reduce API calls  
✔ **Indexes in MySQL** for faster queries  

---

## **10. Future Enhancements**  
### **Q21: What additional features can be added?**  
✔ Wishlist functionality  
✔ Live order tracking  
✔ Google/Facebook login  

---
# **QuitQ E-Commerce Project - Complete Technical & Logical Q&A Cheat Sheet**  
**Version:** 1.0  
**Prepared By:** [Your Name]  
**Date:** [Current Date]  

---

# **1. Project Overview & Tech Stack**
## **Q1: What is QuitQ E-Commerce and why was it built?**  
**A:** QuitQ is an **e-commerce platform** where users can **browse products, add them to their cart, checkout with payment methods, and track their orders**. It also has an **admin panel** for managing products and orders. It was built to **demonstrate full-stack development** using **FastAPI (Backend) and React.js (Frontend)**.

## **Q2: What are the core features of this project?**  
✔ **User Authentication** (Register/Login with JWT)  
✔ **Product Management** (View, Search, Sort, Filter)  
✔ **Cart System** (Add/Remove/Update Cart)  
✔ **Order Processing & Checkout**  
✔ **Admin Panel** (Product & Order Management)  
✔ **Role-Based Access Control (RBAC)**  

## **Q3: What technologies are used?**  
✔ **Frontend:** React.js, React Router, Context API, Tailwind CSS  
✔ **Backend:** FastAPI, SQLAlchemy, Pydantic  
✔ **Database:** MySQL (Hosted on AWS)  
✔ **Authentication:** JWT (JSON Web Token)  
✔ **HTTP Client:** Axios  
✔ **Deployment:** Vercel (Frontend), Render (Backend)  

---

# **2. Backend Architecture & API Routes**
## **Q4: What is the backend file structure and its purpose?**  
| **Folder/File**  | **Purpose**  |
|-----------------|-------------|
| `app/main.py`  | Entry point for the FastAPI backend |
| `app/database.py`  | Configures MySQL database connection |
| `app/models.py`  | Defines database schemas using SQLAlchemy |
| `app/schemas.py`  | Defines data validation models with Pydantic |
| `app/utils/security.py`  | Handles password hashing and JWT authentication |
| `app/routes/auth.py`  | User authentication API (Register, Login) |
| `app/routes/products.py`  | Product management API (CRUD operations) |
| `app/routes/cart.py`  | Cart management API (Add, Remove, Update) |
| `app/routes/orders.py`  | Order management API (Place, View, Update) |
| `app/routes/admin.py`  | Admin API for managing products and orders |

---

## **3. Authentication & Protected Routes**
### **Q5: How does user authentication work in the backend?**  
1. **User registers** → Password is hashed with bcrypt and stored in the database.  
2. **User logs in** → Credentials are verified, and a JWT token is issued.  
3. **JWT Token is stored** in localStorage (Frontend) and sent in API headers.  
4. **Protected Routes** require a valid JWT token.

### **Q6: Explain the JWT authentication flow step by step.**  
✔ User registers → `POST /auth/register`  
✔ User logs in → `POST /auth/login` (Returns JWT Token)  
✔ Protected routes require a token:  
```python
from fastapi import Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt
from app.database import get_db
from app.models import User

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/auth/login")

def get_current_user(token: str = Depends(oauth2_scheme), db=Depends(get_db)):
    """Extract user from JWT token"""
    try:
        payload = jwt.decode(token, "SECRET_KEY", algorithms=["HS256"])
        email: str = payload.get("sub")
        user = db.query(User).filter(User.email == email).first()
        if user is None:
            raise HTTPException(status_code=401, detail="Invalid credentials")
        return user
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
```
✔ If the user is **admin**, they can access `/admin` routes.  

---

### **Q7: How do protected routes work in the frontend?**
- **Frontend** stores JWT in localStorage and **adds it to headers** in API calls.  
- **React Context (`AuthContext.jsx`)** stores user data.  
- **If token expires**, the user is logged out automatically.  

#### **Code for Protected Route Handling in React**:
```jsx
import { Navigate } from "react-router-dom";
import { useAuth } from "../context/AuthContext";

const ProtectedRoute = ({ element }) => {
  const { token } = useAuth();
  return token ? element : <Navigate to="/login" />;
};

export default ProtectedRoute;
```
✔ **Without a valid token**, the user is redirected to the login page.

---

# **4. Product & Cart Management**
### **Q8: How does product listing and filtering work?**  
✔ Products are fetched from `/products/` API.  
✔ Users can **search** by name or **filter by category**.  
✔ **Sorting** by price (`asc` or `desc`) is implemented.  

#### **Backend Code for Filtering & Sorting**
```python
@router.get("/", response_model=List[ProductResponse])
def get_products(db: Session = Depends(get_db), category: Optional[str] = None, search: Optional[str] = None, sort_by: Optional[str] = None):
    query = db.query(Product)
    if category:
        query = query.filter(Product.category == category)
    if search:
        query = query.filter(Product.name.ilike(f"%{search}%"))
    if sort_by == "asc":
        query = query.order_by(Product.price.asc())
    elif sort_by == "desc":
        query = query.order_by(Product.price.desc())
    return query.all()
```

---

### **Q9: How does the cart system work?**  
✔ When a user **adds a product to the cart**, it is stored in the `carts` table.  
✔ Users can **increase/decrease quantity** or **remove items**.  
✔ When checking out, cart items are moved to `orders` and `order_items`.

#### **Backend Code for Adding Items to Cart**
```python
@router.post("/")
def add_to_cart(cart_item: CartItem, db: Session = Depends(get_db), current_user=Depends(get_current_user)):
    user_id = current_user.id
    product = db.query(Product).filter(Product.id == cart_item.product_id).first()
    if not product:
        raise HTTPException(status_code=404, detail="Product not found")
    cart_entry = db.query(Cart).filter(Cart.user_id == user_id, Cart.product_id == cart_item.product_id).first()
    if cart_entry:
        cart_entry.quantity += cart_item.quantity
    else:
        cart_entry = Cart(user_id=user_id, product_id=cart_item.product_id, quantity=cart_item.quantity)
        db.add(cart_entry)
    db.commit()
    return {"message": "Product added to cart successfully"}
```

---

# **5. Order Processing & Admin Panel**
### **Q10: How does order processing work?**  
✔ User clicks **checkout** → Order is created.  
✔ Products are moved from `cart` to `orders` & `order_items`.  
✔ Admin updates **order status** (Pending → Processing → Shipped → Delivered).  

#### **Backend Code for Order Processing**
```python
@router.post("/")
def place_order(order_data: OrderCreate, db: Session = Depends(get_db), current_user=Depends(get_current_user)):
    user_id = current_user.id
    cart_items = db.query(Cart).filter(Cart.user_id == user_id).all()
    if not cart_items:
        raise HTTPException(status_code=400, detail="Cart is empty")
    total_price = sum(item.quantity * db.query(Product).filter(Product.id == item.product_id).first().price for item in cart_items)
    new_order = Order(user_id=user_id, total_price=total_price, status="Pending")
    db.add(new_order)
    db.commit()
    return {"message": "Order placed successfully", "order_id": new_order.id}
```

---

### **Q11: How does the admin panel work?**  
✔ Admin can **add/update/delete products**.  
✔ Admin can **view and update orders**.  
✔ Only **admin users** can access these routes.  

---
