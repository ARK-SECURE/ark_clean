# ARK CLEAN v3.2

Limpiador de **metadatos ultrarrápido** con **detección de amenazas avanzada** para archivos de cualquier tipo. Compilado a WebAssembly para máximo rendimiento y compatibilidad universal.

[![npm version](https://img.shields.io/npm/v/ark_clean.svg)](https://www.npmjs.com/package/ark_clean)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Rust](https://img.shields.io/badge/Built%20with-Rust-CE422B)](https://www.rust-lang.org/)
[![WebAssembly](https://img.shields.io/badge/Compiled%20to-WebAssembly-654FF0)](https://webassembly.org/)

---

## ¿Qué es ARK CLEAN v3.2?

ARK CLEAN es un **sanitizador de archivos con enfoque en privacidad y seguridad** que elimina metadatos, detecta amenazas y verifica integridad de archivos. Construido en **Rust puro** y compilado a **WebAssembly**, se ejecuta a velocidad casi nativa en cualquier entorno JavaScript/Node.js.

### Cambios en v3.2

- ✨ **Soporte DOCX/XLSX/PPTX**: Eliminación inteligente de metadatos en archivos Office (ZIP parsing interno)
- ⚡ **GZIP selectivo**: Compresión automática en archivos TEXT y FINANCIAL (solo si reduce tamaño)
- 🔧 **Análisis de entropía mejorado**: Detección de malware sin red neuronal (50% más rápido)
- 🏦 **Soporte financiero**: Procesamiento seguro de archivos ACH y MT103 (SWIFT)
- 📊 **Verificación de integridad**: Garantía de que archivos no se corrompen al limpiar
- ✅ **100% sin warnings**: Compilación limpia con `#![deny(warnings)]`

### Zero-Knowledge Architecture

ARK CLEAN opera bajo una **arquitectura de conocimiento cero**:

- **Sin conexión externa**: Todo el procesamiento ocurre localmente en tu navegador o servidor
- **Sin envío de datos**: Tus archivos nunca abandonan tu máquina
- **Sin servidores centrales**: No hay servidores backend, APIs remotas ni almacenamiento en la nube
- **Sin registro de datos**: No se registran logs, cookies ni identificadores
- **Privacidad garantizada**: El código fuente está disponible para auditoría pública
- **Funciona offline**: Ejecuta perfectamente sin conexión a internet

El WASM se compila directamente en tu navegador o proceso Node.js. Todo sucede **en memoria, sin intermediarios**.

---

## Características Principales

- **🛡️ Seguridad robusta**: Detección de firmas de malware, análisis de entropía y comportamiento sospechoso
- **🔒 Eliminación de metadatos**: EXIF, IPTC, XMP, comentarios, docProps y datos sensibles
- **⚡ Ultrarrápido**: Procesa archivos en milisegundos (threshold de 8KB)
- **🌐 Universal**: Navegadores, Node.js, React, Vue, Angular, Electron
- **📊 Reportes detallados**: Puntuación de seguridad (0-100%), nivel de amenaza, reducción de tamaño
- **🔍 Multi-formato**: JPEG, PNG, PDF, DOCX, XLSX, PPTX, ACH, MT103, TXT, JSON, y más
- **🔄 Procesamiento paralelo**: Maneja hasta 10 archivos simultáneamente

---

## Instalación

### NPM

```bash
npm install ark_clean
```

### Yarn

```bash
yarn add ark_clean
```

### PNPM

```bash
pnpm add ark_clean
```

---

## Inicio Rápido

### Navegador (HTML + ES Modules)

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>ARK CLEAN v3.2</title>
  <style>
    body {
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
      max-width: 900px;
      margin: 28px auto;
      padding: 18px;
    }
    h1 { font-size: 24px; margin-bottom: 12px; }
    .card {
      border: 1px solid #e6e6e6;
      border-radius: 8px;
      padding: 16px;
      margin: 16px 0;
      background: #fafafa;
    }
    button {
      padding: 10px 16px;
      border-radius: 6px;
      border: none;
      background: #0b74de;
      color: white;
      cursor: pointer;
      font-weight: 500;
    }
    button:hover { background: #0956a8; }
    button[disabled] { opacity: 0.5; cursor: not-allowed; }
    input[type="file"] { padding: 8px; }
    .row { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
    .file-list { margin-top: 12px; }
    .file-item { padding: 8px; border-left: 3px solid #0b74de; background: white; margin: 4px 0; }
    .result-item {
      padding: 12px;
      margin: 8px 0;
      border-left: 4px solid #0b74de;
      background: white;
      border-radius: 4px;
    }
    .result-item.success { border-left-color: #28a745; }
    .result-item.failed { border-left-color: #dc3545; }
    .result-item.warning { border-left-color: #ffc107; }
    pre {
      background: #1e1e1e;
      color: #d4d4d4;
      padding: 12px;
      border-radius: 6px;
      overflow-x: auto;
      max-height: 300px;
      font-size: 12px;
    }
    .download {
      display: inline-block;
      margin-left: 12px;
      padding: 4px 8px;
      background: #28a745;
      color: white;
      text-decoration: none;
      border-radius: 4px;
      font-size: 12px;
    }
    .download:hover { background: #218838; }
    .badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 12px;
      font-size: 11px;
      font-weight: bold;
      margin: 0 4px;
    }
    .badge.safe { background: #d4edda; color: #155724; }
    .badge.low { background: #d1ecf1; color: #0c5460; }
    .badge.medium { background: #fff3cd; color: #856404; }
    .badge.high { background: #f8d7da; color: #721c24; }
    .badge.critical { background: #dc3545; color: white; }
  </style>
</head>
<body>
  <h1>ARK CLEAN v3.2</h1>
  <p>Selecciona hasta 10 archivos para procesar. ARK CLEAN elimina metadatos y detecta amenazas de seguridad.</p>

  <div class="card">
    <div class="row">
      <input id="fileInput" type="file" multiple />
      <button id="validateBtn">Validar</button>
      <button id="processBtn">Procesar</button>
      <button id="clearBtn">Limpiar</button>
    </div>
    <div class="file-list" id="fileList"></div>
  </div>

  <div class="card">
    <h3>Resultados</h3>
    <div id="results"></div>
  </div>

  <div class="card">
    <h3>Consola</h3>
    <pre id="log">Inicializando...</pre>
  </div>

  <script type="module">
    import init, { WasmFileProcessor } from './node_modules/ark_clean/ark_clean.js';

    const logEl = document.getElementById('log');
    const fileInput = document.getElementById('fileInput');
    const validateBtn = document.getElementById('validateBtn');
    const processBtn = document.getElementById('processBtn');
    const clearBtn = document.getElementById('clearBtn');
    const fileListEl = document.getElementById('fileList');
    const resultsEl = document.getElementById('results');

    function log(...args) {
      const msg = args.map(a => typeof a === 'object' ? JSON.stringify(a, null, 2) : String(a)).join(' ');
      console.log(...args);
      logEl.textContent += '\n' + msg;
      logEl.scrollTop = logEl.scrollHeight;
    }

    let processor;
    let selectedFiles = [];

    async function boot() {
      try {
        await init();
        processor = new WasmFileProcessor();
        log('✅ WASM inicializado - ARK CLEAN v3.2 listo');
      } catch (err) {
        log('❌ Error al inicializar:', err.message);
      }
    }

    fileInput.addEventListener('change', (e) => {
      selectedFiles = Array.from(e.target.files).slice(0, 10);
      renderFileList();
      log(`📁 ${selectedFiles.length} archivo(s) seleccionado(s)`);
    });

    clearBtn.addEventListener('click', () => {
      selectedFiles = [];
      fileInput.value = '';
      resultsEl.innerHTML = '';
      renderFileList();
      log('🗑️ Selección limpiada');
    });

    validateBtn.addEventListener('click', () => {
      if (!processor) { log('❌ Processor no inicializado'); return; }
      if (selectedFiles.length === 0) { log('⚠️ No hay archivos seleccionados'); return; }

      resultsEl.innerHTML = '';
      log(`🔍 Validando ${selectedFiles.length} archivo(s)...`);

      selectedFiles.forEach(file => {
        try {
          const { valid, fileType } = processor.validate_file(file.name, file.size);
          const status = valid ? '✅' : '❌';
          log(`${status} ${file.name} - Tipo: ${fileType} (${file.size} bytes)`);

          const div = document.createElement('div');
          div.className = `result-item ${valid ? 'success' : 'failed'}`;
          div.innerHTML = `
            <strong>${file.name}</strong>
            <span class="badge">${fileType}</span>
            <br>
            <small>${valid ? 'Válido para procesar' : 'No válido'} - ${file.size} bytes</small>
          `;
          resultsEl.appendChild(div);
        } catch (err) {
          log(`❌ Error validando ${file.name}: ${err.message}`);

          const div = document.createElement('div');
          div.className = 'result-item failed';
          div.innerHTML = `<strong>${file.name}</strong><br><small style="color:red">${err.message}</small>`;
          resultsEl.appendChild(div);
        }
      });
    });

    processBtn.addEventListener('click', async () => {
      if (!processor) { log('❌ Processor no inicializado'); return; }
      if (selectedFiles.length === 0) { log('⚠️ No hay archivos seleccionados'); return; }

      processBtn.disabled = true;
      resultsEl.innerHTML = '';
      log(`⏳ Procesando ${selectedFiles.length} archivo(s)...`);

      try {
        const filesForWasm = [];
        for (const file of selectedFiles) {
          const buf = await file.arrayBuffer();
          const u8 = new Uint8Array(buf);
          filesForWasm.push({ name: file.name, data: u8 });
        }

        const results = processor.process_files(filesForWasm);
        log(`✅ Procesamiento completado - ${results.length} resultado(s)`);
        renderResults(results);
      } catch (err) {
        log(`❌ Error: ${err.message}`);
      } finally {
        processBtn.disabled = false;
      }
    });

    function renderFileList() {
      fileListEl.innerHTML = '';
      if (selectedFiles.length === 0) {
        fileListEl.textContent = 'Ningún archivo seleccionado';
        return;
      }
      selectedFiles.forEach((f, i) => {
        const div = document.createElement('div');
        div.className = 'file-item';
        div.innerHTML = `${i + 1}. ${f.name} <small>(${(f.size / 1024).toFixed(2)} KB)</small>`;
        fileListEl.appendChild(div);
      });
    }

    function renderResults(results) {
      resultsEl.innerHTML = '';
      results.forEach((r, i) => {
        const div = document.createElement('div');
        const threatColor = {
          'Safe': 'safe',
          'Low': 'low',
          'Medium': 'medium',
          'High': 'high',
          'Critical': 'critical'
        }[r.threatLevel] || 'medium';

        div.className = `result-item ${r.success ? 'success' : r.threatLevel === 'Critical' ? 'failed' : 'warning'}`;

        const score = Math.round(r.securityScore * 100);
        const ext = r.originalName.match(/(\.[^.]+)$/) ? r.originalName.match(/(\.[^.]+)$/)[0] : '';

        div.innerHTML = `
          <div style="display: flex; justify-content: space-between; align-items: center;">
            <strong>${r.originalName}</strong>
            <span class="badge ${threatColor}">${r.threatLevel}</span>
          </div>
          <div style="margin: 8px 0; font-size: 13px;">
            <div>Tipo: ${r.fileType}</div>
            <div>Seguridad: <strong>${score}%</strong></div>
            <div>Integridad: ${r.integrityVerified ? '✅ Verificada' : '❌ Falló'}</div>
            <div>Reducción: ${r.sizeReduction} bytes</div>
            <div>Estado: ${r.success ? '<span style="color: green;">✓ Éxito</span>' : '<span style="color: red;">✗ Falló</span>'}</div>
            ${r.message ? `<div style="color: #666; font-size: 12px; margin-top: 4px;">${r.message}</div>` : ''}
          </div>
          ${r.cleanedData && r.cleanedData.length > 0 ? `
            <a href="#" class="download" onclick="downloadFile(event, ${i})">
              📥 Descargar limpio
            </a>
          ` : ''}
        `;
        resultsEl.appendChild(div);
      });

      window.resultsData = results;
    }

    window.downloadFile = (event, index) => {
      event.preventDefault();
      const r = window.resultsData[index];
      const blob = new Blob([r.cleanedData.buffer || r.cleanedData]);
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      const ext = r.originalName.match(/(\.[^.]+)$/) ? r.originalName.match(/(\.[^.]+)$/)[0] : '';
      a.download = r.originalName.replace(/\.[^.]+$/, '_clean') + ext;
      a.click();
      URL.revokeObjectURL(url);
    };

    boot();
  </script>
</body>
</html>
```

### Node.js / React / Vue / Angular

**Opción 1: Con import dinámico (Recomendado)**

```javascript
import init, { WasmFileProcessor } from 'ark_clean';
import wasmUrl from 'ark_clean/ark_clean_bg.wasm?url';

async function limpiarArchivos(listaArchivos) {
  // Inicializar WASM (una sola vez)
  await init(wasmUrl);
  
  // Crear procesador
  const processor = new WasmFileProcessor();
  
  // Preparar archivos
  const filesData = listaArchivos.map(file => ({
    name: file.name,
    data: file.buffer // Uint8Array
  }));
  
  try {
    // Procesar (máximo 10 archivos)
    const results = processor.process_files(filesData);
    
    // Procesar resultados
    results.forEach(result => {
      console.log(`${result.originalName}:`);
      console.log(`  Seguridad: ${Math.round(result.securityScore * 100)}%`);
      console.log(`  Amenaza: ${result.threatLevel}`);
      console.log(`  Integridad: ${result.integrityVerified ? 'Verificada' : 'Falló'}`);
      console.log(`  Reducción: ${result.sizeReduction} bytes`);
      
      if (result.success && result.cleanedData.length > 0) {
        // Guardar archivo limpio
        const blob = new Blob([result.cleanedData]);
        saveToFile(blob, generateCleanFilename(result.originalName));
      }
    });
    
    return results;
  } catch (error) {
    console.error('ARK CLEAN Error:', error.message);
    throw error;
  }
}

function generateCleanFilename(original) {
  const ext = original.match(/(\.[^.]+)$/)?.[0] || '';
  return original.replace(/\.[^.]+$/, '_clean') + ext;
}
```

**Opción 2: Con CommonJS (Node.js legacy)**

```javascript
const { WasmFileProcessor } = require('ark_clean');

async function procesar(archivos) {
  const processor = new WasmFileProcessor();
  const filesData = archivos.map(f => ({
    name: f.name,
    data: f.buffer
  }));
  
  return processor.process_files(filesData);
}
```

---

## Referencia API

### `WasmFileProcessor`

#### Constructor

```javascript
const processor = new WasmFileProcessor();
```

#### Método: `process_files(filesData)`

Procesa 1-10 archivos simultáneamente.

**Parámetros:**

```typescript
filesData: Array<{
  name: string,        // Nombre con extensión
  data: Uint8Array     // Datos binarios
}>
```

**Retorna:**

```typescript
Array<{
  originalName: string,          // Nombre original
  success: boolean,              // Éxito del procesamiento
  message: string,               // Descripción
  fileType: string,              // Tipo detectado
  securityScore: number,         // 0.0-1.0
  threatLevel: string,           // "Safe"|"Low"|"Medium"|"High"|"Critical"
  integrityVerified: boolean,    // Verificación de integridad
  sizeReduction: number,         // Bytes ahorrados
  cleanedData: Uint8Array        // Archivo limpio
}>
```

**Lanza error si:**
- < 1 o > 10 archivos
- Archivo > 500 MB
- Archivo es script bloqueado

#### Método: `validate_file(fileName, fileSize)`

Valida antes de procesar.

**Parámetros:**
- `fileName` (string): Nombre con extensión
- `fileSize` (number): Tamaño en bytes

**Retorna:**

```typescript
{
  valid: boolean,
  fileType: string
}
```

---

## Formatos Soportados

| Categoría | Extensiones | Procesamiento |
|-----------|-------------|---------------|
| **Imágenes** | jpg, jpeg, png, gif, bmp, tiff, webp, heic | Elimina metadatos + Verifica integridad |
| **Office** | docx, xlsx, pptx | Elimina docProps, _rels, [Content_Types].xml |
| **Documentos** | pdf, doc, xls | Escaneo de seguridad |
| **Texto** | txt, log, json, xml, csv, ini, yaml, yml | Redacta datos + Comprime GZIP |
| **Financiero** | ach, mt103 | Limpia comentarios + Comprime GZIP |
| **Multimedia** | mp3, wav, mp4, mov, etc. | Escaneo de seguridad |
| **Archivos** | zip, rar, 7z, tar, gz, bz2 | Escaneo de seguridad |
| **Bases de Datos** | sql, sqlite, db, mdb, accdb | Escaneo de seguridad |
| **⛔ Scripts** | js, py, php, sh, bat, ps1, rb, pl | **BLOQUEADO** |
| **⛔ Ejecutables** | exe, dll, bin, sys, so | **BLOQUEADO** |

---

## Detección de Amenazas

### Niveles de Amenaza

| Puntuación | Nivel | Descripción |
|-----------|-------|-------------|
| ≥ 95% | **Safe** | Sin amenazas |
| 80-94% | **Low** | Anomalías menores |
| 60-79% | **Medium** | Patrones sospechosos |
| 40-59% | **High** | Probablemente malicioso |
| < 40% | **Critical** | Malware confirmado |

### Métodos de Detección

1. **Firmas de malware**: MZ, ELF, scripts `eval()`, shells
2. **Análisis de entropía**: Detecta datos comprimidos/encriptados anormales
3. **Redacción automática**: Rutas, claves API, contraseñas, secretos

### Redacción de Datos (TEXT/FINANCIAL)

```
Rutas:        C:\Users\Admin\data.txt    → [PATH]\data.txt
Claves:       sk_live_abc123            → [REDACTED]
Secretos:     password, token, secret   → [REDACTED]
Rutas Linux:  /var/lib/app.db           → [PATH]/app.db
```

---

## Ejemplos de Uso

### Validación Pre-procesamiento

```javascript
const processor = new WasmFileProcessor();

try {
  const { valid, fileType } = processor.validate_file('documento.pdf', 2048000);
  console.log(`Válido: ${valid}, Tipo: ${fileType}`);
} catch (error) {
  console.error(`Error: ${error.message}`);
}
```

### Manejo de Amenazas

```javascript
const results = processor.process_files(filesData);

results.forEach(result => {
  if (result.threatLevel === 'Critical') {
    console.warn(`⚠️ ALERTA: ${result.originalName} contiene malware`);
    // No descargar, reportar
  } else if (!result.integrityVerified) {
    console.error(`❌ Integridad falló: ${result.originalName}`);
    // Usar original
  }
});
```

### Procesamiento por Lotes

```javascript
async function procesarMuchosArchivos(archivos) {
  const processor = new WasmFileProcessor();
  const lotes = [];

  // Dividir en lotes de 10
  for (let i = 0; i < archivos.length; i += 10) {
    const lote = archivos.slice(i, i + 10);
    const filesData = await Promise.all(
      lote.map(async (file) => ({
        name: file.name,
        data: new Uint8Array(await file.arrayBuffer())
      }))
    );

    const resultados = processor.process_files(filesData);
    lotes.push(resultados);
  }

  return lotes.flat();
}
```

---

## Cambios de v2.0 a v3.2

### Nuevas Características

- Soporte DOCX/XLSX/PPTX con parseo ZIP interno
- Compresión GZIP selectiva para TEXT/FINANCIAL
- Análisis de entropía optimizado (50% más rápido)
- Soporte para archivos ACH y MT103 (SWIFT)
- Verificación de integridad mejorada

### Mejoras de Rendimiento

- Escaneo limitado a 8 KB
- Eliminación de red neuronal (análisis determinista)
- Compilación con LTO activado
- WASM optimizado a `-z`

### Correcciones

- 100% sin warnings
- Uso de `.contains()` en lugar de operadores manuales
- `.div_ceil()` para cálculos correctos
- Imports corregidos para npm

---

## Configuración y Límites

- Máximo 10 archivos por lote
- Tamaño máximo: 500 MB
- Mínimo: 1 archivo
- Tipos bloqueados: Scripts, ejecutables
- Procesamiento: Paralelo interno

---

## Compatibilidad

- Node.js: 14+
- Navegadores: Todos con WASM
- Frameworks: React, Vue, Angular, Svelte, Next.js
- Plataformas: Windows, macOS, Linux

---

## Solución de Problemas

| Problema | Solución |
|----------|----------|
| "WASM module not initialized" | Llama `await init(wasmUrl)` primero |
| "Maximum 10 files allowed" | Procesa en lotes ≤10 |
| "File too large" | Máximo 500 MB |
| "Scripts blocked" | No procesa .js, .py, .php, etc. |
| Import falla en HTML | Usa ruta correcta: `./node_modules/ark_clean/ark_clean.js` |

---

## Performance

| Operación | Tiempo | Nota |
|-----------|--------|------|
| Inicialización | ~50ms | Una sola vez |
| Archivo 1 MB | ~2ms | Texto limpio |
| Archivo 10 MB | ~10ms | Imagen con metadatos |
| Lote 10 archivos | ~30ms | Paralelo interno |
| Detección malware | <1ms | Por archivo |

**Tamaño WASM**: ~2.5 MB (brotli comprimido)

---

## Verificación de Integridad

Para verificar que tu instalación es auténtica, compara el hash SHA256:

```
SHA256 (ark_clean_bg.wasm):
9C3C906DC673F52E71A547CDF89BCC1DB3474BC9435B00C81D3982014F82E199
```

En Windows:
```powershell
Get-FileHash node_modules/ark_clean/ark_clean_bg.wasm -Algorithm SHA256
```

En macOS/Linux:
```bash
shasum -a 256 node_modules/ark_clean/ark_clean_bg.wasm
```

---

## Licencia

MIT - Libre para usar en proyectos comerciales y privados

---

## Soporte

- **GitHub**: [arkteam/ark_clean](https://github.com/ARK-SECURE/ark_clean.git)
- **Issues**: Reporta bugs o solicita características

---

## Créditos

Construido con:
- **Rust**: Lenguaje seguro y rápido
- **WebAssembly**: Compilación universal
- **wasm-bindgen**: Interoperabilidad JS/WASM

**Versión**: 3.2.0  
**Mantenedor**: ARK SECURE