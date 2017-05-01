#!/bin/bash
#
# smartbc - shell multibase calculator base on bc
#
#
# This program is provided under the Gnu General Public License (GPL)
# version 2 ONLY. This program is distributed WITHOUT ANY WARRANTY.
# See the LICENSE file, which should have accompanied this program,
# for the text of the license.
#
# 2017-05-01 by zqb-all <zhuangqiubin@gmail.com>
#
#
# CHANGELOG:
#  2017.05.01 - Version 1.0.0 - The first version

scale_num=10
output_format='a'

Name=$(basename "$0")
usage() {
	cat << EOF
Usage: $Name [OPTION]... [EQUATION]

EQUATION may contain numbers with different base:
	hexadecimal : '0x' or '0X',
	decimal	    : '0d' or '0D' or ''(default is decimal),
	octonary    : '0o' or '0O',
	binary      : '0b' or '0B',

You can use the following options to specify the result:
	-x : display the result in hexadecimal
	-d : display the result in decimal
	-o : display the result in octonary
	-b : display the result in binary
	-a : display the result in hexadecimal,decimal,binary,binary (this is the default option)

Default scale is 10. It can be overidden by -s option
	-s : set the scale

Example:
	$Name "0xa-0o5+0b110*(5+1)"
	$Name -x 0xA-5+0b110*5
	the EQUATION contain () will cause error, like this:
		$Name (1+1)
	please add "", like this:
		$Name "(1+1)"
	the EQUATION start with '-' will cause error, like this:
		$Name -d -s 2 -5+0xA/3.4 (error)
	please add 0 or add '--', like this:
		$Name -d -s 2 0-5+0xA/3.4
		$Name -d -s 2 -- -5+0xA/3.4

Use $Name everywhere:
	run
		sudo ln -s `pwd`/$Name /usr/bin/smartbc
	or
		sudo ln -s `pwd`/$Name /usr/bin/mycalc
	then you can use "smartbc" or "mycalc" to invoke $Name

bc is an arbitrary precision calculator language
See \`man bc\` for legal EQUATION operators.


EOF
}


while getopts "s:xodbah" arg
do
        case $arg in
             h)
                usage
		exit
                ;;
             s)
		scale_num=$OPTARG
		echo "scale : $scale_num"
                ;;
             a)
                output_format='a'
                ;;
             x)
                output_format='x'
                ;;
             d)
                output_format='d'
                ;;
             o)
                output_format='o'
                ;;
             b)
                output_format='b'
                ;;
             ?)
            	echo "error: unkonw argument"
		usage
		exit
		;;
        esac
done
shift $((OPTIND-1))  #This tells getopts to move on to the next argument.


EQUATION=$1
if [ -z "$EQUATION" ];then
	echo "error: no EQUATION"
	usage
	exit
fi

#handle the hexadecimal
while echo $EQUATION | grep -q "0[xX]";do
	VA=`echo $EQUATION | sed "s/.*0[xX]\([A-Fa-f0-9]*\).*/\1/" | tr "[a-f]" "[A-F]"`
	VA=`echo "ibase=16;$VA" | bc`
	EQUATION=`echo $EQUATION | sed "s/\(.*\)0[xX][A-Fa-f0-9]*/\1$VA/" `
done

#handle the decimal
while echo $EQUATION | grep -q "0[dD]";do
	#only hexadecimal have A-F, but it work well enough
	VA=`echo $EQUATION | sed "s/.*0[dD]\([A-Fa-f0-9]*\).*/\1/" | tr "[a-f]" "[A-F]"`
	VA=`echo "ibase=10;$VA" | bc`
	EQUATION=`echo $EQUATION | sed "s/\(.*\)0[dD][A-Fa-f0-9]*/\1$VA/" `
done

#handle the octonary
while echo $EQUATION | grep -q "0[oO]";do
	VA=`echo $EQUATION | sed "s/.*0[oO]\([A-Fa-f0-9]*\).*/\1/" | tr "[a-f]" "[A-F]"`
	VA=`echo "ibase=8;$VA" | bc`
	EQUATION=`echo $EQUATION | sed "s/\(.*\)0[oO][A-Fa-f0-9]*/\1$VA/" `
done

#handle the binary
while echo $EQUATION | grep -q "0[bB]";do
	VA=`echo $EQUATION | sed "s/.*0[bB]\([A-Fa-f0-9]*\).*/\1/" | tr "[a-f]" "[A-F]"`
	VA=`echo "ibase=2;$VA" | bc`
	EQUATION=`echo $EQUATION | sed "s/\(.*\)0[bB][A-Fa-f0-9]*/\1$VA/" `
done

#handle the octonary start with 0
while echo $EQUATION | grep -q "[^0-9]0[0-7]";do
	VA=`echo $EQUATION | sed "s/.*[^0-9a-fA-FxX]0\([0-7]*\).*/\1/"`
	VA=`echo "ibase=8;$VA" | bc`
	EQUATION=`echo $EQUATION | sed "s/\(.*[^0-9a-fA-FxX]\)\(0[0-7]*\)/\1$VA/" `
done
echo "Original EQUATION: $1 "
echo "Decimal  EQUATION: $EQUATION"
VA=`echo "scale=$scale_num;$EQUATION" | bc `

case $output_format in
	'a')
		echo base2 : `echo "obase=2;$VA" | bc `
		echo base8 : `echo "obase=8;$VA" | bc `
		echo base10: `echo "obase=10;$VA" | bc `
		echo base16: `echo "obase=16;$VA" | bc `
		;;
	'b')
		echo base2 : `echo "obase=2;$VA" | bc `
		;;
	'o')
		echo base8 : `echo "obase=8;$VA" | bc `
		;;
	'd')
		echo base10 : `echo "obase=10;$VA" | bc `
		;;
	'x')
		echo base16 : `echo "obase=16;$VA" | bc `
		;;
	esac


