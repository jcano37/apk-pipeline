# 🚀 Configuración de GitHub Actions para Build Automático de APK

Este proyecto incluye dos workflows de GitHub Actions para generar automáticamente APKs de Android:

## 📋 Workflows Disponibles

### 1. `build-apk.yml` - Build Básico
- **Trigger**: Push a `main` o Pull Request
- **Funcionalidad**: 
  - Compila APK Debug y Release
  - Sube artefactos descargables
  - Crea releases automáticos

### 2. `build-signed-apk.yml` - Build Avanzado con Firma
- **Trigger**: Tags `v*` o ejecución manual
- **Funcionalidad**:
  - Ejecuta tests unitarios
  - Compila APKs firmados
  - Crea releases con información detallada

## ⚙️ Configuración Inicial

### Paso 1: Habilitar GitHub Actions y Permisos
1. Ve a tu repositorio en GitHub
2. Navega a la pestaña **Actions**
3. Habilita GitHub Actions si no está activado
4. Ve a **Settings** → **Actions** → **General**
5. En **Workflow permissions**, selecciona **Read and write permissions**
6. Marca **Allow GitHub Actions to create and approve pull requests**

### Paso 2: Configurar Secrets (Opcional - Para APK Firmado)
Para generar APKs firmados, configura estos secrets en GitHub:

1. Ve a **Settings** → **Secrets and variables** → **Actions**
2. Añade los siguientes secrets:

```
KEYSTORE_BASE64     # Tu keystore codificado en base64
KEYSTORE_PASSWORD   # Contraseña del keystore
KEY_ALIAS          # Alias de la clave
KEY_PASSWORD       # Contraseña de la clave
```

#### Generar Keystore (si no tienes uno):
```bash
keytool -genkey -v -keystore release-key.keystore -alias key0 -keyalg RSA -keysize 2048 -validity 10000
```

#### Convertir Keystore a Base64:
```bash
base64 -i release-key.keystore | pbcopy  # macOS
base64 -i release-key.keystore           # Linux
certutil -encode release-key.keystore keystore.txt  # Windows
```

## 🎯 Cómo Usar

### Build Automático (build-apk.yml)
1. Haz push a la rama `main`
2. El workflow se ejecutará automáticamente
3. Los APKs estarán disponibles en:
   - **Artifacts**: En la página del workflow
   - **Releases**: Se creará un release automático

### Build Firmado (build-signed-apk.yml)
1. **Opción 1 - Con Tag**:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. **Opción 2 - Manual**:
   - Ve a **Actions** → **Build Signed APK**
   - Haz clic en **Run workflow**
   - Selecciona si crear un release

## 📱 Descargar APKs

### Desde Artifacts
1. Ve a **Actions** → Selecciona el workflow ejecutado
2. Scroll hacia abajo hasta **Artifacts**
3. Descarga el APK deseado

### Desde Releases
1. Ve a la pestaña **Releases**
2. Selecciona la versión deseada
3. Descarga el APK desde **Assets**

## 🔧 Personalización

### Modificar Configuración de Build
Edita `app/build.gradle.kts` para:
- Cambiar versiones
- Modificar configuración de firma
- Ajustar optimizaciones

### Personalizar Workflows
Los archivos están en `.github/workflows/`:
- `build-apk.yml`: Build básico
- `build-signed-apk.yml`: Build avanzado

## 📊 Características del Pipeline

### ✅ Incluye:
- ☑️ Cache de Gradle para builds más rápidos
- ☑️ Compilación de APK Debug y Release
- ☑️ Subida automática de artefactos
- ☑️ Creación automática de releases
- ☑️ Ejecución de tests unitarios
- ☑️ Soporte para firma de APKs
- ☑️ Información detallada en releases

### 🛠️ Requisitos del Sistema:
- **Java**: 17 (configurado automáticamente)
- **Android SDK**: Última versión (configurado automáticamente)
- **Gradle**: Usa el wrapper incluido en el proyecto

## 🚨 Solución de Problemas

### Error de Permisos
Si el build falla por permisos de `gradlew`:
```bash
git update-index --chmod=+x gradlew
git commit -m "Fix gradlew permissions"
```

### Error de Firma
Si el APK release no se firma correctamente:
1. Verifica que todos los secrets estén configurados
2. Asegúrate de que el keystore esté en base64 correcto
3. Descomenta la línea de `signingConfig` en `build.gradle.kts`

### Error 403 al Crear Release
Si el workflow falla con error 403 al crear releases:
1. Ve a **Settings** → **Actions** → **General**
2. En **Workflow permissions**, selecciona **Read and write permissions**
3. Asegúrate de que **Allow GitHub Actions to create and approve pull requests** esté marcado
4. Los workflows ya incluyen los permisos necesarios (`contents: write`)

### Build Lento
Para acelerar los builds:
- Los workflows ya incluyen cache de Gradle
- Considera usar `org.gradle.parallel=true` en `gradle.properties`

## 📞 Soporte

Si tienes problemas:
1. Revisa los logs del workflow en GitHub Actions
2. Verifica que la configuración de Gradle sea correcta
3. Asegúrate de que todos los secrets estén configurados correctamente

