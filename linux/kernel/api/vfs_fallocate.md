I don't know if this correct or not, but here a manpage ugly draft:

```
vfs_fallocate ( struct file *file, int mode, off_t offset, off_t len )
```

Read `man 2 fallocate`.

A wrapper example:
```
int file_allocate ( struct file *file, int mode, off_t offset, off_t len )
{
  int error;
  mm_segment_t old_fs;

  old_fs = get_fs();
  set_fs(KERNEL_DS);

  error = vfs_fallocate ( file, mode, offset, len ); 
  set_fs(old_fs);

  return error;
}
```