I don't know if this correct or not, but:

```
#include <linux/uaccess.h>

vfs_stat ( char *path, struct kstat *kstat )
```

`vfs_stat()` — retrieve information about the file pointed to by `path`. Read `man 2 stat`.

About `struct kstat` see file linux-source/include/linux/stat.h:
```
#include <asm/stat.h>
#include <uapi/linux/stat.h>

#define S_IRWXUGO       (S_IRWXU|S_IRWXG|S_IRWXO)
#define S_IALLUGO       (S_ISUID|S_ISGID|S_ISVTX|S_IRWXUGO)
#define S_IRUGO         (S_IRUSR|S_IRGRP|S_IROTH)
#define S_IWUGO         (S_IWUSR|S_IWGRP|S_IWOTH)
#define S_IXUGO         (S_IXUSR|S_IXGRP|S_IXOTH)

#define UTIME_NOW       ((1l << 30) - 1l)
#define UTIME_OMIT      ((1l << 30) - 2l)

#include <linux/types.h>
#include <linux/time.h>
#include <linux/uidgid.h>

struct kstat {
        u64             ino;
        dev_t           dev;
        umode_t         mode;
        unsigned int    nlink;
        kuid_t          uid;
        kgid_t          gid;
        dev_t           rdev;
        loff_t          size;
        struct timespec  atime;
        struct timespec mtime;
        struct timespec ctime;
        unsigned long   blksize;
        unsigned long long      blocks;
};
```

Function source code: http://lxr.linux.no/linux+v4.6.4/fs/stat.c#L121

A wrapper example:

```
int file_stat ( char *fpath, struct kstat stat )
{
  int error;
  mm_segment_t old_fs;

  old_fs = get_fs();
  set_fs(KERNEL_DS);

  error = vfs_stat (fpath, &stat);
  set_fs(old_fs);

  return error;
}
```

Getting a file size:

```
static int __init my_module_init()
{
  struct kstat stat
  mm_segment_t old_fs;
  old_fs = get_fs();
  set_fs(KERNEL_DS);
  vfs_stat ("/bin/ls", &stat)
  printk (KERN_INFO "mode of ls: %o\n", stat.size);
  set_fs(old_fs);
}

module_init ( my_module_init )
```