- ```C
  
  #include <unistd.h>
  #include <sys/mman.h>
  #include <stdio.h>
  #include <fcntl.h>
  
  
  int main(int argc, char *argv[]) {
  
      char *addr;
      int fd = open(argv[1], O_RDONLY);
      if (fd < 0) {
          printf("Error while open.");
          exit(0);
      }
      struct stat sb;
      fstat(fd, &sb);
  
      addr = mmap(NULL, sb.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
  
      write(STDOUT_FILENO, addr, sb.st_size);
  
      return 0;
  }
  ```