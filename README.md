## Abrir [PowerShell](http://www.idi.cl/powershell.html) como administrador

### Instalar **WSL2**
```
wsl --install
```

Mientras se instala, abrir otro PowerShell de admin y continuar:

### Utilidades Windows
<details>
<summary>Detalles</summary>

* Configura WSL para usar máximo 8gb de RAM
* Añade 2 comandos que se pueden ejecutar con <kbd>Win</kbd>+<kbd>R</kbd>:
    * `choko <programa>` instala programa desde Chocolatey
    * `shoko <texto>` busca paquetes de Chocolatey que contengan `<texto>`
* Modifica registro de Windows 11 para:
    * Mostrar opción "Administrador de tareas" en click secundario de barra inferior
    * Activar los menús clasicos al hacer clic derecho
</details>

```bat
Invoke-WebRequest -Uri "https://gist.githubusercontent.com/sebolio/b38f7ef6db673fd32b5f5366f0d97e86/raw/bd3eea8019b3803c59ce5415d92e88d0f56fb474/choko.bat" -OutFile "$HOME\choko.bat"
Invoke-WebRequest -Uri "https://gist.githubusercontent.com/sebolio/b38f7ef6db673fd32b5f5366f0d97e86/raw/bd3eea8019b3803c59ce5415d92e88d0f56fb474/shoko.bat" -OutFile "$HOME\shoko.bat"
Invoke-WebRequest -Uri "https://gist.githubusercontent.com/sebolio/b38f7ef6db673fd32b5f5366f0d97e86/raw/bd3eea8019b3803c59ce5415d92e88d0f56fb474/wslconfig" -OutFile "$HOME\.wslconfig"
Invoke-WebRequest -Uri "https://gist.github.com/sebolio/b38f7ef6db673fd32b5f5366f0d97e86/raw/a28fccff561fc20595a04260f5c87be343337904/utils.reg" -OutFile "$HOME\seb-utils.reg"
reg import $HOME\seb-utils.reg
```

### Instalar Chocolatey
```bat
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### Instalar mis programas favoritos:
```bat
choco install -y --allow -empty-checksums --ignore-checksum googlechrome winrar vscode spotify slack telegram qbittorrent firefox tableplus epicgameslauncher steam battle.net goggalaxy vlc auto-dark-mode evernote postman vmwareworkstation WhatsApp windirstat unity-hub
```

### Instalar fuentes (<kbd>Ctrl</kbd>+🖱️)
1. [Descargar aquí](https://1drv.ms/u/s!An9eKsg-lFZRsJIzweujNblNSrMUQg?e=3K7l8C) (no descomprimir)
2. Ejecutar esto para instalar:
```
Expand-Archive -Path 'C:\Users\Yo\Downloads\Fuentes Terminal.zip' -DestinationPath $ENV:TEMP\Fuentes
$SourceFolder = "C:\Users\Yo\AppData\Local\Temp\Fuentes"
Add-Type -AssemblyName System.Drawing
$WindowsFonts = [System.Drawing.Text.PrivateFontCollection]::new()
Get-ChildItem -Path $SourceFolder -Include *.ttf, *.otf -Recurse -File |
Copy-Item -Destination "$env:SystemRoot\Fonts" -Force -Confirm:$false -PassThru |
ForEach-Object {
$WindowsFonts.AddFontFile($_.fullname)
$RegistryValue = @{
Path = 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts'
Name = $WindowsFonts.Families[-1].Name
Value = $_.Fullname
}
$RemoveRegistry = "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts"
Remove-ItemProperty -name $($WindowsFonts.Families[-1].Name) -path $RemoveRegistry
New-ItemProperty @RegistryValue
}
```

### Reiniciar PC
Al volver a iniciar se instalará Ubuntu, luego puedes continuar con las demás instrucciones.
```
shutdown /r /t 0 /f
```
---
### Configurar Terminal
Abrir Windows Terminal y seguir instrucciones para dejarlo como predeterminado, luego elegir Ubunutu (con el pigüino) como default, guardar y abrir una nueva pestaña del terminal.

### Habilitar sudo sin password
```
sudo sed -i 's/) ALL/) NOPASSWD:ALL/' /etc/sudoers
```

### Instalar zsh + ohmy + p10k + node
```
sudo apt install zsh -y
curl https://gist.githubusercontent.com/sebolio/b38f7ef6db673fd32b5f5366f0d97e86/raw/3d2d9802708bb276a5360dd8356bc1bebea2074a/z-p10k.zsh -o .p10k.zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Se solicitará presionar <kbd>Enter</kbd> y cuando termine, pegar esto para implementar `nvm`, `fuente`, `p10k` y `alias`.
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
. $HOME/.nvm/nvm.sh
nvm install --lts
sed -i 's/robbyrussell/powerlevel10k\/powerlevel10k/' .zshrc
sed -i '1s/^/source "${XDG_CACHE_HOME:-$HOME\/.cache}\/p10k-instant-prompt-${(%):-%n}.zsh"\n/' .zshrc
sed -i 's/"name": "Ubuntu",/"name": "Ubuntu", "font": { "face": "CaskaydiaCove Nerd Font", "size": 10 },/' /mnt/c/Users/Yo/AppData/Local/Packages/Microsoft.WindowsTerminal_8wekyb3d8bbwe/LocalState/settings.json
echo "
#alias y comandos
unalias gp
function gp { git add -A; git commit -m \"\$*\"; git push }
alias d=\"npm run dev\"
alias dev=\"npm run dev\"
alias doc=\"npm run storybook\"
alias json=\"npm run stub\"
alias pu=\"git pull\"
\n[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh">>~/.zshrc
```
Ignorar el error de `operation not permitted`

### Presionar <kbd>Win</kbd>+<kbd>R</kbd> y pegar, repetir por cada línea:
```
ms-windows-store://pdp/?ProductId=9PF4KZ2VN4W9
steam://run/431960
```

### Instalar brew para poder tener `gh`
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install gh
```

### Configurar fuente en VSCode
Presionar <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> y elegir `User Settings (JSON)` para añadir:
```
"terminal.integrated.fontFamily": "MesloLGS NF",
```
