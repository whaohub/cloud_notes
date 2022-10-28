# libyuv install

## libyuv 安装步骤

## step 1

- 安装depot_tools

```git
 git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

- Add *depot_tools* to the *front* of your PATH (you will probably want to put this in your `~/.bashrc` or `~/.zshrc`). Assuming you cloned *depot_tools* to `/path/to/depot_tools`:

```shell
 export PATH=/path/to/depot_tools:$PATH
```
