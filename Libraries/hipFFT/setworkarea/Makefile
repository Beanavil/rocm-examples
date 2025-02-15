# MIT License
#
# Copyright (c) 2024 Advanced Micro Devices, Inc. All rights reserved.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

EXAMPLE := hipfft_setworkarea
COMMON_INCLUDE_DIR := ../../../Common
GPU_RUNTIME := HIP

# HIP variables
ROCM_INSTALL_DIR := /opt/rocm
CUDA_INSTALL_DIR := /usr/local/cuda

HIP_INCLUDE_DIR    := $(ROCM_INSTALL_DIR)/include
HIPCUB_INCLUDE_DIR := $(HIP_INCLUDE_DIR)

HIPCXX  ?= $(ROCM_INSTALL_DIR)/bin/hipcc
CUDACXX ?= $(CUDA_INSTALL_DIR)/bin/nvcc

# Common variables and flags
CXX_STD   := c++17
ICXXFLAGS := -std=$(CXX_STD)
ICPPFLAGS := -isystem $(HIPCUB_INCLUDE_DIR) -I $(COMMON_INCLUDE_DIR)
ILDFLAGS  := -L $(ROCM_INSTALL_DIR)/lib
ILDLIBS   := -lhipfft

ifeq ($(GPU_RUNTIME), CUDA)
	ICXXFLAGS += -x cu
	ICPPFLAGS += -isystem $(HIP_INCLUDE_DIR) -D__HIP_PLATFORM_NVIDIA__
	COMPILER  := $(CUDACXX)
else ifeq ($(GPU_RUNTIME), HIP)
	CXXFLAGS  ?= -Wall -Wextra
	ICPPFLAGS += -D__HIP_PLATFORM_AMD__
	COMPILER  := $(HIPCXX)
else
	$(error GPU_RUNTIME is set to "$(GPU_RUNTIME)". GPU_RUNTIME must be either CUDA or HIP)
endif

ICXXFLAGS += $(CXXFLAGS)
ICPPFLAGS += $(CPPFLAGS)
ILDFLAGS  += $(LDFLAGS)
ILDLIBS   += $(LDLIBS)

$(EXAMPLE): main.cpp $(COMMON_INCLUDE_DIR)/example_utils.hpp
	$(COMPILER) $(ICXXFLAGS) $(ICPPFLAGS) $(ILDFLAGS) -o $@ $< $(ILDLIBS)

clean:
	$(RM) $(EXAMPLE)

.PHONY: clean
