Aquí tienes un ejemplo claro y completo para una **pastelería** usando un modelo tipo **MongoDB (NoSQL)**, donde trabajas con **base de datos → colecciones → documentos → atributos (campos)**.

---

## 🗂️ 1. Base de datos

Nombre de la base de datos:

```bash
use pasteleria
```

---

## 🍰 2. Colección: `productos`

Guarda los pasteles, postres y productos que vendes.

### Ejemplo de documento:

```json
{
  "_id": 1,
  "nombre": "Pastel de chocolate",
  "precio": 350.50,
  "categoria": "Pasteles",
  "disponible": true,
  "stock": 10,
  "ingredientes": ["harina", "huevo", "chocolate", "azúcar"],
  "fecha_creacion": "2026-04-27"
}
```

### Tipos de datos:

* `_id`: Number (int)
* `nombre`: String
* `precio`: Number (double)
* `categoria`: String
* `disponible`: Boolean
* `stock`: Number (int)
* `ingredientes`: Array (String)
* `fecha_creacion`: Date/String

---

## 👩‍🍳 3. Colección: `clientes`

### Ejemplo:

```json
{
  "_id": 101,
  "nombre": "María López",
  "telefono": "6561234567",
  "correo": "maria@gmail.com",
  "direccion": {
    "calle": "Av. Juárez",
    "numero": 123,
    "ciudad": "Ciudad Juárez"
  },
  "fecha_registro": "2026-04-20"
}
```

### Tipos:

* `_id`: Number
* `nombre`: String
* `telefono`: String
* `correo`: String
* `direccion`: Object
* `fecha_registro`: Date/String

---

## 🧾 4. Colección: `ventas`

### Ejemplo:

```json
{
  "_id": 5001,
  "id_cliente": 101,
  "productos": [
    {
      "id_producto": 1,
      "nombre": "Pastel de chocolate",
      "cantidad": 1,
      "precio_unitario": 350.50
    }
  ],
  "total": 350.50,
  "metodo_pago": "Efectivo",
  "fecha": "2026-04-27"
}
```

### Tipos:

* `_id`: Number
* `id_cliente`: Number (referencia)
* `productos`: Array (Object)
* `total`: Number
* `metodo_pago`: String
* `fecha`: Date/String

---

## 🎂 5. Colección: `pedidos`

### Ejemplo:

```json
{
  "_id": 7001,
  "id_cliente": 101,
  "descripcion": "Pastel personalizado de cumpleaños",
  "fecha_entrega": "2026-05-01",
  "estado": "En proceso",
  "anticipo": 150.00,
  "total": 500.00
}
```

### Tipos:

* `_id`: Number
* `id_cliente`: Number
* `descripcion`: String
* `fecha_entrega`: Date/String
* `estado`: String
* `anticipo`: Number
* `total`: Number

---

## 📌 Resumen sencillo

* **Base de datos:** `pasteleria`
* **Colecciones:**

  * `productos`
  * `clientes`
  * `ventas`
  * `pedidos`
* **Cada colección** contiene documentos (JSON)
* **Cada documento** tiene atributos con su tipo de dato

---

Si quieres, puedo adaptarlo exactamente a como te lo pide tu profe (por ejemplo: usando `insertOne()` como en tu tarea de MongoDB 👀).
