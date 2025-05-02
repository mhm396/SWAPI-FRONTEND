# Proyecto de Formación - Frontend

Este proyecto consiste en desarrollar el frontend de una aplicación web en Nuxt.js que interactúa con el backend (SWAPI-BACKEND) a través de una API REST para mostrar un listado de naves de Star Wars y sus pilotos.

## Funciones

1. **Listado de Naves**:
   - El frontend muestra un listado de todas las naves junto con sus pilotos asociados. 
   - Los datos se cargan mediante peticiones AJAX a la API REST del backend.

2. **Vincular Pilotos**:
   - Cada nave permite la opción de vincular pilotos nuevos a través de un desplegable.
   - Al seleccionar un piloto, este se agregará a la base de datos y se vinculará a la nave.

3. **Eliminar Pilotos**:
   - Cada piloto tiene un botón para eliminarlo.
   - La eliminación no solo es visual, también se eliminar la relación entre la nave y el piloto en la base de datos.

4. **Transformación de Precios**:
   - Los precios de las naves se muestran en un formato especial (base 15) con símbolos personalizados.

5. **Single Page Application (SPA)**:
   - La aplicación se comporta como una SPA.

## Requisitos Técnicos

- **Nuxt.js**: Framework de Vue.js para crear aplicaciones universales.
- **Vuex**: Estado centralizado para manejar los datos de las naves y pilotos.


# Nuxt Minimal Starter

Look at the [Nuxt documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.

## Setup

Make sure to install dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install

# bun
bun install
```

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev

# bun
bun run dev
```

## Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm build

# yarn
yarn build

# bun
bun run build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm preview

# yarn
yarn preview

# bun
bun run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.
