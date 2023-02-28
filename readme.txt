CS525: Advanced Database Organization
Assignment 2 - Buffer Manager
Group 25


Team Members
Rushikesh Kadam A20517258
Haren Amal A20513547
Gabriel Baranes A20521263


How to run the program:
1. Go to the project root folder (assign2) from the terminal.
2. Execute "make clearall" command to clear all the previously compiled files (.o files).
3. Execute "make" command to compile the program files, link and create executables, and then run the test cases.


Functions Overview:


Note: The overview of the each functions are described below. For more detailed understanding, read through the comments along with the code.


removeBufferItem :
This procedure changes the back of a buffer queue after removing an item from the front and returning the removed item's page number.
The function returns the error code RC QUEUE IS EMPTY if the queue is empty. After searching the queue for the page to delete, the function
adjusts the back pointer as necessary and provides the page number of the deleted page.


addBufferItem :
The buffer queue receives a page thanks to this function. The least recently accessed page is removed first if the queue is already full.
The new node for the new page is subsequently created and brought to the front of the queue. The function reads the page from the file
and assigns the contents to the BM PageHandle if the queue is empty. Once finished, the method returns RC OK. A few global variables that
track the total number of I/O operations are also updated by the function.


pinPageLRU :
This function uses the LRU (Least Recently Used) approach to pin a page to the buffer pool. If the requested page is already in the buffer
queue,  the method raises the page's fix count and returns its data. If the page is not already in the buffer queue, addBufferItem()
is called to add it. After that, the method modifies the page's fix count and returns its data.


pinPage :
This function pins a page identified by pageNum to a frame in the buffer pool using either the FIFO or LRU page replacement strategy based
on the strategy selected in the buffer pool passed as an argument. It returns a result code indicating the success or failure of the operation.


updateBM_BufferPool :
The function modifies a buffer pool structure's characteristics according to the page file name, the amount of pages, and the replacement
method that are provided. Moreover, memory is allotted for the data in the buffer pool, and calloc is used to set it to zero.
Lastly, it configures the buffer pool's mgmtData pointer to point at the memory block that was allotted.


initBufferPool :
The initBufferPool function initializes the buffer pool based on the parameters provided. First it initializes some variables, allocates memory
for file handles, and buffers queue structures. Then it updates the buffer pool with the specified file name, page number and replacement method.
Opens the paging file using a file handle and initializes the buffer pool queue. Frees the allocated memory and returns an error code if an error occurs.
Finally, it returns RC_OK to indicate successful initialization.


shutdownBufferPool :
This function iterates through all the pages in the buffer pool and writes any dirty pages back to disk, if they are not pinned by any client.
If any write operation fails, the function returns RC_WRITE_FAILED. Finally, the function closes the file handle associated with the buffer pool.

