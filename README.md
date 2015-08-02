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
##lab5
