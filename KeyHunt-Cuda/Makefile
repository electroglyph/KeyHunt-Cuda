#---------------------------------------------------------------------
# Makefile for KeyHunt
#
# Author : Jean-Luc PONS

SRC = Base58.cpp IntGroup.cpp Main.cpp Bloom.cpp Random.cpp \
      Timer.cpp Int.cpp IntMod.cpp Point.cpp SECP256K1.cpp \
      KeyHunt.cpp GPU/GPUGenerate.cpp hash/ripemd160.cpp \
      hash/sha256.cpp hash/sha512.cpp hash/ripemd160_sse.cpp \
      hash/sha256_sse.cpp GmpUtil.cpp CmdParse.cpp

OBJDIR = obj

ifdef gpu

OBJET = $(addprefix $(OBJDIR)/, \
        Base58.o IntGroup.o Main.o Bloom.o Random.o Timer.o Int.o \
        IntMod.o Point.o SECP256K1.o KeyHunt.o GPU/GPUGenerate.o \
        hash/ripemd160.o hash/sha256.o hash/sha512.o \
        hash/ripemd160_sse.o hash/sha256_sse.o \
        GPU/GPUEngine.o GmpUtil.o CmdParse.o)

else

OBJET = $(addprefix $(OBJDIR)/, \
        Base58.o IntGroup.o Main.o Bloom.o Random.o Timer.o Int.o \
        IntMod.o Point.o SECP256K1.o KeyHunt.o GPU/GPUGenerate.o \
        hash/ripemd160.o hash/sha256.o hash/sha512.o \
        hash/ripemd160_sse.o hash/sha256_sse.o \
        GmpUtil.o CmdParse.o)

endif

CXX        = g++
CUDA       = /usr/local/cuda
CXXCUDA    = /usr/bin/g++
NVCC       = $(CUDA)/bin/nvcc
# nvcc requires joint notation w/o dot, i.e. "5.2" -> "52"
ccap       = $(shell echo $(CCAP) | tr -d '.')

ifdef gpu
ifdef debug
CXXFLAGS   = -DWITHGPU -m64  -mssse3 -Wno-write-strings -g -I. -I$(CUDA)/include
else
CXXFLAGS   =  -DWITHGPU -m64 -mssse3 -Wno-write-strings -O2 -I. -I$(CUDA)/include
endif
LFLAGS     = -lgmp -lpthread -lcrypto -L$(CUDA)/lib64 -lcudart
else
ifdef debug
CXXFLAGS   = -m64 -mssse3 -Wno-write-strings -g -I. -I$(CUDA)/include
else
CXXFLAGS   =  -m64 -mssse3 -Wno-write-strings -O2 -I. -I$(CUDA)/include
endif
LFLAGS     = -lgmp -lpthread -lcrypto
endif


#--------------------------------------------------------------------

ifdef gpu
ifdef debug
$(OBJDIR)/GPU/GPUEngine.o: GPU/GPUEngine.cu | $(OBJDIR) $(OBJDIR)/GPU
	$(NVCC) -G -maxrregcount=0 --ptxas-options=-v --compile --compiler-options -fPIC -ccbin $(CXXCUDA) -m64 -g -I$(CUDA)/include -gencode=arch=compute_$(ccap),code=sm_$(ccap) -o $(OBJDIR)/GPU/GPUEngine.o -c GPU/GPUEngine.cu
else
$(OBJDIR)/GPU/GPUEngine.o: GPU/GPUEngine.cu | $(OBJDIR) $(OBJDIR)/GPU
	$(NVCC) -maxrregcount=0 --ptxas-options=-v --compile --compiler-options -fPIC -ccbin $(CXXCUDA) -m64 -O3 -I$(CUDA)/include -gencode=arch=compute_$(ccap),code=sm_$(ccap) -o $(OBJDIR)/GPU/GPUEngine.o -c GPU/GPUEngine.cu
endif
endif

.DEFAULT_GOAL := all

# pony-tail: explicit prereqs let make detect which .cpp provides each .o
$(OBJDIR)/Base58.o: Base58.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/IntGroup.o: IntGroup.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Main.o: Main.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Bloom.o: Bloom.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Random.o: Random.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Timer.o: Timer.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Int.o: Int.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/IntMod.o: IntMod.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/Point.o: Point.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/SECP256K1.o: SECP256K1.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/KeyHunt.o: KeyHunt.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/GPU/GPUGenerate.o: GPU/GPUGenerate.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/hash/ripemd160.o: hash/ripemd160.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/hash/sha256.o: hash/sha256.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/hash/sha512.o: hash/sha512.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/hash/ripemd160_sse.o: hash/ripemd160_sse.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/hash/sha256_sse.o: hash/sha256_sse.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/GmpUtil.o: GmpUtil.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<
$(OBJDIR)/CmdParse.o: CmdParse.cpp | $(OBJDIR) $(OBJDIR)/GPU $(OBJDIR)/hash
	$(CXX) $(CXXFLAGS) -o $@ -c $<

all: KeyHunt

KeyHunt: $(OBJET)
	@echo Making KeyHunt...
	$(CXX) $(OBJET) $(LFLAGS) -o KeyHunt

$(OBJDIR):
	mkdir -p $(OBJDIR)

$(OBJDIR)/GPU: $(OBJDIR)
	cd $(OBJDIR) &&	mkdir -p GPU

$(OBJDIR)/hash: $(OBJDIR)
	cd $(OBJDIR) &&	mkdir -p hash

clean:
	@echo Cleaning...
	@rm -f obj/*.o
	@rm -f obj/GPU/*.o
	@rm -f obj/hash/*.o

