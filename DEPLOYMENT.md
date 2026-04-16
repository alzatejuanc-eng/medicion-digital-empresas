# 🚀 Guía de Despliegue - Madurez Digital

## Opciones de Despliegue

Este proyecto puede desplegarse de múltiples formas. Aquí están las opciones en orden de facilidad:

---

## 1️⃣ Vercel (Recomendado) ⭐

### Método A: Desde GitHub (Automático)

**Paso 1: Subir a GitHub**
```bash
# Inicializar repositorio si no lo has hecho
git init
git add .
git commit -m "Initial commit"

# Crear repositorio en GitHub y conectar
git remote add origin https://github.com/TU-USUARIO/madurez-digital.git
git branch -M main
git push -u origin main
```

**Paso 2: Conectar con Vercel**
1. Ve a [vercel.com](https://vercel.com)
2. Click en **"New Project"**
3. Importa tu repositorio de GitHub
4. Vercel detecta automáticamente la configuración
5. Click en **"Deploy"**
6. ¡Listo! Tu app estará en `https://tu-proyecto.vercel.app`

**Ventajas:**
- ✅ Despliegue automático en cada push
- ✅ Preview deployments para PRs
- ✅ SSL gratis
- ✅ CDN global
- ✅ Analytics incluidos

### Método B: Vercel CLI

```bash
# 1. Instalar Vercel CLI
npm install -g vercel

# 2. Login
vercel login

# 3. Desplegar (primera vez - modo desarrollo)
vercel

# 4. Desplegar a producción
vercel --prod
```

**Comandos útiles:**
```bash
# Ver logs
vercel logs

# Listar deployments
vercel ls

# Ver dominios
vercel domains ls

# Agregar dominio personalizado
vercel domains add tudominio.com
```

---

## 2️⃣ Netlify

### Método A: Desde GitHub

**Paso 1: Crear `netlify.toml`**
```bash
cat > netlify.toml << 'EOF'
[build]
  publish = "."
  command = "echo 'No build step needed'"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
EOF
```

**Paso 2: Conectar con Netlify**
1. Ve a [netlify.com](https://netlify.com)
2. Click en **"New site from Git"**
3. Selecciona GitHub y tu repositorio
4. Click en **"Deploy site"**
5. Tu sitio estará en `https://tu-sitio.netlify.app`

### Método B: Netlify CLI

```bash
# 1. Instalar Netlify CLI
npm install -g netlify-cli

# 2. Login
netlify login

# 3. Inicializar
netlify init

# 4. Desplegar
netlify deploy --prod
```

---

## 3️⃣ GitHub Pages

**Paso 1: Configurar GitHub Pages**
```bash
# 1. Asegúrate de estar en main
git checkout main

# 2. Crear rama gh-pages
git checkout -b gh-pages

# 3. Push
git push origin gh-pages

# 4. Volver a main
git checkout main
```

**Paso 2: Activar en GitHub**
1. Ve a tu repositorio en GitHub
2. Settings > Pages
3. Source: Selecciona rama `gh-pages`
4. Click **"Save"**

Tu sitio estará en: `https://TU-USUARIO.github.io/madurez-digital`

**Actualizar:**
```bash
# En main
git checkout gh-pages
git merge main
git push origin gh-pages
git checkout main
```

---

## 4️⃣ Cloudflare Pages

```bash
# 1. Ir a pages.cloudflare.com
# 2. Connect to Git
# 3. Seleccionar repositorio
# 4. Build settings:
#    - Build command: (vacío)
#    - Build output directory: /
# 5. Deploy
```

---

## 5️⃣ Firebase Hosting

**Paso 1: Instalar Firebase CLI**
```bash
npm install -g firebase-tools
```

**Paso 2: Login e inicializar**
```bash
firebase login
firebase init hosting
```

**Configurar firebase.json:**
```json
{
  "hosting": {
    "public": ".",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

**Paso 3: Desplegar**
```bash
firebase deploy --only hosting
```

---

## 6️⃣ Render

1. Ve a [render.com](https://render.com)
2. New > Static Site
3. Conecta tu repositorio de GitHub
4. Build Command: (vacío)
5. Publish Directory: `.`
6. Deploy

---

## 7️⃣ Servidor Propio (Apache/Nginx)

### Apache (.htaccess)

```apache
# .htaccess
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.html [L]

# Security headers
Header set X-Content-Type-Options "nosniff"
Header set X-Frame-Options "DENY"
Header set X-XSS-Protection "1; mode=block"
```

### Nginx (nginx.conf)

```nginx
server {
    listen 80;
    server_name tudominio.com;
    root /var/www/madurez-digital;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Security headers
    add_header X-Frame-Options "DENY";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";

    # Cache static files
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

---

## 🔧 Variables de Entorno (Si usas)

Si necesitas variables de entorno en el futuro, créalas en cada plataforma:

### Vercel
```bash
vercel env add VARIABLE_NAME
```

### Netlify
```bash
netlify env:set VARIABLE_NAME valor
```

### GitHub Pages
No soporta variables de entorno en tiempo de ejecución

---

## 📊 Comparación de Plataformas

| Plataforma | Facilidad | SSL | CDN | Costo | Deploy Auto |
|------------|-----------|-----|-----|-------|-------------|
| **Vercel** | ⭐⭐⭐⭐⭐ | ✅ | ✅ | Gratis | ✅ |
| **Netlify** | ⭐⭐⭐⭐⭐ | ✅ | ✅ | Gratis | ✅ |
| **GitHub Pages** | ⭐⭐⭐⭐ | ✅ | ✅ | Gratis | ⚠️ |
| **Cloudflare** | ⭐⭐⭐⭐ | ✅ | ✅ | Gratis | ✅ |
| **Firebase** | ⭐⭐⭐ | ✅ | ✅ | Gratis | ❌ |
| **Render** | ⭐⭐⭐⭐ | ✅ | ✅ | Gratis | ✅ |

---

## ✅ Checklist Pre-Despliegue

- [ ] Todos los archivos HTML funcionan localmente
- [ ] README.md completo y actualizado
- [ ] package.json con información correcta
- [ ] vercel.json configurado
- [ ] .gitignore incluido
- [ ] LICENSE agregado
- [ ] Rutas relativas (no absolutas) en HTML
- [ ] Links internos funcionan
- [ ] Probado en diferentes navegadores

---

## 🐛 Troubleshooting

### Error: "404 Not Found"
**Solución:** Verifica `vercel.json` o configuración de rewrites

### Error: "CORS"
**Solución:** Agrega headers CORS en vercel.json/netlify.toml

### Archivos no se actualizan
**Solución:** Limpia caché del navegador (Ctrl+Shift+R)

### Build falla
**Solución:** Este proyecto no requiere build - verifica que no haya errores de sintaxis en HTML

---

## 📞 Soporte

¿Problemas con el despliegue?

- 📧 jalzate@transformacionexo.com
- 💬 Abre un issue en GitHub

---

**¡Buena suerte con tu despliegue! 🚀**
