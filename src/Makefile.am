if USE_WIN
.rc.o:
	windres $^ -o $@
%.o : %.rc
	windres $^ -o $@

bin_PROGRAMS = foobillardplus

foobillardplus_SOURCES = \
   foobillardplus.rc \
   billard3d.h \
   billard3d.c \
   language.c \
   language.h \
   billmove.c \
   billmove.h \
   billard.c \
   billard.h \
   vmath.c \
   vmath.h \
   ball.c \
   ball.h \
   png_loader.c \
   png_loader.h \
   table.c \
   table.h \
   queue.c \
   queue.h \
   aiplayer.c \
   aiplayer.h \
   options.c \
   options.h \
   evaluate_move.c \
   evaluate_move.h \
   helpscreen.c \
   helpscreen.h \
   textobj.c \
   textobj.h \
   font.c \
   font.h \
   sys_stuff.c \
   sys_stuff.h \
   net_socket.c \
   net_socket.h \
   sound_stuff.c \
   sound_stuff.h \
   menu.c \
   menu.h \
   player.h \
   bumpref.c \
   bumpref.h \
   room.c \
   room.h \
   mesh.h \
   mesh.c \
   Ceiling_light.h \
   barchair.h \
   bartable.h \
   burlap_sofa.h \
   fireplace.h \
   history.h \
   history.c \
   fire.h \
   fire.c \
   fireplace_high.h \
   fireplace_high.c \
   ceilinglamp_high.h \
   ceilinglamp_high.c \
   cartoonguy.h \
   cartoonguy.c \
   sittingboy.h \
   sittingboy.c \
   flower.h \
   flower.c \
   bottle.h \
   bottle.c \
   chess.h \
   chess.c \
   getopt_long.h \
   getopt_long.c
else

bin_PROGRAMS = foobillardplus

foobillardplus_SOURCES = \
   billard3d.c \
   billard3d.h \
   language.c \
   language.h \
   billmove.c \
   billmove.h \
   billard.c \
   billard.h \
   vmath.c \
   vmath.h \
   ball.c \
   ball.h \
   png_loader.c \
   png_loader.h \
   table.c \
   table.h \
   queue.c \
   queue.h \
   aiplayer.c \
   aiplayer.h \
   options.c \
   options.h \
   evaluate_move.c \
   evaluate_move.h \
   helpscreen.c \
   helpscreen.h \
   textobj.c \
   textobj.h \
   font.c \
   font.h \
   sys_stuff.c \
   sys_stuff.h \
   net_socket.c \
   net_socket.h \
   sound_stuff.c \
   sound_stuff.h \
   menu.c \
   menu.h \
   player.h \
   bumpref.c \
   bumpref.h \
   room.c \
   room.h \
   mesh.h \
   mesh.c \
   Ceiling_light.h \
   barchair.h \
   bartable.h \
   burlap_sofa.h \
   fireplace.h \
   history.h \
   history.c \
   fire.h \
   fire.c \
   fireplace_high.h \
   fireplace_high.c \
   ceilinglamp_high.h \
   ceilinglamp_high.c \
   cartoonguy.h \
   cartoonguy.c \
   sittingboy.h \
   sittingboy.c \
   flower.h \
   flower.c \
   bottle.h \
   bottle.c \
   chess.h \
   chess.c\
   getopt_long.h \
   getopt_long.c
endif

SDL_CFLAGS = `sdl-config --cflags`
SDL_LIBS   = `sdl-config --libs`

if USE_MATHSINGLE
MATH_CFLAGS = -DVMATH_SINGLE_PRECISION
else
MATH_CFLAGS =
endif

if USE_SSE
MATH_CFLAGS = -DVMATH_SINGLE_PRECISION
USESSE_CFLAGS = -DUSE_SSE -msse
else
USESSE_CFLAGS =
endif

if USE_NETWORK
NET_LIB = -lSDL_net
NET_CFLAGS = -DNETWORKING
else
NET_LIB =
NET_CFLAGS =
endif

if USE_TOUCH
TOUCH_CFLAGS = -DTOUCH
else
TOUCH_CFLAGS =
endif

if USE_WETAB
WETAB_CFLAGS = -DWETAB
TOUCH_CFLAGS = -DTOUCH
else
WETAB_CFLAGS =
endif

if USE_NVIDIA
NVIDIA_CFLAGS =
else
NVIDIA_CFLAGS = -DNO_NV_BUMPREF -DNO_NV_FRESNEL
endif

if USE_SOUND
SOUND_MIXER = -lSDL_mixer
SOUND_CFLAGS = -DUSE_SOUND
else
SOUND_MIXER =
SOUND_CFLAGS =
endif

if FAST_MATH
FASTMATH_CFLAGS = -DFAST_MATH
else
FASTMATH_CFLAGS =
endif

if USE_DEBIAN
DEBIAN_CFLAGS = -DDATA_DIRECTORY=${DATA_DIRECTORY} -DUSE_DEBIAN
else
DEBIAN_CFLAGS =
endif

if USE_WIN
USEWIN_CFLAGS = -DUSE_WIN -static-libgcc -lsupc++ -Wl,-nostdlib -Wl,-s
LDADD_FLAGS = -lpng -lz -lfreetype -lglu32 -lopengl32 ${SOUND_MIXER}
else
USEWIN_CFLAGS =
LDADD_FLAGS = -lGL -lGLU -lpng -lfreetype ${NET_LIB} ${SOUND_MIXER}
endif

AM_CFLAGS = -Wall ${FT2_CFLAGS} ${SDL_CFLAGS} ${NVIDIA_CFLAGS} ${SOUND_CFLAGS} ${DEBUG_CFLAGS} ${WETAB_CFLAGS} ${TOUCH_CFLAGS} ${NET_CFLAGS} ${MATH_CFLAGS} \
            ${OPTIMIZATION_CFLAGS} ${USESSE_CFLAGS} ${USEWIN_CFLAGS} ${FASTMATH_CFLAGS} ${DEBIAN_CFLAGS} -lm

AM_LDFLAGS = ${all_libraries} ${FT2_LIBS} ${SDL_LIBS}
LDADD = ${LDADD_FLAGS}
