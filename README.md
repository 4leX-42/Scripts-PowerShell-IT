# Scripts-PowerShell-IT

Soporte de Windows y Office 365. Autonomos: se copian al equipo y funcionan. PowerShell 5.1 y 7.

## Permitir la ejecucion de scripts

Si sale `la ejecucion de scripts esta deshabilitada en este sistema`:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Sin tocar la politica, solo para esa vez:

```powershell
powershell -ExecutionPolicy Bypass -File .\script.ps1
```

Si el `.ps1` viene descargado o de una unidad de red, ademas:

```powershell
Unblock-File .\script.ps1
```

## Ensayo antes de aplicar

Todo lo que modifica acepta `-DryRun`: enumera lo que haria y no cambia nada.

```powershell
.\01-office-accounts\02-clear-office-creds-tenant.ps1 -DryRun
```

## Office accounts
Ejecutar con la sesion del usuario, **sin elevar**.

- [`01-list-office-accounts`](01-office-accounts/01-list-office-accounts.ps1) — cuentas guardadas en el equipo y donde esta cada una
- [`02-clear-office-creds-tenant`](01-office-accounts/02-clear-office-creds-tenant.ps1) — las limpia todas; en el menu eliges cual conservar

## Device info

- [`01-device-report`](02-device-info/01-device-report.ps1) — hardware, Windows, Entra, Intune, disco, red, Office
- [`02-disk-usage`](02-device-info/02-disk-usage.ps1) — carpetas grandes y basura recuperable

<sub>Nativo de Windows: `W + R` → `msinfo32` · `dxdiag` · `winver`</sub>

## Network

- [`01-test-m365-connectivity`](03-network/01-test-m365-connectivity.ps1) — DNS, 443 y latencia contra los 7 endpoints

## Copy-paste

- [`01-long-paths`](00-copy-paste/01-long-paths.txt) — rutas largas OneDrive / iManage `[ADMIN]`
- [`README`](00-copy-paste/README.md) — comandos sueltos: disco, Office, identidad, Intune, red, impresion, Windows Update, software

## Repo tools

- [`01-validate-scripts`](99-repo-tools/01-validate-scripts.ps1) — ASCII, BOM y sintaxis 5.1 + 7. Antes de subir cambios.

---

## Admin o usuario

`HKCU`, `%APPDATA%`, `%LOCALAPPDATA%`, credenciales, Office, OneDrive → **cuenta del usuario, sin elevar**.
`HKLM`, servicios, `C:\Windows`, red, spooler, Windows Update → **admin**.

Elevar con **otra** cuenta desvia `HKCU` y `%LOCALAPPDATA%` a ese perfil: el script sale limpio
sin haber mirado el del usuario. El prompt sigue mostrando su carpeta, pero no es su perfil.
Comprobar con `$env:USERPROFILE`. Los scripts de cuentas lo detectan y abortan.

## Estandar

Sin tildes · UTF-8 con BOM · 5.1 y 7 · `-DryRun` en lo que escribe · `param()`, nada cableado
