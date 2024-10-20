TODO: Test on target (static linking might be needed or use of other standard library)

A template for programming the https://milkv.io/duo with Rust.
It includes a build container to be able to directly start with one click in github codespaces.

The elf file can simply be started in the dev container with qemu-riscv64 and is configured in the tasks settings for vscode.
Debugging with gdb is also possible with qemu-riscv64 and is configured in the lauch settings for vscode.
