# Ichigo Window Manager

Experimento em **C** com a API Win32: o executável carrega `wm.dll`, instala um hook `WH_SHELL` e, quando uma janela é criada ou destruída, chama `TileWindows` em layout vertical. Não é um gerenciador i3/dwm completo — é um protótipo de hook + tiling nativo do Windows.

## Tecnologias

- C, Windows API (`windows.h`), DLL carregada em tempo de execução

## Build

Requer toolchain para Windows (**MinGW-w64** ou **MSVC**):

```bash
gcc -shared -o wm_dll.dll wm_dll.c -luser32
gcc -o ichigo.exe main.c
```

Coloque `wm_dll.dll` no mesmo diretório que `ichigo.exe` (`main.c` usa `LoadLibraryW(L"wm_dll")`).

## Execução

Execute `ichigo.exe`. Encerre com Ctrl+C no terminal.

## Acompanhe o projeto

Relate problemas ou sugestões em [Issues no GitHub](https://github.com/lucassilvasoftware/ichigo-wm/issues).
