#wimlib 文档

##1. 基本 WIM 处理概念

wimlib生成一个WIM文件（使用WIMSstruct结构体）。有两种方式去创建这样一个结构体：

（1）wimlib_open_wim()  

这是打开一个WIM文件并且创建一个WIMStruct代替它

（2）wimlib_create_new_wim()  

这是创建一个新的WIMStruct，它初始没有包含任何镜像并且还没有一个备份磁盘文件

See Creating and Opening WIMs for more details.

一个WIM文件，被一个WIMStruct文件表示，它包含0个或多个镜像。

镜像被提取（或者applied）： wimlib_extract_image()

镜像被增加（"captured" or "appended"）：wimlib_add_image()

删除镜像： wimlib_delete_image()

镜像导出： wimlib_export_image()

镜像更新或修改：wimlib_update_image()

实际写到磁盘文件中： wimlib_write()

！！如果WIM初始打开使用的是 wimlib_open_wim()，则需要使用 wimlib_overwrite()替代wimlib_write()来实现写入

See Extracting WIMs, Modifying WIMs, and Writing and Overwriting WIMs for more details.

注意在 WIMStruct 这个抽象的情况下，执行许多任务在WIM文件上是一个多步骤的过程。举例如下：为了增加一个镜像到一个已经存在的WIM文件中（wimlib-imagex append），必须依次使用以下函数：

    wimlib_open_wim()
    wimlib_add_image()
    wimlib_overwrite()

这个设计的目的是使这个库更加有用（通过允许不同方式的函数组合）。比如你可以多次改变WIM并且提交这些改变去磁盘文件中只使用一次 overwrite操作，从而提高效率

注意：在调用任何在 wimlib.h中声明的函数之前，wimlib_global_init() 可以被调用（一些时候是必需的），See its documentation for more details.

##清理

在你完成了任何 WIMStruct结构体相关操作之后，你可以调用wimlib_free()去释放与它相联的资源。当然，当你使用wimlib在你的程序中完成所有任务以后，你可以调用wimlib_global_cleanup()去释放任何其他为库分配的资源。

##错误处理

wimlib大部分函数成功返回0，并且在失败时会返回一个正数的wimlib_error_code值。使用wimlib_get_error_string()去获得一个描述错误代码的字符串。wimlib当然也会在发生一个错误时，在标准错误中输出错误信息。这个可能比错误代码会有更多的信息；为了使用它，需要调用wimlib_set_print_errors()。请注意这个只是为了便利，并且一些错误可能发生却并没有信息输出。当前，错误信息和字符串只能使用英语。

##语言环境和字符编码

为了支持Windows和UNIX-like系统，wimlib的API通常传递和返回wimlib_tchar格式的字符串，这是一种平台依赖的编码。

在Windows上，每个wimlib_tchar是2字节的并且它和一个“wchar_t"相同，并且编码使用UTF-16LE

在UNIX-like系统上，每个wimlib_tchar是1字节的并且如一个“char”，并且编码是语言环境依赖多种编码。推荐设置语言环境为UTF-8来避免出现问题。当然，默认的wimlib在UNIX将假定语言环境是UTF-8，除非你在设置满意的语言环境以后调用了wimlib_global_init().

#2.详细描述

##（1）Creating and Opening WIMs

简介：打开一个存在的WIM文件使用一个WIMStruct，或者创建一个新的WIMstruct用来创建一个新的WIM文件

###宏

    #define 	WIMLIB_OPEN_FLAG_CHECK_INTEGRITY   0x00000001
 	检查WIM内容是否与WIM的完整性表（如果存在）相同。完整性表存储一个校验值，检查将会计算校验值并且与存储的校验值进行比较。如果这里有任何不匹配，之后
 	WIMLIB_ERR_INTEGRITY将会发出。如果WIM文件没有包含一个完整性表，那么这个flag没有效果。
    
    #define 	WIMLIB_OPEN_FLAG_ERROR_IF_SPLIT   0x00000002
 	发出一个错误（WIMLIB_ERR_IS_SPLIT_WIM），如果这个WIM是一个拆分WIM的一部分，软件可以提供这个flag如果确实不想支持拆分WIMs。
 
    #define 	WIMLIB_OPEN_FLAG_WRITE_ACCESS   0x00000004
    检查是否WIM是可写的，如果不是可写的，会发出一个错误(WIMLIB_ERR_WIM_IS_READONLY)
    一个WIM被认为是可写的只有它在文件系统层次上可写的，没有 WIM_HDR_FLAG_READONLY 位设置，并且不是spanned集的一部分。在尝试修改WIM之前不需要提供这个
    flag，但是有了这个flag，你可以立刻获得一个错误，而不是当wimlib_overwrite()最后调用时才能得到信息。
    
### 函数

####I.wimlib_create_new_wim:

创建一个WIMStruct,这是初始没有包含镜像并且没有在磁盘上存储的。

    int wimlib_create_new_wim 	( 	enum wimlib_compression_type  	ctype,
		  WIMStruct **  	wim_ret 
	   ) 		

####参数

####ctype
给WIMStruct指定“输出压缩类型”，如果使用 wimlib_write()来使WIMStruct保存在磁盘文件中将使用这个压缩类型

在调用wimlib_write()之前可以通过wimlib_set_output_compression_type()来进行修改，另外如果你想使用非默认的chunk大小，你需要调用wimlib_set_output_chunk_size().


####wimret   

在成功的情况下，这个参数是一个对WIMStruct指针，它被写在内存中。当结束的时候这个WIMStruct必须被释放，使用wimlib_free() 

####返回

0代表成功，一个wimlib_error_code 代表失败

####返回值	

WIMLIB_ERR_INVALID_COMPRESSION_TYPE：ctype不是一个支持的压缩类型

WIMLIB_ERR_NOMEM：不充分的内存去分配一个新的WIMStruct

####II.int wimlib_open_wim：

打开一个WIM文件并且创建为它创建一个WIMStruct。

    int wimlib_open_wim 	( 	const wimlib_tchar *  	wim_file,
    		int  	open_flags,
    		WIMStruct **  	wim_ret 
	   ) 
	   	   
####参数

####wim_file

要打开WIM文件的路径

####open_flags

按位或者前缀符的WIMLIB_OPEN_FLAG

####wim_ret 

成功的话，一个指针指向新的WIMStruct（指定磁盘WIM文件）被写在的内存位置，当结束的时候这个WIMStruct必须被释放，使用wimlib_free() 
    
####返回

0代表成功，一个wimlib_error_code 代表失败

####返回值

WIMLIB_ERR_IMAGE_COUNT:这个WIM文件中的metadata资源数没有和指定的WIM头中的image数匹配，或者这个WIM文件中在XML数据中的IMAGE元素数没有和WIM头中的image数匹配

WIMLIB_ERR_INTEGRITY：WIMLIB_OPEN_FLAG_CHECK_INTEGRITY被指定在open_flags,并且这个WIM文件在完整性检查里失败

WIMLIB_ERR_INVALID_CHUNK_SIZE：对于WIM的压缩类型，这个library没有认出压缩的chunk size

WIMLIB_ERR_INVALID_COMPRESSION_TYPE：这个library没有认出这个WIM的压缩类型

WIMLIB_ERR_INVALID_HEADER：WIM的头是无效的

WIMLIB_ERR_INVALID_INTEGRITY_TABLE：WIMLIB_OPEN_FLAG_CHECK_INTEGRITY被指定在open_flags中，并且这个WIM包含一个完成性表，但是这个完整性表是无效的

WIMLIB_ERR_INVALID_LOOKUP_TABLE_ENTRY：WIM的查找表是无效的

WIMLIB_ERR_INVALID_PARAM：wim_ret是null; 或者wim_file不是一个非空字符

WIMLIB_ERR_IS_SPLIT_WIM：这个WIM是一个分离的WIM并且WIMLIB_OPEN_FLAG_ERROR_IF_SPLIT 被指定在open_flags的时候

WIMLIB_ERR_NOT_A_WIM_FILE：检查WIM文件时，这个文件没有开始的magic字符

WIMLIB_ERR_OPEN：打开WIM读的时候失败。一些可能的原因：这个WIM文件不存在，或者调用流程没有权限去打开它

WIMLIB_ERR_READ：从WIM文件中读取数据失败

WIMLIB_ERR_UNEXPECTED_END_OF_FILE：当从WIM文件中读数据时，发生不期待的end-of-file

WIMLIB_ERR_UNKNOWN_VERSION:这个WIM的版本号没有认出

WIMLIB_ERR_WIM_IS_ENCRYPTED：这个WIM不能打开因为它包含了加密段

WIMLIB_ERR_WIM_IS_INCOMPLETE：这个WIM文件不是完整的

WIMLIB_ERR_WIM_IS_READONLY：WIMLIB_OPEN_FLAG_WRITE_ACCESS被指定但是这个WIM文件被认为是只读的

WIMLIB_ERR_XML ：这个WIM中的XML数据是无效的

##（2）Extracting WIMs

简介：从一个WIM中抽取文件，目录和镜像

###宏

    #define 	WIMLIB_EXTRACT_FLAG_NTFS   0x00000001
 	直接提取镜像目录去一个NTFS卷，而不是一个普通的目录
 
    #define 	WIMLIB_EXTRACT_FLAG_UNIX_DATA   0x00000020
    UNIX-like systems only: Extract special UNIX data captured with WIMLIB_ADD_FLAG_UNIX_DATA. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_NO_ACLS   0x00000040
 	不提取安全描述符
 
    #define 	WIMLIB_EXTRACT_FLAG_STRICT_ACLS   0x00000080
 	立刻失败如果在指定的WIM文件中不能准确设置任何文件和目录的full安全描述符
 
    #define 	WIMLIB_EXTRACT_FLAG_RPFIX   0x00000100
 	这个提取等同WIMLIB_ADD_FLAG_RPFIX. 
 
    #define 	WIMLIB_EXTRACT_FLAG_NORPFIX   0x00000200
 	Force reparse-point fixups on extraction off, regardless of the state of the WIM_HDR_FLAG_RP_FIX flag in the WIM header. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_TO_STDOUT   0x00000400
 	For wimlib_extract_paths() and wimlib_extract_pathlist() only: Extract the paths, each of which must name a regular file, to standard output. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_REPLACE_INVALID_FILENAMES   0x00000800
 	Instead of ignoring files and directories with names that cannot be represented on the current platform (note: Windows has more restrictions on filenames than POSIX-compliant systems), try to replace characters or append junk to the names so that they can be extracted in some form. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_ALL_CASE_CONFLICTS   0x00001000
 	On Windows, when there exist two or more files with the same case insensitive name but different case sensitive names, try to extract them all by appending junk to the end of them, rather than arbitrarily extracting only one. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_STRICT_TIMESTAMPS   0x00002000
 	不忽略在提取文件上设置时间戳的错误
 
    #define 	WIMLIB_EXTRACT_FLAG_STRICT_SHORT_NAMES   0x00004000
 	不忽略在提取文件上设置短名的错误
 
    #define 	WIMLIB_EXTRACT_FLAG_STRICT_SYMLINKS   0x00008000
 	不忽略由于权限问题的提取符号链接错误
 
    #define 	WIMLIB_EXTRACT_FLAG_RESUME   0x00010000
 	保存在未来使用
 
    #define 	WIMLIB_EXTRACT_FLAG_GLOB_PATHS   0x00040000
 	只为wimlib_extract_paths()和wimlib_extract_pathlist(): Treat the paths to extract as wildcard patterns ("globs") which may contain the wildcard characters ? and *. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_STRICT_GLOB   0x00080000
 	与WIMLIB_EXTRACT_FLAG_GLOB_PATHS结合, 产生一个错误 (WIMLIB_ERR_PATH_DOES_NOT_EXIST) 而不是提出一个提醒当one of the provided globs did not match a file. 
 
    #define 	WIMLIB_EXTRACT_FLAG_NO_ATTRIBUTES   0x00100000
 	不提取如只读，隐藏属性的Windows文件
 	
    #define 	WIMLIB_EXTRACT_FLAG_NO_PRESERVE_DIR_STRUCTURE   0x00200000
 	For wimlib_extract_paths() and wimlib_extract_pathlist() only: Do not preserve the directory structure of the archive when extracting — that is, place each extracted file or directory tree directly in the target directory. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_WIMBOOT   0x00400000
 	Windows only: Extract files as "pointers" back to the WIM archive. More...
 
    #define 	WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS4K   0x01000000
 	自wimlib v1.8.2以来并且Windows-only：压缩提取文件使用系统压缩，当需要的时候
 
    #define 	WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS8K   0x02000000
 	如WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS4K, 但是使用8192字节chunks进行XPRESS压缩
 
    #define 	WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS16K   0x04000000
 	如WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS4K, 但是使用16384字节chunks进行XPRESS压缩
 
    #define 	WIMLIB_EXTRACT_FLAG_COMPACT_LZX   0x08000000
 	如WIMLIB_EXTRACT_FLAG_COMPACT_XPRESS4K, 但是使用32768字节chunks进行XPRESS压缩
 	
###函数

####I.int 	wimlib_extract_image (WIMStruct *wim, int image, const wimlib_tchar *target, int extract_flags)
 	
从一个WIMStruct中提取一个镜像，或所有镜像
 
####II.int 	wimlib_extract_image_from_pipe (int pipe_fd, const wimlib_tchar *image_num_or_name, const wimlib_tchar *target, int extract_flags)
 	
提取一个镜像从一个正在传送的pipable WIM的管道中
 
####III.int 	wimlib_extract_image_from_pipe_with_progress (int pipe_fd, const wimlib_tchar *image_num_or_name, const wimlib_tchar *target, int          extract_flags, wimlib_progress_func_t progfunc, void *progctx)

和wimlib_extract_image_from_pipe()类似,但是允许指定一个progress函数
 
####IV.int 	wimlib_extract_pathlist (WIMStruct *wim, int image, const wimlib_tchar *target, const wimlib_tchar *path_list_file, int extract_flags)

和wimlib_extract_paths()类似, 但是从WIM镜像中提取的路径指定为ASCII, UTF-8, or UTF-16LE文本文件被path_list_file(它本身包含使用的路径表，每行一个)命名
 
####V.int 	wimlib_extract_paths (WIMStruct *wim, int image, const wimlib_tchar *target, const wimlib_tchar *const *paths, size_t num_paths, int extract_flags)

从指定的WIM镜像中提取0或更多路径（文件或目录树）
 
 
##(3)修改WIMs

简介：对一个WIMStruct进行改变，为保存WIMStuct到磁盘文件做准备

###捕获和增加镜像

正如上面基本WIM处理概念所描述。捕获一个新的WIM或者添加一个镜像到一个已存在的WIM是一个多步骤的过程。但是它的核心是 wimlib_add_image()或一个相同的函数。正常来讲， 

wimlib_add_image()得到一个磁盘目录树并且逻辑添加它到一个WIMStruct作为一个新镜像。然而，这里还有一种特别NTFS卷捕获模式（当WIMLIB_ADD_FLAG_NTFS被指定的时候进

入），它允许直接从一个没有挂载NTFS卷添加镜像。

另一个函数wimlib_add_image_multisource()也提供类似功能，但是它允许结合多个文件或目录树加入到一个单个WIM镜像

对于大多数WIM镜像的创建，也可以增加一个完全空的WIM镜像（使用wimlib_add_empty_image()），之后更新它（使用wimlib_update_image()）。

###删除镜像

wimlib_delete_image() 可以从一个WIMStruct中删除一个镜像。但是通常，必须调用wimlib_write()或者wimlib_overwrite()来其改变进行保存到磁盘的WIM文件中

###输出WIM镜像

wimlib_export_image() 可以从一个WIM复制，或者“输出”一个镜像到另一个WIM中。

###其他修改

wimlib_update_image()可以在一个WIM镜像中增加，删除和重命名文件

wimlib_set_image_name(), wimlib_set_image_descripton(), wimlib_set_image_flags(), wimlib_set_image_property()可以改变其他镜像metadata

wimlib_set_wim_info()可以改变关于WIM文件本身的信息，比如启动编号

###结构体

    wimlib_capture_source
    一个传给wimlib_add_image_multisource()数据结构数组，用来指定创建一个WIM镜像的源 
    struct  	wimlib_add_command
    用来 WIMLIB_UPDATE_OP_ADD 操作的数据
    struct  	wimlib_delete_command
    用来 WIMLIB_UPDATE_OP_DELETE 操作的数据
    struct  	wimlib_rename_command
    用来 WIMLIB_UPDATE_OP_RENAME 操作的数据
    struct  	wimlib_update_command
    在一个WIM镜像上执行指定的一个更新操作
    
###宏

###枚举

    enum  	wimlib_update_op { WIMLIB_UPDATE_OP_ADD = 0, WIMLIB_UPDATE_OP_DELETE, WIMLIB_UPDATE_OP_RENAME }
    用来指定执行的更新类型
    
###函数

    I.int 	wimlib_add_empty_image (WIMStruct *wim, const wimlib_tchar *name, int *new_idx_ret)
 	添加一个空镜像到一个WIMStruct中
 
    II.int 	wimlib_add_image (WIMStruct *wim, const wimlib_tchar *source, const wimlib_tchar *name, const wimlib_tchar *config_file, int add_flags)
 	增加一个镜像（从一个磁盘目录树或者NTFS卷）到WIMStruct中
 
    III.int 	wimlib_add_image_multisource (WIMStruct *wim, const struct wimlib_capture_source *sources, size_t num_sources, const wimlib_tchar *name, const wimlib_tchar *config_file, int add_flags)
 	这个函数和wimlib_add_image()相同，除了它允许多个源结合到单一的WIM镜像
 
    IV.int 	wimlib_add_tree (WIMStruct *wim, int image, const wimlib_tchar *fs_source_path, const wimlib_tchar *wim_target_path, int add_flags)
 	增加文件或者目录树Add the file or directory tree at fs_source_path on the filesystem to the location wim_target_path within the specified image of the wim. More...
 
    V.int 	wimlib_delete_image (WIMStruct *wim, int image)
 	从一个WIMStruct中删除一个镜像，或者所有镜像
 
    VI.int 	wimlib_delete_path (WIMStruct *wim, int image, const wimlib_tchar *path, int delete_flags)
 	从wim指定的镜像中删除路径
 
    VII.int 	wimlib_export_image (WIMStruct *src_wim, int src_image, WIMStruct *dest_wim, const wimlib_tchar *dest_name, const wimlib_tchar *dest_description, int export_flags)
 	从一个WIMStruct到另一个WIMStruct中输出一个镜像或者所有镜像
 
    VIII.int 	wimlib_reference_template_image (WIMStruct *wim, int new_image, WIMStruct *template_wim, int template_image, int flags)
    声明一个新的增加镜像大部分和之前的镜像相同，但是捕获在之后的时间点，在间隔时间可能有一些修改
 
    IX.int 	wimlib_rename_path (WIMStruct *wim, int image, const wimlib_tchar *source_path, const wimlib_tchar *dest_path)
 	在wim指定的镜像中重命名source_path到dest_path
 	
    X.int 	wimlib_set_image_descripton (WIMStruct *wim, int image, const wimlib_tchar *description)
 	改变一个WIM镜像的描述符
 	
    XI.int 	wimlib_set_image_flags (WIMStruct *wim, int image, const wimlib_tchar *flags)
 	改变在WIM的XML文档中存在<FLAGS>中的内容
 	
    XII.int 	wimlib_set_image_name (WIMStruct *wim, int image, const wimlib_tchar *name)
 	改变WIM镜像的名字
 
    XIII.int 	wimlib_set_image_property (WIMStruct *wim, int image, const wimlib_tchar *property_name, const wimlib_tchar *property_value)
 	自从wimlib1.8.3：从WIM的XML文档中增加，修改或者移除一个per-image property
 
    XIV.int 	wimlib_set_wim_info (WIMStruct *wim, const struct wimlib_wim_info *info, int which)
 	设置关于WIM的基本信息
 
    XV.int 	wimlib_update_image (WIMStruct *wim, int image, const struct wimlib_update_command *cmds, size_t num_cmds, int update_flags)
 	更新一个WIM镜像通过增加，删除，重命名文件或目录
    
##(4)写和覆写WIMs

简介：在一个磁盘WIM文件中创建或者更新。wimlib_write()创建一个新的磁盘WIM文件，而wimlib_overwrite()更新一个已经存在WIM文件

###宏

###函数

    I.int 	wimlib_overwrite (WIMStruct *wim, int write_flags, unsigned num_threads)
 	提交一个WIMStruct到磁盘，更新它的备份文件
 
    II.int 	wimlib_set_output_chunk_size (WIMStruct *wim, uint32_t chunk_size)
 	设置一个WIMStruct的输出压缩chunk size
 
    III.int 	wimlib_set_output_pack_chunk_size (WIMStruct *wim, uint32_t chunk_size)
 	和wimlib_set_output_chunk_size()相似,但是对写固态资源设置chunk size 
 	
    IV.int 	wimlib_set_output_compression_type (WIMStruct *wim, enum wimlib_compression_type ctype)
 	设置一个WIMStruct的输出压缩类型
 
    V.int 	wimlib_set_output_pack_compression_type (WIMStruct *wim, enum wimlib_compression_type ctype)
 	和wimlib_set_output_compression_type()相似,但是对写固态资源设置压缩类型 
 
    VI.int 	wimlib_write (WIMStruct *wim, const wimlib_tchar *path, int image, int write_flags, unsigned num_threads)
 	对一个新的磁盘WIM文件写一个WIMStructs
 	
    VII.int 	wimlib_write_to_fd (WIMStruct *wim, int fd, int image, int write_flags, unsigned num_threads)
 	和wimlib_write()类似, 但是直接对一个文件描述符写WIM
 	
#3.使用相关描述

##（1）wimlib-imagex append

##（2）wimlib-imagex apply

##（3）wimlib-imagex capture

##（4）wimlib-imagex delete

##（5）wimlib-imagex dir 

##（6）wimlib-imagex export arguments...

##（7）wimlib-imagex extract arguments...

##（8）wimlib-imagex info arguments...

##（9）wimlib-imagex join arguments...

##（10）wimlib-imagex mount arguments...

##（11）wimlib-imagex mountrw arguments...

##（12）wimlib-imagex optimize arguments...

##（13）wimlib-imagex split arguments...

##（14）wimlib-imagex unmount arguments...

##（15）wimlib-imagex update arguments...

##（16）wimlib-imagex verify arguments...

