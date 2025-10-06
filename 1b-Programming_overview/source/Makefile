all: cpu_code gpu_code gpu_code_managed apu_code openmp_code openmp_code1

AMDGPU_GFXMODEL ?= $(strip $(shell rocminfo |grep -m 1 -E gfx[^0]{1} | sed -e 's/ *Name: *//'))

OPENMP_FLAGS = -fopenmp --offload-arch=${AMDGPU_GFXMODEL}
CFLAGS = -g -O3 -fstrict-aliasing ${OPENMP_FLAGS}
LDFLAGS = ${OPENMP_FLAGS} -fno-lto -lm

cpu_code: cpu_code.c
	amdclang -g -O3 cpu_code.c -o cpu_code

gpu_code: gpu_code.hip
	hipcc -g -O3 --offload-arch=${AMDGPU_GFXMODEL} gpu_code.hip -o gpu_code

gpu_code_managed: gpu_code_managed.hip
	hipcc -g -O3 --offload-arch=${AMDGPU_GFXMODEL} gpu_code_managed.hip -o gpu_code_managed

apu_code: apu_code.hip
	hipcc -g -O3 --offload-arch=${AMDGPU_GFXMODEL} apu_code.hip -o apu_code

openmp_code: openmp_code.o
	$(CC) $(LDFLAGS) $^ -o $@

openmp_code1: openmp_code1.o
	$(CC) $(LDFLAGS) $^ -o $@

clean:
	rm -f cpu_code gpu_code gpu_code_managed apu_code openmp_code openmp_code1 *.o
