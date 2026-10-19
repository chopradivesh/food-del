Food Delivery Full-Stack Application

A full-stack food ordering web application built with React, Vite, Node.js, Express, and MongoDB. It includes a customer-facing website for browsing food and placing orders, a separate admin dashboard for managing menu items and orders, and a REST API that connects both interfaces.

Features

Customer website

• Browse food items and explore menu categories.
• Add and remove items from the shopping cart.
• Register and log in using the app’s authentication flow.
• Enter delivery details and place orders.
• View previous orders and order status.
• Integrates with Stripe Checkout for online payments.

Admin dashboard

• Add food items, including an image.
• View and remove menu items.
• View customer orders and update their status.

Backend API

• REST endpoints for users, food items, cart data, and orders.
• MongoDB persistence through Mongoose.
• Password hashing with bcrypt and token-based authentication with JSON Web Tokens.
• Image uploads handled with Multer.
• Stripe Checkout integration.

Tech stack

|Area             |Technologies                                       |
|-----------------|---------------------------------------------------|
|Customer frontend|React 18, Vite, React Router, Axios, React Toastify|
|Admin dashboard  |React 18, Vite, React Router, Axios                |
|Backend          |Node.js, Express                                   |
|Database         |MongoDB, Mongoose                                  |
|Authentication   |bcrypt, JSON Web Token (JWT)                       |
|Payments         |Stripe                                             |
|Uploads          |Multer                                             |

Project structure

food-del/
├── frontend/       # Customer-facing React application
├── admin/          # Admin dashboard React application
└── backend/        # Express REST API and database models
    ├── config/     # Database connection
    ├── controllers/# Request handlers
    ├── middleware/ # Authentication middleware
    ├── models/     # Mongoose schemas
    ├── routes/     # API routes
    └── uploads/    # Uploaded food images

Prerequisites

Install the following before running the project:

• Node.js (preferably a current LTS release) and npm
• MongoDB: a local MongoDB instance or a MongoDB Atlas cluster
• Stripe account and test secret key if testing online checkout

Check your installation:

node -v
npm -v

Setup and run locally

Open three separate terminal windows. Commands below assume you have extracted the ZIP and are in the food-del project directory.

1. Configure the backend

cd backend
npm install

Configure the database connection in config/db.js. Replace the placeholder MongoDB URI with your own connection string. The application appends /food-del as the database name, so use a URI appropriate for your local MongoDB or Atlas setup.

Create or update backend/.env with your own values:

PORT=4000
STRIPE_SECRET_KEY=sk_test_replace_with_your_test_secret_key

Use your own Stripe test-mode secret key. Never commit real credentials to a public repository. If the supplied .env contains credentials, rotate them and replace them with local environment variables.

Start the API:

npm run server

The backend should start on http://localhost:4000. Open that address in a browser; the root route should respond with API Working. Check the terminal for database connection errors if MongoDB does not connect.

2. Start the customer website

In a second terminal, from the food-del directory:

cd frontend
npm install
npm run dev

Vite will print a local URL, typically http://localhost:5173. Open the URL shown in your terminal.

3. Start the admin dashboard

In a third terminal, from the food-del directory:

cd admin
npm install
npm run dev

Vite will print another local URL, often http://localhost:5174 if port 5173 is already occupied. Open the URL printed by Vite. The admin app has routes for adding items (/add), listing items (/list), and managing orders (/orders).

Keep all three processes running while using the application.

Configuration notes

• The frontend and admin currently call the backend at http://localhost:4000.
• The online checkout flow redirects to the customer frontend at http://localhost:5173; if Vite chooses a different port, update the frontend_URL value in backend/controllers/orderController.js to match.
• The MongoDB URI is configured in backend/config/db.js in the supplied code. For production, move it to an environment variable such as MONGODB_URI rather than hard-coding it.
• Stripe payments require a valid STRIPE_SECRET_KEY in the backend environment. Use Stripe test keys and test payment details while developing.
• The application also contains a cash-on-delivery order handler.
• Do not expose secret keys in frontend code or commit real .env files.

Useful commands

Run these from the relevant folder (frontend, admin, or backend):

|Command         |Purpose                                              |
|----------------|-----------------------------------------------------|
|`npm install`   |Install dependencies                                 |
|`npm run dev`   |Start a Vite development server (frontend/admin only)|
|`npm run server`|Start the backend with Nodemon                       |
|`npm run build` |Build the frontend/admin for production              |
|`npm run lint`  |Run ESLint in the frontend/admin                     |

Troubleshooting

• MongoDB connection fails: Confirm MongoDB is running or your Atlas URI, username, password, and network access are correct.
• API requests fail: Make sure the backend is running on port 4000 and that the frontend/admin API URL matches it.
• Stripe checkout fails: Check STRIPE_SECRET_KEY, use a test-mode key, and confirm the checkout redirect URL matches the frontend’s actual port.
• Port already in use: Accept the alternate port printed by Vite, or stop the process using the port. Update the checkout redirect URL if the frontend port changes.
• Images do not load: Check that image uploads are saved in backend/uploads and that the API is serving /images from that directory.
• npm install says package.json is missing: Run the command inside the correct subfolder (backend, frontend, or admin), not in a directory that does not contain a package.json.

Security and production checklist

Before deploying, move all secrets and database credentials to environment variables, restrict CORS to trusted origins, validate request data, verify payment status server-side, and review authorization for admin-only actions. Use HTTPS and configure production URLs for the API, frontend, and Stripe redirects. The included code is a development project and should be security-reviewed before handling real customer data or payments.

License

No license is specified in the supplied project. Add a license file if you intend to publish or distribute this project.