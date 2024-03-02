# dumpifs-linux
This is a dirty hack of QNX's dumpifs utility, intended for use on non-QNX Linux systems. A compiled x86-64 ELF is included in the repository, along with the resources to compile the tool on a system without the QNX SDP.

**Credit:**
This tool was originally forked from [askac's dumpifs hack](https://github.com/askac/dumpifs), with [sickcodes'](https://github.com/sickcodes) contributions merged in. I decided to create my own fork after the original one went dormant in 2020.

# Example Usage

**View IFS File Structure:** `dumpifs <image.ifs>`

**Decompress IFS and write to a new file:** `dumpifs -u <image_uncompressed.ifs> <image.ifs>`

**Extract all files from IFS to current directory:** `dumpifs -b -x <image.ifs>`

## Options

```
dumpifs - dump an image file system
dumpifs	[-mvxbz -u file] [-f file] image_file_system_file [files]
 -b           Extract to basenames of files
 -u file      Put a copy of the uncompressed image file here
 -d directory The directory to which to extract files. The default is the current working directory. 
 -v           Verbose
 -x           Extract files
 -m           Display MD5 Checksum
 -f file      Extract named file
 -z           Disable the zero check while searching for the startup header.
              This option should be avoided as it makes the search for the
              startup header less reliable.
              Note: this may not be supported in the future.
```

Additional documentation about `dumpifs` may be found on the [QNX Developer Docs Website](https://www.qnx.com/developers/docs/7.0.0/index.html#com.qnx.doc.neutrino.utilities/topic/d/dumpifs.html).

## dumpifs-folderized.sh
In addition to the basic dumpifs tool, this repository contains the `dumpifs-folderized.sh` script.

This script creates a folderized IFS dump, preserving the directory structure of the IFS file (which is usually lost when using `dumpifs` alone)

Usage: `dumpifs-folderized.sh <image.ifs>`

This script was originally contributed by [sickcodes](https://github.com/sickcodes) and created by [bertelsmann](https://turbo-quattro.com/showthread.php?15648-How-to-extract-a-IFS-file&p=367809&viewfull=1#post367809).

# Specific Use Cases

## MIB2 Firmware IFS Extraction and Modification
This information has been paraphrased from the original dumpifs repo, and is courtesy of [askac](https://github.com/askac/dumpifs):

For those who are interested in hacking MIB2 firmware, the compress tool only supports ucl and lzo compressed images. _Use this tool at your own risk_.

### Basic Idea
1. Get firmware  *.ifs.
2. Get an decompressed image copy by -u command
3. Modify the decompressed image using hex editor to bypass login (for example, the login script)
4. Compress the modified image by uuu or zzz (depends on original modified type)
5. Use dd to repack it, verify using dumpifs again (some padding bytes may need to be manually added). You can also use the helper script to do that
6. Get into IPL, rz to retrive the modified ifs and get into the emergency shell. You might want to kill MIBEmergency (directly modify the binary by hex editor or slay command after boot) to stop the emergency boot after seconds which will lock you out again. or you can replace the original shadow file in system with yours.

# Reference
The original code is from
https://github.com/vocho/openqnx
