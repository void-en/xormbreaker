
## XORmBreaker

Yes, XORmBreaker, like StormBreaker, that's what I called it. I built a very short script (more like the code in the Jupyter Notebook) when I was on an engagement in 2022, as I noticed repeating patterns in a `.bin` file which turned out to be a malicious `.dll` file. This code has since come in handy several times.

This solution is not perfect, nor is it meant to be, that being said I hope you enjoy the guide, and the tool!

## Walkthrough

`xormbreaker.ipynb` walks through cracking a weak repeating-key XOR cipher step by step, against two real samples (`samples/cannon.bin` and `samples/cannon-100.bin`, both a publicly known tool, LOIC), explaining how to spot the pattern, recover the key, and work out the correct rotation.

## Usage

`xormbreaker.py` is the standalone script version of the same technique, no dependencies beyond the Python standard library.

```
usage: xormbreaker.py [-h] --input input_file.bin [--output output_file(.exe)]
                       [--key qwe123asd]
                       [--crib \x00\x00\x00\x00\x00\x00\x00\x00] [--all]
                       [--dev] [--version]

options:
  -h, --help            show this help message and exit
  --input input_file.bin
                        input file path
  --output output_file(.exe)
                        output file path, with or without extension
  --key qwe123asd       if you know the key
  --crib \x00\x00\x00\x00\x00\x00\x00\x00
                        known pattern, like null padding or potato
  --all                 save all rotations of all found keys, regardless of
                        crib
  --dev                 debug mode, allows errors
  --version             show program's version number and exit
```

Example, searching for the key automatically and saving anything that matches a known file-type magic bytes or null padding:

```
python3 xormbreaker.py --input samples/cannon.bin --output out/cannon
```

If you already suspect a crib (a known string that should appear in the plaintext), pass it directly:

```
python3 xormbreaker.py --input samples/cannon.bin --output out/cannon --crib "MZ"
```