# AOAC唤醒按键补丁

## 描述

- ***SSDT-DeepIdle*** 补丁可以使机器进入深度空闲状态，延长机器待机时间。但同时也会出现：
  - 唤醒机器比较困难，需要采取特殊方法来唤醒机器
  - `AOAC` 睡眠的独特方式可能导致唤醒后电池图标无法及时更新
- 有关***SSDT-DeepIdle*** 方面的内容参见《电源空闲管理》。

## AOAC唤醒方法

- 电源键
- 指定某个按键作为唤醒按键，添加必要的补丁。补丁要求
  - 满足 `PNP0C0D` 唤醒条件
  - 执行电池刷新指令

## AOAC唤醒按键补丁

- 通常情况下，`ACPI` 的`_WAK`包含了上述唤醒方法中按键补丁所要求的内容，因此直接调用`_WAK`即可，如：

  ```
  		External (_WAK, MethodObj)
  		......
  		If (_OSI ("Darwin"))
  		{
  				\_WAK (0x03)
  		}
  		......
  ```

- ***SSDT-FnQ-AOACWake*** ——小新PRO13唤醒按键补丁示例

  按键：`Fn+Q` ，`ACPI` 位置 `_Q50` ，具体内容见补丁文件。

## 附件

- **PNP0C0D唤醒条件** 
  - `_LID`  返回 `One` 。 `_LID` 是 `PNP0C0D` 设备当前状态
  - 执行 `Notify(***.LID0, 0x80)`。 `LID0` 是 `PNP0C0D` 设备名称
- **电池刷新** 
  - 执行`Notify(***.BAT0, 0x80)` 
  - 执行`Notify(***.BAT0, 0x81)` 