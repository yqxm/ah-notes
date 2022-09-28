- ## CMD_X
	- ```C
	  static int cmd_x(char *args) {
	    char *first_arg = strtok(args, " ");
	  
	    int N;
	    char *expr;
	    unsigned long address;
	    if (first_arg[0] == '0' && first_arg[1] == 'x') {
	      N = 1;
	      expr = first_arg;
	    } else {
	      sscanf(first_arg, "%d", &N);
	      expr = strtok(NULL, " ");
	    }
	  
	    int parse_res;
	    if ( (parse_res = sscanf(expr, "%lx", &address)) <= 0) {
	      printf("usage: x N EXPR. EXPR is Hex with prefix \"0x\"");
	    }
	  
	    printf("address: 0x%lx\n", address);
	    word_t *ap = (word_t *) address;
	    for (int i = 0; i < N; i++) {
	      if (i % 4 == 0) {
	        printf("0x%lx: ", address);
	      }
	      printf("0x%x\t",*ap);
	      if (i % 4 == 3) {
	        printf ("\n");
	      }
	      address += 4;
	    }
	  
	    return 0;
	  }
	  ```