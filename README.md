# CSAPP（深入理解计算机系统）
This is based mooc The Hardware/Software Interface(https://class.coursera.org/hwswinterface-002/lecture)
<p>The Hardware/Software Interface这门课程是由华盛顿大学讲授，授课教材为CSAPP，5个实验也移植了CSAPP的课程实验。
  <p>环境配置链接为https://class.coursera.org/hwswinterface-002/wiki/VirtualMachine
  <p>版本库的代码为Coursera上的 The Hardware/Software  Interface课程的实验代码，由于在做习题时系统已经关闭，不保证正确性，有疑问欢迎联系lk375273991@gmail.com.
  
  <p>目前完成的实验有lab2 lab4，lab1 部分完成，剩下lab3 lab5未完成。

##lab0
lab0是一个热身项目，旨在测试环境是否配置正确以进行接下来的几个实验。同时展示了Linux下如何使用gcc来执行C程序。
##lab1
##lab2
##lab3
##lab4
实验lab4旨在考察对cache的理解。在实验中，要完成3个任务：
* 计算cache中块（block）的大小
* 计算cache的大小
* 计算cache的关联性，如2路组相连、直接映射、全相连等。

为了帮助完成这个任务，提供了三个函数以供使用：

        /** Initializes the cache. This function must be called so that the
              cache can initialize its data structures, though the mystery
              caches will ignore the provided arguments (as their parameters are
              hard-coded). */
        void cache_init(int size, int block_size);
          
          /** Lookup an address in the cache. Returns TRUE if the access hits,
              FALSE if it misses. */
        bool_t access_cache(addr_t address);
          
          /** Clears all words in the cache (and the victim buffer, if
              present). Useful for helping you reason about the cache
              transitions, by starting from a known state. */
        void flush_cache(void);
+ cache_init()用于初始化
+ access_cache()用于访问某个地址，这里的地址不是真实的物理地址也不是虚拟地址，事实上为了简化，而是使用0、1、2这样的地址。返回值为TRUE表示命中，FALSE为不命中。
+ flush_cache()用于清除cache。

在正式开始编程计算时，我们先了解一点基础知识，这会为接下来的编程带来极大的帮助。我们使用(addr,hit/miss)来表示对某个地址addr的访问是否为命中/不命中，如(10,miss)表示对地址10访问不命中。刚开始Cache为空，接下来的3次访问(10,miss) (11,hit) (12,miss)。由于Cache为空，第一次访问地址10时结果当然是不命中。在Cache与主存的数据传输是以块为基本单位进行传输的，因此(11,hit)则告诉我们块的大小至少为2.随后对地址12访问的结果miss表明12不在这个块里面，因此我们就能确定block的大小为2.再比如，cache初始为空，访问序列为(10,miss) (18,miss) (10,miss) 。第二次访问地址10为miss意味着我们在访问地址18的时候替换了第一次访问得到的块，因此得到映射方式为直接映射。替换算法为LRU算法。
在了解这些后，就可以着手写代码了。
### int get_block_size(void) 
在计算block时，由上述例子可知，先访问地址0，然后依次访问地址1，2，3，，，第一次访问不命中时终止，此时地址的值就是我们的block的大小。

        int get_block_size(void) {
          /* YOUR CODE GOES HERE */
        	access_cache(0);
        	int i;
        	for(i=0;;i++)
        	{
        		if(access_cache(i))
        		{
        			continue;
        		}
        		else
        			break;
        	}
        	return i;
        }
### int get_cache_size(int size) 
对于每个可能的Cache的大小，访问Cache中的每个块，再额外访问可能引起地址0被替换的块，然后检测地址0是否还在Cache中，如果不在则我们找到了Cache的大小。

    int get_cache_size(int size) {
      /* YOUR CODE GOES HERE */
    	int possible_cache_size;    
    	int tmp = 0;
    	for(possible_cache_size = 1; ;possible_cache_size<<=1)  
    	{
    		flush_cache();
    		for(tmp = 0;tmp <= possible_cache_size;tmp += size)
            {           
    			access_cache(tmp);
            }       
    		if(!access_cache(0))
            {           
    			break;      
    		}   
    	}   
    	return possible_cache_size;
      //return -1;
    }
    
### int get_cache_assoc(int size) 
考虑大小为8的Cache。如果(2,miss)(10,miss)(2,hit)(18,miss)(2,hit)(10,miss)则意味着Cache为2路组相连，而(2,miss)(10,miss)(2,miss)意味着映射方式为直接映射。因此判断映射方式只需要看一个块在第几次被替换掉。这个函数也只需要Cache的大小一个参数，无需知道block的大小。

        int get_cache_assoc(int size) {
          /* YOUR CODE GOES HERE */
        	int i,j;
        	for(i=0;;i+=size)
        	{
        		flush_cache();
        		access_cache(0);
        		for(j=0;j<=i;j+=size)
        		{
        			access_cache(j);
        		}
        		if(!access_cache(0))
        		{
        			return i/size;
        		}
        	}
        	return -1;
        }
##lab5
