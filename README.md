# WireGuard per a Windows

Còpia de l'instal·lador oficial de **WireGuard per a Windows** (MSI), per si la pàgina oficial de descàrrega (`download.wireguard.com`) està bloquejada pel tallafocs de la xarxa on et trobes.

Aquest repositori **s'actualitza sol**: cada setmana un procés automàtic comprova si hi ha una versió nova i, si n'hi ha, la descarrega de la web oficial i la publica a [Releases](../../releases).

## Descàrrega

Enllaços directes a la darrera versió:

| Equip | Fitxer |
|---|---|
| 64 bits (el més habitual) | [wireguard-amd64.msi](https://github.com/manuelmovilla/wireguard-msi/releases/latest/download/wireguard-amd64.msi) |
| ARM64 | [wireguard-arm64.msi](https://github.com/manuelmovilla/wireguard-msi/releases/latest/download/wireguard-arm64.msi) |
| 32 bits | [wireguard-x86.msi](https://github.com/manuelmovilla/wireguard-msi/releases/latest/download/wireguard-x86.msi) |

## Instal·lació des de PowerShell

Obre PowerShell **com a administrador** i executa:

```powershell
$url  = "https://github.com/manuelmovilla/wireguard-msi/releases/latest/download/wireguard-amd64.msi"
$dest = "$env:TEMP\wireguard-amd64.msi"
Invoke-WebRequest $url -OutFile $dest -UseBasicParsing

# Comprova que el fitxer està signat per WireGuard LLC abans d'instal·lar-lo
$sig = Get-AuthenticodeSignature $dest
if ($sig.Status -ne 'Valid' -or $sig.SignerCertificate.Subject -notmatch 'WireGuard LLC') {
    Write-Error "La signatura no és vàlida. No s'instal·la."; return
}
Start-Process msiexec.exe -ArgumentList "/i `"$dest`" /qn /norestart DO_NOT_LAUNCH=1" -Wait
Remove-Item $dest
```

S'instal·la sense finestres i sense obrir l'aplicació en acabar.

Per importar un túnel (fitxer `.conf`) i deixar-lo actiu com a servei:

```powershell
& "C:\Program Files\WireGuard\wireguard.exe" /installtunnelservice "C:\ruta\al\client.conf"
```

## Seguretat

Els fitxers són els originals, sense cap modificació, i estan signats digitalment per **WireGuard LLC**. L'script anterior comprova la signatura abans d'instal·lar. També pots comprovar-ho a mà: botó dret sobre el fitxer → *Propietats* → *Signatures digitals*.

## Llicència

WireGuard per a Windows és programari lliure (llicència MIT) desenvolupat per WireGuard LLC. Aquest repositori només en redistribueix l'instal·lador i no hi té cap relació oficial.
