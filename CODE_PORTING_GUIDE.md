# Code Porting Guide: MongoDB Compass Web

This guide documents the steps for setting up and configuring the MongoDB Compass Web project for local development with the full compass monorepo.

## Table of Contents

- [1. Local Monorepo Setup](#1-local-monorepo-setup)
- [2. Future Steps](#2-future-steps)

---

## 1. Local Monorepo Setup

### Overview
Configure pnpm overrides to use local compass monorepo packages instead of published versions for development.

### Prerequisites
- Git installed and configured
- pnpm package manager installed
- Access to MongoDB Compass repository

### Step 1.1: Install Compass as Git Submodule

First, add the compass monorepo as a git submodule to ensure you have the latest source code:

```bash
# Add compass monorepo as submodule
git submodule add https://github.com/mongodb-js/compass.git compass

# Initialize and update the submodule
git submodule update --init --recursive

# Navigate to compass directory and checkout specific commit
# This ensures reproducible builds and compatibility
# Commit 87d62e7ca59075cd8301664e33be18ce1f655dfa corresponds to tag v1.46.7
cd compass
git checkout 87d62e7ca59075cd8301664e33be18ce1f655dfa
cd ..
```

**Alternative: Clone manually (if submodules aren't preferred)**
```bash
# Clone compass monorepo to compass directory
git clone https://github.com/mongodb-js/compass.git compass
cd compass
git checkout 87d62e7ca59075cd8301664e33be18ce1f655dfa
cd ..
```

**Verify compass installation:**
```bash
# Check that compass directory exists and has packages
ls -la compass/packages/
ls -la compass/configs/
```

You should see directories like `compass-components`, `compass-aggregations`, etc.

**Why pin to a specific commit?**
- ✅ **Reproducible builds**: Everyone gets the same compass version (v1.46.7)
- ✅ **Stability**: Prevents breaking changes from latest commits  
- ✅ **Controlled updates**: Update compass version intentionally, not accidentally
- 📌 **Version reference**: Commit `87d62e7ca` corresponds to compass tag `v1.46.7`

### Step 1.2: Add Comprehensive pnpm Overrides

Add the following `pnpm.overrides` section to your `package.json`:

```json
{
  "pnpm": {
    "overrides": {
      "@mongodb-js/atlas-service": "link:./compass/packages/atlas-service",
      "@mongodb-js/compass-aggregations": "link:./compass/packages/compass-aggregations",
      "@mongodb-js/compass-app-registry": "link:./compass/packages/compass-app-registry",
      "@mongodb-js/compass-app-stores": "link:./compass/packages/compass-app-stores",
      "@mongodb-js/compass-collection": "link:./compass/packages/compass-collection",
      "@mongodb-js/compass-components": "link:./compass/packages/compass-components",
      "@mongodb-js/compass-connection-import-export": "link:./compass/packages/compass-connection-import-export",
      "@mongodb-js/compass-connections": "link:./compass/packages/compass-connections",
      "@mongodb-js/compass-connections-navigation": "link:./compass/packages/compass-connections-navigation",
      "@mongodb-js/compass-context-menu": "link:./compass/packages/compass-context-menu",
      "@mongodb-js/compass-crud": "link:./compass/packages/compass-crud",
      "@mongodb-js/compass-data-modeling": "link:./compass/packages/compass-data-modeling",
      "@mongodb-js/compass-databases-collections": "link:./compass/packages/databases-collections",
      "@mongodb-js/compass-editor": "link:./compass/packages/compass-editor",
      "@mongodb-js/compass-explain-plan": "link:./compass/packages/compass-explain-plan",
      "@mongodb-js/compass-export-to-language": "link:./compass/packages/compass-export-to-language",
      "@mongodb-js/compass-field-store": "link:./compass/packages/compass-field-store",
      "@mongodb-js/compass-find-in-page": "link:./compass/packages/compass-find-in-page",
      "@mongodb-js/compass-generative-ai": "link:./compass/packages/compass-generative-ai",
      "@mongodb-js/compass-global-writes": "link:./compass/packages/compass-global-writes",
      "@mongodb-js/compass-import-export": "link:./compass/packages/compass-import-export",
      "@mongodb-js/compass-indexes": "link:./compass/packages/compass-indexes",
      "@mongodb-js/compass-intercom": "link:./compass/packages/compass-intercom",
      "@mongodb-js/compass-logging": "link:./compass/packages/compass-logging",
      "@mongodb-js/compass-maybe-protect-connection-string": "link:./compass/packages/compass-maybe-protect-connection-string",
      "@mongodb-js/compass-query-bar": "link:./compass/packages/compass-query-bar",
      "@mongodb-js/compass-saved-aggregations-queries": "link:./compass/packages/compass-saved-aggregations-queries",
      "@mongodb-js/compass-schema": "link:./compass/packages/compass-schema",
      "@mongodb-js/compass-schema-validation": "link:./compass/packages/compass-schema-validation",
      "@mongodb-js/compass-scripts": "link:./compass/scripts",
      "@mongodb-js/compass-serverstats": "link:./compass/packages/compass-serverstats",
      "@mongodb-js/compass-settings": "link:./compass/packages/compass-settings",
      "@mongodb-js/compass-shell": "link:./compass/packages/compass-shell",
      "@mongodb-js/compass-sidebar": "link:./compass/packages/compass-sidebar",
      "@mongodb-js/compass-smoke-tests": "link:./compass/packages/compass-smoke-tests",
      "@mongodb-js/compass-telemetry": "link:./compass/packages/compass-telemetry",
      "@mongodb-js/compass-test-server": "link:./compass/packages/compass-test-server",
      "@mongodb-js/compass-user-data": "link:./compass/packages/compass-user-data",
      "@mongodb-js/compass-utils": "link:./compass/packages/compass-utils",
      "@mongodb-js/compass-web": "link:./compass/packages/compass-web",
      "@mongodb-js/compass-welcome": "link:./compass/packages/compass-welcome",
      "@mongodb-js/compass-workspaces": "link:./compass/packages/compass-workspaces",
      "@mongodb-js/connection-form": "link:./compass/packages/connection-form",
      "@mongodb-js/connection-info": "link:./compass/packages/connection-info",
      "@mongodb-js/connection-storage": "link:./compass/packages/connection-storage",
      "@mongodb-js/databases-collections-list": "link:./compass/packages/databases-collections-list",
      "@mongodb-js/eslint-config-compass": "link:./compass/configs/eslint-config-compass",
      "@mongodb-js/eslint-plugin-compass": "link:./compass/configs/eslint-plugin-compass",
      "@mongodb-js/explain-plan-helper": "link:./compass/packages/explain-plan-helper",
      "@mongodb-js/mocha-config-compass": "link:./compass/configs/mocha-config-compass",
      "@mongodb-js/my-queries-storage": "link:./compass/packages/my-queries-storage",
      "@mongodb-js/prettier-config-compass": "link:./compass/configs/prettier-config-compass",
      "@mongodb-js/reflux-state-mixin": "link:./compass/packages/reflux-state-mixin",
      "@mongodb-js/testing-library-compass": "link:./compass/configs/testing-library-compass",
      "@mongodb-js/tsconfig-compass": "link:./compass/configs/tsconfig-compass",
      "@mongodb-js/webpack-config-compass": "link:./compass/configs/webpack-config-compass",
      "bson-transpilers": "link:./compass/packages/bson-transpilers",
      "compass-e2e-tests": "link:./compass/packages/compass-e2e-tests",
      "compass-preferences-model": "link:./compass/packages/compass-preferences-model",
      "hadron-build": "link:./compass/packages/hadron-build",
      "hadron-document": "link:./compass/packages/hadron-document",
      "hadron-ipc": "link:./compass/packages/hadron-ipc",
      "hadron-type-checker": "link:./compass/packages/hadron-type-checker",
      "mongodb-collection-model": "link:./compass/packages/collection-model",
      "mongodb-compass": "link:./compass/packages/compass",
      "mongodb-data-service": "link:./compass/packages/data-service",
      "mongodb-database-model": "link:./compass/packages/database-model",
      "mongodb-explain-compat": "link:./compass/packages/mongodb-explain-compat",
      "mongodb-instance-model": "link:./compass/packages/instance-model",
      "mongodb-query-util": "link:./compass/packages/mongodb-query-util"
    },
    "onlyBuiltDependencies": [
      "cpu-features",
      "electron",
      "ssh2"
    ]
  }
}
```

### Step 1.3: Understanding `link:` vs `file:` Protocol

**Why we use `link:` protocol:**

- ✅ **Instant Updates**: Changes reflect immediately without rebuilds
- ✅ **Faster Development**: No compilation step required  
- ✅ **True Workspace Behavior**: Exactly like a monorepo workspace
- ✅ **Resource Efficient**: No duplicate files or builds

**Alternative `file:` protocol would:**
- ❌ Require rebuilding packages after changes
- ❌ Create copies/symlinks with separate build processes
- ❌ Need working build toolchain for all packages

### Step 1.4: Configure Built Dependencies

Add `onlyBuiltDependencies` to ensure native modules are properly compiled:

```json
{
  "pnpm": {
    "onlyBuiltDependencies": [
      "cpu-features",
      "electron", 
      "ssh2"
    ]
  }
}
```

This ensures these packages are always built from source rather than using cached/linked versions.

### Step 1.5: Installation and Setup

1. **Clear package manager cache:**
   ```bash
   pnpm store prune
   ```

2. **Install dependencies:**
   ```bash
   pnpm install
   ```

3. **Verify setup:**
   - Check that `node_modules` contains symlinks to compass packages
   - Verify changes in compass packages reflect immediately in your app

### Benefits Achieved

- 🔄 **Complete Monorepo Coverage**: All 70 compass packages use local versions
- ⚡ **Zero Build Overhead**: Instant updates without compilation
- 🎯 **Future-Proof**: Any new compass dependencies automatically use local versions
- 🛡️ **No Version Conflicts**: Guaranteed consistency across development environment

---

## 2. Future Steps

<!-- Add additional porting steps here as needed -->

*This section is reserved for future porting steps and configurations.*

---

## Troubleshooting

### Common Issues

1. **Symlink not working**: Ensure the compass monorepo is properly set up as submodule or cloned to `./compass/`
2. **Submodule issues**: Run `git submodule update --init --recursive` if compass directory is empty
3. **Build errors**: Check that native dependencies in `onlyBuiltDependencies` are correctly specified  
4. **TypeScript errors**: Verify tsconfig paths are correctly configured for monorepo structure

### Validation Commands

```bash
# Verify compass submodule is properly set up
ls -la compass/packages/ | head -10

# Check if overrides are working
pnpm list @mongodb-js/compass-components

# Verify symlinks
ls -la node_modules/@mongodb-js/compass-components

# Update submodule to specific commit (when needed)
# Commit corresponds to compass v1.46.7
cd compass && git checkout 87d62e7ca59075cd8301664e33be18ce1f655dfa && cd ..

# Clear cache if needed
pnpm store prune && pnpm install
```

---

*Last updated: $(date +%Y-%m-%d)*
