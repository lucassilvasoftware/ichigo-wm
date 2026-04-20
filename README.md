# Ichigo Window Manager

Gerenciador de janelas em **tiling** para **Windows**, em linguagem **C**, usando a API Win32: o executável carrega `wm.dll` (`wm_dll`), instala um hook `WH_SHELL` e delega o tratamento de eventos de janela à biblioteca. Objetivo: organizar janelas em layouts (por exemplo BSP, mestre-pilha, grade ou flutuante) e atalhos no estilo de gerenciadores como i3 ou dwm.

## Tecnologias

- C, Windows API (`windows.h`), DLL carregada em tempo de execução

## Build

Requer toolchain para Windows (**MinGW-w64** ou **MSVC**). Exemplo genérico com `gcc` (ajuste nomes de saída e flags conforme seu ambiente):

```bash
gcc -shared -o wm_dll.dll wm_dll.c -luser32
gcc -o ichigo.exe main.c
```

Coloque `wm_dll.dll` no mesmo diretório que `ichigo.exe` (o `main.c` usa `LoadLibraryW(L"wm_dll")` — o nome efetivo do arquivo deve corresponder à convenção de carregamento do Windows, por exemplo `wm_dll.dll`).

## Execução

Execute `ichigo.exe` com permissões adequadas ao hook global. Encerre com Ctrl+C no terminal ou pelo gerenciador de tarefas se o processo estiver associado ao console.

## Configuração

Se o projeto evoluir para arquivo `ichigo.conf` ou exclusão de classes de janela, consulte o código em `wm_dll.c` e comentários no repositório.

## Acompanhe o projeto

Relate problemas ou sugestões em [Issues no GitHub](https://github.com/lucassilvasoftware/ichigo/issues) (ajuste a URL se o fork oficial for outro).
