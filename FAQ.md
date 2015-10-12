### What are these strange `void *map` and `void *source` I keep seeing? ###

`void *map` is a pointer to your map data. In C, this should be an instance of a C struct you define. In C++, this can be an object.

`void *source` is a pointer to information about the light source. Again, in C, this should be an instance of a C struct you define. In C++, this can be an object. For instance, the light source might hold information about the colour or strength or intensity of the light. You need to know this information in your "apply" callback if you want to do several fancy types of lighting such as coloured lights, flickering torches or variable intensity lights.

These pointers are not used by the library itself, except to pass back to your callback functions so that you can use the information to decide whether a particular tile is opaque and how to apply lighting to it. You should define your own map data structure.

If you do not care about light sources because you are using libfov to calculate line-of-sight or because you want very simple lighting, you can pass NULL as the source pointer.

### Are the 'map data structure' and 'source data structure' structs? Where are they defined? ###

You define these yourself, pass them into any libfov functions that ask for them, and libfov will make sure it passes them back to you when it knows you'll need them - i.e. in your callbacks. This mechanism, although slightly tricky at first,
  * avoids nasty global variables and
  * gives you the liberty of defining your own data structures to suit your own needs.

### The example (simple.cc) seems to give libfov instances of C++ classes... but how can a C lib accept a C++ class? ###

You're right, you can't pass a C++ object to a C library but you _can_ pass a pointer to a C++ object to a C library as long as that C library does not know about the C++ class (i.e. uses void rather than a class), does not try to dereference the pointer, cast the pointer, or do anything with the data being pointed to. In that case, the pointer is just like an integer. In libfov's case, the pointer is never dereferenced, just passed straight back to your callback, which itself can then cast to a C structure or C++ object pointer (because you're back in your own territory where you know what data type that pointer should be).

### Can we please have a Visual C++ example? ###

I do not own a copy of Visual C++, but I am happy to accept any contributions of a Visual C++ project file and a Visual C++ example. I will commit them to libfov's source distribution so people running Visual C++ can get started quicker.

In the long run, I would like to get Visual C++ running in a virtual machine, so I can properly support it, but that's some way off so don't hold your breath.

### Is there a Java wrapper or version? ###

Check out Egor Tsinko's Java port of libfov:
> http://code.google.com/p/libfov/source/browse/java