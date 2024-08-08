# 定义编译器
CC = mpicc

# 定义编译标志
CFLAGS = -Wall

# 定义目标可执行文件
TARGET = hello_mpi

# 定义源文件
SRC = hello_mpi.c

# 目标：编译生成可执行文件
$(TARGET): $(SRC)
	$(CC) $(CFLAGS) -o $(TARGET) $(SRC)

all: $(TARGET)

# 清理目标：删除生成的可执行文件
clean:
	rm -f $(TARGET)
