# Disign Abstract Class

1. Do not provide an implementation for pure virtual methods unless it is necessary.
2. Do not make your destructor pure virtual.
3. Do not make your constructor protected. You cannot create an instance of an abstract class.
4. Better hide an implementation of constructor and destructor inside a source file not to pollute other object files.
5. Make your interface non-copyable.

If this is an interface, better do not have any variables there. Otherwise it would be an abstract base class and not an interface.

Too many pure functions is OK unless you can do it with less pure functions.