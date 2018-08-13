![alt text](https://img.shields.io/docker/automated/kolyadin/pngquant.svg)
![alt text](https://img.shields.io/docker/build/kolyadin/pngquant.svg)
![alt text](https://img.shields.io/docker/pulls/kolyadin/pngquant.svg)

# About

This docker image is based on [pngquant](https://pngquant.org/) image utility in alpine docker container

### How to use

```bash
$ docker run -it --rm kolyadin/pngquant

pngquant, 2.11.2 (November 2017), by Kornel Lesinski, Greg Roelofs.
   Color profiles are supported via Little CMS. Using libpng 1.6.34.

usage:  pngquant [options] [ncolors] -- pngfile [pngfile ...]
        pngquant [options] [ncolors] - >stdout <stdin

options:
  --force           overwrite existing output files (synonym: -f)
  --skip-if-larger  only save converted files if they're smaller than original
  --output file     destination file path to use instead of --ext (synonym: -o)
  --ext new.png     set custom suffix/extension for output filenames
  --quality min-max don't save below min, use fewer colors below max (0-100)
  --speed N         speed/quality trade-off. 1=slow, 3=default, 11=fast & rough
  --nofs            disable Floyd-Steinberg dithering
  --posterize N     output lower-precision color (e.g. for ARGB4444 output)
  --strip           remove optional metadata (default on Mac)
  --verbose         print status messages (synonym: -v)
```

### Demo with creating new file

```bash
$ docker run -it --rm \
    -v $(pwd)/source/:/var/workdir/ \
    kolyadin/pngquant --verbose --output reduced.png --quality 80-90 original.png
    
original.png:
  read 933KB file
  made histogram...61820 colors found
  selecting colors...3%
  selecting colors...24%
  selecting colors...27%
  selecting colors...48%
  selecting colors...68%
  selecting colors...89%
  selecting colors...100%
  moving colormap towards local minimum
  eliminated opaque tRNS-chunk entries...37 entries transparent
  mapped image to new colors...MSE=1.963 (Q=93)
  writing 256-color image as reduced.png
Quantized 1 image.

$ ls -lh source/

933K original.png
269K reduced.png
```

### Demo with overwriting original image

```bash
$ ls -lh source/

933K original.png

$ docker run -it --rm \
    -v $(pwd)/source/:/var/workdir/ \
    kolyadin/pngquant --verbose -f --ext .png --quality 80-90 original.png
    
original.png:
  read 933KB file
  made histogram...61820 colors found
  selecting colors...3%
  selecting colors...24%
  selecting colors...27%
  selecting colors...48%
  selecting colors...68%
  selecting colors...89%
  selecting colors...100%
  moving colormap towards local minimum
  eliminated opaque tRNS-chunk entries...37 entries transparent
  mapped image to new colors...MSE=1.963 (Q=93)
  writing 256-color image as original.png
Quantized 1 image.
    
$ ls -lh source/

269K original.png
```


### Demo with optimizing all png files in directory

```bash
$ ls -lh source/

933K a.png
933K b.png
933K c.png

$ docker run -it --rm \
    -v $(pwd)/source/:/var/workdir/ \
    kolyadin/pngquant find . -maxdepth 1 -type f -name "*.png" -exec pngquant --verbose -f --ext .png --quality 80-90 {} \;
    
./b.png:
  read 933KB file
  made histogram...61820 colors found
  selecting colors...3%
  selecting colors...24%
  selecting colors...27%
  selecting colors...48%
  selecting colors...68%
  selecting colors...89%
  selecting colors...100%
  moving colormap towards local minimum
  eliminated opaque tRNS-chunk entries...37 entries transparent
  mapped image to new colors...MSE=1.963 (Q=93)
  writing 256-color image as b.png
Quantized 1 image.
./c.png:
  read 933KB file
  made histogram...61820 colors found
  selecting colors...3%
  selecting colors...24%
  selecting colors...27%
  selecting colors...48%
  selecting colors...68%
  selecting colors...89%
  selecting colors...100%
  moving colormap towards local minimum
  eliminated opaque tRNS-chunk entries...37 entries transparent
  mapped image to new colors...MSE=1.963 (Q=93)
  writing 256-color image as c.png
Quantized 1 image.
./a.png:
  read 933KB file
  made histogram...61820 colors found
  selecting colors...3%
  selecting colors...24%
  selecting colors...27%
  selecting colors...48%
  selecting colors...68%
  selecting colors...89%
  selecting colors...100%
  moving colormap towards local minimum
  eliminated opaque tRNS-chunk entries...37 entries transparent
  mapped image to new colors...MSE=1.963 (Q=93)
  writing 256-color image as a.png
Quantized 1 image.
    
$ ls -lh source/

269K a.png
269K b.png
269K c.png
```