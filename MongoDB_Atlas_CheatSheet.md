# MongoDB Atlas - Cheat Sheet

## ✅ Step 1: Create Account & Cluster

- Go to: https://www.mongodb.com/cloud/atlas
- Create a **Free Tier Cluster**
- Choose cloud provider (AWS/GCP/Azure) and region (e.g., ap-southeast-1)

## ✅ Step 2: Create Database and Collection

- Click “Browse Collections”
- Create:
  - Database: `product-db`
  - Collection: `products`

## ✅ Step 3: Create Database User

- Go to **Database Access**
- Add User:
  - Username: `nest`
  - Password: `password123`
  - Role: Read and write to any database

## ✅ Step 4: Whitelist Your IP

- Go to **Network Access**
- Add IP Address:
  - `0.0.0.0/0` (Allow from anywhere - for development)
  - or your actual IP address

## ✅ Step 5: Connect NestJS to Atlas

In `app.module.ts`:

```ts
import { MongooseModule } from "@nestjs/mongoose";

@Module({
  imports: [
    MongooseModule.forRoot(
      "mongodb+srv://nest:password123@cluster0.mongodb.net/product-db?retryWrites=true&w=majority"
    ),
  ],
})
export class AppModule {}
```

## 🛡️ Best Practice

- Store your connection string in `.env`:

```env
MONGO_URI=mongodb+srv://nest:password123@cluster0.mongodb.net/product-db
```

- Use `ConfigModule`:

```ts
MongooseModule.forRoot(process.env.MONGO_URI),
```

## ✅ Useful Commands

- View all databases:

```bash
show dbs
```

- Use a specific database:

```bash
use product-db
```

- Insert a product:

```bash
db.products.insertOne({ name: "Sample", price: 100 })
```
