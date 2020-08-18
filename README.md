# 💻 My NextJS App

## 📝 Características

- Utilizando el framework NextJS
- Codificado en TypeScript
- Linter ESLint
- Formateo con Prettier

## Añadir soporte para TypeScript

### 1. Crear proyecto

```bash
yarn create next-app
```

### 2. Añadir TypeScript a NextJS

Deberemos cambiar la extensión de los ficheros de .js a .tsx

**\_app.tsx**
Puedes ver un ejemplo de este archivo en la [documentación](https://nextjs.org/docs/basic-features/typescript#custom-app)

**index.tsx**
Puedes encontrar un ejemplo [aquí](pages/index.tsx)

### 3. Arrancar el proyecto en modo desarrollo

```bash
yarn dev
```

Una vez ejecutado el comando, no nos dejará arrancar el proyecto y nos devolverá un error mencionando que deberemos añadir las dependencias relacionadas con TypeScript:

```bash
yarn add --dev typescript @types/react @types/node
```

Una vez realizado esto, arrancamos el proyecto de nuevo con `yarn dev`.
NextJS detectará que estamos usando TypeScript y nos creará automáticamente los archivos [tsconfig.json](tsconfig.json) y [next-env.d.ts](next-env.d.ts).
Puedes leer más en la [documentación](https://nextjs.org/docs/advanced-features/module-path-aliases)
