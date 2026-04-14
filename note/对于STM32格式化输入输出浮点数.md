## 对于STM32格式化输入输出浮点数 

默认情况下，`arm-none-eabi-gcc` 的 `newlib-nano` 库不仅禁用 `printf` 浮点输出，也禁用 `scanf` 浮点输入

GCC 嵌入式标准库默认禁用浮点格式化以节省 Flash

| 配置                     | Flash 增量 | RAM 增量 | 说明                    |
| ------------------------ | ---------- | -------- | ----------------------- |
| 仅 `-u _printf_float`    | ~3~5KB     | ~100B    | 仅支持浮点输出          |
| 同时加 `-u _scanf_float` | ~6~8KB     | ~200B    | 同时支持浮点输入 + 输出 |

如果要格式化输入输出**浮点数**（`printf/%f`、`scanf/%f`），或者启用 `scanf`/`fscanf` 等函数对浮点数（`%f`/`%lf`）的解析能力

**对于有硬件浮点单元 FPU 配置（F4/F7/H7/L4/L5+）**

对于**STM32Cubemx+EIDE+VsCode**

开启硬件浮点选项

![image-20260321003845075](C:\Users\13981\AppData\Roaming\Typora\typora-user-images\image-20260321003845075.png)

打开硬件浮点ABI

![image-20260321004032037](C:\Users\13981\AppData\Roaming\Typora\typora-user-images\image-20260321004032037.png)

需要配置连接器 构建选项

![image-20260321002422469](C:\Users\13981\AppData\Roaming\Typora\typora-user-images\image-20260321002422469.png)

![image-20260321002448852](C:\Users\13981\AppData\Roaming\Typora\typora-user-images\image-20260321002448852.png)

Enable float-point 两个选项



对于 **STM32Cubemx+cmake+VsCode**：

cmke 自动生成浮点编译

只需要在cmakelist里加入

```cmake
target_link_options(${PROJECT_NAME} PRIVATE
  -u _printf_float # 强制启用 printf/sprintf 浮点数格式化
  -u _scanf_float
)
```

