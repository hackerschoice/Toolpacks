### Toolpacks : Packaged Tools &amp; Binaries all in one place
---
#### Contents
> - [**Prerequisite**](https://github.com/Azathothas/Toolpacks/tree/main#prerequisite)
> > 📦 **`Toolpacks`** 📦
> > - [**Android (Termux Pkgs)**](https://github.com/Azathothas/Toolpacks/tree/main/Info/Packages/Termux) [![💾 Fetch || ⏫ Update Termux Package Registry 📦](https://github.com/Azathothas/Toolpacks/actions/workflows/list_termux_pkgs.yaml/badge.svg)](https://github.com/Azathothas/Toolpacks/actions/workflows/list_termux_pkgs.yaml)
> > - [**Android (aarch64_arm64_v8a)**](https://github.com/Azathothas/Toolpacks/tree/main#aarch64_arm64_v8a_Android) --> [Metadata](https://github.com/Azathothas/Toolpacks/tree/main/aarch64_arm64_v8a_Android) [![🛍️ Fetch ⚙️ Weekly 📱 (aarch64_arm64_v8a_Android) Package 📦🗄️](https://github.com/Azathothas/Toolpacks/actions/workflows/fetch_weekly_toolpack_aarch64_arm64_v8a_Android.yaml/badge.svg)](https://github.com/Azathothas/Toolpacks/actions/workflows/fetch_weekly_toolpack_aarch64_arm64_v8a_Android.yaml)
> > - [**Linux (amd_x86_64)**](https://github.com/Azathothas/Toolpacks/tree/main#amd-x86_64) --> [Metadata](https://github.com/Azathothas/Toolpacks/tree/main/x86_64) [Script](https://github.com/Azathothas/Toolpacks/blob/main/.github/scripts/eget_binaries_amd_x86_64.sh) [![🛍️ Publish | Build ⚙️ Weekly (toolpack_x86_64) Package 📦🗄️](https://github.com/Azathothas/Toolpacks/actions/workflows/publish_weekly_toolpack_x86_64.yaml/badge.svg)](https://github.com/Azathothas/Toolpacks/actions/workflows/publish_weekly_toolpack_x86_64.yaml)
> - [**ElseWhere**](https://github.com/Azathothas/Toolpacks/tree/main#Elsewhere)
---
- #### Prerequisite
```bash
!# Install eget: https://github.com/zyedidia/eget

#--------------------------------------------------------------------------------------------#
❯ aarch64 || arm64_v8a [Android]
!# As $USER (TERMUX)
# $PREFIX:/data/data/com.termux/files/usr
curl -qfSL "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/aarch64_arm64_v8a_Android/eget" -o "$PREFIX/bin/eget" && chmod +xwr "$PREFIX/bin/eget"
wget -q "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/eget" -O "$PREFIX/bin/eget" && chmod +xwr "$HOME/bin/eget"
!# Root requires remounting /system/bin as RWR (NOT RECOMMENDED)
#--------------------------------------------------------------------------------------------#

#--------------------------------------------------------------------------------------------#
❯ amd || x86_64
!# As $USER
mkdir -p "$HOME/bin" ; export PATH="$HOME/bin:$PATH"
curl -qfsSL "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/eget" -o "$HOME/bin/eget" && chmod +xwr "$HOME/bin/eget"
wget -q "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/eget" -O "$HOME/bin/eget" && chmod +xwr "$HOME/bin/eget"
!# As ROOT
sudo curl -qfsSL "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/eget" -o "/usr/local/bin/eget" && sudo chmod +xwr "/usr/local/bin/eget"
sudo wget -q "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/eget" -O "/usr/local/bin/eget" && sudo chmod +xwr "/usr/local/bin/eget"
#--------------------------------------------------------------------------------------------#
```
---
- #### [`amd x86_64`](https://github.com/Azathothas/Toolpacks/tree/main/x86_64)
```bash
❯ !# tar.bz2 [Slowest + High RAM Usage]
!# $USER
 eget "Azathothas/Toolpacks" --asset "toolpack_x86_64.tar.bz2" --all --to "$HOME/bin"
!# ROOT
 sudo eget "Azathothas/Toolpacks" --asset "toolpack_x86_64.tar.bz2" --all --to "/usr/local/bin" && sudo chmod +xwr /usr/local/bin/*

❯ !# zip [Highest RAM Usage]
!# $USER
 eget "Azathothas/Toolpacks" --asset "toolpack_x86_64.zip" --all --to "$HOME/bin"
!# ROOT
 sudo eget "Azathothas/Toolpacks" --asset "toolpack_x86_64.zip" --all --to "/usr/local/bin" && sudo chmod +xwr /usr/local/bin/*

❯ !# 7z [Fastest + Minimal RAM Usage] [RECOMMENDED]
!#Install 7z (USER) : eget "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/7z" --to "$HOME/bin/7z"
!#Install 7z (ROOT) : sudo eget "https://raw.githubusercontent.com/Azathothas/Toolpacks/main/x86_64/7z" --to "/usr/local/bin/7z"
!#Download .7z archive
 wget "$(curl -qfsSL "https://api.github.com/repos/Azathothas/Toolpacks/releases" | jq -r '.[] | select(.assets[].name | contains("x86_64")) | .assets[].browser_download_url' | grep -i '.7z$' | sort -u | tail -n 1)" -O "./toolpack_x86_64.7z"
!# $USER
 mkdir -p "$HOME/bin" ; 7z e "./toolpack_x86_64.7z" -o"$HOME/bin" -y && rm -rf "$HOME/bin/toolpack_x86_64" 2>/dev/null && rm -rf "./toolpack_x86_64.7z" ; chmod +xwr $HOME/bin/*
!# ROOT
 sudo 7z e "./toolpack_x86_64.7z" -o"/usr/local/bin" -y && sudo rm -rf "/usr/local/bin/toolpack_x86_64" 2>/dev/null && rm -rf "./toolpack_x86_64.7z" ; sudo chmod +xwr /usr/local/bin/* 2>/dev/null
```
---
- #### [aarch64_arm64_v8a_Android](https://github.com/Azathothas/Toolpacks/tree/main/aarch64_arm64_v8a_Android)
```bash
!# Create tmp dir
pushd "$(mktemp -d)"
!# Download all bins
for url in $(curl -qfsSL "https://api.github.com/repos/Azathothas/Toolpacks/contents/aarch64_arm64_v8a_Android" -H "Accept: application/vnd.github.v3+json" | jq -r '.[].download_url'); do echo -e "\n[+] $url\n" && curl -qfLJO "$url"; done
!# Move all to "$PREFIX/bin"
# $PREFIX=/data/data/com.termux/files/usr
find . -maxdepth 1 -type f ! -name '*.md' -exec mv {} "$PREFIX/bin/" \; 2>/dev/null
#chmod
chmod +xwr $PREFIX/bin/*
#list
ls "$PREFIX/bin" | column -t ; popd
```
---
- #### Elsewhere
> - Main Source for [metis-os/hysp-pkgs](https://github.com/metis-os/hysp-pkgs) used by [pwnwriter/hysp](https://github.com/pwnwriter/hysp)
