.PHONY: all
all: out/libsubstitute.0.dylib

CC=xcrun -sdk iphoneos clang 
CFLAGS= -arch arm64 -I./lib -I./substrate -I./vendor -fvisibility=hidden -O3 -miphoneos-version-min=11.0 -c -o
LDFLAGS= -dylib -undefined dynamic_lookup -o 
LDID= ldid
SIGN_FLAGS= -S./ent.plist

out/generated/darwin-inject-asm.o: generated/darwin-inject-asm.S
	mkdir -p out/generated
	$(CC) $(CFLAGS) out/generated/darwin-inject-asm.o  ./generated/darwin-inject-asm.S

out/lib/darwin/find-syms.o: lib/darwin/find-syms.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/find-syms.o ./lib/darwin/find-syms.c

out/lib/darwin/inject.o: lib/darwin/inject.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/inject.o  ./lib/darwin/inject.c

out/lib/darwin/interpose.o: lib/darwin/interpose.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/interpose.o  ./lib/darwin/interpose.c

out/lib/darwin/objc-asm.o: lib/darwin/objc-asm.S
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/objc-asm.o ./lib/darwin/objc-asm.S

out/lib/darwin/objc.o: lib/darwin/objc.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/objc.o ./lib/darwin/objc.c

out/lib/darwin/read.o: lib/darwin/read.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/read.o  ./lib/darwin/read.c

out/lib/darwin/substrate-compat.o: lib/darwin/substrate-compat.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/substrate-compat.o ./lib/darwin/substrate-compat.c

out/lib/darwin/execmem.o: lib/darwin/execmem.c
	mkdir -p out/lib/darwin
	$(CC) $(CFLAGS) out/lib/darwin/execmem.o ./lib/darwin/execmem.c

out/lib/cbit/vec.o: lib/cbit/vec.c
	mkdir -p out/lib/cbit
	$(CC) $(CFLAGS) out/lib/cbit/vec.o ./lib/cbit/vec.c

out/lib/jump-dis.o: lib/jump-dis.c
	mkdir -p out/lib
	$(CC) $(CFLAGS) out/lib/jump-dis.o  ./lib/jump-dis.c

out/lib/transform-dis.o: lib/transform-dis.c
	mkdir -p out/lib
	$(CC) $(CFLAGS) out/lib/transform-dis.o ./lib/transform-dis.c

out/lib/hook-functions.o: lib/hook-functions.c
	mkdir -p out/lib
	$(CC) $(CFLAGS) out/lib/hook-functions.o ./lib/hook-functions.c

out/lib/strerror.o: lib/strerror.c
	mkdir -p out/lib
	$(CC) $(CFLAGS) out/lib/strerror.o ./lib/strerror.c

OBJS= out/generated/darwin-inject-asm.o \
	out/lib/darwin/find-syms.o \
	out/lib/darwin/inject.o \
	out/lib/darwin/interpose.o \
	out/lib/darwin/objc-asm.o \
	out/lib/darwin/objc.o \
	out/lib/darwin/read.o \
	out/lib/darwin/substrate-compat.o \
	out/lib/darwin/execmem.o \
	out/lib/cbit/vec.o \
	out/lib/jump-dis.o \
	out/lib/transform-dis.o \
	out/lib/hook-functions.o \
	out/lib/strerror.o 
out/libsubstitute.dylib: $(OBJS) ent.plist
	mkdir -p out
	#xcrun --sdk iphoneos cc -arch arm64 -dynamiclib -lobjc -install_name /usr/lib/libsubstitute.0.dylib -O3 -dead_strip -miphoneos-version-min=11.0 $(OBJS) -o $@
	ar -rc out/libsubstitute.a $(OBJS)
	rm -rdf out/lib/
	rm -rdf out/generated/
	#$(LDID) $(SIGN_FLAGS) $@

out/libsubstitute.0.dylib: out/libsubstitute.dylib
	mkdir -p substitute/out
	#ln -nfs libsubstitute.dylib $@
	mkdir -p out/usr/include/substitute
	cp -f lib/substitute.h out/usr/include/substitute/substitute.h
	cp -f substrate/substrate.h  out/usr/include/substitute/substrate.h
	
clean:
	rm -rfd ./out