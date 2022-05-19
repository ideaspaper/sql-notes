# Revisiting SQL

Contents:

- [Existing codes](#existing-codes)
- [SQL](#raw-query)
- [Applying to Sequelize](#applying-to-sequelize)
- [Transaction](#transaction)

## Existing codes

### Migrations

<details>
  <summary>Expand!</summary>

**migrations/xxx-create-user.js**

```js
'use strict';
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('Users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      email: {
        type: Sequelize.STRING
      },
      password: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  async down(queryInterface, Sequelize) {
    await queryInterface.dropTable('Users');
  }
};
```

**migrations/xxx-create-product.js**

```js
'use strict';
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('Products', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      name: {
        type: Sequelize.STRING
      },
      price: {
        type: Sequelize.INTEGER
      },
      stock: {
        type: Sequelize.INTEGER
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  async down(queryInterface, Sequelize) {
    await queryInterface.dropTable('Products');
  }
};
```

**migrations/xxx-create-order.js**

```js
'use strict';
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('Orders', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      UserId: {
        type: Sequelize.INTEGER,
        references: {
          model: 'Users',
          key: 'id'
        },
        onDelete: 'cascade',
        onUpdate: 'cascade'
      },
      ProductId: {
        type: Sequelize.INTEGER,
        references: {
          model: 'Products',
          key: 'id'
        },
        onDelete: 'cascade',
        onUpdate: 'cascade'
      },
      quantity: {
        type: Sequelize.INTEGER
      },
      totalPrice: {
        type: Sequelize.INTEGER
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  async down(queryInterface, Sequelize) {
    await queryInterface.dropTable('Orders');
  }
};
```

</details>

### Models

<details>
  <summary>Expand!</summary>

**models/user.js**

```js
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class User extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
      User.hasMany(models.Order);
    }
  }
  User.init(
    {
      email: DataTypes.STRING,
      password: DataTypes.STRING
    },
    {
      sequelize,
      modelName: 'User'
    }
  );
  return User;
};
```

**models/product.js**

```js
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Product extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
      Product.hasMany(models.Order);
    }
  }
  Product.init(
    {
      name: DataTypes.STRING,
      price: DataTypes.INTEGER,
      stock: {
        type: DataTypes.INTEGER,
        validate: {
          min: 0
        }
      }
    },
    {
      sequelize,
      modelName: 'Product'
    }
  );
  return Product;
};
```

**models/order.js**

```js
'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Order extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
      Order.belongsTo(models.User);
      Order.belongsTo(models.Product);
    }
  }
  Order.init(
    {
      UserId: DataTypes.INTEGER,
      ProductId: DataTypes.INTEGER,
      quantity: DataTypes.INTEGER,
      totalPrice: DataTypes.INTEGER
    },
    {
      sequelize,
      modelName: 'Order'
    }
  );
  return Order;
};
```

</details>

### Routes

<details>
  <summary>Expand!</summary>

**routes/index.js**

```js
const router = require('express').Router();
const {
  orders,
  orders1,
  orders2,
  orders3,
  orders4,
  orders5,
  orders6,
  orders7,
  orders8
} = require('./../controllers/orders-controller');

router.get('/orders', orders);
router.get('/orders1', orders1);
router.get('/orders2', orders2);
router.get('/orders3', orders3);
router.get('/orders4', orders4);
router.get('/orders5', orders5);
router.get('/orders6', orders6);
router.get('/orders7', orders7);
router.post('/orders8', orders8);

module.exports = router;
```

</details>

### Controllers

<details>
  <summary>Expand!</summary>

**controllers/orders-controller.js**

```js
const { Order, Product, sequelize } = require('./../models/index');
const { Op } = require('sequelize');

module.exports = {
  orders: async (req, res) => {
    // blank
  },
  orders1: async (req, res) => {
    // blank
  },
  orders2: async (req, res) => {
    // blank
  },
  orders3: async (req, res) => {
    // blank
  },
  orders4: async (req, res) => {
    // blank
  },
  orders5: async (req, res) => {
    // blank
  },
  orders6: async (req, res) => {
    // blank
  },
  orders7: async (req, res) => {
    // blank
  },
  orders8: async (req, res) => {
    // blank
  }
};
```

</details>

### App

<details>
  <summary>Expand!</summary>

**app.js**

```js
const express = require('express');
const router = require('./routes/index');
const app = express();
const PORT = 3000;

app.use(express.urlencoded({ extended: false }));
app.use(express.json());
app.use(router);

app.listen(PORT, () => {
  console.log('running on port', PORT);
});
```

</details>

## Raw query

### Filtering data

1. Dapatkan record dari tabel `Orders` dimana `quantity`-nya kurang dari 10
1. Dapatkan record dari tabel `Orders` dimana `quantity`-nya antara 10 dan 20

<details>
  <summary>Expand!</summary>

```sql
select * from "Orders" o where o."quantity" < 10;
select * from "Orders" o where o."quantity" between 10 and 20;
```

</details>

### Aggregate functions

1. Hitung jumlah `totalPrice` dari semua record yang ada di tabel `Orders`
1. Hitung rata-rata `totalPrice` dari semua record yang ada di tabel `Orders`
1. Dapatkan nilai terbesar `totalPrice` dari semua record yang ada di tabel `Orders`
1. Dapatkan nilai terkecil `totalPrice` dari semua record yang ada di tabel `Orders`

<details>
  <summary>Expand!</summary>

```sql
select sum(o."totalPrice") from "Orders" o;
select avg(o."totalPrice") from "Orders" o;
select max(o."totalPrice") from "Orders" o;
select min(o."totalPrice") from "Orders" o;
```

</details>

### Group by

1. Hitung jumlah `totalPrice` dari semua record yang ada di tabel `Orders` untuk masing-masing jenis produk yang ada

<details>
  <summary>Expand!</summary>

```sql
select o."ProductId", sum(o."totalPrice") from "Orders" o group by o."ProductId";
```

</details>

### Join

1. Dapatkan semua record dari tabel `Order` disertai dengan nama produknya
1. Hitung jumlah `totalPrice` dan `quantity` dari semua record yang ada di tabel `Orders` untuk masing-masing jenis produk yang ada, sertakan juga nama produknya

<details>
  <summary>Expand!</summary>

```sql
select o.*, p."name" from "Orders" o join "Products" p ON p."id" = o."ProductId";

select
  o."ProductId",
  p."name",
  sum(o."quantity"),
  sum(o."totalPrice")
from
  "Orders" o
join "Products" p on
  o."ProductId" = p."id"
group by
  o."ProductId",
  p."name";
```

</details>

## Applying to Sequelize

### Filtering data

1. Dapatkan record dari tabel `Orders` dimana `quantity`-nya kurang dari 10
1. Dapatkan record dari tabel `Orders` dimana `quantity`-nya antara 10 dan 20

<details>
  <summary>Expand!</summary>

```js
// select * from "Orders" o where o."quantity" < 10;
const orders = await Order.findAll({
  where: {
    quantity: {
      [Op.lt]: 10
    }
  }
});

// select * from "Orders" o where o."quantity" between 10 and 20;
const orders = await Order.findAll({
  where: {
    quantity: {
      [Op.between]: [10, 20]
    }
  }
});
```

</details>

### Aggregate functions

1. Hitung jumlah `totalPrice` dari semua record yang ada di tabel `Orders`
1. Hitung rata-rata `totalPrice` dari semua record yang ada di tabel `Orders`
1. Dapatkan nilai terbesar `totalPrice` dari semua record yang ada di tabel `Orders`
1. Dapatkan nilai terkecil `totalPrice` dari semua record yang ada di tabel `Orders`

<details>
  <summary>Expand!</summary>

```js
// select sum(o."totalPrice") from "Orders" o;
const result = await Order.findAll({
  attributes: [[sequelize.fn('sum', sequelize.col('totalPrice')), 'sumTotalPrice']]
});
```

</details>

### Group by

1. Hitung jumlah `totalPrice` dari semua record yang ada di tabel `Orders` untuk masing-masing jenis produk yang ada

<details>
  <summary>Expand!</summary>

```js
// select o."ProductId", sum(o."totalPrice") from "Orders" o group by o."ProductId";
const result = await Order.findAll({
  attributes: ['ProductId', [sequelize.fn('sum', sequelize.col('totalPrice')), 'sumTotalPrice']],
  group: 'Order.ProductId'
});
```

</details>

### Join

1. Dapatkan semua record dari tabel `Order` disertai dengan nama produknya
1. Hitung jumlah `totalPrice` dan `quantity` dari semua record yang ada di tabel `Orders` untuk masing-masing jenis produk yang ada, sertakan juga nama produknya

<details>
  <summary>Expand!</summary>

```js
// select o.*, p."name" from "Orders" o join "Products" p ON p."id" = o."ProductId";
const result = await Order.findAll({
  include: {
    model: Product,
    attributes: ['name']
  }
});

// select
//   o."ProductId",
//   p."name",
//   sum(o."quantity"),
//   sum(o."totalPrice")
// from
//   "Orders" o
// join "Products" p on
//   o."ProductId" = p."id"
// group by
//   o."ProductId",
//   p."name";
const result = await Order.findAll({
  attributes: [
    'ProductId',
    [sequelize.fn('sum', sequelize.col('totalPrice')), 'sumTotalPrice'],
    [sequelize.fn('sum', sequelize.col('quantity')), 'sumQuantity']
  ],
  include: {
    model: Product,
    attributes: ['name']
  },
  group: ['Order.ProductId', 'Product.id']
});
```

</details>

### Raw Query

1. Hitung jumlah `totalPrice` dan `quantity` dari semua record yang ada di tabel `Orders` untuk masing-masing jenis produk yang ada, sertakan juga nama produknya

<details>
  <summary>Expand!</summary>

```js
const result = await sequelize.query(
  `
  select
    o."ProductId",
    p."name",
    sum(o."quantity") as "sumQuantity",
    sum(o."totalPrice") as "sumTotalPrice"
  from
    "Orders" o
  join "Products" p on
    o."ProductId" = p."id"
  group by
    o."ProductId",
    p."name";
  `,
  {
    type: sequelize.QueryTypes.SELECT
  }
);
```

</details>

## Transaction

### ACID

- **Atomicity**

  Atomicity guarantees that each transaction is treated as a single "unit", which either succeeds completely or fails completely: if any of the statements constituting a transaction fails to complete, the entire transaction fails and the database is left unchanged

- **Consistency**

  Consistency ensures that a transaction can only bring the database from one valid state to another, maintaining database invariants: any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof

- **Isolation**

  Any reads or writes performed on the database will not be impacted by other reads and writes of separate transactions occurring on the same database. A global order is created with each transaction queueing up in line to ensure that the transactions complete in their entirety before another one begins.

  Importantly, this doesn’t mean two operations can’t happen at the same time. Multiple transactions can occur as long as those transactions have no possibility of impacting the other transactions occurring at the same time.

- **Durability**

  Durability guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or crash).

### Applying transaction to Sequelize

<details>
  <summary>Expand!</summary>

```js
const { UserId, ProductId, quantity } = req.body;
const t = await sequelize.transaction();
try {
  const product = await Product.findByPk(ProductId, { transaction: t });
  if (!product) throw { name: 'NotFound' };
  const order = await Order.create(
    {
      UserId,
      ProductId,
      quantity,
      totalPrice: product.price * quantity
    },
    { transaction: t }
  );
  const updatedProduct = await Product.update(
    {
      stock: product.stock - quantity
    },
    {
      where: { id: ProductId },
      returning: true,
      transaction: t
    }
  );
  await t.commit();
  res.status(200).json({ order, product: updatedProduct[1] });
} catch (error) {
  await t.rollback();
  res.status(500).json({ message: error });
}
```

</details>

## References

- [PostgreSQL Aggregate Functions](https://www.postgresqltutorial.com/postgresql-aggregate-functions/)
- [PostgreSQL GROUP BY](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-group-by/)
- [PostgreSQL Joins](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-joins/)
- [Sequelize - Applying WHERE clauses](https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#applying-where-clauses)
- [`sequelize.fn`](https://sequelize.org/api/v6/class/src/sequelize.js~sequelize#static-method-fn)
- [`sequelize.query`](https://sequelize.org/api/v6/class/src/sequelize.js~sequelize#instance-method-query)
- [Wikipedia - ACID](https://en.wikipedia.org/wiki/ACID)
- [ACID Explained: Atomic, Consistent, Isolated & Durable](https://www.bmc.com/blogs/acid-atomic-consistent-isolated-durable/)
- [Sequelize - Transactions](https://sequelize.org/docs/v6/other-topics/transactions/)
