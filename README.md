# MinGW_install_zlib_ligpng

bash script, MinGW install zlib ligpng  


Here's a Bash script that will automate the process of building zlib and libpng, ensuring that the headers and libraries are properly installed in your MinGW environment.
Steps:  

1)    Save the following script as build_libs.sh  
2)    Run the script in your MinGW shell  


### script

```
#!/bin/bash

# Define paths for zlib and libpng
ZLIB_VERSION="1.3.1"
LIBPNG_VERSION="1.6.43"
#MINGW_PATH="/path/to/your/MinGW"  # Replace with your actual MinGW path
MINGW_PATH="c:/MinGW"  # my actual MinGW path


# Directories for zlib and libpng source code
ZLIB_DIR="zlib-$ZLIB_VERSION"
LIBPNG_DIR="lpng$LIBPNG_VERSION"

# Step 1: Download zlib
if [ ! -d "$ZLIB_DIR" ]; then
    echo "Downloading zlib-$ZLIB_VERSION..."
    wget https://zlib.net/zlib-$ZLIB_VERSION.tar.gz
    tar -xzf zlib-$ZLIB_VERSION.tar.gz
else
    echo "zlib-$ZLIB_VERSION already downloaded."
fi

# Step 2: Build zlib
cd "$ZLIB_DIR"
echo "Building zlib..."
make -f win32/Makefile.gcc

# Copy zlib headers and library to MinGW
echo "Installing zlib..."
cp zlib.h zconf.h "$MINGW_PATH/include"
cp libz.a "$MINGW_PATH/lib"

cd ..

# Step 3: Download libpng
if [ ! -d "$LIBPNG_DIR" ]; then
    echo "Downloading libpng-$LIBPNG_VERSION..."
    wget https://download.sourceforge.net/libpng/libpng-$LIBPNG_VERSION.tar.gz
    tar -xzf libpng-$LIBPNG_VERSION.tar.gz
else
    echo "libpng-$LIBPNG_VERSION already downloaded."
fi

# Step 4: Build libpng
cd "$LIBPNG_DIR"
echo "Building libpng..."

# Use makefile.gcc to build libpng and specify paths for zlib
make -f scripts/makefile.gcc ZLIBINC="../$ZLIB_DIR" ZLIBLIB="../$ZLIB_DIR"

# Copy libpng headers and library to MinGW
echo "Installing libpng..."
cp png.h pngconf.h "$MINGW_PATH/include"
cp libpng.a "$MINGW_PATH/lib"

echo "Build and installation of zlib and libpng completed."

cd ..
```


### Script doing:  

1) Variables: Set the versions of zlib and libpng to be downloaded. Adjust the MINGW_PATH to point to your actual MinGW installation directory.  
2) Download zlib: The script downloads zlib and extracts it if it’s not already present.  
3) Build and Install zlib: It builds zlib using Makefile.gcc and copies the necessary files (zlib.h, zconf.h, and libz.a) to MinGW's include and lib directories.  
4) Download libpng: Similarly, the script downloads and extracts libpng if it’s not already present.  
5) Build and Install libpng: It builds libpng with the correct paths for zlib, and then copies the necessary files (png.h, pngconf.h, and libpng.a) to the appropriate MinGW directories.  

### Usage:


Make sure the script is executable:

```
chmod +x build_libs.sh
```

Run the script in the MinGW shell:

```
./build_libs.sh
```

This script will automate the downloading, building, and installation process for both zlib and libpng.
