¡Excelente iniciativa! Implementar una estructura de **Agente Global (.agents)** para orquestar el desarrollo de una app es una forma muy eficiente de mantener la modularidad. Vamos a construir este ecosistema paso a paso, asegurándonos de que tu entorno esté blindado y tu código de Flutter para la pastelería sea sólido.

---

## 1. Estructura de la Habilidad Agente Global: `.agents`

Primero, configuramos el cerebro de tu automatización. Esta carpeta vivirá en la raíz de tu espacio de trabajo.

### Estructura de Carpetas
* `.agents/`
    * `SKILL.md` (Documentación y lógica del agente)
    * `scripts/` (Automatizaciones en Dart/Bash)
    * `ejemplos/` (Templates de UI y lógica)
    * `resources/` (Assets y configuración Firebase)

### Contenido de `.agents/SKILL.md`
```markdown
# Skill: Agente Global de Pastelería
**Objetivo:** Automatizar la creación y mantenimiento del CRUD de productos.

## Sub-Skills
1. **Skill Diseño:** Generación de UI con Material 3.
2. **Skill Código:** Implementación de lógica Dart y Repositorios.
3. **Skill Scraping:** Obtención de datos de tendencias de repostería (opcional).

## Flujo de Trabajo
- Verificar entorno -> Vincular Firebase -> Generar Modelos -> Construir UI -> CRUD.
```

---

## 2. Preparación del Entorno (Prerrequisitos)

Antes de tocar el código de la pastelería, validemos que tus herramientas funcionen en **VS Code** o **Antigravity**.

### Instalación de Herramientas
Si no tienes las CLI, ejecuta en tu terminal:
1. **Flutter:** [Descargar SDK](https://docs.flutter.dev/get-started/install)
2. **Firebase CLI:** ```bash
   npm install -g firebase-tools
   ```

### Verificación y Conexión
Ejecuta estos comandos para asegurar la conectividad:
* `flutter doctor` (Verifica el estado de Flutter).
* `firebase login` (Abre el navegador para autenticar tu cuenta).
* `dart pub global activate flutterfire_cli` (Necesario para vincular Flutter con Firebase).

---

## 3. Configuración del Proyecto "Proyecto Pastelería"

Creamos la carpeta principal y vinculamos Firebase.

```bash
flutter create proyecto_pasteleria
cd proyecto_pasteleria
flutterfire configure
```
*(Selecciona tu proyecto de Firebase Console y marca la opción Android).*

### Dependencias en `pubspec.yaml`
Añade estas líneas para habilitar Firestore:
```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^latest_version
  cloud_firestore: ^latest_version
  cupertino_icons: ^1.0.6
```

---

## 4. Desarrollo del CRUD (Arquitectura Dart)

Siguiendo la lógica de agentes, dividiremos el código en una estructura limpia.

### Estructura de Archivos
* `lib/`
    * `models/product_model.dart`
    * `services/firebase_service.dart`
    * `pages/home_page.dart`
    * `pages/product_form_page.dart`

#### A. Modelo de Datos (`models/product_model.dart`)
```dart
class Producto {
  String id;
  String nombre;
  double precio;

  Producto({required this.id, required this.nombre, required this.precio});

  Map<String, dynamic> toMap() => {'nombre': nombre, 'precio': precio};

  factory Producto.fromFirestore(Map<String, dynamic> data, String id) {
    return Producto(id: id, nombre: data['nombre'], precio: data['precio'].toDouble());
  }
}
```

#### B. Lógica Firestore (`services/firebase_service.dart`)
```dart
import 'cloud_firestore/cloud_firestore.dart';
import '../models/product_model.dart';

class FirebaseService {
  final CollectionReference _db = FirebaseFirestore.instance.collection('productos');

  Stream<List<Producto>> getProductos() {
    return _db.snapshots().map((snap) =>
        snap.docs.map((doc) => Producto.fromFirestore(doc.data() as Map<String, dynamic>, doc.id)).toList());
  }

  Future<void> addProducto(String nombre, double precio) => _db.add({'nombre': nombre, 'precio': precio});
  Future<void> updateProducto(String id, String nombre, double precio) => _db.doc(id).update({'nombre': nombre, 'precio': precio});
  Future<void> deleteProducto(String id) => _db.doc(id).delete();
}
```

#### C. Interfaz de Usuario (`pages/home_page.dart`)
Esta pantalla muestra la lista y permite borrar o navegar a edición.

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';

class HomePage extends StatelessWidget {
  final FirebaseService _service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Pastelería - Inventario")),
      body: StreamBuilder(
        stream: _service.getProductos(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          var productos = snapshot.data!;
          return ListView.builder(
            itemCount: productos.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(productos[index].nombre),
                subtitle: Text("\$${productos[index].precio}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => _service.deleteProducto(productos[index].id),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _mostrarFormulario(context),
      ),
    );
  }

  void _mostrarFormulario(BuildContext context) {
    // Aquí llamarías a la página de creación (ProductFormPage)
  }
}
```

---

## 5. Pruebas de Verificación

Para asegurar que todo funcione antes de desplegar:
1.  **Firebase Console:** Ve a Firestore y crea manualmente la colección `productos` para verificar que el `StreamBuilder` los lea en tiempo real.
2.  **Terminal:** Ejecuta `flutter run` en un emulador de Android.
3.  **Logs:** Observa el Debug Console de VS Code para capturar errores de permisos de Firebase (Recuerda configurar las **Rules** de Firestore en "test mode" inicialmente).

¿Deseas que profundice en el **Skill de Scraping** para obtener automáticamente nombres y precios de pasteles de la competencia?
