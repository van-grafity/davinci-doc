Untuk merapikan Blade Laravel di VS Code, yang paling bagus dan stabil saat ini:

## Extension Utama

### 1. Laravel Blade Formatter

[Laravel Blade Formatter VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=shufo.vscode-blade-formatter&utm_source=chatgpt.com)

Extension:

```text id="6m1rgo"
shufo.vscode-blade-formatter
```

Fitur:

* auto format `.blade.php`
* rapikan indent
* format HTML + Blade syntax
* support Tailwind
* sangat cocok untuk project Laravel besar

---

# Cara Setup

## Install dependency formatter

Di terminal project:

```bash
npm install -D prettier prettier-plugin-blade
```

---

## Tambahkan `.prettierrc`

```json id="36duvq"
{
  "plugins": ["prettier-plugin-blade"],
  "bladeBracketSameLine": false,
  "bladeIndentSize": 4,
  "printWidth": 120,
  "singleQuote": true
}
```

---

# VS Code Settings

Buka:

```text id="0vud6k"
Ctrl + Shift + P
```

Ketik:

```text id="l9jvt2"
Preferences: Open Settings (JSON)
```

Tambahkan:

```json id="31ij7g"
"[blade]": {
    "editor.defaultFormatter": "shufo.vscode-blade-formatter"
},
"editor.formatOnSave": true
```

---

# Shortcut Rapikan Otomatis

## Windows

```text id="g0h5ut"
Shift + Alt + F
```

## Mac

```text id="kw7wo9"
Shift + Option + F
```

---

# Extension Laravel Tambahan yang Sangat Direkomendasikan

## Laravel Extra Intellisense

[Laravel Extra Intellisense VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=amiralizadeh9480.laravel-extra-intellisense&utm_source=chatgpt.com)

## Laravel Blade Snippets

[Laravel Blade Snippets VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=onecentlin.laravel-blade&utm_source=chatgpt.com)

## Laravel Goto View

[Laravel Goto View VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=codingyu.laravel-goto-view&utm_source=chatgpt.com)

## PHP Intelephense

[PHP Intelephense VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client&utm_source=chatgpt.com)

Ini kombinasi terbaik untuk development Laravel + Blade modern.
