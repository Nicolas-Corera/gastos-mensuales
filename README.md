# Gestor Financiero

Aplicación web para llevar el control de las finanzas personales: suscripciones, tarjetas de crédito, gastos extras e inversiones, todo en un solo lugar y pensado para el contexto argentino (pesos, dólares y cotizaciones en tiempo real).

**[Ver demo en vivo →](https://gmensuales.vercel.app/)**

---

## Funcionalidades

- **Autenticación de usuarios**: registro e inicio de sesión con email y contraseña (Firebase Authentication). Cada usuario ve únicamente sus propios datos.
- **Dashboard**: resumen global del mes con gráficos (Chart.js), estado del presupuesto y próximos vencimientos.
- **Suscripciones**: alta, edición y baja de servicios recurrentes, con categorías (incluidas personalizadas) y monto mensual.
- **Tarjetas de crédito**: carga de resúmenes, pagos en cuotas, fechas de vencimiento y totales por tarjeta/banco.
- **Gastos extras**: registro de gastos únicos con fecha, categoría y método de pago.
- **Inversiones**: seguimiento de capital inicial y valor actual, con gráfico de distribución y proyección de crecimiento.
- **Multimoneda (ARS / USD)**: conversión usando las cotizaciones **Oficial, Blue y MEP** obtenidas desde [DolarAPI](https://dolarapi.com).
- **Presupuesto mensual** configurable.
- **Tema claro / oscuro** persistente.
- **Configuración de cuenta**: cambio de email y contraseña, exportación de todos los datos en JSON (backup) y eliminación de cuenta.
- **Sincronización en tiempo real** entre dispositivos gracias a Firebase Realtime Database.

## Tecnologías

| Área | Herramientas |
|---|---|
| Frontend | HTML5, CSS3 (variables CSS, grid/flexbox), JavaScript (ES Modules) |
| Backend as a Service | Firebase Authentication, Firebase Realtime Database |
| Gráficos | [Chart.js](https://www.chartjs.org/) |
| Animaciones | [GSAP](https://gsap.com/) |
| Íconos y tipografía | Font Awesome, Montserrat (Google Fonts) |
| API externa | [DolarAPI](https://dolarapi.com) |

No requiere build ni dependencias de npm: todo se carga por CDN.

## Diseño

Estética minimalista en blanco y negro, **sin bordes redondeados**, con soporte completo de modo oscuro mediante variables CSS y responsive para escritorio y móvil.

## Estructura

```
.
├── index.html          # App principal (dashboard + pestañas)
├── auth/
│   └── index.html      # Login y registro
├── config/
│   └── index.html      # Configuración de cuenta, presupuesto y datos
└── README.md
```

## Modelo de datos (Realtime Database)

Los datos de cada usuario viven bajo su `uid`:

```
usuarios/
└── {uid}/
    ├── budget          { amount, currency }
    ├── subscriptions   { ... }
    ├── tarjetas        { ... }
    ├── oneTimeExpenses { ... }
    ├── extras          { ... }
    ├── inversiones     { ... }
    └── theme
```

## Correr el proyecto localmente

1. Cloná el repositorio:
   ```bash
   git clone https://github.com/<tu-usuario>/<tu-repo>.git
   cd <tu-repo>
   ```
2. Creá un proyecto en [Firebase Console](https://console.firebase.google.com/) y activá:
   - **Authentication** → proveedor *Email/Password*
   - **Realtime Database**
3. Reemplazá el objeto `firebaseConfig` con el de tu proyecto en `index.html`, `auth/index.html` y `config/index.html`.
4. Configurá las reglas de la base de datos para que cada usuario acceda solo a sus datos:
   ```json
   {
     "rules": {
       "usuarios": {
         "$uid": {
           ".read": "$uid === auth.uid",
           ".write": "$uid === auth.uid"
         }
       }
     }
   }
   ```
5. Levantá un servidor estático (los ES Modules no funcionan abriendo el archivo con `file://`):
   ```bash
   npx serve .
   # o con la extensión Live Server de VS Code
   ```

## Nota sobre seguridad

La `apiKey` de Firebase en una app web **no es un secreto**: identifica el proyecto, no da acceso por sí sola. La seguridad real la dan las **reglas de la base de datos** (ver arriba) y, opcionalmente, restringir la key por dominio desde Google Cloud Console.

## Próximos pasos

- [ ] Separar CSS y JavaScript en archivos independientes / modularizar
- [ ] Agregar tests para la lógica de cálculo (conversión de monedas, cuotas, proyecciones)
- [ ] Exportar también a CSV
- [ ] Gráficos de evolución mensual histórica
- [ ] Convertir en PWA para usarla como app instalable

## Autor

**Nicolás Corera**
[LinkedIn](https://www.linkedin.com/in/nicolas-corera/) · [GitHub](https://github.com/Nicolas-Corera) · [Email](nicolas.corera@gmail.com)
