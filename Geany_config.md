# Ubuntu and Geany Setup for Competitive Programming

This guide details the setup of **Ubuntu** (a popular free Linux distribution) dual-booted alongside Windows, optimized for competitive programming. My preferred IDE for short codes in CP is **Geany**, which is perfect for this purpose, though not recommended for larger projects or professional work.

## Video Tutorial
For a visual walkthrough, watch the video tutorial: [https://youtu.be/ePZEkbbf3fc](https://youtu.be/ePZEkbbf3fc)

## Setup Instructions

1.  **Download Ubuntu:** Obtain the Ubuntu ISO from [https://ubuntu.com/download](https://ubuntu.com/download).
2.  **Create Bootable USB:** Use [https://rufus.ie](https://rufus.ie) or similar software to create a bootable USB drive.
3.  **Install Ubuntu:** Install Ubuntu from your USB, preferably alongside your existing OS (e.g., Windows).
4.  **Open Terminal:** Access the terminal via Activities or by pressing `Ctrl+Alt+T`.
5.  **Install Geany and g++:**
    ```bash
    sudo apt install geany
    sudo apt install g++
    ```
6.  **Configure Geany Preferences:**
    * **Open Preferences:** Press `Ctrl+Alt+P`.
    * **Keybindings:**
        * Set **Switch to Editor** as `F1` (confirm overriding).
        * Set **Switch to VTE** (built-in terminal) as `F2`.
    * **Terminal Tab:** Mark **Follow path of the current file**. The terminal will now automatically change its path when you open a new file.
    * **(Optional) General -> Miscellaneous:** Unmark **Beep on errors**.
    * **(Optional) Editor:** Change **Comment toggle marker** to an empty string or a single space.
    * **(Optional) Editor > Display:** Unmark **Stop scrolling at last line** and **Long line marker Enabled**.
7.  **Set Build Commands (C++):**
    Open any C++ file in Geany, then go to `Build -> Set Build Commands` and copy these flags:

    * **Compile (F8):**
        ```
        g++ -std=c++17 -Wshadow -Wall -o "%e" "%f" -O2 -Wno-unused-result
        ```
    * **Build (F9):**
        ```
        g++ -std=c++17 -Wshadow -Wall -o "%e" "%f" -g -fsanitize=address -fsanitize=undefined -D_GLIBCXX_DEBUG
        ```
    * *Troubleshooting:* If you encounter compilation errors, try changing `-std=c++17` to `-std=c++14` and/or removing the sanitizers (the two `-fsanitize` flags).
8.  **Configure `.bashrc`:**
    Open the file `~/.bashrc`.
    * Uncomment the line `#force_color_prompt=yes` for a colorful terminal in Geany.
    * Add the line `ulimit -s 2000123` to set the stack limit to approximately 2GB.
    * **(Optional)** Add `alias python=python3`.
    * Restart Geany (or your PC) or run `source ~/.bashrc` to apply the changes.

---

## Optional Steps

* **Hide Toolbar and Sidebar:** Unmark them in `View`.
* **Install Guake (Dropdown Terminal):**
    ```bash
    sudo apt install guake
    ```
    Open Guake (`guake` command), right-click inside the terminal window, go to `Preferences`, and mark **Start Guake at login**.
    In `System Settings > Keyboard > Keyboard Shortcuts`, add a new shortcut:
    * **Command:** `guake-toggle`
    * **Button:** `F12`
    * *Note:* Guake allows a fullscreen terminal by pressing `F12`, which can be more comfortable than `Ctrl+Alt+T`, though it's less critical if you primarily use Geany's built-in terminal.
* **Auto-hide the Dock:** In `System Settings` (top-right corner of the screen) > `Dock` tab, mark **Auto-hide the Dock**.
* **Monokai Theme:**
    Download the Monokai theme from [https://www.geany.org/download/themes/](https://www.geany.org/download/themes/).
    Save it to `~/.config/geany/colorschemes`.
    **(Optional)** Change comment color to `comment=#FF00FF`.
    Apply the color scheme in `View > Change Color Scheme`.
* **Underscore Visibility Fix:** If underscores are not visible for certain font sizes, add `[styling]\nline_height=0;2;` to your configuration (refer to [https://github.com/geany/geany/issues/1387](https://github.com/geany/geany/issues/1387)).
* **Install `testlib.h`:**
    Grab `testlib` from [https://github.com/MikeMirzayanov/testlib/blob/master/testlib.h](https://github.com/MikeMirzayanov/testlib/blob/master/testlib.h).
    Add the first line: `#pragma GCC diagnostic ignored "-Wshadow"`
    Add the last line: `#pragma GCC diagnostic pop`
    Move the file to `/usr/local/include`.

---

## More Information and Details

* **Comment Toggle Marker:** This refers to the special character added to comments created with the `Ctrl+E` shortcut in Geany. The default `~` means commented lines will look like `//~ int a = 2 + 2;`. It's fine to remove the `~` character.
* **Geany Terminal Position:** In `Preferences -> Interface`, you can switch the position of Geany's terminal (and message window) between the bottom and right.

---


## Template Configuration

This section details how to set up a C++ template and a convenient script for creating new competitive programming files.

### 1. Create `cpp` file in `~/.config/geany/templates/files`

Create a file named `cpp` (without an extension) in the directory `~/.config/geany/templates/files`.

The content of this file should be your C++ template:

```cpp
#include <cstdio>
#include <iostream>
using namespace std;

// the argument is the input filename without the extension
void setIO(string s) {
    freopen((s + ".in").c_str(), "r", stdin);
    freopen((s + ".out").c_str(), "w", stdout);
}

int main() {
    setIO("problemname");
}
```
### 2. Create `~home/user/bin` folder


### 3. Create `gcpp` file with contend
```
#!/bin/bash

# Verifica se foi passado um nome de arquivo
if [ -z "$1" ]; then
    echo "Uso: gcpp nome_do_arquivo.cpp"
    exit 1
fi

FILENAME="$1"
TEMPLATE="$HOME/.config/geany/templates/files/cpp"

# Se o arquivo já existir, apenas abre no Geany
if [ -f "$FILENAME" ]; then
    geany "$FILENAME" &
else
    # Se não existir, cria com o template e abre
    cp "$TEMPLATE" "$FILENAME"
    geany "$FILENAME" &
fi
```

Now, go to folder and type gcpp `file.cpp`. 
