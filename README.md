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

# ESLint + Prettier

### 1. Añadimos ESLint como dependencia

```bash
yarn add --dev eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-plugin-react
```

### 2. Iniciamos el setup del archivo de configuración

Lo iniciaremos con la siguiente configuración:

- How would you like to use ESLint? **> To check syntax, find problems, and enforce code style**
- What type of modules does your project use? **> JavaScript modules (import/export)**
- Which framework does your project use? **> React**
- Does your project use TypeScript? **> Yes**
- Where does your code run? **> a** _(browser, node)_
- How would you like to define a style for your project? **> Inspect your JavaScript file(s)**
- Which file(s), path(s), or glob(s) should be examined? **> .**
- What format do you want your config file to be in? **> JSON**

Una vez hecha la configuración, nos creará dos archivos .eslintrc.json y package-lock.json. Eliminaremos el archivo package-lock.json. Este archivo pertenece a NPM, que es el gestor de paquetes que utiliza el setup de configuración de ESLint, y lo eliminaremos porque nosotros ya instalamos los paquetes anteriormete con yarn.

También crear el archivo .eslintrc y copiar y pegar el siguiente contenido:

```json
{
  "env": {
    "browser": true,
    "es2020": true,
    "node": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 11,
    "sourceType": "module"
  },
  "plugins": ["react", "@typescript-eslint"],
  "rules": {}
}
```

### 3. Instalar otros plugins de ESLint

Instalaremos otros plugins adicionales de ESLint que nos serán de ayuda a la hora de desarrollar con React

Vamos a instalar el [plugin de los hooks](https://es.reactjs.org/docs/hooks-rules.html) para cumplir correctamente con las reglas de los Hooks:

```bash
``yarn add --dev eslint-plugin-react-hooks
```

Y lo añadiremos dentro de las reglas del linter, en _eslintrc_:

```json
"extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended"
  ],
```

### 4. Instalar prettier

```
yarn add --dev prettier eslint-config-prettier eslint-plugin-prettier
```

Como en ESLint, prettier necesita un archivo de configuración. Para ello, crearemos el archivo _.prettierrc_ en la raíz de nuestro proyecto con la siguiente configuración:

```json
{
  "jsxBracketSameLine": true,
  "singleQuote": true
}
```

Comprobamos que funciona con el comando `npx prettier . --check`.
Esto nos dará la lista de archivos que no han pasado por prettier. Como se puede observar, también incluye la carpeta .next. La excluiremos a continuación.

Añadiremos el archivo _.prettierignore_ a la raíz de nuestro proyecto, ya que este no se puede sustituir en el package.json tal y como dice la documentación. Ignoraremos la carpeta .next/ , ya que la carpeta node_modules ya la ignora por defecto, al igual que el ESLint.

```json
.next/
```

Volvemos a comprobar los archivos con el comando `npx prettier . —check` y nos volverá a dar la lista con los archivos que no han pasado por prettier. Si tenemos la extensión de VSCode podemos ir formateandolos archivo por archivo además de que si tenemos activada la opción de autoguardado al guardar, con Ctrl + S bastará. Y si queremos sobreescribirlos todos, ejecutaremos el comando: `npx prettier . --write`.

### 5. Integrar Prettier con ESLint

Añadiremos las dos dependencias anteriores a las reglas de ESLint en el archivo _eslintrc_:

```json
"extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended",
    "plugin:prettier/recommended",
    "prettier/@typescript-eslint"
  ],
```

### 6. Configurar reglas pre-commit

Una vez que ya tenemos integrado prettier con el linter, pasaremos a añadirlo a nuestros scripts dentro del _package.json_ para poder ejecutarlo desde el cli:

```json
"scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint . --fix"
  }
```

Configuraremos reglas precommit para asegurarnos de que todos los commits tienen el mismo formateo.
Instalamos lint-staged con `bash npx mrm lint-staged`

Este comando nos creará la configuración de husky y lint-staged. Lo que haremos será copiarnos la siguiente configuración para poder soportar TypeScript:

```json
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
```

### 7. Factorizar la raíz del proyecto

Para tener más limpio la raíz del proyecto vamos a integrar el archivo _.eslintrc_ y _.prettierrc_ dentro de nuestro _package.json_:

```json
"eslintConfig": {
    "root": true,
    "env": {
      "browser": true,
      "es2020": true,
      "node": true
    },
    "extends": [
      "eslint:recommended",
      "plugin:react/recommended",
      "plugin:@typescript-eslint/recommended",
      "plugin:react-hooks/recommended",
      "plugin:prettier/recommended",
      "prettier/@typescript-eslint"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
      "ecmaFeatures": {
        "jsx": true
      },
      "ecmaVersion": 11,
      "sourceType": "module"
    },
    "plugins": [
      "react",
      "@typescript-eslint"
    ],
    "rules": {
      "react/react-in-jsx-scope": "off",
      "react/prop-types": "off",
      "@typescript-eslint/no-unused-vars": [
        "error"
      ],
      "@typescript-eslint/explicit-function-return-type": [
        "error"
      ]
    },
    "settings": {
      "react": {
        "version": "detect"
      }
    }
  },
  "prettier": {
    "jsxBracketSameLine": true,
    "singleQuote": true
  }
```

Una vez tenemos esta configuración añadida al _package.json_, eliminaremos los dos archivos mencionados, ya que estos tendrán prioridad sobre lo que exista en el _package.json_.

Nota: el archivo **.prettierignore** no es posible incluirlo.

### 8. Añadir algunas reglas

Añadiremos algunas reglas útiles dentro de la key eslintConfig en el package.json que nos servirán para nuestro proyecto en particular de NextJS:

```json
"rules": {
      "react/react-in-jsx-scope": "off",
      "react/prop-types": "off",
      "@typescript-eslint/no-unused-vars": [
        "error"
      ],
      "@typescript-eslint/explicit-function-return-type": [
        "error"
      ]
    }
```
