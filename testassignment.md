(for this assignment, and this assignment only, I used my M4 Macbook air)
I started by installing the Go language package from Homebrew, then I copied a basic hello world script, and analyzed it, I found the code on this page https://gobyexample.com/hello-world


	package main

	

	import "fmt"
	func main() {
    	fmt.Println("hello world")
	}


I then built it with the go build command, and then analyzed it with various tools

File yielded "gohelloworld: Mach-O 64-bit executable arm64"
xxd yielded a lot of lines most of it just empty with a few characters inbetween, far too many to analyze but it started with the header, which i then extracted with the head command
It was as follows
	����

	 H__PAGEZERO8__TEXT�
                    �
                     __text__TEXT��	�__symbol_stub1__TEXT�	��    ��__rodata__TEXT��	{���	(__DATA_CONST�
                                      xH
	�
	 xH
	__rodata__DATA_CONST�
                     �p�
                        		__got__DATA_CONST�����9__typelink__DATA_CONST�����__itablink__DATA_CONST`��`�__gosymtab__DATA_CONST����__gopclntab__DATA_CONST�x��(__DATA�__go_buildinfo__DATA0__go_fipsinfo__DATA@x@__noptrdata__DATA�bM�__data__DATA@O�P@O_	_bss__DATA��i__noptrbss__DATA�	�7x__DWARF��:
	__zdebug_abbrev__DWARF�J�__zdebug_line__DWARFJ�2J�__zdebug_frame__DWARF|��r|�__debug_gdb_scri__DWARF
                    C
                     R__zdebug_info__DWARFN��NR__zdebug_loc__DWARF��!!'�__zdebug_ranges__DWARF��#(��?!H__LINKEDIT��o"�o2

                                        (�0�"�0"��"�p�"�	8)#��
                                                                     P�	�	�9p'#r
      /usr/lib/dyld
                   8/usr/lib/libSystem.B.dylib
                                              8/usr/lib/libresolv.9.dylib�zj�
                                                                             @1W��!{#�z��&$�H� Go build ID: "gY8fRqptP9YF49cZhcUE/sWdz-1Js0hbwLZYUNkkD/qw-uCRehigYvh7P1y5ll/mjkP_zNZtI2Zh7BFLlu0"
	 ��

Tail did not output anything aside from random characters, mostly question marks which I interpret as characters iTerm did not recognize
