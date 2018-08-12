
  1. What is dos64stb good for?

  dos64stb is a small stub that is supposed to be added to a 64-bit 
  PE binary ( thru the link step ). It is the program that is executed
  when the binary is launched in DOS.
  dos64stb will do this:
    - check if the cpu is 64-bit
    - check if the PE image is "acceptable"
    - check if enough XMS memory is available to load the image
    - load the image into extended memory
    - setup GDT and switch to protected-mode
    - handle relocation info ( the image MUST contain base relocations )
    - setup IDT and page tables for 64-bit PAE paging
    - install a small "OS" (int 21h) that allows the image to
      + display a character onto the screen
      + read a character from keyboard
      + terminate and return to DOS
    - call the entry point of the loaded 64-bit image


  2. Requirements
  
  to run an image with the dos64stb-stub one needs:

  - a 64-bit CPU
  - an installed DOS
  - an installed XMS host
  - enough extended memory to load the image

  3. Hot to use?

  The stub is added to a 64-bit binary thru the link step. See file
  Makefile for how to do this with MS link or jwlink. Subsystem must
  be "native", to avoid the image being loaded in Win64. See file
  Mon64.asm, which is a 64-bit sample program that allows to display
  a few 64-bit resources.

  Note that when jwasm is used to assemble a 64-bit binary, it may
  emit a warning that an underscore is needed for the start label.
  This warning is to be ignored.

  The 64-bit binary runs in ring 0, 64-bit protected-mode. There is
  no DOS or Windows API available. The first 64 GB of memory are 
  "identity mapped" by the stub. The memory that is "owned" by the
  binary is everything between the image base and the stackpointer.


  4. License
  
  the source is distributed under the GNU GPL 2 license. See file
  COPYING for details. It was written by Andreas Grech.
