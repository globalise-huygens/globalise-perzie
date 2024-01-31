# Bronnen tot de geschiedenis der Oostindische Compagnie in Perzië, 1611-1638

## Download pdf files
<https://resources.huygens.knaw.nl/retroboeken/perzie/>

## Extract ppm images from pdf files

```bash
mkdir ppm

for file in /path/to/folder/*.pdf; do
  prefix=$(basename "$file" .pdf)
  pdftoppm -r 300 "$file" /path/to/folder/ppm/"$prefix"
done
```

## OCR

Using [Tesseract](https://tesseract-ocr.github.io/) version 5.3.1

```bash
$ tesseract --version
tesseract 5.3.1
 leptonica-1.82.0
  libgif 5.2.1 : libjpeg 8d (libjpeg-turbo 2.1.5.1) : libpng 1.6.39 : libtiff 4.5.0 : zlib 1.2.11 : libwebp 1.3.0 : libopenjp2 2.5.0
 Found SSE4.1
 Found libarchive 3.6.2 zlib/1.2.11 liblzma/5.4.1 bz2lib/1.0.8 liblz4/1.9.4 libzstd/1.5.4
 Found libcurl/7.87.0 SecureTransport (LibreSSL/3.3.6) zlib/1.2.11 nghttp2/1.51.0
```

### TXT

```bash
mkdir txt

for f in images/*.ppm; do
  output_file="txt/$(basename "${f%.ppm}")"
  tesseract "$f" "$output_file" \
  -l nld \
  -c tessedit_char_whitelist="\"-()'’.,1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ;:" \
  -c preserve_interword_spaces=1;
done
```

### Sort files into folders
```bash
sourcedir="txt/"
prefixes=$(find "$sourcedir" -type f -name "*.txt" -exec basename {} \; | awk -F '-' '{print $1}' | sort -u)

for prefix in $prefixes; do
    folder_path="$sourcedir/$prefix"
    mkdir -p "$folder_path"

    mv "$sourcedir/$prefix"* "$folder_path/"
done
```

## License

[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg

### Notes on use
* pages 1-672 contain original transcriptions
* contains footnotes

