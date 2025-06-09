# 🚀 Ultimate Vite Cheat Sheet 2025 ⚡

> The most comprehensive Vite guide for modern web development. Star ⭐ this repo if it helps you!

[![GitHub stars](https://img.shields.io/github/stars/MianAliKhalid/ultimate-vite-cheatsheet-2025?style=social)](https://github.com/MianAliKhalid/ultimate-vite-cheatsheet-2025)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Last Commit](https://img.shields.io/github/last-commit/MianAliKhalid/ultimate-vite-cheatsheet-2025)](https://github.com/MianAliKhalid/ultimate-vite-cheatsheet-2025)

## 📑 Quick Navigation

| Section | Description | Jump To |
|---------|-------------|---------|
| 🚀 **Getting Started** | Installation & Basic Setup | [→](#-installation--quick-start) |
| ⚙️ **Configuration** | Config files & options | [→](#️-configuration-guide) |
| 🔌 **Plugins** | Essential plugins & usage | [→](#-essential-plugins) |
| 🎨 **CSS & Assets** | Styling & asset handling | [→](#-css--asset-management) |
| ⚡ **Performance** | Optimization techniques | [→](#-performance-optimization) |
| 🚀 **Deployment** | Production & hosting | [→](#-deployment-strategies) |
| 🔧 **Troubleshooting** | Common issues & fixes | [→](#-troubleshooting-guide) |

## 🤔 What Makes Vite Special?

Vite (French for "quick" `/vit/`) revolutionizes web development with:

- ⚡ **Lightning-fast cold start** - Start in milliseconds, not seconds
- 🔥 **Instant HMR** - See changes immediately without refresh
- 📦 **Optimized builds** - Powered by Rollup for production
- 🛠️ **Framework agnostic** - Works with React, Vue, Svelte, and more
- 🎯 **Modern by default** - ESM, TypeScript, CSS modules out-of-the-box
- 🔌 **Rich ecosystem** - Extensive plugin library

## 🚀 Installation & Quick Start

### Create New Project (2025 Method)
```bash
# Using npm (recommended)
npm create vite@latest my-awesome-project

# Using yarn
yarn create vite my-awesome-project

# Using pnpm (fastest)
pnpm create vite my-awesome-project

# Using bun (ultra-fast)
bun create vite my-awesome-project
```

### Framework-Specific Templates
```bash
# React Projects
npm create vite@latest my-react-app -- --template react
npm create vite@latest my-react-ts -- --template react-ts
npm create vite@latest my-react-swc -- --template react-swc-ts

# Vue Projects
npm create vite@latest my-vue-app -- --template vue
npm create vite@latest my-vue-ts -- --template vue-ts

# Svelte Projects
npm create vite@latest my-svelte-app -- --template svelte
npm create vite@latest my-svelte-ts -- --template svelte-ts

# Vanilla JavaScript
npm create vite@latest my-vanilla-app -- --template vanilla
npm create vite@latest my-vanilla-ts -- --template vanilla-ts

# Other Frameworks
npm create vite@latest my-preact-app -- --template preact
npm create vite@latest my-lit-app -- --template lit
npm create vite@latest my-solid-app -- --template solid
npm create vite@latest my-qwik-app -- --template qwik
```

### Quick Setup Commands
```bash
cd my-awesome-project
npm install          # Install dependencies
npm run dev         # Start development server
npm run build       # Build for production
npm run preview     # Preview production build
```

## 📁 Optimal Project Structure

```
my-vite-project/
├── 📁 public/                 # Static assets (not processed)
│   ├── favicon.ico
│   ├── robots.txt
│   └── manifest.json
├── 📁 src/                   # Source code
│   ├── 📁 assets/           # Build-time assets
│   │   ├── images/
│   │   ├── fonts/
│   │   └── icons/
│   ├── 📁 components/       # Reusable components
│   │   ├── ui/
│   │   └── layout/
│   ├── 📁 pages/           # Page components
│   ├── 📁 hooks/           # Custom hooks
│   ├── 📁 utils/           # Utility functions
│   ├── 📁 stores/          # State management
│   ├── 📁 styles/          # Global styles
│   │   ├── globals.css
│   │   └── variables.css
│   ├── main.tsx            # Entry point
│   └── App.tsx             # Root component
├── 📄 index.html            # HTML entry point
├── 📄 package.json
├── 📄 vite.config.ts       # Vite configuration
├── 📄 tsconfig.json        # TypeScript config
├── 📄 .env                 # Environment variables
└── 📄 .gitignore
```

## ⚙️ Configuration Guide

### Basic vite.config.ts
```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

export default defineConfig({
  plugins: [react()],
  
  // Development server
  server: {
    port: 3000,
    open: true,
    cors: true,
    hmr: {
      overlay: false
    }
  },
  
  // Build configuration
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'esbuild',
    target: 'esnext',
    chunkSizeWarningLimit: 1000
  },
  
  // Path aliases
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
      '@components': resolve(__dirname, 'src/components'),
      '@assets': resolve(__dirname, 'src/assets'),
      '@utils': resolve(__dirname, 'src/utils'),
      '@pages': resolve(__dirname, 'src/pages')
    }
  },
  
  // Global constants
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
    __BUILD_DATE__: JSON.stringify(new Date().toISOString())
  }
})
```

### Advanced Configuration (Production-Ready)
```typescript
import { defineConfig, loadEnv } from 'vite'
import react from '@vitejs/plugin-react'
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig(({ command, mode }) => {
  const env = loadEnv(mode, process.cwd(), '')
  
  return {
    plugins: [
      react(),
      
      // Bundle analyzer (only in analyze mode)
      mode === 'analyze' && visualizer({
        filename: 'dist/stats.html',
        open: true,
        gzipSize: true,
        brotliSize: true
      })
    ].filter(Boolean),
    
    // Environment-specific configuration
    server: {
      port: parseInt(env.VITE_PORT) || 3000,
      proxy: {
        '/api': {
          target: env.VITE_API_URL || 'http://localhost:8080',
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/api/, '')
        }
      }
    },
    
    build: {
      rollupOptions: {
        output: {
          // Chunk splitting strategy
          manualChunks: {
            vendor: ['react', 'react-dom'],
            router: ['react-router-dom'],
            ui: ['@mui/material', '@emotion/react'],
            utils: ['lodash', 'date-fns', 'axios']
          },
          
          // Asset naming
          chunkFileNames: 'js/[name]-[hash].js',
          entryFileNames: 'js/[name]-[hash].js',
          assetFileNames: (assetInfo) => {
            const info = assetInfo.name.split('.')
            const ext = info[info.length - 1]
            if (/png|jpe?g|svg|gif|tiff|bmp|ico/i.test(ext)) {
              return `images/[name]-[hash][extname]`
            }
            if (/woff2?|eot|ttf|otf/i.test(ext)) {
              return `fonts/[name]-[hash][extname]`
            }
            return `assets/[name]-[hash][extname]`
          }
        }
      }
    },
    
    // Optimization
    esbuild: {
      drop: command === 'build' ? ['console', 'debugger'] : []
    }
  }
})
```

## 🛠️ Essential Commands & Scripts

### Development Commands
```bash
# Start development server
npm run dev

# Start with specific port
npm run dev -- --port 3001

# Start with host exposure
npm run dev -- --host

# Start in debug mode
DEBUG=vite:* npm run dev
```

### Production Commands
```bash
# Build for production
npm run build

# Build with bundle analysis
npm run build:analyze

# Build for specific environment
npm run build -- --mode staging

# Preview production build
npm run preview

# Preview on specific port
npm run preview -- --port 4000
```

### Optimized package.json Scripts
```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "build:analyze": "vite build --mode analyze",
    "build:staging": "vite build --mode staging",
    "preview": "vite preview",
    "clean": "rm -rf dist node_modules/.vite",
    "type-check": "tsc --noEmit",
    "lint": "eslint src --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "lint:fix": "eslint src --ext ts,tsx --fix",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "coverage": "vitest run --coverage"
  }
}
```

## 🔌 Essential Plugins

### Framework Plugins
```bash
# React
npm install @vitejs/plugin-react
npm install @vitejs/plugin-react-swc  # Faster alternative

# Vue
npm install @vitejs/plugin-vue
npm install @vitejs/plugin-vue-jsx

# Svelte
npm install @vitejs/plugin-svelte

# Solid
npm install vite-plugin-solid
```

### Development Plugins
```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

// Auto import plugins
import AutoImport from 'unplugin-auto-import/vite'
import { TanStackRouterVite } from '@tanstack/router-vite-plugin'

// Development tools
import { defineConfig } from 'vite'
import eslint from 'vite-plugin-eslint'
import checker from 'vite-plugin-checker'

export default defineConfig({
  plugins: [
    react(),
    
    // Auto import React hooks and utilities
    AutoImport({
      imports: [
        'react',
        'react-router-dom',
        {
          'react': ['useState', 'useEffect', 'useCallback', 'useMemo']
        }
      ],
      dts: true,
      eslintrc: {
        enabled: true
      }
    }),
    
    // Type checking
    checker({
      typescript: true,
      eslint: {
        lintCommand: 'eslint "./src/**/*.{ts,tsx}"'
      }
    }),
    
    // ESLint integration
    eslint({
      cache: false,
      include: ['./src/**/*.ts', './src/**/*.tsx']
    })
  ]
})
```

### Production Plugins
```typescript
// PWA support
import { VitePWA } from 'vite-plugin-pwa'

// Bundle optimization
import { visualizer } from 'rollup-plugin-visualizer'
import { compression } from 'vite-plugin-compression'

export default defineConfig({
  plugins: [
    // PWA configuration
    VitePWA({
      registerType: 'autoUpdate',
      includeAssets: ['favicon.ico', 'apple-touch-icon.png'],
      manifest: {
        name: 'My Awesome App',
        short_name: 'MyApp',
        description: 'An awesome Vite-powered application',
        theme_color: '#ffffff',
        icons: [
          {
            src: 'pwa-192x192.png',
            sizes: '192x192',
            type: 'image/png'
          }
        ]
      },
      workbox: {
        runtimeCaching: [
          {
            urlPattern: /^https:\/\/api\.myapp\.com\/.*/i,
            handler: 'CacheFirst',
            options: {
              cacheName: 'api-cache',
              expiration: {
                maxEntries: 10,
                maxAgeSeconds: 60 * 60 * 24 * 365 // 1 year
              }
            }
          }
        ]
      }
    }),
    
    // Gzip compression
    compression({
      algorithm: 'gzip',
      ext: '.gz'
    }),
    
    // Brotli compression
    compression({
      algorithm: 'brotliCompress',
      ext: '.br'
    })
  ]
})
```

## 🌍 Environment Variables

### Environment File Structure
```bash
# .env (loaded in all environments)
VITE_APP_TITLE=My Awesome App
VITE_APP_VERSION=1.0.0

# .env.local (ignored by git, loaded in all environments)
VITE_API_SECRET=your-secret-here

# .env.development
VITE_API_URL=http://localhost:8080/api
VITE_DEBUG=true
VITE_LOG_LEVEL=debug

# .env.staging
VITE_API_URL=https://staging-api.myapp.com
VITE_DEBUG=false
VITE_LOG_LEVEL=warn

# .env.production
VITE_API_URL=https://api.myapp.com
VITE_DEBUG=false
VITE_LOG_LEVEL=error
VITE_ANALYTICS_ID=GA_TRACKING_ID
```

### TypeScript Environment Variables
```typescript
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_API_URL: string
  readonly VITE_APP_TITLE: string
  readonly VITE_DEBUG: string
  readonly VITE_LOG_LEVEL: 'debug' | 'info' | 'warn' | 'error'
  readonly VITE_ANALYTICS_ID?: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}

// Usage in components
const config = {
  apiUrl: import.meta.env.VITE_API_URL,
  appTitle: import.meta.env.VITE_APP_TITLE,
  isDebug: import.meta.env.VITE_DEBUG === 'true',
  isDev: import.meta.env.DEV,
  isProd: import.meta.env.PROD,
  mode: import.meta.env.MODE
}
```

## 🎨 CSS & Asset Management

### CSS Preprocessors Setup
```bash
# Install preprocessors
npm install -D sass           # Sass/SCSS
npm install -D less           # Less
npm install -D stylus         # Stylus
npm install -D postcss        # PostCSS
```

### CSS Modules
```tsx
// Component.module.css
.container {
  padding: 1rem;
  background: var(--bg-color);
}

.title {
  font-size: 2rem;
  color: var(--text-color);
}

// Component.tsx
import styles from './Component.module.css'

export const Component = () => (
  <div className={styles.container}>
    <h1 className={styles.title}>Hello World</h1>
  </div>
)
```

### PostCSS Configuration
```javascript
// postcss.config.js
export default {
  plugins: {
    'postcss-import': {},
    'tailwindcss/nesting': {},
    tailwindcss: {},
    autoprefixer: {},
    ...(process.env.NODE_ENV === 'production' && {
      cssnano: {
        preset: ['default', {
          discardComments: { removeAll: true }
        }]
      }
    })
  }
}
```

### Asset Imports
```typescript
// Static assets
import logoUrl from './assets/logo.png'
import iconUrl from './assets/icon.svg'

// Dynamic imports
const modules = import.meta.glob('./assets/*.png', { eager: true })
const images = import.meta.glob('./assets/images/*.{png,jpg,jpeg}')

// Raw imports
import shaderSource from './shaders/vertex.glsl?raw'
import workerUrl from './worker.ts?worker&url'

// URL imports
import styleUrl from './component.css?url'
import inlineStyle from './component.css?inline'
```

### Asset Optimization
```typescript
// vite.config.ts
export default defineConfig({
  // Asset handling
  assetsInclude: ['**/*.gltf', '**/*.hdr', '**/*.glb'],
  
  build: {
    assetsInlineLimit: 4096, // 4kb threshold
    
    rollupOptions: {
      output: {
        assetFileNames: (assetInfo) => {
          const info = assetInfo.name.split('.')
          const ext = info[info.length - 1]
          
          // Organize assets by type
          if (/png|jpe?g|svg|gif|tiff|bmp|ico/i.test(ext)) {
            return `images/[name]-[hash][extname]`
          }
          if (/woff2?|eot|ttf|otf/i.test(ext)) {
            return `fonts/[name]-[hash][extname]`
          }
          if (/mp4|webm|ogg|mp3|wav|flac|aac/i.test(ext)) {
            return `media/[name]-[hash][extname]`
          }
          return `assets/[name]-[hash][extname]`
        }
      }
    }
  }
})
```

## ⚡ Performance Optimization

### Code Splitting Strategies
```typescript
// Route-based splitting
import { lazy, Suspense } from 'react'

const Home = lazy(() => import('./pages/Home'))
const About = lazy(() => import('./pages/About'))
const Dashboard = lazy(() => import('./pages/Dashboard'))

// Component-based splitting
const HeavyComponent = lazy(() => 
  import('./components/HeavyComponent').then(module => ({
    default: module.HeavyComponent
  }))
)

// Conditional loading
const AdminPanel = lazy(() => {
  if (user.role !== 'admin') {
    return Promise.reject(new Error('Unauthorized'))
  }
  return import('./components/AdminPanel')
})

// Usage with Suspense
<Suspense fallback={<div>Loading...</div>}>
  <HeavyComponent />
</Suspense>
```

### Bundle Optimization
```typescript
// vite.config.ts
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: (id) => {
          // Vendor chunks
          if (id.includes('node_modules')) {
            if (id.includes('react') || id.includes('react-dom')) {
              return 'react-vendor'
            }
            if (id.includes('lodash') || id.includes('date-fns')) {
              return 'utils-vendor'
            }
            if (id.includes('@mui') || id.includes('@emotion')) {
              return 'ui-vendor'
            }
            return 'vendor'
          }
          
          // Feature-based chunks
          if (id.includes('src/features/auth')) {
            return 'auth'
          }
          if (id.includes('src/features/dashboard')) {
            return 'dashboard'
          }
        }
      }
    }
  },
  
  // Optimize dependencies
  optimizeDeps: {
    include: [
      'react',
      'react-dom',
      'react-router-dom'
    ],
    exclude: [
      'large-unused-library'
    ]
  }
})
```

### Preloading & Prefetching
```typescript
// Preload critical resources
const preloadCriticalAssets = () => {
  // Preload critical routes
  import('./pages/Home')
  import('./pages/Dashboard')
}

// Prefetch on user interaction
const handleMouseEnter = () => {
  import('./components/HeavyModal')
}

// Intersection Observer for lazy loading
const LazySection = () => {
  const [isVisible, setIsVisible] = useState(false)
  const ref = useRef()
  
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true)
          observer.disconnect()
        }
      },
      { threshold: 0.1 }
    )
    
    if (ref.current) {
      observer.observe(ref.current)
    }
    
    return () => observer.disconnect()
  }, [])
  
  return (
    <div ref={ref}>
      {isVisible && <HeavyComponent />}
    </div>
  )
}
```

## 📘 TypeScript Integration

### TypeScript Configuration
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    
    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitReturns": true,
    "noImplicitOverride": true,
    
    /* Path mapping */
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@pages/*": ["src/pages/*"],
      "@utils/*": ["src/utils/*"],
      "@assets/*": ["src/assets/*"]
    }
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

### Advanced TypeScript Features
```typescript
// Type-safe environment variables
type Environment = 'development' | 'staging' | 'production'

interface Config {
  apiUrl: string
  environment: Environment
  features: {
    analytics: boolean
    debugMode: boolean
  }
}

const createConfig = (): Config => ({
  apiUrl: import.meta.env.VITE_API_URL,
  environment: import.meta.env.MODE as Environment,
  features: {
    analytics: import.meta.env.VITE_ANALYTICS === 'true',
    debugMode: import.meta.env.DEV
  }
})

// Type-safe asset imports
declare module '*.svg' {
  const content: string
  export default content
}

declare module '*.png' {
  const content: string
  export default content
}

// HMR types
if (import.meta.hot) {
  import.meta.hot.accept('./components/App', (newModule) => {
    if (newModule) {
      // Type-safe hot reload
      render(newModule.App)
    }
  })
}
```

## 🚀 Deployment Strategies

### Netlify Deployment
```toml
# netlify.toml
[build]
  base = "."
  publish = "dist"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "18"
  NPM_FLAGS = "--version"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "max-age=31536000, immutable"

[[headers]]
  for = "/*.js"
  [headers.values]
    Cache-Control = "max-age=31536000, immutable"

[[headers]]
  for = "/*.css"
  [headers.values]
    Cache-Control = "max-age=31536000, immutable"
```

### Vercel Deployment
```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": {
        "buildCommand": "npm run build",
        "outputDirectory": "dist"
      }
    }
  ],
  "routes": [
    {
      "src": "/assets/(.*)",
      "headers": {
        "Cache-Control": "max-age=31536000, immutable"
      }
    },
    {
      "handle": "filesystem"
    },
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ]
}
```

### GitHub Actions CI/CD
```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run type check
      run: npm run type-check
    
    - name: Run linting
      run: npm run lint
    
    - name: Run tests
      run: npm run test

  build-and-deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build application
      run: npm run build
      env:
        VITE_API_URL: ${{ secrets.VITE_API_URL }}
        VITE_ANALYTICS_ID: ${{ secrets.VITE_ANALYTICS_ID }}
    
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
        cname: yourdomain.com
```

### Docker Deployment
```dockerfile
# Multi-stage Dockerfile
FROM node:18-alpine as builder

WORKDIR /app

# Copy package files
COPY package*.json ./
RUN npm ci --only=production

# Copy source code
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy built assets
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
```

```nginx
# nginx.conf
events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    
    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types
        text/plain
        text/css
        text/xml
        text/javascript
        application/javascript
        application/xml+rss
        application/json;
    
    server {
        listen 80;
        server_name localhost;
        root /usr/share/nginx/html;
        index index.html;
        
        # Cache static assets
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }
        
        # Handle SPA routing
        location / {
            try_files $uri $uri/ /index.html;
        }
        
        # Security headers
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
    }
}
```

## 🔧 Troubleshooting Guide

### Common Issues & Solutions

#### 🚨 Build Errors

**Issue: "Cannot resolve dependency"**
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install

# Clear Vite cache
rm -rf node_modules/.vite
npm run dev
```

**Issue: TypeScript errors in build**
```bash
# Type check without emitting
npm run type-check

# Skip type checking in build
npm run build -- --mode production --skipTypeCheck
```

#### 🚨 Development Issues

**Issue: HMR not working**
```typescript
// vite.config.ts
export default defineConfig({
  server: {
    hmr: {
      overlay: true,
      clientPort: 3000
    },
    watch: {
      usePolling: true // For Docker/WSL
    }
  }
})
```

**Issue: CORS errors**
```typescript
export default defineConfig({
  server: {
    cors: true,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        secure: false,
        configure: (proxy, _options) => {
          proxy.on('error', (err, _req, _res) => {
            console.log('proxy error', err)
          })
        }
      }
    }
  }
})
```

#### 🚨 Performance Issues

**Issue: Slow build times**
```typescript
export default defineConfig({
  // Optimize dependencies
  optimizeDeps: {
    force: true,
    include: ['react', 'react-dom']
  },
  
  build: {
    // Parallel processing
    minify: 'esbuild',
    
    // Reduce bundle analysis overhead
    rollupOptions: {
      onwarn(warning, warn) {
        if (warning.code === 'MODULE_LEVEL_DIRECTIVE') return
        warn(warning)
      }
    }
  }
})
```

**Issue: Large bundle size**
```bash
# Analyze bundle
npm run build:analyze

# Check for duplicate dependencies
npx depcheck

# Use dynamic imports
const HeavyComponent = lazy(() => import('./HeavyComponent'))
```

#### 🚨 Memory Issues

**Issue: JavaScript heap out of memory**
```bash
# Increase Node.js memory limit
node --max-old-space-size=8192 ./node_modules/vite/bin/vite.js build

# Or in package.json
{
  "scripts": {
    "build": "node --max-old-space-size=8192 ./node_modules/vite/bin/vite.js build"
  }
}
```

### Debug Configuration
```typescript
// vite.config.ts for debugging
export default defineConfig({
  logLevel: 'info',
  
  server: {
    hmr: {
      overlay: true
    }
  },
  
  build: {
    sourcemap: true,
    minify: false, // Disable for debugging
    
    rollupOptions: {
      onwarn(warning, warn) {
        console.log('Rollup warning:', warning)
        warn(warning)
      }
    }
  },
  
  // Enable detailed logging
  define: {
    __DEV__: true,
    'process.env.NODE_ENV': '"development"'
  }
})
```

## 🧪 Testing with Vitest

### Setup Testing
```bash
# Install Vitest
npm install -D vitest @vitest/ui jsdom
npm install -D @testing-library/react @testing-library/jest-dom
```

```typescript
// vite.config.ts
/// <reference types="vitest" />
import { defineConfig } from 'vite'

export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
    css: true,
    coverage: {
      reporter: ['text', 'json', 'html'],
      exclude: [
        'node_modules/',
        'src/test/'
      ]
    }
  }
})
```

### Test Examples
```typescript
// src/test/setup.ts
import '@testing-library/jest-dom'

// Component test
import { render, screen } from '@testing-library/react'
import { describe, it, expect } from 'vitest'
import { Button } from './Button'

describe('Button', () => {
  it('renders correctly', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button')).toHaveTextContent('Click me')
  })
})
```

## 🚀 Advanced Features

### Server-Side Rendering (SSR)
```typescript
// vite.config.ts for SSR
export default defineConfig({
  build: {
    ssr: 'src/entry-server.tsx',
    rollupOptions: {
      input: {
        client: 'src/entry-client.tsx',
        server: 'src/entry-server.tsx'
      }
    }
  },
  
  ssr: {
    noExternal: ['react', 'react-dom']
  }
})
```

### Micro-frontends with Module Federation
```bash
npm install @originjs/vite-plugin-federation
```

```typescript
import federation from '@originjs/vite-plugin-federation'

export default defineConfig({
  plugins: [
    federation({
      name: 'host-app',
      remotes: {
        'micro-app': 'http://localhost:3001/assets/remoteEntry.js'
      },
      shared: ['react', 'react-dom']
    })
  ]
})
```

### Web Workers
```typescript
// worker.ts
self.onmessage = async (e: MessageEvent) => {
  const { data } = e
  const result = await heavyComputation(data)
  self.postMessage(result)
}

// main.tsx
const worker = new Worker(
  new URL('./worker.ts', import.meta.url),
  { type: 'module' }
)

worker.postMessage(data)
worker.onmessage = (e) => {
  console.log('Worker result:', e.data)
}
```

## 📊 Performance Monitoring

### Bundle Analysis
```bash
# Install analyzer
npm install -D rollup-plugin-visualizer

# Generate bundle report
npm run build:analyze
```

### Web Vitals Integration
```typescript
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals'

// Log performance metrics
getCLS(console.log)
getFID(console.log)
getFCP(console.log)
getLCP(console.log)
getTTFB(console.log)
```

### Lighthouse CI
```yaml
# .github/workflows/lighthouse.yml
name: Lighthouse CI

on:
  pull_request:
    branches: [main]

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 18
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build app
        run: npm run build
      
      - name: Run Lighthouse CI
        run: |
          npm install -g @lhci/cli@0.12.x
          lhci autorun
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
```

## 🎯 Best Practices 2025

### 1. **Project Structure**
- Use feature-based organization
- Implement absolute imports with path mapping
- Separate concerns (components, hooks, utils)

### 2. **Performance**
- Implement route-based code splitting
- Use React.memo() for expensive components
- Optimize bundle chunks strategically

### 3. **Type Safety**
- Enable strict TypeScript settings
- Use type-safe environment variables
- Implement proper error boundaries

### 4. **Development Experience**
- Set up ESLint + Prettier
- Use Husky for git hooks
- Implement automated testing

### 5. **Production Readiness**
- Configure proper caching headers
- Implement error monitoring
- Set up performance monitoring

## 🔗 Essential Resources

### Official Documentation
- [🏠 Vite Official Guide](https://vitejs.dev/guide/)
- [⚙️ Config Reference](https://vitejs.dev/config/)
- [🔌 Plugin API](https://vitejs.dev/guide/api-plugin.html)

### Community Resources
- [⭐ Awesome Vite](https://github.com/vitejs/awesome-vite)
- [🔌 Plugin Ecosystem](https://vitejs.dev/plugins/)
- [💬 Discord Community](https://chat.vitejs.dev/)

### Performance Tools
- [📊 Bundle Analyzer](https://www.npmjs.com/package/rollup-plugin-visualizer)
- [🔍 Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [📈 Web Vitals](https://web.dev/vitals/)

## 🤝 Contributing

Found something missing or incorrect? Contributions are welcome!

1. 🍴 Fork this repository
2. 🌟 Create a feature branch
3. 📝 Make your improvements
4. 🚀 Submit a pull request

## 📜 License

MIT License - feel free to use this cheat sheet in your projects!

## ⭐ Show Your Support

If this cheat sheet helped you:
- ⭐ **Star this repository**
- 🐦 **Share on Twitter**
- 💼 **Share on LinkedIn**  
- 📝 **Write a blog post about it**
- 💬 **Tell your developer friends**

---

<div align="center">

**Made with ❤️ for the developer community**

*Last updated: January 2025*

[![GitHub stars](https://img.shields.io/github/stars/MianAliKhalid/ultimate-vite-cheatsheet-2025?style=social)](https://github.com/MianAliKhalid/ultimate-vite-cheatsheet-2025)

[🚀 Back to Top](#-ultimate-vite-cheat-sheet-2025-)

</div>
