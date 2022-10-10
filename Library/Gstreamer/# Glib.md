# Glib

## Option

* 基本数据结构

```
struct GOptionEntry 
{
  const gchar* long_name;   // --完整命令  ，--name
  gchar short_name;               //短命令，  -n
  gint flags;                                //GOptionFlags枚举的值
  GOptionArg arg;                   // GOptionArg枚举的值
  gpointer arg_data;                 // 解析出来的数据，所要存储的位置
  const gchar* description;   // 参数描述，--help可以查看到
  const gchar* arg_description;
}


```
* g_malloc0() 函数

```
gpointer g_malloc0 (gsize n_bytes) 

Allocates n_bytes bytes of memory, initialized to 0’s. If n_bytes is 0 it returns NULL.

```

* gpointer g_memdup2 (gconst pointer mem,gsize byte_size)

```
Allocates byte_size bytes of memory, and copies byte_size bytes into it from mem. If mem is NULL it returns NULL.

This replaces g_memdup(), which was prone to integer overflows when converting the argument from a #gsize to a #guint.

```


## glib 解析配置文件
 
 函数
 ```
1. GKeyFile* g_key_file_new (void)
 ```
 description # 
 Creates a new empty GKeyFile object. Use g_key_file_load_from_file(), g_key_file_load_from_data(), g_key_file_load_from_dirs() or g_key_file_load_from_data_dirs() to read an existing key file.

2. 

```
gboolean g_key_file_load_from_file
 (
  GKeyFile* key_file,
  const gchar* file,
  GKeyFileFlags flags,
  GError** error
)

```

3. 

```
gchar** g_key_file_get_keys (
  GKeyFile* key_file,
  const gchar* group_name,
  gsize* length,
  GError** error
)
```
description #  
Returns all keys for the group name group_name. The array of returned keys will be NULL-terminated, so length may optionally be NULL. In the event that the group_name cannot be found, NULL is returned and error is set to

Tags:
  3rdlib, gllib