# RustSBI support software for HiFive Unmatched boards

The purpose of this project is to support RustSBI on SiFive [HiFive Unmatched](https://www.sifive.com/boards/hifive-unmatched) boards.
RustSBI is a bootloader environment; when the motherboard is powered up, RustSBI will start first, then it will find a bootable operating system and boot it.
After startup, RustSBI still resides in the background, providing the functionality required by the operating system.
The design of RustSBI fully complies with the RISC-V SBI specification standard. As long as the operating system supports this standard, RustSBI can be used to boot.

## Compile and Run

This project uses the xtask framework and can be compiled with the following instructions:

```shell
cargo image
```

(If the --release parameter is added, it means that the release version without debugging symbols is compiled)

At this time, the compilation produces an elf file and an img image. Note that the generated intermediate data bin file cannot be directly used for burning.

Use the following operations to burn the img format image to the sd card partition. (Dangerous! Data must be backed up first)

```shell
sudo dd if=target/sd-card-partition-2.img of=\\?\Device\Harddisk????\Partition2 --progress
```

After the burning is complete, you can use RustSBI to boot.

## Rust Version

Compiling this project requires at least the Rust version of `rustc 1.59.0-nightly (c5ecc1570 2021-12-15)`.

## Documentation

Please refer to the [Project Wiki](https://github.com/rustsbi/rustsbi-hifive-unmatched/wiki) for complete documentation.

The parts of the project that still need to be perfected are also documented in [here](https://github.com/rustsbi/rustsbi-hifive-unmatched/wiki/Next…).

## Support scheme for large and small core design

The HiFive Unmatched motherboard has a SiFive Freedom U740 processor onboard. The FU740 is a heterogeneous multi-core processor, it has a total of five cores.
Its five cores are four U74 application processor cores and one S7 embedded processor core.

As developers of the software implementation of RustSBI, we noticed that the S7 management corelet will have a wide range of uses.
Therefore, RustSBI does not block any cores on HiFive Unmatched for the operating system to choose and use.

## useful links

- HiFive Unmatched Getting Started Guide (Chinese) Version 1.4 [PDF](https://sifive.cdn.prismic.io/sifive/b9376339-5d60-45c9-8280-58fd0557c2f0_hifive-unmatched-gsg-v1p4_EN.pdf)
- SiFive FU740-000 Manual v1p3 [PDF](https://sifive.cdn.prismic.io/sifive/de1491e5-077c-461d-9605-e8a0ce57337d_fu740-c000-manual-v1p3.pdf)

## Command Line

View assembly code

```
cargo asm
```
