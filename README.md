# FileTypes.jl

Small and dependency free [Julia](https://julialang.org/) package to infer file and MIME type checking the [magic numbers](<https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files>) signature.

[![Build Status](https://github.com/JuliaIO/FileTypes.jl/actions/workflows/CI.yml/badge.svg)](https://github.com/JuliaIO/FileTypes.jl)
[![Coverage](https://codecov.io/gh/JuliaIO/FileType.jl/branch/main/graph/badge.svg)](https://codecov.io/gh/JuliaIO/FileType.jl)

## Features

- Supports a [wide range](#supported-types) of file types
- Provides file extension and proper MIME type
- File discovery by extension or MIME type
- File discovery by class (image, video, audio...)
- Provides a bunch of helpers and file matching shortcuts
- Simple and semantic API



## General Info
currently under developement.

Take a look on [contribution](#Contribution)



## Examples

#### Simple file type checking

```julia
  using FileTypes

  julia> kind = matcher("example.gif")
  FileType.Types("gif", "image/gif")

  julia> kind.extension
  "gif"

  julia> kind.mime
  "image/gif"

```

#### Check type class

```julia
  using FileTypes
  julia> Is(FileType.Image,"example.gif")
  true
```

#### additional checker function

```julia
  using FileTypes

  julia> is_mime_supported("image/jpeg")
  true

  julia> is_extension_supported("png")
  true

```

#### You can see the supported file type using 

```julia
  julia> using FileTypes

  julia> FileType
  FileTypes.FileType

  julia> FileType.Image
  Dict{FileTypes.FileType.Type, Function} with 12 entries:
    Type("gif", MIME type image/gif)                 => Gif
    Type("cr2", MIME type image/x-canon-cr2)         => CR2
    Type("jp2", MIME type image/jp2)                 => Jpeg2000
    Type("psd", MIME type image/vnd.adobe.photoshop) => Psd
    Type("bmp", MIME type image/bmp)                 => Bmp
    Type("tif", MIME type image/tiff)                => Tiff
    Type("jpg", MIME type image/jpeg)                => Jpeg
    Type("ico", MIME type image/vnd.microsoft.icon)  => Ico
    Type("png", MIME type image/png)                 => Png
    Type("webp", MIME type image/webp)               => Webp
    Type("jxr", MIME type image/vnd.ms-photo)        => Jxr
    Type("dwg", MIME type image/vnd.dwg)             => Dwg

```

## Supported types

#### Image

- **jpg** - `image/jpeg`
- **png** - `image/png`
- **gif** - `image/gif`
- **webp** - `image/webp`
- **cr2** - `image/x-canon-cr2`
- **tif** - `image/tiff`
- **bmp** - `image/bmp`
- **heif** - `image/heif`
- **jxr** - `image/vnd.ms-photo`
- **psd** - `image/vnd.adobe.photoshop`
- **ico** - `image/vnd.microsoft.icon`
- **dwg** - `image/vnd.dwg`

#### Video

- **m4v** - `video/x-m4v`
- **webm** - `video/webm`
- **mov** - `video/quicktime`
- **avi** - `video/x-msvideo`
- **wmv** - `video/x-ms-wmv`
- **mpg** - `video/mpeg`
- **flv** - `video/x-flv`
- **3gp** - `video/3gpp`

#### Audio

- **mid** - `audio/midi`
- **mp3** - `audio/mpeg`
- **m4a** - `audio/m4a`
- **ogg** - `audio/ogg`
- **flac** - `audio/x-flac`
- **wav** - `audio/x-wav`
- **amr** - `audio/amr`
- **aac** - `audio/aac`

#### Archive

- **epub** - `application/epub+zip`
- **zip** - `application/zip`
- **tar** - `application/x-tar`
- **rar** - `application/vnd.rar`
- **gz** - `application/gzip`
- **bz2** - `application/x-bzip2`
- **7z** - `application/x-7z-compressed`
- **xz** - `application/x-xz`
- **pdf** - `application/pdf`
- **exe** - `application/vnd.microsoft.portable-executable`
- **swf** - `application/x-shockwave-flash`
- **rtf** - `application/rtf`
- **iso** - `application/x-iso9660-image`
- **eot** - `application/octet-stream`
- **ps** - `application/postscript`
- **sqlite** - `application/vnd.sqlite3`
- **nes** - `application/x-nintendo-nes-rom`
- **crx** - `application/x-google-chrome-extension`
- **cab** - `application/vnd.ms-cab-compressed`
- **deb** - `application/vnd.debian.binary-package`
- **ar** - `application/x-unix-archive`
- **Z** - `application/x-compress`
- **lz** - `application/x-lzip`
- **rpm** - `application/x-rpm`
- **elf** - `application/x-executable`
- **dcm** - `application/dicom`

#### Documents

- **doc** - `application/msword`
- **xls** - `application/vnd.ms-excel`
- **ppt** - `application/vnd.ms-powerpoint`


#### Font

- **woff** - `application/font-woff`
- **woff2** - `application/font-woff`
- **ttf** - `application/font-sfnt`
- **otf** - `application/font-sfnt`

#### Application

- **wasm** - `application/wasm`
- **dex** - `application/vnd.android.dex`
- **dey** - `application/vnd.android.dey`

## Contribution

- Currently, the ``Is`` API takes a parameter as ``FileType.Images.Image`` which is a dictionary of all the supported image. I think it should be an (immutable)namedtuple as the custom filetype should be of unknown type only.    
- There should be an API to add new custom Types.
- Currently, Filetypes(publicly accessed) is a tuple that should be an Array as this library has to provide an API to add a new type, and that new type could we handled in ``Is`` API like FileType.Unknown.
- The ``FileType.Images.Image`` should be ``FileType.Images``


## Benchmark

system info:
SMP Debian 5.8.7-1kali1 (2020-09-14) x86_64 GNU/Linux
Intel(R) Core(TM) i3-6006U CPU @ 2.00GHz

```julia

julia> @benchmark matcher("example.png")
BenchmarkTools.Trial: 
  memory estimate:  1.69 KiB
  allocs estimate:  16
  --------------
  minimum time:     12.304 μs (0.00% GC)
  median time:      12.859 μs (0.00% GC)
  mean time:        14.708 μs (0.00% GC)
  maximum time:     329.586 μs (0.00% GC)
  --------------
  samples:          10000
  evals/sample:     1


```

## License

MIT - Prince Roshan
