# DevSecOps Demo test2

Aplicación demo preparada como **Secure Development Template** para el laboratorio DevSecOps.

## Carga inicial en VM01

Traslade el ZIP a VM01 y ejecute:

```bash
unzip devsecops-demo-ready-secret-scan.zip
cd devsecops-demo-ready
npm install
npm run format
npm run format:check
npm run lint
npm test
npm start
```

Pruebe: `/`, `/health`, `/api/users`, `/metrics`.

`npm install` genera `package-lock.json`. Conserve y suba ese archivo a Git antes de usar `npm ci` o construir la imagen Docker.

## Pre-commit de seguridad

El baseline incluye `.husky/pre-commit` con tres controles locales:

1. ESLint.
2. Pruebas unitarias Jest.
3. Detección de secretos con Trivy.

El hook ejecuta:

```bash
npm run lint
npm test
trivy fs --scanners secret --exit-code 1 .
```

Por tanto, **Trivy debe estar instalado y disponible en PATH antes del primer commit**. Si Trivy no existe, el hook falla de forma intencional. No use `git commit --no-verify` para evadir el baseline.

El secret scanning también debe repetirse en Jenkins. El pre-commit es un control Shift Left para feedback temprano, mientras que CI constituye el control central porque un hook local puede ser omitido.

## Activación de Husky

Después de `npm install`, confirme que el hook sea ejecutable:

```bash
chmod +x .husky/pre-commit
```

Si el proyecto todavía no es un repositorio Git, inicialícelo antes del primer commit:

```bash
git init
git branch -M main
```
