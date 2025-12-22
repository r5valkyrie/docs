## Installing R5Valkyrie on linux

### Debian/Ubuntu 

#### Using the Repository (recommended):

1. Add the R5Valkyrie repository to your system
```bash
echo 'deb [trusted=yes] https://r5valkyrie.github.io/launcher/deb/ ./' | sudo tee /etc/apt/sources.list.d/r5valkyrie.list
```
2. Update your package lists
```bash
sudo apt update
```
3. Install the launcher
```bash
sudo apt install r5vlauncher
```
4. ???
5. Profit

#### Manual Installation:

1. Download the latest release of the launcher from https://github.com/r5valkyrie/launcher/releases/latest
* Make sure that you download the release marked as being for Debian.
2. Open a terminal window and navigate to your downloads folder with
```bash
cd Downloads
```
3. After that install the file with
```bash
sudo dpkg -i R5Valkyrie.Launcher-x.x.xx-Debian.deb
```
* Make sure to replace the x.x.xx with the proper version number from your download. Or type out R5 and hit the tab key to autofill the full file name.
4. ???
5. Profit

### Fedora/RHEL/openSUSE

#### Using the Repository (recommended):

1. Add the R5Valkyrie repository to your system
```bash
sudo tee /etc/yum.repos.d/r5valkyrie.repo << EOF
[r5valkyrie]
name=R5Valkyrie Repository
baseurl=https://r5valkyrie.github.io/launcher/rpm/
enabled=1
gpgcheck=0
EOF
```
2. Install the launcher
```bash
sudo dnf install r5vlauncher  # Fedora/RHEL
# or
sudo zypper install r5vlauncher  # openSUSE
```
3. ???
4. Profit

#### Manual Installation:

1. Download the latest release from https://github.com/r5valkyrie/launcher/releases/latest
* Make sure to download the RPM package.
2. Install the package
```bash
sudo dnf install ./R5Valkyrie.Launcher-x.x.xx.rpm  # Fedora/RHEL
# or
sudo zypper install ./R5Valkyrie.Launcher-x.x.xx.rpm  # openSUSE
```
3. ???
4. Profit

### Arch Linux

#### Using the AUR (recommended):

1. Use your favorite AUR helper to install the package like this
```bash
yay -S r5valkyrie-launcher-bin
# or
paru -S r5valkyrie-launcher-bin
```
2. Follow the on screen instructions to further install the AUR package.
3. ???
4. Profit

#### Installing it manually:

1. Download the [latest release](https://github.com/r5valkyrie/launcher/releases/latest)
* Make sure to download the `.pkg.tar.zst` file for Arch.
2. Install the package with
```bash
sudo pacman -U R5Valkyrie.Launcher-x.x.xx-Arch.pkg.tar.zst
```
3. ???
4. Profit

### NixOS / Nix

#### Run without installing:

1. Run the launcher directly
```bash
nix run github:r5valkyrie/launcher
```
2. ???
3. Profit

#### Install to your profile:

1. Install the launcher
```bash
nix profile install github:r5valkyrie/launcher
```
2. ???
3. Profit

#### Add to NixOS configuration:

1. Add to your `configuration.nix` or flake inputs
```nix
{
  inputs.r5vlauncher.url = "github:r5valkyrie/launcher";
  
  # Then add to environment.systemPackages:
  environment.systemPackages = [
    inputs.r5vlauncher.packages.${pkgs.system}.default
  ];
}
```
2. Rebuild your system
```bash
sudo nixos-rebuild switch
```
3. ???
4. Profit

### AppImage (Universal)

1. Download the AppImage from https://github.com/r5valkyrie/launcher/releases/latest
2. Make it executable
```bash
chmod +x R5Valkyrie-Launcher-x.x.xx.AppImage
```
3. Run it
```bash
./R5Valkyrie-Launcher-x.x.xx.AppImage
```
4. ???
5. Profit

