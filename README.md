# App de control de gastos 👋

Esta es una aplicación de control de gastos.

Dentro de esta aplicación se implementará el uso de la librería [Nativewind](https://www.nativewind.dev/) para agilizar el desarrollo.

## Empezar
1. Crear proyecto
    ```bash
    npx create-expo-app@latest
    ```

2. Instalar dependendia

   ```bash
   npm install
   ```

3. Instalar [nativewind](https://www.nativewind.dev/) y sus dependencias
    ```bash
    npm install nativewind react-native-reanimated react-native-safe-area-context
    npm install --dev tailwindcss@^3.4.17 prettier-plugin-tailwindcss@^0.5.11 babel-preset-expo
    ```

4. Configurar Tailwind Css

    Ejecuta el comando `npx tailwindcss init` para crear el archivo `tailwind.config.js`

    Añade las rutas de todos tus archivos de componentes en el archivo `tailwind.config.js.`


    ```tailwind.config.ts
    /** @type {import('tailwindcss').Config} */
    module.exports = {
    content: ["./App.tsx", "./app/**/*.{js,jsx,ts,tsx}", "./components/**/*.{js,jsx,ts,tsx}"],
    presets: [require("nativewind/preset")],
    theme: {
        extend: {},
    },
    plugins: [],
    }
    ```

    Crear archivo `global.css` y agregar las directivas de Tailwind
    ```
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

5. Agrega o configura el archivo **babel.config.js** :
    ```
    module.exports = function (api) {
        api.cache(true);
        return {
        presets: [
            ["babel-preset-expo", { jsxImportSource: "nativewind" }],
            "nativewind/babel",
        ],
        };
    };
    ```

6. Agrega o configura el archivo `metro.config.js`
 
    Crea el archivo `metro.config.js` en el directorio principal del proyecto sí no tienes creado uno y agrega la siguiente configuración:

    ```
    const { getDefaultConfig } = require("expo/metro-config");
    const { withNativeWind } = require('nativewind/metro');
    
    const config = getDefaultConfig(__dirname)
    
    module.exports = withNativeWind(config, { input: './global.css' })
    ```

7. Modifica el archivo `app.json` 
    
    Cambia el bundler para implementar el [Metro bundler](https://docs.expo.dev/guides/customizing-metro/#web-support)
    ```
    {
        "expo": {
            "web": {
            "bundler": "metro"
            }
        }
    }
    ```
8. Importar el archivo global css con importación de Tailwind

    En este caso lo agregaremos en un archivo donde se importe de forma general en toda la aplicación, con base al ruteo implementado por expo se hara en el archivo `app/_layout.tsx`

    ```
    import { DarkTheme, DefaultTheme, ThemeProvider } from '@react-navigation/native';
    import { Stack } from 'expo-router';
    import { StatusBar } from 'expo-status-bar';
    import 'react-native-reanimated';
    import "../global.css";

    import { useColorScheme } from '@/hooks/use-color-scheme';

    export const unstable_settings = {
    anchor: '(tabs)',
    };

    export default function RootLayout() {
    const colorScheme = useColorScheme();

        return (
            <ThemeProvider value={colorScheme === 'dark' ? DarkTheme : DefaultTheme}>
            <Stack>
                <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
                <Stack.Screen name="modal" options={{ presentation: 'modal', title: 'Modal' }} />
            </Stack>
            <StatusBar style="auto" />
            </ThemeProvider>
        );
    }

    ```

9. Iniciar el proyecto

   ```bash
   npx expo start --clear
   ```
