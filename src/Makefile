C := gcc
CXX := g++

AR      := ar
ARFLAGS := crv
RANLIB  := ranlib
SHARED  := -shared
SRC_DIR := 
LIBCXXFLAGS := -fPIC 
LIBCFLAGS := -fPIC
INC := \
	-I. \
	-I/usr/local/include/ \
	-I/usr/local/include/opencv/ \
	-I/usr/local/include/opencv2/ \
	-I../include/ \
	-I../include/common
LDDIR := \
	-L/usr/local/lib/ \
	-L../lib/ \
	-L.
CFLAGS = -Wall ${LIBCFLAGS} ${INC}
CXXFLAGS = -Wall ${LIBCXXFLAGS} ${INC}

EXTRA=
EXTRALIBS=
OPENCVLDLIBS= -lopencv_highgui \
							-lopencv_core \
							-lopencv_imgproc \
							-lopencv_objdetect \
							-lopencv_features2d
OPENCVLIBS= /usr/local/lib/libopencv_highgui.so \
						/usr/local/lib/libopencv_core.so \
						/usr/local/lib/libopencv_imgproc.so \
						/usr/local/lib/libopencv_objdetect.so \
						/usr/local/lib/libopencv_features2d.so
LDLIBS= ${OPENCVLDLIBS}

# turn depend_flag off
DEPEND_FLAGS := -MMD

BIN_NAME=facerecg

# Compiler switches. Default -g.
ifeq ($(RELEASE), 1)
    CFLAGS += -O3 -DNDEBUG -DSHMEM 
    CXXFLAGS += -O3 -DNDEBUG -DSHMEM 
else
    CFLAGS +=  -DDEBUG  -pg -g3 -DSHMEM 
    CXXFLAGS +=  -DDEBUG  -pg -g3 -DSHMEM
endif

# Compiler switches. Defult no multi thread support
ifeq ($(MTHREADS), 1)
    CFLAGS += -DMULTI_THREAD_SUPPORT
    CXXFLAGS += -DMULTI_THREAD_SUPPORT
    EXTRALIBS += -pthread
endif

DEPEND_DIR  := ../obj
DEPEND_TARG := $(DEPEND_DIR)/MADE_FLAG

OBJS = $(patsubst %.cpp,%.o,$(wildcard *.cpp))

all: $(OBJS) main 
	@ echo $^ is up-to-date;

main: $(OBJS)
	$(CXX) $(CXXFLAGS) $(LDDIR) -Wl,-R../lib  -o ../bin/$(BIN_NAME) $^ $(LDLIBS) $(EXTRALIBS) $(INC)

libtbmat.a: $(OBJS)
	$(AR) -$(ARFLAGS) ../lib/$@ $(OBJS) $(OPENCVLIBS)

libtbmat.so: $(OBJS)
	$(CXX) $(SHARED) -o ../lib/$@ $(OBJS)

$(DEPEND_TARG):
	mkdir -p $(DEPEND_DIR);
	touch $@

clean:
	rm -f *.o ;
	rm -f *.d *.gcno;
	rm ../lib/libtbmat.a;
	rm ../lib/libtbmat.so;

#
# Dependency files
#

D_FILES := $(wildcard $(DEPEND_DIR)/*.d)
ifneq ($(strip $(D_FILES)),)
  ifeq ($(origin NODEPEND),undefined)
    include $(D_FILES)
  endif
endif

mkdbg:
	echo DEPEND_DIR: $(DEPEND_DIR)
