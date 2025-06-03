# NestJS Product Catalog - Cheat Sheet

## 1. Create NestJS Project

```bash
nest new product-api
cd product-api
```

## 2. Install MongoDB Driver

```bash
npm install @nestjs/mongoose mongoose
```

## 3. Generate Module, Controller, Service

```bash
nest g module product
nest g controller product
nest g service product
```

## 4. Create Schema

📄 `src/product/schemas/product.schema.ts`

```ts
import { Prop, Schema, SchemaFactory } from "@nestjs/mongoose";
import { Document } from "mongoose";

@Schema()
export class Product extends Document {
  @Prop() name: string;
  @Prop() price: number;
}

export const ProductSchema = SchemaFactory.createForClass(Product);
```

## 5. Connect MongoDB in app.module.ts

```ts
import { MongooseModule } from "@nestjs/mongoose";

@Module({
  imports: [
    MongooseModule.forRoot("mongodb://localhost/product-api"),
    ProductModule,
  ],
})
export class AppModule {}
```

## 6. Register Schema in product.module.ts

```ts
import { MongooseModule } from "@nestjs/mongoose";
import { Product, ProductSchema } from "./schemas/product.schema";

@Module({
  imports: [
    MongooseModule.forFeature([{ name: Product.name, schema: ProductSchema }]),
  ],
  controllers: [ProductController],
  providers: [ProductService],
})
export class ProductModule {}
```

## 7. Create Controller & Service

📄 `product.controller.ts`

```ts
@Controller("products")
export class ProductController {
  constructor(private readonly productService: ProductService) {}

  @Get()
  findAll() {
    return this.productService.findAll();
  }

  @Post()
  create(@Body() body) {
    return this.productService.create(body);
  }

  @Put(":id")
  update(@Param("id") id: string, @Body() body) {
    return this.productService.update(id, body);
  }

  @Delete(":id")
  delete(@Param("id") id: string) {
    return this.productService.delete(id);
  }
}
```

📄 `product.service.ts`

```ts
@Injectable()
export class ProductService {
  constructor(@InjectModel(Product.name) private model: Model<Product>) {}

  findAll() {
    return this.model.find().exec();
  }

  create(data: any) {
    return this.model.create(data);
  }

  update(id: string, data: any) {
    return this.model.findByIdAndUpdate(id, data, { new: true }).exec();
  }

  delete(id: string) {
    return this.model.findByIdAndDelete(id).exec();
  }
}
export class ProductService {
  constructor(@InjectModel(Product.name) private model: Model<Product>) {}

  findAll() {
    return this.model.find().exec();
  }

  create(data: any) {
    return this.model.create(data);
  }
}
```

## 8. Run the Project

```bash
npm run start:dev
```

## 9. Test API

- `GET http://localhost:3000/products`
- `POST http://localhost:3000/products` with JSON body
- `PUT http://localhost:3000/products/:id` to update
- `DELETE http://localhost:3000/products/:id` to delete
- `GET http://localhost:3000/products`
- `POST http://localhost:3000/products` with JSON body

## Folder Structure

```
src/
├── app.module.ts
├── product/
│   ├── product.module.ts
│   ├── product.controller.ts
│   ├── product.service.ts
│   └── schemas/product.schema.ts
```
# product-api
