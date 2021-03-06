TARGET := rdp-test

SOURCES := $(wildcard *.cpp)
OBJECTS := $(SOURCES:.cpp=.o)
DEPS := $(OBJECTS:.o=.d)

#CXX := clang++

CXXFLAGS += -std=c++11 -pthread -Wall -Wextra \
	-Wno-shift-negative-value \
	-Wno-missing-field-initializers -Wno-unused-function \
	-fno-strict-overflow -g -I. -I../../libretro-common/include \
	-DPARALLEL_HAVE_DYLIB

LDFLAGS += -L. -ldl -pthread

DEBUG = 0

ifeq ($(DEBUG), 1)
	CXXFLAGS += -O0
	CXXFLAGS += -DVULKAN_DEBUG
else
	CXXFLAGS += -O2
endif

ifeq ($(SANITIZE), 1)
	CXXFLAGS += -fsanitize=address
	LDFLAGS += -fsanitize=address
endif

build-glsl:
	$(MAKE) -C glsl

all: build-glsl build

build:
	$(MAKE) rdp-test -C.

$(TARGET): $(OBJECTS)
	$(CXX) -o $@ $(OBJECTS) $(LDFLAGS)

-include $(DEPS)

%.o: %.cpp
	$(CXX) -c -o $@ $< $(CXXFLAGS) -MMD

clean:
	rm -f $(OBJECTS) $(TARGET) $(DEPS)
	$(MAKE) -C glsl clean

.PHONY: all clean
