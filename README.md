# Flutter Commerce (Local Marketplace)

A lightweight local marketplace demo with a Node.js + Express backend and a Flutter frontend. This README explains project features, how to run the app from a fresh machine, and troubleshooting tips.

**Contents**
- Features
- Requirements
- Quick start (backend)
- Quick start (frontend)
- Database setup
- Important environment variables
- Useful API endpoints
- Testing the main flows
- Troubleshooting


## Features
- Full-stack local marketplace demo
- Backend (Node.js + Express) with MySQL persistence
  - CRUD for shops, products, orders
  - Auth (register / login / fetch current user)
  - Profile update endpoint (`PUT /api/auth/profile/:userId`)
  - Shop update endpoint (`PUT /api/shops/:shopId`)
  - Order status update (`PATCH /api/orders/:orderId/status`) with allowed statuses: `pending`, `shipped`, `delivered`
- Frontend (Flutter)
  - Browse, product details, shop management (for sellers)
  - Cart and checkout (for buyers)
  - Orders and invoices
  - Theme switching (multiple themes) persisted across restarts
  - Profile editing (name, email, optional password)
  - Shop editing (name, description) for sellers
  - Theme selection available on start, auth, and profile screens
- Guest mode with navigation (Shop / Cart / Profile). Cart/Profile show sign-in prompts for guests.

<table>
<tr>
<td align="center">

<img width="424" height="899" alt="image" src="https://github.com/user-attachments/assets/54cc9006-bce6-4d37-b071-b950eb72cd41" />
</td>

<td align="center">


<img width="420" height="874" alt="image" src="https://github.com/user-attachments/assets/ead911fd-38ea-47b2-a04c-eb7ce8ca22e6" />

</td>
</tr>
</table>
<table>
<tr>
<td align="center">

<img width="429" height="897" alt="image" src="https://github.com/user-attachments/assets/f411037c-57e2-484c-8db4-5b0adebe58b2" />
</td>

<td align="center">


<img width="420" height="874" alt="image" src="https://github.com/user-attachments/assets/ead911fd-38ea-47b2-a04c-eb7ce8ca22e6" />

</td>
</tr>
</table>
<table>
<tr>
<td align="center">

<img width="419" height="890" alt="image" src="https://github.com/user-attachments/assets/812fbb91-3523-461b-bc9f-cfe36ad2b999" />
</td>

<td align="center">

<img width="412" height="894" alt="image" src="https://github.com/user-attachments/assets/7708d00f-7323-440f-9cd3-909cd50257b2" />


</td>
</tr>
</table>
</table>
<table>
<tr>
<td align="center">

<img width="415" height="896" alt="image" src="https://github.com/user-attachments/assets/2e8d9a2c-a1d0-4f09-bad6-1380b94e1c9c" />
</td>

<td align="center">

<img width="418" height="891" alt="image" src="https://github.com/user-attachments/assets/12c4501c-cf7b-443b-a8d0-cdf34b431eb0" />


</td>
</tr>
</table>
<table>
<tr>
<td align="center">

<img width="412" height="890" alt="image" src="https://github.com/user-attachments/assets/af7f3ee1-32ff-4151-b395-d4939cd31a44" />
</td>

<td align="center">

<img width="417" height="895" alt="image" src="https://github.com/user-attachments/assets/a173b1bb-90a0-476e-94f7-dbcbe0d3ad40" />


</td>
</tr>
</table>
<table>
<tr>
<td align="center">

<img width="415" height="896" alt="image" src="https://github.com/user-attachments/assets/fa7418d1-828b-4f0f-9603-c16bfd19936a" />
</td>

<td align="center">

<img width="413" height="898" alt="image" src="https://github.com/user-attachments/assets/8f7d6785-fd3b-4106-89f3-467a1ab247b4" />


</td>
</tr>
</table>

## Requirements (fresh machine)
- Node.js (LTS, e.g., >= 18)
- npm
- MySQL server (or compatible MariaDB)
- Flutter SDK (for frontend; stable channel recommended)
- Git (to fork/clone)


## Quick start — Backend (Windows PowerShell example)
1. Open PowerShell and go to the backend folder:

```powershell
cd backend
```

2. Install Node deps:

```powershell
npm install
```

3. Create/import database and schema (run from terminal with your MySQL root/user):

```powershell
# create database (only run once)
mysql --user=root --password="YourRootPassword" -e "CREATE DATABASE IF NOT EXISTS ecommerce_db;"
# import schema
mysql --user=root --password="YourRootPassword" ecommerce_db < database/schema.sql
```

4. Copy the example env (if present) or create `.env.development.local` in the `backend/` folder with values similar to:

```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=YourRootPassword
DB_NAME=ecommerce_db
PORT=5000
RESET_DB_ON_START=false
```

Make sure `RESET_DB_ON_START=false` to preserve your data across restarts.

5. Start the backend (nodemon is used for dev):

```powershell
npm run dev
```
 or if you want to run without nodemon:
```powershell
npm start
```
- If you see `Error: listen EADDRINUSE: address already in use :::5000`, another process is using port 5000. Stop that process or change `PORT` in the env file.


## Quick start — Frontend (Flutter)
1. Install dependencies and launch the app from `frontend/`:

```powershell
cd C:\Users\User\Desktop\flutter_commerce\frontend
flutter pub get
flutter run
```

2. The app uses `shared_preferences` to persist the selected theme across restarts. Theme controls are available on the start screen (top-right dropdown), auth screen (top-right), and profile screen (profile settings).


## Database notes
- Schema file is at `backend/database/schema.sql`.
- If you set `RESET_DB_ON_START=true` the backend will re-seed the database on server start (development only). Keep this `false` on a machine where you want persistent data.


## Important environment variables (backend)
- `DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` — database connection
- `PORT` — server port (default 5000)
- `RESET_DB_ON_START` — if `true` will reset & seed DB at startup (development convenience only)


## Useful backend API endpoints
(Assuming backend served at http://localhost:5000)

- POST /api/auth/register — Register a user
- POST /api/auth/login — Login
- GET  /api/auth/me/:userId — Fetch user + shop
- PUT  /api/auth/profile/:userId — Update user profile (name, email, optional password)

- GET  /api/shops — List shops
- GET  /api/shops/owner/:ownerUserId — Get a shop by owner
- GET  /api/shops/:shopId — Get shop
- PUT  /api/shops/:shopId — Update shop (name, description)
- POST /api/shops — Create shop

- GET  /api/products — List products
- POST /api/products — Create product
- PUT  /api/products/:productId — Update product

- PATCH /api/orders/:orderId/status — Update order status (only accepts `pending|shipped|delivered`)

Sample curl to update shop (replace IDs and payload):

```bash
curl -X PUT http://localhost:5000/api/shops/42 \
  -H "Content-Type: application/json" \
  -d '{"name":"My New Shop","description":"Updated description"}'
```


## Frontend: main user flows to test
- Launch app → Use theme dropdown (top-right) on start screen to change theme
- Visit as guest → browse products
- Sign in / register as buyer or seller (Auth screen still has theme menu)
- As seller: create a shop (if needed), create products, open Profile → Shop Settings → Edit shop details
- As buyer: add items to cart → checkout
- Editing profile: Profile → Edit → change name/email/password


## Troubleshooting
- Route not found when updating shop/profile: make sure backend was restarted after code changes and is running on the configured `PORT`.
  - Restart backend: `cd backend && npm run dev`
  - Confirm it serves routes by opening `http://localhost:5000/api/shops` in the browser.
- `EADDRINUSE` on port 5000: another process uses that port. Stop it or change `PORT` in `.env.development.local`.
- MySQL connection refused: ensure MySQL is running and credentials in the env file match.
- Flutter build fails: run `flutter doctor` to verify SDK and platform tools are installed.


## Notes for forking and running on a fresh machine
1. Fork/clone repo.
2. Install prerequisites listed above.
3. Setup database and import `database/schema.sql`.
4. Create `.env.development.local` under `backend/` with DB and PORT values. Set `RESET_DB_ON_START=false` unless you want the seed applied on every start.
5. Start backend: `npm run dev`.
6. Start frontend: `flutter pub get` then `flutter run`.


## Where to look in the code
- Backend routes: `backend/src/routes/` (shops.routes.js, auth.routes.js, products.routes.js, orders.routes.js)
- Backend controllers: `backend/src/controllers/marketplaceController.js`
- Frontend screens: `frontend/lib/screens/` (profile_screen.dart, start_screen.dart, auth_screen.dart)
- Theme definitions: `frontend/lib/utils/themes.dart`
- Theme provider: `frontend/lib/providers/theme_provider.dart`



ENV example for backend `.env.development.local`:

```md
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_password_here
DB_NAME=ecommerce_db
PORT=5000
# If true the server will drop existing tables and re-seed sample data on startup
RESET_DB_ON_START=false
```
