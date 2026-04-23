# Evidencia de despliegue - Pokedex Angular en Azure

## Fecha
23/04/2026

## Estudiante
Juan David Lopez

## Contexto de la evidencia
En esta evidencia documente el proceso real que segui para desplegar la aplicacion `pokedex-angular` en Azure Static Web Apps sin modificar el codigo fuerte de la aplicacion, cumpliendo los criterios del profesor.

## Descripcion general del despliegue
El despliegue que hice fue de una aplicacion frontend Angular publica, conectada a PokéAPI, usando GitHub como fuente y Azure Static Web Apps como hosting.

Este despliegue funciona asi:

1. Yo hago cambios de configuracion y documentacion en el repositorio.
2. Hago `push` a la rama `main`.
3. GitHub Actions ejecuta el workflow de Azure.
4. Azure compila y publica la app en la URL final.

## Arquitectura usada en este proyecto

### Componentes
- Cliente Angular compilado a archivos estaticos (`dist/pokedex-angular`).
- Azure Static Web Apps para publicar el frontend.
- GitHub Actions para CI/CD.
- Consumo de APIs externas:

```bash
https://pokeapi.co/api/v2
https://beta.pokeapi.co/graphql/v1beta
```

### Flujo de arquitectura
```bash
Developer -> GitHub (main) -> GitHub Actions -> Azure Static Web Apps -> URL publica
```

## Servicios en la nube utilizados

- Azure Static Web Apps (Plan Free).
- GitHub Actions.
- GitHub Secrets para el token de despliegue.

Secreto usado en el workflow:

```bash
AZURE_STATIC_WEB_APPS_API_TOKEN_BRAVE_WATER_0FE172B1E
```

## Como cree la cuenta de Azure (caso real)
No pude usar Azure for Students, entonces use el plan gratuito normal de Azure con mi tarjeta Nequi para activar la cuenta y los creditos promocionales.

Pasos que hice:

1. Entre a:

```bash
https://azure.microsoft.com/es-es/free
```

2. Hice clic en **Comenzar gratis**.
3. Inicie sesion con cuenta Microsoft.
4. Registre mi tarjeta Nequi como metodo de verificacion.
5. Azure me habilito el acceso al portal y creditos del plan gratuito.
6. Entre a:

```bash
https://portal.azure.com
```

## Configuracion de infraestructura

Estos son los valores que configure en Azure Static Web Apps:

- Tipo de recurso: `Static Web App`
- Nombre: `pokedex-angular`
- Plan: `Free`
- Region: `East US 2`
- Proveedor de codigo: `GitHub`
- Repositorio: `juandavidlopez3004-svg/pokedex`
- Rama: `main`
- Build preset: `Angular`
- Output location: `dist/pokedex-angular`

## Configuracion de seguridad aplicada
Sin tocar logica de negocio, configure headers en `staticwebapp.config.json`.

Headers aplicados:

- `Content-Security-Policy`
- `Strict-Transport-Security`
- `X-Content-Type-Options`
- `X-Frame-Options`
- `Referrer-Policy`
- `Permissions-Policy`

Tambien deje configurado fallback para rutas SPA:

```bash
"navigationFallback": {
  "rewrite": "/index.html"
}
```

## Comandos exactos que use para desplegar

### 1) Preparar proyecto
```bash
git clone https://github.com/juandavidlopez3004-svg/pokedex.git
cd pokedex-angular
node --version
npm --version
```

### 2) Instalar dependencias
```bash
npm install
```

Si salia error de Husky:

```bash
npm install --ignore-scripts
npm run prepare
```

### 3) Validar local antes del push
```bash
npm run lint
npm run test:cov
npm start
```

### 4) Enviar cambios al repositorio
```bash
git add .
git commit -m "docs: evidencia de despliegue en azure"
git push origin main
```

### 5) Validar compilacion de produccion
```bash
npm run build
```

## Configuracion CI/CD (real en el repo)

### Workflow de despliegue
Archivo:

```bash
.github/workflows/azure-static-web-apps-brave-water-0fe172b1e.yml
```

Accion usada:

```bash
Azure/static-web-apps-deploy@v1
```

Parametros de build/deploy detectados:

```bash
app_location: "/"
api_location: ""
output_location: "dist/pokedex-angular"
```

### Workflow de pruebas
Archivo:

```bash
.github/workflows/pipelines.yml
```

Comandos:

```bash
npm install
npm run test:cov
./node_modules/.bin/codecov
```

## Errores que tuve en el despliegue y como los solucione

### Error 1: fallo de Husky
Error visto:

```bash
husky - Git hooks failed
```

Solucion aplicada:

```bash
npm install --ignore-scripts
npm run prepare
```

### Error 2: archivo `staticwebapp.config.json` invalido
Error visto:

```bash
Could not read and deserialize the provided routes file
```

Solucion aplicada:

1. Corregi la estructura JSON.
2. Guarde el archivo en UTF-8.
3. Reintente despliegue con nuevo push.

### Error 3: comando `ng` no encontrado en proceso de build
Error visto:

```bash
ng: command not found
```

Solucion aplicada:

1. Verifique instalacion de dependencias del proyecto.
2. Revise configuracion del workflow de Azure.
3. Reejecute pipeline con configuracion correcta.

### Error 4: rutas internas con 404 al refrescar
Error visto:

```bash
404 Not Found al abrir /pokemon/25
```

Solucion aplicada:

```bash
"navigationFallback": {
  "rewrite": "/index.html"
}
```

## Errores reales que pueden volver a aparecer

1. Token de Azure vencido o mal configurado.
2. `output_location` diferente al `outputPath` de Angular.
3. Version de Node distinta entre local y CI.
4. Falla temporal de PokéAPI que parece error del despliegue.

## Soluciones detalladas para esos errores

### A) Token invalido o ausente
Validar secreto en GitHub Actions con nombre exacto:

```bash
AZURE_STATIC_WEB_APPS_API_TOKEN_BRAVE_WATER_0FE172B1E
```

Si falta, regenerarlo en Azure y actualizarlo en GitHub.

### B) Carpeta de salida mal configurada
Verificar que Angular construya en:

```bash
dist/pokedex-angular
```

Si cambia en `angular.json`, actualizar tambien el workflow.

### C) Diferencia de version de Node
Alinear local con CI (Node 18.x recomendado para este repo):

```bash
node --version
```

Si es necesario, cambiar version con nvm.

### D) Falla externa de PokéAPI
Probar conectividad:

```bash
curl https://pokeapi.co/api/v2/pokemon/1
```

Si este comando falla, el problema no esta en Azure.

## Validacion final del despliegue

### Validacion funcional
1. Abrir la URL publica.
2. Entrar a home y detalle de Pokemon.
3. Refrescar una ruta interna y confirmar que no haya 404.

URL final:

```bash
https://brave-water-0fe172b1e.7.azurestaticapps.net
```

### Validacion tecnica
- Workflow de Azure en estado exitoso.
- Workflow de pruebas/cobertura en estado exitoso.

### Validacion de seguridad
- Revisar headers HTTP en respuesta.
- Ejecutar escaneo de seguridad de headers.

## Restriccion del profesor cumplida
Durante esta actividad no modifique codigo fuerte (logica principal de la aplicacion). Los cambios fueron solo de despliegue, infraestructura, seguridad y documentacion.
