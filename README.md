# UBUS - Monorepo

Repositório central unificado contendo todo o ecossistema UBUS:
* **`app/`**: Aplicativo móvel (Kotlin Multiplatform / Compose Multiplatform - Android & iOS).
* **`web/`**: Aplicações web (Vite + React + Tailwind CSS + Docker + Nginx):
  * `landing`: Landing page institucional da UBUS.
  * `manager`: Painel administrativo e de gestão da frota/rotas.
* **`api/`**: Espaço reservado para o backend (API central UBUS).

---

## 📁 Estrutura de Diretórios

```text
ubus/
├── .github/
│   └── workflows/
│       ├── app-release-apk.yml        # Build & Release do APK (tags v*)
│       ├── web-deploy-landing.yml     # Deploy da Landing Page (push em main)
│       └── web-deploy-manager.yml     # Deploy do Manager (push em main)
├── app/                               # App Mobile (Kotlin Multiplatform)
│   ├── composeApp/                    # Código compartilhado Kotlin & UI Compose
│   ├── ios/                           # Projeto Xcode iOS
│   ├── gradle/                        # Gradle wrapper e catálogos de versão
│   ├── build.gradle.kts
│   ├── settings.gradle.kts
│   └── gradlew
├── web/                               # Aplicações Web
│   ├── landing/                       # Landing Page (React + Vite)
│   └── manager/                       # Painel Gestor / Admin (React + Vite)
├── api/                               # Backend (reservado para PR da API)
│   └── .gitkeep
├── .gitignore
└── README.md
```

---

## 🛠️ Como Desenvolver Localmente

### 1. Aplicativo Mobile (`app/`)
* **IDE Recomendada**: Android Studio ou Fleet.
  > **Dica**: No Android Studio, abra diretamente o diretório `ubus/app` como raiz do projeto para que o Gradle configure o projeto nativamente sem indexar arquivos Node.js.
* **Comandos principais**:
  ```bash
  cd app
  # Gerar APK Debug
  ./gradlew :composeApp:assembleDebug
  # Executar testes unitários
  ./gradlew test
  ```

### 2. Aplicações Web (`web/`)
* **IDE Recomendada**: VS Code (aberto na raiz `ubus/` ou em `ubus/web/`).
* **Landing Page**:
  ```bash
  cd web/landing
  npm install
  npm run dev
  npm run build
  ```
* **Painel Gestor (`manager`)**:
  ```bash
  cd web/manager
  npm install
  npm run dev
  npm run build
  npm run test:e2e
  ```

---

## 🏷️ Política de Versionamento e Tags

* **App Mobile**:
  * Utiliza o padrão `v*` (ex.: `v1.4.1`, `v1.5.0`).
  * Criar e fazer push de uma tag `v*` aciona automaticamente o workflow `app-release-apk.yml`, compilando o APK e publicando um GitHub Release.
* **Web (Landing e Manager)**:
  * Utiliza Entrega Contínua (CD) disparada automaticamente ao fazer push na branch `main`, com filtro por pasta (`web/landing/**` e `web/manager/**`).
* **Regra Importante para Releases Futuras**:
  * Caso o backend (`api/`) ou os projetos `web/` venham a adotar releases acionadas por tags no futuro, **deverá ser utilizado um prefixo de namespace** (ex.: `api-v1.0.0` ou `web-v1.0.0`) para evitar disparar o pipeline do app mobile.

---

## 🔌 Instruções para Integração do Backend (`api/`)

O desenvolvedor de backend pode integrar o `ubus-api` neste monorepo através de um Pull Request:
1. Clonar o monorepo `ubus`.
2. Criar uma nova branch a partir da `main`: `git checkout -b feature/integrate-backend-api`.
3. Inserir os fontes do backend dentro do diretório `api/`.
4. Adicionar o workflow de CI/CD da API em `.github/workflows/api-deploy.yml` (filtrando por caminhos `api/**`).
5. Abrir o PR apontando para a branch `main`.
