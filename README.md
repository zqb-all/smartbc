smartbc
==========

smartbc - shell multibase calculator base on bc

For help with command line options, use:
```bash
smartbc -h
```

### Introduction

bc is an arbitrary precision calculator language

but you need to specify and use only one base number in your equation

smartbc allow you to use multibase numbers in one equation,
all the number will be convert to decimal, then pass to bc.

### Example
```bash
smartbc "0xa-0o5+0b110*(5+1)"
```


