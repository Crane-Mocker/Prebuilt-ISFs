# Prebuilt-ISFs

This is a repo containing prebuilt ISF files for [volatility3](https://github.com/volatilityfoundation/volatility3).

To use a certain ISF file, `cp` the `json.xz` file to `volatility3/volatility3/framework/symbols/linux`

## What is an ISF?

Volatility3 made a move away from profiles and instead uses Symbol Tables. For Linux these tables are generated by parsing a matching debug kernel extracting all the symbol structures and creating an Intermediate Symbol Format (ISF) file that can be processed by volatility3.

To read more:

[Changes between Volatility 2 and Volatility 3](https://volatility3.readthedocs.io/en/latest/vol2to3.html)

## How to build an ISF?

With the help of [volatility_symbols](https://github.com/kevthehermit/volatility_symbols)!

1. **Download an official Linux distribution and create a virtual machine (60GB recommanded)**

2. **Install the right version of the kernel.**
Be sure it matches the output of `python3 vol.py -f dump.mem banners.Banners`
Check current kernel: `uname -r`
Check all installed kernels: `apt list --installed | grep linux-image`
Search for kernel: `sudo apt search linux-*image-*`, 
if you can find the kernel version you want: Install kernel version you want with `apt`. 
For example: `sudo apt install linux-image-5.15.0-84-generic`
If you cannot find the kernel version you want, download the deb for kernel from somewhere, for example [kernel.ubuntu.com](https://kernel.ubuntu.com/~kernel-ppa/mainline/).
Then `sudo dpkg -i *.deb`

3. **Update Grub**
Edit `/etc/default/grub`:`#GRUB_HIDDEN_TIMEOUT=0` 
Or `#GRUB_TIMEOUT_STYLE=hidden` then change `GRUB_TIMEOUT=0` to `GRUB_TIMEOUT=5`
Run `sudo update-grub`

4. **Reboot with right kernel**

5. **Run volatility_symbols** 
`git clone https://github.com/kevthehermit/volatility_symbols.git`
`python3 symbol_maker.py -d <distribution> -k <kernel version> `
Such as `python3 symbol_maker.py -d ubuntu -k '5.15.0-84-generic' `

