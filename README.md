# 🍎 Mac Development Ansible Playbook

Playbook ini menginstall dan mengkonfigurasi sebagian besar software yang saya gunakan di Mac untuk development. Beberapa hal di macOS agak sulit untuk diotomasi, jadi masih ada beberapa langkah instalasi manual, tapi setidaknya semuanya sudah terdokumentasi di sini.

## 📦 Instalasi

1. Pastikan Apple's command line tools sudah terinstall:
   ```bash
   xcode-select --install
   ```

2. Install Ansible:
   ```bash
   # Tambahkan Python 3 ke PATH
   export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"
   
   # Upgrade Pip
   sudo pip3 install --upgrade pip
   
   # Install Ansible
   pip3 install ansible
   ```

3. Clone repository ini ke local drive.

4. Install role Ansible yang dibutuhkan:
   ```bash
   ansible-galaxy install -r requirements.yml
   ```

5. Jalankan playbook:
   ```bash
   ansible-playbook main.yml --ask-become-pass
   ```
   Masukkan password macOS kamu ketika diminta password 'BECOME'.

> **Catatan**: Jika ada command Homebrew yang gagal, mungkin kamu perlu menyetujui lisensi Xcode atau memperbaiki issue Brew lainnya. Jalankan `brew doctor` untuk mengeceknya.

## 🌐 Penggunaan dengan Mac Remote

Kamu bisa menggunakan playbook ini untuk mengelola Mac lain juga. Jika ingin mengelola Mac remote (Mac lain di jaringanmu atau hosted Mac seperti dari [MacStadium](https://www.macstadium.com)):

1. Di Mac yang ingin dikoneksikan, buka **System Settings > Sharing**
2. Aktifkan **'Remote Login'**

> Kamu juga bisa mengaktifkan remote login via command line:
> ```bash
> sudo systemsetup -setremotelogin on
> ```

Kemudian edit file `inventory` dan ubah baris yang dimulai dengan `127.0.0.1` menjadi:

```
[ip address atau hostname mac]  ansible_user=[username ssh mac]
```

Jika perlu menyediakan password SSH (jika tidak menggunakan SSH keys), pastikan untuk menambahkan parameter `--ask-pass` ke command `ansible-playbook`.

## 🏷️ Menjalankan Task dengan Tag Tertentu

Kamu bisa memfilter bagian mana dari proses provisioning yang ingin dijalankan dengan menentukan tag menggunakan flag `--tags`. Tag yang tersedia: `dotfiles`, `homebrew`, `mas`, `extra-packages`, dan `osx`.

```bash
ansible-playbook main.yml -K --tags "dotfiles,homebrew"
```

## ⚙️ Override Konfigurasi Default

Tidak semua development environment dan preferensi konfigurasi software itu sama.

Kamu bisa meng-override default yang sudah dikonfigurasi di `default.config.yml` dengan membuat file `config.yml` dan mengatur override di file tersebut. Contoh:

```yaml
homebrew_installed_packages:
  - git
  - go

homebrew_cask_apps:
  - google-chrome
  - visual-studio-code
  - iterm2

composer_packages:
  - name: laravel/installer

npm_packages:
  - name: pnpm

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
dockitems_persist:
  - name: "Visual Studio Code"
    path: "/Applications/Visual Studio Code.app/"
```

Semua variable bisa di-override di `config.yml`. Lihat dokumentasi role pendukung untuk daftar lengkap variable yang tersedia.

## 📱 Aplikasi & Konfigurasi yang Termasuk

### Aplikasi GUI (via Homebrew Cask)

| Kategori | Aplikasi |
|----------|----------|
| 🎮 Game Dev | Unity Hub, Android Studio |
| 🌐 Browser | Google Chrome, Firefox |
| 💻 Editor/IDE | VS Code, Cursor, Windsurf, Sequel Ace |
| 🖥️ Terminal | iTerm2 |
| 🛠️ Productivity | Rectangle, Stats, Raycast, AppCleaner, Maccy |
| 🐳 Dev Tools | OrbStack, ChromeDriver |

### CLI Tools (via Homebrew)

| Kategori | Package |
|----------|---------|
| 🎮 Game Dev | cocoapods, gradle, cmake |
| 💻 Essential | git, gh, node, nvm, go, php, mysql, composer |
| ✨ Modern Utils | bat, eza, fzf, jq, tldr, tree, htop, zoxide, starship |
| 🔧 System Libs | autoconf, wget, openssl, gpg, nmap |

## 🧪 Testing Playbook

Project ini di-test secara kontinyu menggunakan GitHub Actions. Kamu juga bisa menjalankan macOS di dalam VM untuk testing. Rekomendasi:

- [UTM](https://mac.getutm.app)
- [Tart](https://github.com/cirruslabs/tart)

## 📚 Referensi

- [Ansible for DevOps](https://www.ansiblefordevops.com/) - Buku untuk belajar Ansible

## 👨‍💻 Author

**Ngurah** - Personal Mac development environment automation.
