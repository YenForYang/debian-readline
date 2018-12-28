Summary of some of the crap that had to be done:

head -1 ../tilde.c | grep -q 'READLINE_LIBRARY' || sed -i '1s/^/#define READLINE_LIBRARY 1\n/' ../tilde.c 
ccache icc -ipo-c -DHAVE_CONFIG_H -I. -I.. -D_FORTIFY_SOURCE=2 -DRL_LIBRARY_VERSION='"7.0"' -g -O2  -Wformat -march=native -pipe -Wformat-security -I/usr/include/ncursesw ../readline.c ../vi_mode.c ../funmap.c ../keymaps.c ../parens.c ../search.c ../rltty.c ../complete.c ../bind.c ../isearch.c ../display.c ../signals.c ../util.c ../kill.c ../undo.c ../macro.c ../input.c ../callback.c ../terminal.c ../text.c ../nls.c ../misc.c ../history.c ../histexpand.c ../histfile.c ../histsearch.c ../shell.c ../mbutil.c ../tilde.c ../colors.c ../parse-colors.c ../xmalloc.c ../xfree.c ../compat.c -o libreadline.o
xiar crvs libreadline.a libreadline.o

ccache icc -ipo-c -DHAVE_CONFIG_H -I. -I.. -D_FORTIFY_SOURCE=2 -DRL_LIBRARY_VERSION='"7.0"' -g -O2  -Wformat -march=native -pipe -Wformat-security -I/usr/include/ncursesw ../history.c ../histexpand.c ../histfile.c ../histsearch.c ../shell.c ../mbutil.c ../xmalloc.c ../xfree.c -o libhistory.o

xiar crvs libhistory.a libhistory.o

cd shlib && \
{ icc -shared -DHAVE_CONFIG_H   -I.. -I../../  -D_FORTIFY_SOURCE=2 -DRL_LIBRARY_VERSION='"7.0"' -g -Wformat -march=native -pipe -Wformat-security -I/usr/include/ncursesw -fpic -D_REENTRANT -Wl,-soname,libreadline.so.7 -o libreadline.so.7.0 ../../readline.c ../../vi_mode.c ../../funmap.c ../../keymaps.c ../../parens.c ../../search.c ../../rltty.c ../../complete.c ../../bind.c ../../isearch.c ../../display.c ../../signals.c ../../util.c ../../kill.c ../../undo.c ../../macro.c ../../input.c ../../callback.c ../../terminal.c ../../text.c ../../nls.c ../../misc.c ../../history.c ../../histexpand.c ../../histfile.c ../../histsearch.c ../../shell.c ../../mbutil.c ../../tilde.c ../../colors.c ../../parse-colors.c ../../xmalloc.c ../../xfree.c ../../compat.c -ltinfo && \

 icc -shared -DHAVE_CONFIG_H  -I.. -I../../  -D_FORTIFY_SOURCE=2 -DRL_LIBRARY_VERSION='"7.0"' -g -Wformat -march=native -pipe -Wformat-security -I/usr/include/ncursesw -fpic -D_REENTRANT -Wl,-soname,libhistory.so.7 -o libhistory.so.7.0 ../../history.c ../../histexpand.c ../../histfile.c ../../histsearch.c ../../shell.c ../../mbutil.c ../../xmalloc.c ../../xfree.c -ltinfo } && \

{ ln -sf libhistory.so.7.0 libhistory.so.7 && ln -sf libhistory.so.7 libhistory.so; \
  ln -sf libreadline.so.7.0 libreadline.so.7 && ln -sf libreadline.so.7 libreadline.so }

cp -a ../../examples/rlfe ../examples
ln -sf ../../.. ../examples/rlfe/readline && \
cd ../examples/rlfe && \
./configure --prefix=/usr --host=x86_64-linux-gnu \
	CFLAGS="-g -Wformat -march=native -pipe -Wformat-security -I/usr/include/ncursesw -D_GNU_SOURCE" \
	CPPFLAGS=" -D_FORTIFY_SOURCE=2" \
	LDFLAGS=" -g -L../../shlib" \
	LIBS="-lreadline -ltinfo -lutil"

head -1 ../tilde.c | grep -q 'READLINE_LIBRARY' && printf "%s\n\n" "$(tail -n +2 ../tilde.c)" > ../tilde.c





Mainly for my own future reference, I guess.



