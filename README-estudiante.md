# Pokedex Angular - Evidencia de despliegue (estudiante)

## Estudiante
Juan David Lopez

## Descripcion del proyecto
En este laboratorio desplegue una aplicacion frontend en Angular que consume la API publica de Pokemon (PokéAPI) para mostrar:

- Listado de Pokemon.
- Vista de detalle por Pokemon.
- Navegacion por rutas (home, detalle, about, error, not-found).
- Estilos personalizados con SCSS.

Este proyecto me permitio practicar integracion de frontend con APIs, organizacion modular en Angular y despliegue continuo en Azure Static Web Apps con dominio personalizado.

## Tecnologias utilizadas (detectadas del codigo)

### Frontend
- Angular 14 (`@angular/core`, `@angular/router`, `@angular/forms`).
- TypeScript 4.6.
- SCSS.
- Apollo Angular + GraphQL (`apollo-angular`, `@apollo/client`, `graphql`).
- Font Awesome (`@fortawesome/*`).

### Calidad y desarrollo
- ESLint + Angular ESLint.
- Prettier.
- Husky + lint-staged.
- Karma + Jasmine para pruebas unitarias.

### CI/CD y nube
- GitHub Actions (workflow de pruebas y workflow de despliegue).
- Azure Static Web Apps (hosting).

## Requisitos previos
Antes de iniciar, valida que tengas instalado lo siguiente:

- Git (2.30 o superior recomendado).
- Node.js LTS 18.x (recomendado por el workflow CI).
- npm (incluido con Node.js).
- Angular CLI 14 (opcional global, porque tambien se puede ejecutar con `npx`).
- Cuenta de GitHub.
- Cuenta de Azure (puede ser Azure for Students).

Comandos de verificacion:

```bash
git --version
node --version
npm --version
npx ng version
```

## Crear cuenta en la nube (Azure Free con tarjeta Nequi)
En mi caso no utilice Azure for Students porque no me funciono. Cree la cuenta usando el plan gratuito de Azure con tarjeta Nequi.

1. Entre a la pagina oficial:

```bash
https://azure.microsoft.com/es-es/free/students
```

2. Hice clic en **Comenzar gratis**.
3. Inicie sesion con mi cuenta de Microsoft.
4. Registre mi tarjeta Nequi para completar la validacion de metodo de pago.
5. Azure me habilito los creditos promocionales gratuitos de la cuenta.
6. Accedi al portal para crear los recursos:

```bash
https://portal.azure.com
```

## Configuracion inicial del entorno (lo que hice)

1. Clone el repositorio:

```bash
git clone https://github.com/juandavidlopez3004-svg/pokedex.git
cd pokedex-angular
```

2. Instale dependencias:

```bash
npm install
```

3. Valide calidad de codigo antes de correr la app:

```bash
npm run lint
```

## Instalacion del proyecto
La instalacion la hice con `npm install`. Como en un intento tuve problema con scripts de post-instalacion, deje tambien la alternativa:

```bash
npm install --ignore-scripts
npm run prepare
```

> Nota de evidencia: este flujo alterno lo use cuando Husky genero error en la instalacion.

## Variables de entorno necesarias
Este proyecto no usa archivo `.env` por defecto. La configuracion esta declarada en:

- `src/environments/environment.ts`
- `src/environments/environment.prod.ts`

Valores relevantes detectados:

- `pokeApi`: `https://pokeapi.co/api/v2`
- `pokeApiGraphQL`: `https://beta.pokeapi.co/graphql/v1beta`
- `imagesPath`: `/assets/images`
- `language`: `en`

En este laboratorio no cree archivo `.env` ni cambie estos valores.

## Ejecutar el proyecto localmente

### Modo desarrollo
```bash
npm start
```

La app me quedo disponible en:

```bash
http://localhost:4200
```

### Ejecutar pruebas
```bash
npm test
```

### Ejecutar pruebas con cobertura (modo CI)
```bash
npm run test:cov
```

### Compilar para produccion
```bash
npm run build
```

## Evidencia del despliegue realizado

- Recurso creado en Azure: `Static Web App`.
- Repositorio conectado: `juandavidlopez3004-svg/pokedex`.
- Rama de despliegue: `main`.
- Build de salida usado: `dist/pokedex-angular`.
- Dominio personalizado configurado: `https://www.jimuax.app`.
- Proveedor del dominio: `NAME.COM`.
- Dominio adquirido con correo institucional de la universidad.
- URL final obtenida:

```bash
https://www.jimuax.app
```

## Errores que tuve durante el despliegue

### 1) Error con instalacion de Husky
Error que me aparecio:

```bash
husky - Git hooks failed
```

Como lo solucione:

```bash
npm install --ignore-scripts
npm run prepare
```

### 2) Error en `staticwebapp.config.json`
Error que me aparecio:

```bash
Could not read and deserialize the provided routes file
```

Como lo solucione:

- Revise el JSON para dejarlo valido.
- Guarde el archivo correctamente en UTF-8.
- Hice nuevo commit y push para reintentar el despliegue.

### 3) Error por version/entorno de build
Error que me aparecio en pruebas de pipeline:

```bash
ng: command not found
```

Como lo solucione:

- Verifique dependencias y scripts del proyecto.
- Reintente el pipeline con la configuracion correcta del workflow de Azure.

## Restriccion aplicada del profesor
Durante todo el proceso no modifique el codigo fuerte (logica principal de la app). Solo realice cambios de configuracion y despliegue:

- `staticwebapp.config.json`
- Archivos de workflow en `.github/workflows/`
- Ajustes de entorno de ejecucion e instalacion

## Buenas practicas que aplique

1. Ejecute lint y pruebas antes de hacer `push`.
2. No subi secretos ni tokens al repositorio.
3. Mantuve consistencia de version de Node.js entre local y CI.
4. Revise `staticwebapp.config.json` cada vez que toque rutas o headers.
5. Mantuve `package-lock.json` versionado para builds reproducibles.

## Recursos del proyecto

- Repositorio: `https://github.com/juandavidlopez3004-svg/pokedex`
- Aplicacion desplegada: `https://www.jimuax.app`
