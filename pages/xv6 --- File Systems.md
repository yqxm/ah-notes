- ## File
	- A file's name is distinct from the file itself;The same underlying file, called and _inode_, can have multiple names, called _links_.
		- Each _link_ consist of an entry in a directory; The entry contains a file name and a reference to an _inode_.
		- An _inode_ holds _metadata_ about a file, including its type (file or directory or device), its length, the location of the file's content on disk, and the number of links to a file.
		- Each _inode_ is identified by a unique _inode number_.
		- The file's inode and the disk space holding its content are only freed when the file's link count is zero and no file descriptors refer to it.
- ## System call
	- `mkdir` create new directory.
	- `open` with `O_CREATE` flag create a new data file.
	- `mknod` creates a new device file.
		- The two argument to `mknod` are the major and minor device numbers, which uniquely identify a kernel device.
	- ```C
	  mkdir("/dir");
	  fd = open("/dir/file", O_CREATE|O_WRONLY);
	  close(fd);
	  mknod("/console", 1, 1);
	  ```
	- The `fstat` system call retrieves information from the inode that a file descriptor refers to. It fills in a `struct stat`, defined in `stat.h`
		- ```C
		  #define T_DIR  		1
		  #define T_FILE		2
		  #define T_DEVICE	3
		  
		  struct stat {
		  	int dev;		// File system’s disk device
		  	uint ino;		// Inode number
		  	short type; 	// Type of file
		  	short nlink; 	// Number of links to file
		  	uint64 size; 	// Size of file in bytes
		  };
		  ```
	- The `link` system call creates another file system name referring to the same inode as an existing file.
		- ```c
		  open("a", O_CREATE | O_WRONLY);
		  link("a", "b");
		  ```
	- The `unlink` system call removes a name from the file system. `unlink("a");`
		- Create tempory inode with no name that will be cleaned up when the process closes fd or exits.
			- ```C
			  fd = open("/tmp/xyz", O_CREATE | O_RDWR);
			  unlink("/tmp/xyz");
			  ```
		-